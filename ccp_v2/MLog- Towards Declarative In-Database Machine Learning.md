MLog: Towards Declarative In-Database Machine Learning

Xupeng Li†

Bin Cui†

Yiru Chen† Wentao Wu∗ Ce Zhang‡

†School of EECS & Key Laboratory of High Conﬁdence Software Technologies (MOE),
Peking University {lixupeng, bin.cui, chen1ru}@pku.edu.cn
∗Microsoft Research, Redmond wentao.wu@microsoft.com
‡ETH Zurich ce.zhang@inf.ethz.ch

ABSTRACT
We demonstrate MLOG, a high-level language that integrates ma-
chine learning into data management systems. Unlike existing ma-
chine learning frameworks (e.g., TensorFlow, Theano, and Caffe),
MLOG is declarative, in the sense that the system manages all
data movement, data persistency, and machine-learning related op-
timizations (such as data batching) automatically. Our interactive
demonstration will show audience how this is achieved based on
the novel notion of tensoral views (TViews), which are similar
to relational views but operate over tensors with linear algebra.
With MLOG, users can succinctly specify not only simple mod-
els such as SVM (in just two lines), but also sophisticated deep
learning models that are not supported by existing in-database an-
alytics systems (e.g., MADlib, PAL, and SciDB), as a series of
cascaded TViews. Given the declarative nature of MLOG, we fur-
ther demonstrate how query/program optimization techniques can
be leveraged to translate MLOG programs into native TensorFlow
programs. The performance of the automatically generated Tensor-
Flow programs is comparable to that of hand-optimized ones.

1.

INTRODUCTION

As of 2016, it is no longer easy to argue against the importance of
supporting machine learning in data systems. In fact, most modern
data management systems support certain types of machine learn-
ing and analytics. Notable examples include MADlib, SAP PAL,
MLlib, and SciDB. These systems are tightly integrated into the re-
lational data model, but treat machine learning as black-box func-
tions over relations/tensors. This results in a lack of ﬂexibility in
the types of machine learning models that can be supported.

On the other hand, machine learning systems such as Tensor-
Flow, Theano, and Caffe are made much more expressive and ﬂex-
ible by exposing the mathematical structure of machine learning
models to the users. However, these systems do not have a declar-
ative data management layer — it is the user’s responsibility to
deal with such tedious and often error-prone tasks as data loading,
movement, and batching in these systems.

Given these limitations, it is not surprising that higher-level ma-
chine learning libraries such as Keras have become increasingly

licensed under

This work is
the Creative Commons Attribution-
NonCommercial-NoDerivatives 4.0 International License. To view a copy
of this license, visit http://creativecommons.org/licenses/by-nc-nd/4.0/. For
any use beyond those covered by this license, obtain permission by emailing
info@vldb.org.

Copyright 2017 VLDB Endowment 2150-8097/17/08.

Figure 1: Schematic Comparison with Existing Systems.

popular. Given the data as a numpy array, Keras is fully declar-
ative and users only need to specify the logical dependencies be-
tween these arrays, rather than specifying how to solve the result-
ing machine learning model. However, a system like Keras does not
provide data management for these numpy arrays and it is still the
user’s responsibility to take care of low-level programming details
such as whether these arrays ﬁt in memory, or textbook database
functionality such as reuse computation across runs or “time travel”
through training snapshots. Moreover, Keras cannot be integrated
into the standard relational database ecosystem that hosts most of
the enterprise data.

In this paper, we demonstrate MLOG, a system that aims for
marrying Keras-like declarative machine learning to SciDB-like
declarative data management. In MLOG, we build upon a standard
data model similar to SciDB, to avoid neglecting and reinventing
decades of study of data management. Our approach is to extend
the query language over the SciDB data model to allow users to
specify machine learning models in a way similar to traditional re-
lational views and relational queries. Speciﬁcally, we demonstrate
the following three main respects of MLOG:
(Declarative Query Language) We demonstrate a novel query
language based on tensoral views that has formal seman-
tics compatible with existing relational-style data models and
query languages. We also demonstrate how this language al-
lows users to specify a range of machine learning models,
including deep neural networks, very succinctly.

(Automated Query Optimization) We demonstrate how to auto-
matically compile MLOG programs into native TensorFlow
programs using textbook static analysis techniques.

(Performance) We demonstrate the performance of automatically
generated TensorFlow programs on a range of machine learn-
ing tasks. We show that the performance of these programs
is often comparable to (up to 2× slower than) manually op-
timized TensorFlow programs.

Limitations. As a preliminary demonstration of our system, the
current version of MLOG has the following limitations. First, our
ultimate goal is to fully integrate MLOG into SciDB and Post-

1933

Data Model IntegrationQuery Language IntegrationSciDBMADlibSAP PALOur  GoalRelational ModelTensor ModelRelational  QueriesBlackbox ML FunctionsLinear Algebra PrimitivesMathematical Optimization QueriesFigure 2: MLOG Examples for (a) Matrix Factorization and (b) Recurrent Neural Network. (c) User Interface of MLOG.

greSQL such that it runs on the same data representation and users
can use a mix of MLOG and SQL statements. Although our formal
model provides a principled way for this integration, this function-
ality has not been implemented yet. In this demonstration, we focus
on the machine learning component and leave the full integration
as future work. Second, the current MLOG optimizer does not con-
duct special optimizations for sparse tensors. (In fact, it does not
even know the sparsity of the tensor.) Sparse tensor optimization is
important for a range of machine learning tasks, and we will sup-
port it in the near future. Third, the current performance of MLOG
can still be up to 2× slower than hand-optimized TensorFlow pro-
grams. It is our ongoing work to add more optimization rules into
the optimizer and to build our system on a more ﬂexible backend,
e.g. Angel [6, 5]. Despite these limitations, we believe the current
MLOG prototype can stimulate discussions with the audience about
the ongoing trend of supporting machine learning in data manage-
ment systems.
Reproducibility and Public Release. The online demo will
be up before the conference. For now, all MLOG programs, hand-
crafted TensorFlow programs, and automatically generated Tensor-
Flow programs are available at github.com/DS3Lab/MLog.

2. THE MLOG LANGUAGE

In this section, we present basics for the audience to understand

the syntax and semantics of the MLOG language.

2.1 Data Model

The data model of MLOG is based on tensors–all data in MLOG
are tensors and all operations are a subset of linear algebra over
tensors. In MLOG, the tensors are closely related to the relational
model; in fact, logically, a tensor is deﬁned as a special type of re-
lation. Let T be a tensor of dimension dim(T ) and let the index of
each dimension j range from {1, ..., dom(T, j)}. Logically, each
tensor T corresponds to a relation R
with dim(T )+1 attributes
T
(cid:74)
(a1, ..., adim(T ), v), where the domain of aj is {1, ..., dom(T, j)}
and the domain of v is the real space R. Given a tensor T ,

(cid:75)

R

T
(cid:74)

(cid:75)

= {(a1, ..., adim(T ), v)|T [a1, ..., adim(T )] = v},

where T [a1, ..., adim(T )] is the tensor indexing operation that gets
the value at location (a1, ..., adim(T )).

Algebra over Tensors. We deﬁne a simple algebra over tensors
and deﬁne its semantics with respect to R
−
, which allows us to
(cid:75)
(cid:74)
tightly integrate operation over tensors into a relational database
and Spark with uniﬁed semantics. This algebra is very similar to
DataCube with extensions that support linear algebra operations.
We illustrate it with two example operators:

1. Slicing σ. The operator σ¯x(T ) subselects part of the input
tensor and produces a new “subtensor.” The j-th element
of ¯x, i.e., ¯xj ∈ 2{1,...,dom(T,j)}, deﬁnes the subselection
on dimension j. The semantic of this operator is deﬁned as
R

=

σ¯x(T )
(cid:74)

(cid:75)

{(a1, ..., adim(T ), v)|aj ∈ ¯xj∧(a1, ..., adim(T ), v) ∈ R

}.
(cid:75)
2. Linear algebra operators op. We support a range of linear
algebra operators, including matrix multiplication and con-
volution. These operators all have the form op(T1, T2) and
their semantics are deﬁned as R

T
(cid:74)

=

op(T1, T2)
(cid:74)

(cid:75)

{(a1, ..., adim(T ), v)|op(T1, T2)[a1, ..., adim(T )] = v}.

2.2 MLog Program

An MLOG program Π consists of a set of TRules (tensoral rules).

TRule. Each TRule is of the form

T (¯x) : −op (T1(¯x1), ..., Tn(¯xn)) ,

where n ≥ 0. Similar to Datalog, we call T (¯x) the head of the
rule, and T1(¯x1), ..., Tn(¯xn) the body of the rule. We call op the
operator of the rule. Each ¯xi and ¯x speciﬁes a subselection that
can be used by the slicing operator σ. To simplify notation, we use
“−” to donate the whole domain of each dimension–for example,
if ¯x = (5, −), σ¯x(T ) returns a subtensor that contains the entire
ﬁfth row of T . We deﬁne the forward evaluation of a TRule as the
process that takes as input the current instances of the body tensors,
and outputs an assignment for the head tensor by evaluating op.
Semantics. Similar to Datalog programs, we can deﬁne ﬁxed-
point semantics for MLOG programs. Let I be a data instance that
contains the current result of each tensor. We deﬁne the immediate
consequence of program Π on I as SΠ(I), which contains I and
all forward evaluation results for each TRule. The semantic of an
MLOG program is the least ﬁxed point of SΠ(I), i.e., S∞
Π (I) = I.

1934

(UFEAT, MFEAT) <- \argmin_{UFEAT, MFEAT} LOSS>LOSS    <- \sum_{i,j} (R_{i,j} - RATINGS_{i,j})^2;R_{i,j} <- UFEAT_{i,-} * MFEAT_{j,-}’;>>create tensor MFEAT  (movie, fid 1:100) -> feature;create tensor UFEAT  (user, fid 1:100) -> feature;create tensor RATINGS(user, movie) -> rating;>>>SchemaViewQuery(1) MLog Program(2) Datalog ProgramLOSS(v) :- R(i,j,v1), RATINGS(i,j,v2), v=op(i,j,v1,v2)R(i,j,v) :- UFEAT(i,j1,v1), MFEAT(j,j2,v2), v=op(j1,v1,j2,v2)(3.2) Executable Program“Datalog-ify”(3.1) Human-readable MathStatic Analysis & OptimiserY_{s,t}   <- \sigma(Wy*H_{s,t,-});H_{s,t,-} <- \sigma(Wh*X_{s,t,-} + Uh*H_{s,t-1,-});>>create tensor Y(sent, word)-> feat;create tensor H(sent, word, fid 1:100)-> feat;create tensor X(sent, word, fid 1:100)-> feat;>>>SchemaView(1) MLog Program(2) Datalog ProgramH(s,t,v) :- Wh(w),X(s,t,v1),Uh(u),H(s,t-1,v2), v=op(w,v1,u,v2)H(s,t,v) :- sent(s), t=0, v=0(3.2) Executable Program“Datalog-ify”(3.1) Human-readable MathStatic Analysis & OptimiserH_{s,0,-} <- 0;>create tensor ANS(sent, word)-> feat;>LOSS      <- \sum_{i,j} (Y_{s,t} - ANS_{s,t})^2;>(Wh, Wy, Uh) <- \argmin_{Wh, Wy, Uh} LOSS>Querycreate tensor Uh(fid 1:100, fid 1:100)-> feat;create tensor Wh(fid 1:100, fid 1:100)-> feat;>>create tensor Wy(fid 1:100)-> feat;>LOSS(v) :- Y(s,t,v1), ANS(s,t,v2), v=op(s,t,v1,v2)Y(s,t,v) :- Wy(w),H(s,t,v1), v=op(w,v1)(a)(b)(c)Query. Given an MLOG program Π and an initial data instance
I0, there are two ways to query the system. The forward query is
similar to querying a Datalog program—calculating the least ﬁxed
point of the program S∞
Π (I). The MLOG language also supports
another type of query that we call backward query, which we de-
ﬁne in more detail below. A backward query consists of a tuple
(T, ¯x, T1, ..., Tm), which returns

arg max

T1,...,Tm

S∞

Π (I ∪ T1... ∪ Tm).T [¯x].

That is, it tries to ﬁnd the optimal instance of T1, ..., Tm to mini-
mize T [¯x] as a result of a forward query, given T1, ..., Tm.

3. USER INTERACTION MODEL

Like most SQL databases, users interact with our system by ex-
ecuting a sequence of MLOG statements in a REPL or a script.
Each MLOG statement can be either a standard SQL statement, a
TView, an MLOG query, or an MLOG tensor construction state-
ment. Figure 2 illustrates an example MLOG REPL session for
matrix factorization and recurrent neural network.
Tensor Construction Statements. The MLOG language ex-
tends the data deﬁnition sublanguage of SQL by adding a CREATE
TENSOR primitive that is largely consistent with SciDB’s syntax
and semantics. Moreover, MLOG also supports the operation that
“casts” a relational table into a tensor and loads data from ﬁles.
Tensoral Views (TViews). Figure 2(b) illustrates three TViews
that deﬁne the following relationships between tensors:

Hs,0 = 0,
Hs,t = σ(W h ∗ Xs,t + U h ∗ Hs,t−1),
Ys,t = σ(W y ∗ Hs,t),

(1)

(2)
(3)

which encode a standard recurrent neural network model. Each
TView corresponds to one formula. In this example, there is a re-
cursive relationship between the tensor H and itself — the value
of one slice of the tensor Hs,t depends on the value of the “previ-
ous slice” Hs,t−1. The ﬁxed point semantics we introduced in the
previous section provides well-founded semantics for this scenario.
MLOG Queries. Figure 2(b) also illustrates one query in MLOG
that corresponds to the following optimization problem:

arg max

W h,U j,W y

LOSS,

where LOSS is a TView deﬁned over, among others, W h, U j,
and W y. This query will ﬁnd the optimal instance for W h, U j,
and W y that maximizes the value of LOSS.
3.1 Query Optimization

The execution of MLOG programs follows a typical procedure in
a database system: the input high-level language is ﬁrst converted
into a logical plan, and then an automated optimizer translates the
logical plan into a physical plan. In MLOG, the logical plan is a set
of TViews and the physical plan is a TensorFlow (or other backend)
program. We next outline some query optimization techniques we
have employed to compile MLOG programs.

Query optimization is undertaken by ﬁrst translating an MLOG
program into a Datalog program, a process that we call “Data-
logify.” Given the Datalog program, the optimizer uses a standard
static analysis technique to reason about the property of the pro-
gram and generate a TensorFlow program as the physical plan. We
illustrate this process by using the following query in a recurrent
neural network as a running example:

Hs,t,− = σ(W h ∗ Xs,t,− + U h ∗ Hs,t−1,−),

where H and X are 3D tensors, and W h and U h are 2D matrices.

“Datalogify”. The goal of “Datalogify”-ing an MLOG program
is to analyze the data dependencies between tensors and provide
a way to optimize the execution statically without grounding out
the whole dependency graph. During this process, each TView is
translated into a conjunctive aggregate query [2]. The process is
simple: for each tensor T in the rule, we replace it with its relational
representation R
. We abuse notation by still using the symbol
(cid:75)
T
T for R
(cid:74)

, and the “Datalogify”-ed RNN query is:
(cid:75)

T
(cid:74)

H(s, t, v) : −W h(w), X(s, t, v1), U h(u), H(s, t − 1, v2),

v = σ(w, v1, u, v2).

We can infer many properties of this query by analyzing it stat-
ically. For example, for each s, the forward process forms a chain
(because of t − 1 and t) and the length of the chain for a given s is
decided by |{(s, t, v1) ∈ X}|, a quantity that one can obtain with
a standard database optimizer. Second, to calculate for each (s, t),
the whole relation of W h and U h will be used. One can use this
fact to estimate the communication overhead of broadcasting W h
and U h for different execution strategies. The MLOG optimizer
takes advantage of these, and we describe one example that leads
to one of the most signiﬁcant improvements of performance.
Batch Optimization with Pivoting Method. One important
optimization for speeding up machine learning is data batching.
For example, in our previous work [3], we ﬁnd that deep learning
systems without proper batch optimization can be up to 5× slower.
As a declarative language, MLOG does not expose the batching
operation to the user. Therefore, it is important to automatically
detect batching opportunities.

The MLOG optimizer detects batching optimization using a text-
book static analysis technique for Datalog programs called the piv-
oting method [11]. The goal is to detect connected components in
a Datalog program — components that can be evaluated in parallel
without communication. For example, in the RNN query, different
values of s form different connected components conditioned on
W h and U h, which can be easily detected by the pivoting method.
After discovering these batching opportunities, the MLOG op-
timizer automatically expands all involved n-dimensional tensors
into n + 1-dimensional tensors by adding the batch dimension. For
example, the 3D-tensors H and X will become 4D-tensors in the
physical plan. All operators such as matrix multiplication (e.g.,
∗) will also be translated into the batched version. Note that in the
above example, each chain is of a different length and zero-padding
is a standard practice for batching them together — although this
is not currently implemented: MLOG’s optimizer has enough in-
formation to choose batching different chains together to minimize
zero-padding overhead because it knows how to estimate the length
of each chain with a standard cardinality estimator.

4. DEMONSTRATION SCENARIOS
Predeﬁned Models. We demonstrate three example MLOG pro-
grams that implement three popular machine learning models—
Support Vector Machines (SVM), Convolutional Neural Networks
(CNN), and Long Short Term Memory networks (LSTM). These
models cover a large range of machine learning applications —
SVM for linear classiﬁcation, CNN for image processing, and LSTM
for speech and natural language processing. To the best of our
knowledge, none of the existing in-database analytics systems sup-
ports all of these models.
Data Sets. We will use three public data sets and implement the
three predeﬁned models using MLOG:

1935

Time per Batch (ms)

Dataset
epsilon
CIFAR-10
LMDB

MLog
16.9
119.6
365.3

TF
16.8
118.3
224.7

Figure 3: Experiment results on GPU

epsilon (SVM) is a binary classiﬁcation dataset. The dataset has
400,000 training examples and 100,000 testing examples. Each ex-
ample is a pair (xi, yi), where xi ∈ R2000 and yi ∈ {1, −1}. We
implement an SVM model with hinge loss in MLOG.

CIFAR-10 (CNN) is a ten-class image dataset. It contains 50,000
training and 10,000 testing images. The size of each image is
32 × 32 × 3. We implement a ﬁve-layer NiN CNN [8] in MLOG.
LMDB (LSTM) is a binary-class short English text paragraphs
dataset. It contains 25,000 training and 25,000 testing examples.
We make a vocabulary of 20,000 words with highest frequency in
the training set. We then implement a one-layer LSTM model fol-
lowed by logistic regression in MLOG.
Scenarios. As our ﬁrst scenario prepared before the presenta-
tion, we will compare the performance of the automatically gen-
erated TensorFlow programs (by the MLOG optimizer) with that
of the ones we manually optimized.1 We will evaluate the perfor-
mance on both CPU and GPU. The CPU machine contains an Intel
Xeon E5530 @ 2.40GHz CPU and 70GB RAM and the GPU ma-
chine contains a Pascal TITAN X GPU and 256GB RAM. We will
measure the training loss and testing accuracy.

Here we present preliminary results that we will showcase dur-
ing our presentation. Figure 3 shows the results on GPU. We see
that both MLOG and TensorFlow achieve the same accuracy and
loss on all three datasets. In terms of performance, both systems
have comparable speed per batch or epoch for SVM and CNN. For
LSTM, MLOG takes 1.6× longer than TensorFlow — in Tensor-
Flow we use an optimized LSTM layer as a whole while in MLOG
the full structure of the LSTM layer is written at the level of ma-
trix multiplication. Clearly, in this case, MLOG needs more op-
timizations to match Tensorﬂow’s performance; after performance
analysis, we ﬁnd that this difference is because TensorFlow merges
several matrix multiplications to one so that it can get higher GPU
utilization (about 32% in MLOG whereas 40% in TensorFlow). For
example, AW, BW can be calculated in one matrix multiplication
[AT BT ]T W as long as A and B have the same shape. For CPUs,
we get the same accuracy and performance for all three data sets.

As our second scenario, we will invite the audience to try out
their own MLOG programs via the user interface presented in Fig-
ure 2. Foreseeably, the audience may need some warm-up before
they can write simple yet correct MLOG programs. We plan to
provide some guidance during this “training” phase. We will then
show the automatically generated TensorFlow programs to the au-
dience. However, before that we would like to encourage the audi-
ence to come up with their own TensorFLow programs ﬁrst. (We
can ease this task by choosing standard machine learning models
with publicly available TensorFlow implementations.) We expect

1https://github.com/DS3Lab/MLog

some intriguing and perhaps intensive technical questions and dis-
cussions regarding query optimization here, most likely from peo-
ple with deeper expertise in TensorFlow and/or general query op-
timization. We ﬁnally run the automatically generated and user-
provided TensorFlow programs to compare their performance.
5. RELATED WORK

Modern data systems often support libraries for analytics and
machine learning. Examples include MADlib [4] for Greenplum
and PostgreSQL, SAP PAL [9] for SAP HANA, ORE for Oracle
databases. These libraries tightly integrate with the host data sys-
tem and support traditional machine learning algorithms such as
SVM or K-means. Our work is built upon these previous work,
however, advocates a more ﬂexible higher-level language that sup-
ports more sophisticated machine learning models, such as deep
neural networks, inside existing data systems. SciDB [1] is a re-
cent effort to extend relational database with data representations
and operations for linear algebra. However, as far as we know there
is no machine learning library existing for SciDB, and we hope
MLOG could ﬁll that vacancy. There have been efforts to train lin-
ear models over joins [7, 10]. Compared with these efforts, MLOG
advocates a more uniﬁed data model based on tensors instead of
relations and also provides a more expressive way to encode cor-
relations among tensors. As a result, MLOG is able to encode so-
phisticated machine learning models beyond linear models.
6. CONCLUSION

We have demonstrated MLOG, a high-level declarative language
that integrates machine learning into database systems. An MLOG
program is very similar to a SQL program but extends relational
algebra over relations to linear algebra over tensors. This extension
allows MLOG to encode a range of machine learning models that
are not supported in current data analytics systems. To optimize the
performance of an MLOG program, MLOG contains a database-
style query optimizer. In many cases, the resulting performance of
automatically compiled MLOG programs is comparable with hand-
tuned TensorFlow programs.
Acknowledgement. Bin Cui is supported by the NSFC under Grant
No. 61572039 and U1536201, 973 program under No. 2014CB340
405, and Tecent Research Grant (PKU). Ce Zhang gratefully ac-
knowledges the support from the Swiss National Science Founda-
tion NRP 75 407540 167266, NVIDIA Corporation for its GPU
donation, and Microsoft Azure for Research award program.
7. REFERENCES
[1] P. G. Brown. Overview of scidb: Large scale array storage,

processing and analysis. In SIGMOD, 2010.

[2] S. Cohen, W. Nutt, and Y. Sagiv. Deciding equivalences among

conjunctive aggregate queries. J. ACM, 2007.

[3] S. Hadjis, F. Abuzaid, C. Zhang, and C. R´e. Caffe Con Troll: Shallow

ideas to speed up deep learning. In DanaC, 2015.

[4] J. M. Hellerstein et al. The MADlib analytics library: Or mad skills,

the sql. Proc. VLDB Endow., 2012.

[5] J. Jiang, B. Cui, C. Zhang, and L. Yu. Heterogeneity-aware
distributed parameter servers. In SIGMOD. ACM, 2017.

[6] J. Jiang, L. Yu, J. Jiang, Y. Liu, and B. Cui. Angel: a new large-scale

machine learning system. National Science Review, 2017.

[7] A. Kumar, J. Naughton, and J. M. Patel. Learning generalized linear

models over normalized data. In SIGMOD, 2015.

[8] M. Lin, Q. Chen, and S. Yan. Network In Network. ICLR, 2014.
[9] J. MacGregor. Predictive Analysis with SAP: The Comprehensive

Guide. SAP PRESS, 2013.

[10] M. Schleich, D. Olteanu, and R. Ciucanu. Learning linear regression

models over factorized joins. In SIGMOD, 2016.

[11] J. Seib and G. Lausen. Parallelizing Datalog programs by generalized

pivoting. In PODS, 1991.

1936


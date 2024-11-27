AppliedIntelligence
https://doi.org/10.1007/s10489-022-03979-2
Asurveyonmachinelearninginarraydatabases
Sebasti´anVillarroya1,2 · PeterBaumann3
Accepted:7July2022
© TheAuthor(s)2022
Abstract
This paper provides an in-depth survey on the integration of machine learning and array databases. First,machine learning
support in modern database management systems is introduced. From straightforward implementations of linear algebra
operations in SQL to machine learning capabilities of specialized database managers designed to process specific types of
data, a number of different approaches are overviewed. Then, the paper covers the database features already implemented
in current machine learning systems. Features such as rewriting, compression, and caching allow users to implement more
efficient machine learning applications. The underlying linear algebra computations in some of the most used machine
learning algorithms are studied in order to determine which linear algebra operations should be efficiently implemented by
array databases. An exhaustive overview of array data and relevant array database managers is also provided. Those database
features that have been proven of special importance for efficient execution of machine learning algorithms are analyzed in
detail for each relevant array database management system. Finally, current state of array databases capabilities for machine
learning implementation is shown through two example implementations in Rasdaman and SciDB.
Keywords Array data · Array database managers · Machine learning · Efficient array machine learning
1Introduction
Machine learning (ML) has become in recent years an essen-
tial tool not only in the field of academic research but also
in business and industrial applications. Due to the improve-
ment experienced by hardware devices in recent years, an
increasing number of companies in many different business
areas have decided to take advantage of ML technologies to
analyze the large amounts of data they need to manage.
Peter Baumann contributed equally to this work.
/envelopebackSebasti´an Villarroya
s.villarroya@usc.es
Peter Baumann
p.baumann@jacobs-university.de
1 Centro Singular de Investigaci ´on en Tecnolox´ıas Intelixentes
(CiTIUS), Universidade de Santiago de Compostela, Santiago
de Compostela, 15782, Spain
2 Work performed while at Jacobs University Bremen,
Bremen, Germany
3 Department of Computer Science and Electrical Engineering,
Jacobs University Bremen, Campus Ring 1, Bremen,
28759, Germany
For many years a huge research effort has been devoted
to the integration of ML tools and relational database man-
agement systems (RDBMSs), either implementing ML algo-
rithms in RDBMSs [ 1–9] or implementing well known
database optimization techniques in ML tools [10–16].
However, this research effort devoted to relational databases
has not been done in the field of array databases until very
recently [17–19]. Taking into account that increasingly large
amounts of data are generated every day and a relevant
part of them are array data, it is essential to leverage the
efficiency of array databases for the analysis of array data
in ML applications. Therefore, the most efficient tools in
array data analysis will allow users to boost analysis capa-
bilities in many and diverse application domains such as
cancer detection, pollution analysis, weather forecasting and
environmental classification.
This paper aims to provide a broad overview of the
current state of the integration of ML algorithms and array
databases.
First, an overview of recent solutions and approaches
providing support for ML algorithms in RBDMSs is intro-
duced. We begin with straightforward SQL implemen-
tations of basic linear algebra operations, e.g., matrix
multiplication. Then, more efficient solutions make use of
user-defined functions (UDFs). Beyond UDFs, user-defined

S.VillarroyaandP.Baumann
aggregates (UDAs) have shown to be very useful to imple-
ment those ML algorithms that need to iterate over large
datasets, e.g.,Gradient Descent and k-means. More com-
plex solutions are focused on leveraging the underlying data
model of RDBMSs when implementing ML algorithms,
e.g.,learning over joins and statistical relational learning
(SRL). Finally, those solutions that provide deeper modi-
fications to RDBMSs are introduced. Here, we have spe-
cialized systems for the analysis of specific data types, e.g.,
array DBMSs.
Then, an overview of main database features imple-
mented in relevant ML systems is provided. Classical
logical rewrites (e.g. loop vectorization and algebraic sim-
plification) have shown to be relevant in ML systems.
Also physical rewrites (e.g., flow rewrites, execution type
and physical operation selection) are of key importance in
ML implementations. Similarly to data access methods in
databases, ML tools can leverage data access optimization
techniques (e.g, compression and caching) to implement
more efficient ML algorithms.
The underlying linear algebra of three widely used ML
algorithms, namely linear regression , logistic regression
and feed-forward neural networks , has been extensively
studied to determine the linear algebra operations. Efficient
implementations of such operations are required in array
databases in order to provide a efficient performance of ML
algorithms on array data.
A brief introduction on array data and array databases
is also provided. The array data model and main database
properties (e.g., parallelization and query rewriting) are
specifically pointed out for array databases. Next, several
state-of-art approaches for array data processing are
classified into groups depending on their characteristics.
A description of main features is also provided for each
of them. Then, the aforementioned database features that
are relevant for ML algorithms are used to provide
a detailed comparison between such different solutions.
Finally, examples of ML algorithm implementation are
discussed for two relevant array databases.
2Motivatingexamples
Many application domains require from machine learning
analytics. And many of such application domains need to
process very large amounts of array data.
For example, in geoscience applications we can find
challenging problems that naturally fit many machine learn-
ing methods.
• Estimating variables from observations. Climate
events such as tornadogenesis and cyclogenesis can be
detected by analyzing array datasets. Also, they can be
predicted by discovering their precursors. Examples are
[20, 21].
• Characterizing objects and events. Some critical
geoscience variables (e.g., methane concentrations in
air) are difficult to be monitored directly. Thus, machine
learning methods can be used to infer such variables
by analyzing other variables collected via satellite or
ground sensors. An example approach is [22].
• Long-term forecasting. Early adaptation policies and
resource planning can be devised if prediction of
geoscience variables are provided ahead of time.
Machine learning methods for forecasting climate
variables using the spatial and temporal structure of
geoscience data have been explored in recent works
such as [23].
Life sciences also leverage analytics on array data. For
example, machine learning systems have multiple applica-
tions in medical imaging for different scenarios.
• Screening. ML is used as a screening tool to classify
studies according to the probability of the presence
of a disease. Application examples are: breast cancer
screening with mammography [24], and early diagnosis
of Alzheimer’s disease [25].
• Replacement. There are some areas in which ML has
the ability to replace doctors. Examples are: estimation
of bone age by artificial intelligence software [ 26],
calculation of breast radiological density and risk
scoring [27].
• Complementation. In this scenario there is an analysis
and a report by the doctor, and a complementary
contribution of the machine learning system. Examples
are: registration and segmentation, interpretation and
automatic classification, radiation dose calculation,
automatic data integration, improvement in the quality
of radiological reports.
3MLsupportinDBMS
Traditionally, database management systems have been used
in ML as simple data stores. ML algorithms pull data out
from databases, and pre-process and analyze them. How-
ever, as stated in [ 28], performing ML inside a database
system has some advantages. First, applying data pro-
cessing capabilities of DBMSs (e.g., storage, manage-
ment, transformation and querying) to operations over ML
models (e.g., selecting, training, deploying, updating and
managing) is aconvenient way to obtain an end-to-end
ML platform. Second, performing data analysis inside the
DBMS may improve efficiency by (1) avoiding to copy
ASurveyonmachinelearninginarraydatabases
massive amounts of data to external systems, (2) lever-
aging their efficient and scalable data processing tech-
niques, and (3) allowing optimization across tasks. Finally,
declarative task specification might be adequate for ML,
enabling automatic optimization and simplifying develop-
ment.
3.1SQLimplementation
Matrices and linear algebra are on the basis of many ML
algorithms. As a first straightforward approach to the imple-
mentation of ML in RDBMSs, tables can be used to store
matrices, and linear algebra operations can be expressed in
SQL. This solution is considered in systems like [4]a n d[2].
An example of this basic approach is the matrix multiplica-
tion with sparse representation. Considering matricesAm×l
and Bl×n, the resulting matrix C is a m × n matrix where
cij = ∑ l
k=1 aikbkj. Figure 1(a) shows the tables we may
use to represent matrices A and B, where columns i, j store
the matrix index, and column val stores the matrix value at
this specific index. We can now compute the multiplication
with the following SQL query.
3.2UDFimplementation
Although the previous solution works well for sparse
matrices, it is quite inefficient for dense matrices. These
inefficiencies can be eliminated using new user data types
(UDTs) and functions (UDFs). Systems like [2]a n d[3]a l s o
implement this solution. An example of this approach is
the matrix multiplication with vector representation. A new
data typevector may be defined as a UDT to efficiently
store matrix rows and columns. Figure1(b) shows the tables
used to represent matrices A and B. Table A stores the row
index i and the relevant row value, row. Table B stores the
column index j and the relevant column value, col. Notice
that both row and col are vector UDTs. Defining a UDF
dotproduct(v 1,v 2) to efficiently compute the dot product
of two vectors, we can compute the multiplication with the
following SQL sentence:
3.3Extensions
Many ML algorithms involve iterative computations over
large datasets. Specifically, UDAs have provided an
(a) Sparse representation (b) Vector representation
Fig.1 Matrix storage in tables
S.VillarroyaandP.Baumann
impressive ease to implement iterative ML algorithms.
For example, [ 3]a n d[ 6] implement the Gradient Descent
algorithm following this approach. Another example is the
k-means method for clustering implemented in [ 6]a n d
[7], also following this approach. Iterative algorithms can
be implemented using UDAs by specifying the following
functions:
• Init(state) initializes those variables used to store the
state.
• Accumulate(state,data) uses the parameter data to
update the state.
• Finalize(state) computes the final result using the state.
• Merge(state 1,state 2) merges different state values
computed over disjoint input subsets. Only imple-
mented when needed.
In a very recent work [29], a new approach for declarative
specification of recursive computations in a RDBMS has
been proposed. Multiple versions of a database table,
accessed via array-style indices, are allowed by extending
SQL. For example, in a feed-forward neural network
[29], dependencies among activations, weights and errors
can be declaratively expressed avoiding imperative control
flow by writing the forward-backward passes with such
SQL extensions. Section 5.3 provides the mathematical
foundations of the forward-backward computations in a
feed-forward neural network.
The forward pass computes the activation values of the
neurons at each layer. Table version A[i][j] stores all
the activation values of the layer j at the iteration step i.
Each A[i][j] is supposed to be “chunked” and stored as
matrices ACT in the following table:
A(ITER, LAYER, ROW, COL, ACT)
Activations of layer j at learning iteration i are
computed as a weighted sum of output values of all
the neurons of layerj-1 at iteration i. The weighted
sums in layer j at the iteration step i a r es t o r e di nt h e
table WI[i][j]. A similar approach is used to store the
weighted sums, each “chunk” of WI[i][j] is stored as a
matrix VAL in the following table:
WI(ITER, LAYER, COL, VAL)
Using the above tables, the forward pass can be specified
with the following queries using SQL extensions to allow
the recursive computation of activations and weighted sums.
The forward pass begins by initializing the first layer of
activations with the input data.
Then, the weighted sums for each layer j of the network
at iteration i are computed using the weights of layer j and
the activations of layer j-1 at iteration i.
These weighted sums are then used to compute the sub-
sequent activation values in the network. Thus, activations
in layer j at iteration i are computed applying the acti-
vation function relu to the weighted sums in layer j at
iteration i.
Finally, the activation values of the output layer ( l=9)
at iteration i are computed by applying the function
softmax to the weighted sums of the output layer at
iteration i.
In the backward pass, the errors computed in the output
layer of the network are back-propagated through the
network. Similarly to activations and weighted sums, the
table version E[i][j] stores the errors in the layer j at
learning iteration i. These errors are then used to compute
the network weights. Table W[i][j] stores the weights
of the model in layer j at iteration i.B o t hE and W are also
“chunked” and stored in the following tables:
E(ITER, LAYER, COL, ERR)
W(ITER, LAYER, ROW, COL, MAT)
The backward pass begins with the computation of errors
in the output layer (l=9). The activation values in the output
layer at iteration i and the expected output values are used
to compute the errors in the output layer at iteration i.
ASurveyonmachinelearninginarraydatabases
Then, the errors of the subsequent layers are computed. The errors in the layer j at iteration i are computed using the
activation values in layer j at iteration i, the weights in layer j+1 at iteration i, and the errors in layer j+1 at iteration i.
Finally, the weights of the model are updated for the current iteration. The weights in layer j at iteration i are computed
using the weights in layer j at iteration i-1, the errors in layer j at iteration i-1 and the activation values in layer j-1 at
iteration i-1.
Thus, with extensions in SQL to allow different versions
of a database table, accessed via indices i and j,t h e
recursive computations in a feed-forward neural network
can be specified in a declarative manner.
3.4Learning“over”joins
Some recent lines of research are focused on leveraging
the underlying data model of RDBMS, named multi-
table relational model , during the implementation of ML
algorithms. One of these lines is learning over joins ,
which aims to accelerate ML algorithms by exploiting
common database dependencies in multi-table datasets,
e.g, key-foreign key dependencies (KFKD) and multi-
valued dependencies (MVD). Taking into account that join
computation usually introduces redundancy in the data, and
in turn redundancy in many ML computations, learning
over joins rather than computing the join output will
provide faster results. Current systems may be grouped
into three categories: (1) specific ML algorithms, e.g.,
Kumar et al. [30], Schleich et al. [31], Nikolic and Olteanu
[32], TensorDB [ 1], (2) custom libraries for specific ML
algorithms, e.g., Rendle [ 33], Kumar et al. [ 34], and (3)
generalized learning over joins, e.g., Chen et al. [ 35],
Ghoting et al. [36], Li et al. [37], Abo-Khamis et al. [38].
3.5SRLsystems
Another research line which aims to leverage the under-
lying RDBMS data model is statistical relational learn-
ing (SRL). The “independently-and-identically distributed”
(IID) assumption, made by standard ML algorithms, cannot
be applied to multi-table datasets. One solution to this issue
is provided by SRL ML algorithms which do not make
the IID assumption and exploit the structure information
to learn directly over multi-table datasets. In addition, SRL
allows to predict multiple correlated variables simultane-
ously with one model. Several SRL models have been pro-
posed in the literature, e.g., Markov Logic Network (MLN)
[39] and Probabilistic Soft Logic (PSL) [ 40]. Examples of
modern ML systems implementing SRL include [41–44].
3.6SpecializedDBMSs
All the previous systems have been designed for processing
structured relational data. However, since ML can be
applied to any type of data, many ML applications need to
process large amounts of unstructured data. Thus, several
ML systems have been built for processing stacks of ML
workloads over specific non-relational data types.
3.6.1MultimediaMLsystems
Multimedia query systems have experienced a renewed
interest in recent years mainly due to the popularity of
deep convolutional neural networks (CNNs) for accurate
computer vision. Recent solutions include [45]a n d[46].
3.6.2TextMLsystems
No native support for ML on text data is provided by most
database systems. Thus, some ML systems, such as [ 47],
S.VillarroyaandP.Baumann
have been designed to store, index, and provide analytics
over large amounts of text data.
3.6.3TimeseriesMLsystems
Time series datasets have been an important source of
information for many years. In recent years they also
became important for many ML applications, e.g., weather
forecast. Several ML systems aim to optimize the process-
ing of time series data, e.g., [48]a n d[49].
3.6.4GraphMLsystems
Several graph-oriented ML systems, such as [50], have been
proposed in recent years due the increasingly importance of
graphs in social media and web applications.
3.6.5ArrayMLsystems
Many scientific data processing systems have been designed
for leveraging the huge amount of large multi-dimensional
array datasets generated every day by many monitoring
applications. Block-partitioned arrays are managed by their
storage layer. Both read and write operations are defined
on such array data. Rasdaman [51] is the pioneering array
database management system. It supports a SQL-based
query language called Raster Query Language (rasQL).
Rasdaman also provides APIs for several environments,
e.g., R, Python, Java, C++. Simple ML algorithms, such as
linear regression, can be defined as user defined functions
and called directly in rasQL to be executed in Rasdaman.
SciDB [ 52] also supports a SQL-based query language
called Array Query Language (AQL) and several APIs. A
small number of linear algebra operators allow the execution
of simple regression tasks. Section7 provides an in-depth
analysis of the most important database features for ML,
introduced in Section 4, currently implemented in modern
array database management systems.
4DBfeaturesinMLtools
Database-inspired optimization techniques are applied in
many ML sysems to improve the efficiency of ML pro-
grams. The most important optimization objective is to min-
imize execution time. Nevertheless, recent optimizers allow
for optimization based on different objectives, e.g., mini-
mize time [14], minimize monetary cost [ 53], maximizing
accuracy [54]. To highlight the importance of those database
optimization techniques implemented in ML systems, a
brief overview of some relevant techniques is provided in
this section.
4.1Logicalrewrites
4.1.1Commonsubexpressionelimination
One of the most used programming language rewrite in
ML systems is common subexpression elimination (CSE).
In a bottom-up pass through the graph, this optimization
technique (1) consolides leaf nodes, and (2) merges
equivalent operations, i.e, those with same inputs and data
types. Some operators must be prevented from invalid
elimination, e.g., print. Thus, operation-specific handling
is required for these operations.
4.1.2Loopvectorization
An advanced programming language rewrite used in spe-
cific array operations is loop vectorization. Obviously,
this optimization technique is essential in array databases.
Loop vectorization consists of identifying loops that can
be performed with array operations, and replace them with
element-wise array operations. An example is shown below.
A: for (i=1:10) X[1,i] = Y[i,2]
B: X[1,1:10] = Y’[1:10,2]
Expression A is a loop to copy the first ten values in the
second column of Y into the first ten values in the first row
of X. This loop can be vectorized leading to the expression
B, where the index variablei has been replaced by its range
1:10. Additional changes are also required. Notice that
vector Y has been transposed in order to ensure aligment
for element-wise matrix operations. Nested loops can be
vectorized by applying this technique repeatedly.
4.2Physicalrewrites
Avoiding unnecesary data shuffling helps to improve
efficiency for large-scale ML programs. Thus, many ML
systems implement different techniques for data flow
rewrites. Although there are several data flow rewrite
techniques (e.g., joins, aggregation types), in this section
we briefly discuss two of the most important, distributed
caching and partitioning.
4.2.1Distributedcaching
Deciding upon the caching of intermediates always repre-
sent a tradeoff between memory consumption and redun-
dant computation. Existing systems use very different
approaches. For example, in Mahout Samsara [10], users
control caching manually at script level. All intermediates
that are consumed multiple times are cached in Emma [55].
Large matrices and variables used read-only in loops are
cached in SystemML [13].
ASurveyonmachinelearninginarraydatabases
4.2.2Partitioning
A more efficient execution of joins in a distributed envi-
ronment may be achieved by keeping track of partition-
ing. Dedicated partitioning-aware operations preserving and
exploiting such partitioning are provided by systems like
SystemML, Mahout Samsara, and Emma.
4.3Dataaccessmethods
Similarly to above optimization techniques, many ML
systems leverage different techniques for efficient data
access that are also based on those already implemented in
databases. One of the most relevant data access optimization
technique is briefly discussed in this section .
4.3.1Compression
Based on data compression and query processing over
such compressed data in database systems, ML systems
store tensors, matrices and graphs in compressed form, and
perform linear algebra over such compressed structures.
Depending on whether the compression process loses
information or not, existing techniques can be grouped into
lossless and lossy compression techniques.
• Lossless Compression. Result correctness is guaran-
teed with these techniques. The idea here is to use
general-purpose compression techniques. This requires
block-wise decompression for each operation. Unfortu-
nately, lightweight methods provide modest compres-
sion ratios, while heavyweight techniques show very
slow decompression. Some scientific data formats, like
NetCDF [56], provide compression at chunk granular-
ity, by specifying the chunk size and deflation level.
Distributed-data processing frameworks, like Spark,
support different storage levels and configurations for
data compression.
• Lossy Compression. Loss of information leads to
less accurate results. Thus, this techniques have to
be carefully applied. However, many ML algorithms
can tolerate less accuracy in results because they are
approximate in nature. Tradeoff space between less
accuracy and more efficiency is very wide. A good
knowledge of the lossy compression impact in the ML
algorithm accuracy is required. Sometimes this impact
depends on input data and thus, is difficult to estimate.
5LinearalgebraoperatorsinMLalgorithms
A basic and preliminar step when trying to implement ML
algorithms in Array Databases is to identify the underlying
linear algebra operations. Efficient implementations of such
operations within the database will allow the efficient
implementation of ML algorithms. The underlying linear
algebra foundations of some common ML algorithms are
provided in this section. Then, based on these mathematical
constructs, the required linear algebra operations are
pointed out. There are many different ML models, from
basic mathematical equations to advanced ML algorithms
with increasing complexity. Nevertheless, the underlying
mathematical structures and operators are mostly common
to all of them. Thus, these authors consider that the analysis
of the linear and logistic regression algorithms (as common
examples of prediction methods) and thefeed-forward
neural network (as a common example of classification
method) is enough to obtain the linear algebra operators
required to implement the vast majority of the remainder
ML models.
5.1Linearregressionalgorithm
The linear regression algorithm is one of the most used
algorithms in prediction. In this section, the underlying
linear algebra in linear regression is explored in order to
determine the required linear algebra operations.
To introduce the appropriate notation, a motivating exam-
ple for predicting house prices is going to be used. We want
to predict the price of a house based on the following addi-
tional valuable information: size of the house, number of
bedrooms, number of floors, and age of the house. This
information variables are calledfeatures.T a b l e1 depicts
some example values for this features and the correspond-
ing output variable, i.e., the variable we want to predict.
We will use this set of values to train the model, so this
set is called the training set. We now denote the features
as follows: x1 for the size, x2 for the number of bedrooms,
x3 for number of floors, and x4 for the age. The output
variable to be predicted is denoted by y. The number of
features in the input examples is denoted by n, the num-
ber of examples in the training set is denoted by m,t h eith
input example in the training set (i.e., the ith training exam-
ple) is denoted by vector x(i), and the value of feature j in
Table 1 Example data for predicting house prices
Size (feet2) Number of Number of Age of House Price
bedrooms floors (years) ($1000)
2304 3 2 48 450
2004 4 1 45 385
1804 2 2 39 345
1584 2 1 35 270
1704 2 2 40 250
S.VillarroyaandP.Baumann
the ith training example is denoted by x(i)
j . For the exam-
ple depicted in Table 1,w eh a v em = 5, n = 4, x(2) =
[2004, 4, 1, 45]′ and x(2)
3 = 1.
5.1.1Hypothesisfunction
The job of the learning algorithm is to output a function
which is usually denoted by h,w h e r eh stands for hypothe-
sis. This hypothesis is a function that takes as input the value
of x and it tries to output the estimated value of y.S o h is
a function that maps from x’s to y’s. In our example, we
represent the hypothesis function as
hθ(x) = θ0 + θ1x1 + θ2x2 + θ3x3 + θ4x4 (1)
meaning that we are going to predict that y is a linear
function of x. For example, hθ(x) = 70 +0.1x1 +0.01x2 +
3x3 − 2x4 means that the base price of a house is $70000,
plus $100 per square feet, plus $10 per bedroom, plus $3000
per floor, and minus $2000 per year the house has.
A general formulation of the hypothesis function for any
number of features is as follows
hθ(x) = θ0 + θ1x1 + θ2x2 + ... + θnxn (2)
For convenience of notation, we define x0 = 1 for all the
input examples in the training set, i.e., x(i)
0 = 1. Then, we
can write vectors x and θ as follows
x =
⎡
⎢⎢⎢⎢⎢⎣
cx0
x1
x2
...
xn
⎤
⎥⎥⎥⎥⎥⎦
∈ Rn+1 θ =
⎡
⎢⎢⎢⎢⎢⎣
θ0
θ1
θ2
...
θn
⎤
⎥⎥⎥⎥⎥⎦
∈ Rn+1 (3)
Hence, now we can write our hypothesis function as
hθ(x) = θ0 + θ1x1 + θ2x2 + ... + θnxn
= θT x (4)
5.1.2Gradientdescent
Once the hypothesis function has been defined, the algo-
rithm needs to fit its parameters. In particular we are going
to use the gradient descent for linear regression with multi-
ple features. To provide a measure of the error in the model,
a cost function given by the sum of the square of the error is
defined as follows
J(θ) = 1
2m
m∑
i=1
(hθ(x(i)) − y(i))2 (5)
The gradient descent algorithm, depicted below, repeat-
edly updates each parameter by subtracting alpha times the
partial derivative of the cost function.
Applying vectorization we can obtain a more efficient
implementation. The vectorized version of the gradient
descent is depicted below.
where
X =
⎡
⎢⎢⎢⎢⎣
x(1)
0 ...x (1)
n
x(2)
0 ...x (2)
n
...
x(m)
0 ...x (m)
n
⎤
⎥⎥⎥⎥⎦
y =
⎡
⎢⎢⎢⎣
y(1)
y(2)
...
y(m)
⎤
⎥⎥⎥⎦ ∈ Rn+1 (6)
Thus, from the above linear algebra expressions we can
determine that the next operations are required:
• Vector/Matrix - Scalar operations: +, -, * , /.
• Vector/Matrix unary operations: transpose.
• Vector/Matrix binary operations: +, -, *, /.
• Vector/Matrix element-wise operations: .*.
• Vector/Matrix aggregate operations: sum.
• Iteration constructs.
5.2Logisticregressionalgorithm
A different type of problems are the so called classification
problems, where the output valuey is either 1 or 0. We have
many examples of classification problems, e.g., whether a
tumor is benign or malignant, a mail is spam or not, an
online transaction in fraudulent or not. Linear regression
algorithms do not provide successful results when applied
to classification problems. This section is devoted tologistic
regression, one of the most popular and most widely used
learning algorithms today. Logistic regression is actually a
classification algorithm that we apply to settings where the
labely is a discrete value, either zero or one. Contrarily to
linear regression, logistic regression has the property that
the outputs (predictions) are always between zero and one,
i.e., 0<h θ(x) < 1.
ASurveyonmachinelearninginarraydatabases
5.2.1Hypothesisfunction
As stated above, we would like our classifier to output
values between 0 and 1. For logistic regression, we modify
the hypothesis used in linear regression to satisfy this
condition. The hypothesis for logistic regression is
hθ(x) = g(θT x) (7)
where g is the sigmoid function,
g(z) = 1
1 + e−z (8)
hence,
hθ(x) = 1
1 + e−θT x (9)
5.2.2Gradientdescent
We could take the same approach that we used for linear
regression and use a cost function given by the sum of the
square of the error is defined as follows
J(θ) = 1
m
m∑
i=1
1
2(hθ(x(i)) − y(i))2 (10)
But in this case, J(θ) results in a non-convex function.
Instead, we are going to use a different cost function that
is convex, so we can apply the gradient descent and be
guaranteed to find the global minimum. The cost function
for logistic regression is
Cost(h θ(x), y) =
{
− log(hθ(x)) ify = 1
− log(1 − hθ(x)) ify = 0 (11)
The above cost function definition can be compressed
into one equation
Cost(h θ(x), y) =− y log(hθ(x)) − (1 − y) log(1 − hθ(x))
(12)
Using the above cost function, J(θ) can be calculated as
follows
J(θ) = 1
m
m∑
i=1
Cost(h θ(x(i)), y(i))
= −1
m
m∑
i=1
y(i) log(hθ(x(i)))
+(1 − y(i))log(1 − hθ(x(i))) (13)
As we can see below, the Gradient Descent algorithm
for logistic regression is exactly the same that the Gradient
Descent algorithm for linear regression. The difference
between them is the hypothesis function hθ(x).
We can also apply vectorization to obtain a more efficient
implementation.
In addition to the operations required in linear regression,
we need:
• Vector/Matrix function evaluation:sigmoid(X ∗theta)
5.3Feed-forwardneuralnetwork
One of the most powerful learning algorithms that we have
today are the neural networks. Although neural networks
were originally designed to build machines for mimic the
human brain, currently they are the state-of-the-art machine
learning technique for many applications. Thus, based on its
origins, a neural network is a group of connected neurons
ordered in layers. Figure 2 depicts a neural network with
four layers. The units of the first layer (input layer) store
the attributes of each training example, i.e., x1,x 2 and x3.
Notice that a bias unit (x0 = 1) has been added to the
Fig.2 Example feed-forward neural network
S.VillarroyaandP.Baumann
input layer (Layer1). The example neural networks has two
hidden layers (Layer2 and Layer3) with three units plus
abias unit (a2
0 = a3
0 = 1). The activation of unit i in
layer j is a(j)
i . Finally, one output layer with three units
provides the resulting hypothesis. Thus, this network is
used to performmulti-class classification with three classes
(K = 3), where
hθ(x) =
⎡
⎢⎣
a(4)
1
a(4)
2
a(4)
3
⎤
⎥⎦ (14)
5.3.1Hypothesisfunction
For each layer j, the network has a matrix θ(j) of weights
controlling the mapping from layer j to layer j + 1. For
example, for Layer1 in the example network we have
θ(1) =
⎡
⎢⎣
θ(1)
10 θ(1)
11 θ(1)
12 θ(1)
13
θ(1)
20 θ(1)
21 θ(1)
22 θ(1)
23
θ(1)
30 θ(1)
31 θ(1)
32 θ(1)
33
⎤
⎥⎦ (15)
where θ(1)
xy is the weight from unit y in layer 1 to unit x in
layer 2.
To obtain the expression of the resulting hypothesis we
need to perform a forward propagation from input layer to
output layer in order to compute the activation of the units.
Thus, the activation of the units in Layer2 can be computed
with the expressions below.
a(2)
1 = g(θ(1)
10 x0 + θ(1)
11 x1 + θ(1)
12 x2 + θ(1)
13 x3)
a(2)
2 = g(θ(1)
20 x0 + θ(1)
21 x1 + θ(1)
22 x2 + θ(1)
23 x3) (16)
a(2)
3 = g(θ(1)
30 x0 + θ(1)
31 x1 + θ(1)
32 x2 + θ(1)
33 x3)
where g is again the sigmoid function.
Similarly, we can compute theactivations of Layer3 with
the following expressions.
a(3)
1 = g(θ(2)
10 a(2)
0 + θ(2)
11 a(2)
1 + θ(2)
12 a(2)
2 + θ(2)
13 a(2)
3 )
a(3)
2 = g(θ(2)
20 a(2)
0 + θ(2)
21 a(2)
1 + θ(2)
22 a(2)
2 + θ(2)
23 a(2)
3 ) (17)
a(3)
3 = g(θ(2)
30 a(2)
0 + θ(2)
31 a(2)
1 + θ(2)
32 a(2)
2 + θ(2)
33 a(2)
3 )
Finally, for the output layer we have,
a(4)
1 = g(θ(3)
10 a(3)
0 + θ(3)
11 a(3)
1 + θ(3)
12 a(3)
2 + θ(3)
13 a(3)
3 )
a(4)
2 = g(θ(3)
20 a(3)
0 + θ(3)
21 a(3)
1 + θ(3)
22 a(3)
2 + θ(3)
23 a(3)
3 ) (18)
a(4)
3 = g(θ(3)
30 a(3)
0 + θ(3)
31 a(3)
1 + θ(3)
32 a(3)
2 + θ(3)
33 a(3)
3 )
Hence,
hθ (x) =
⎡
⎢⎣
g(θ(3)
10 a(3)
0 + θ(3)
11 a(3)
1 + θ(3)
12 a(3)
2 + θ(3)
13 a(3)
3 )
g(θ(3)
20 a(3)
0 + θ(3)
21 a(3)
1 + θ(3)
22 a(3)
2 + θ(3)
23 a(3)
3 )
g(θ(3)
30 a(3)
0 + θ(3)
31 a(3)
1 + θ(3)
32 a(3)
2 + θ(3)
33 a(3)
3 )
⎤
⎥⎦ (19)
5.3.2Costfunctionandpartialderivatives
Based on exposed above and the cost function in logistic
regression, we can express the cost function for the neural
network as follows,
J(θ) = 1
m
m∑
i=1
Cost(h θ(x(i)), y(i))
= −1
m
m∑
i=1
K∑
k=1
y(i)
k log(hθ(x(i)))k + (20)
(1 − y(i)
k )log(1 − (hθ(x(i)))k)
where y(i)
k and (hθ(x(i)))k are, respectively, thekth elements
of the expected output and the hypothesis output for
example inputx(i).
5.3.3Gradientdescent
In addition to the cost function, we need the partial deriva-
tives in order to compute the gradient descent of the cost
function. To compute the partial derivatives we need to exe-
cute the backpropagation algorithm. Such algorithm allows
us to compute the error for each input sample and then com-
pute the partial derivatives by backpropagating the errors
from the output layer to the input layer.
First, for each training example (x, y),t h eactivations of
the units can be computed by forward propagation with the
vectorized implementation below,
a(1) = x
z(2) = θ(1)a(1)
a(2) = g(z(2))( add a(2)
0 )
z(3) = θ(2)a(2) (21)
a(3) = g(z(3))( add a(3)
0 )
z(4) = θ(3)a(3)
a(4) = hθ(x) = g(z(4))
Once we have the resulting hypothesis, the backpropaga-
tion algorithm is used to compute the error in each unit for
the training example. The errors in layerl is denoted by δ(l).
First, we compute the errors for the output layer. And then,
we can compute the errors of the layerl by backpropagating
the errors in layer l + 1. The vectorized implementation of
the backpropagation step is as follows,
δ(4) = hθ(x) − y = a(4) − y
δ(3) = (θ(3))T δ(4). ∗ g′(z(3)) (22)
δ(2) = (θ(2))T δ(3). ∗ g′(z(2))
When training a neural network, these errors are com-
puted for each training example and used to compute the
overall partial derivatives for the entire training set. The
ASurveyonmachinelearninginarraydatabases
algorithm of the backpropagation for the entire training set
is shown below.
As we can see, first the accumulated errors /Delta1(l) are
initialized to zero for all the units. Then, for each training
example, the activations a(l) for all the units are computed
by forward propagation. The errors δ(L) for the output
layer are computed next. Then, the error in the output
layer is backpropagated to compute the errors δ(l) in the
remainder layers. The errors are accumulated in /Delta1(l) for
all the examples in the training set. Finally, we use the
accumulated errors to compute the partial derivatives∂J(θ)
∂θ (l)
ij
to be used in the gradient descent algorithm.
Thus, as we can see, thefeed-forward neural network can
be implemented with the same operations used in logistic
and linear regressions algorithms. Furthermore, most of the
ML algorithms can be implemented with those operators.
The following Table 2 summarizes such operations.
6Arraydatabasemanagementsystems
A detailed overview of array data and array database man-
agers is provided in this section. Arrays, also called raster
data, gridded data or datacubes, are core data structures
that appear in almost every area of engineering and science,
Table 2 Linear Algebra Operations in ML algorithms
Vector/Matrix - Scalar operations: +, -, * , /
Vector/Matrix unary operations: transpose
Vector/Matrix binary operations: +, -, *, /
Vector/Matrix element-wise operations: .*
Vector/Matrix aggregate operations: sum
Iteration constructs
Vector/Matrix function evaluation: sigmoid(X * theta)
e.g., life sciences, statistics, earth sciences, space sciences.
Ralational DBMSs show many problems when dealing with
large binary datasets of structured information. Thus, array
databases [57] have been specifically designed to solve such
drawbacks. Storage and processing functionality for multi-
dimensional arrays are provided by array databases through
flexible and scalable services. Large sets of operations, from
statistics to general Linear Algebra, build the core of such
databases and provide functionality for array processing. A
comprehensive survey on array databases has been recently
published [58].
6.1Arraydatamodel
Formally, a n-dimensional array is a function f : D →
T which domain is the n-fold Euclidean cross product of
closed integer intervals:
D ={ lo1,...,h i 1}× ... ×{ lon,...,h i n} (23)
loi ≤ hii for 1 ≤ i ≤ n
where T is a non-empty set called the cell type of the
array. Elements in T are called cells. This definition have
a equivalence in mathematics where vectors (or sequences)
represent 1-D arrays, matrices represent 2-D arrays, and
tensors represents higher-dimensional arrays. The core
operations on arrays have been defined by Tomlin in
the well known Map Algebra. This operations have been
categorized depending of the number of cells of an input
array that are taken into account to compute each cell of the
resulting array.
6.2Arraydatabases
For decades, relational DBMSs principles have been proven
successful. Now, array databases are offering their benefits
as well.
• Declarative query language. Rather than write a
procedural algorithm to obtain the desired result, users
provide a description of such result. Thus, an array
query written in 2-3 lines of code would be translated
into several pages of procedural code.
• Transparent storage management. Although many
data centers are used to know the exact location of
each byte on disk, this transparency has two important
advantages: (1) the user access is more simple, and
(2) internal reorganizations may be performed without
affecting users.
• Concurrency and access control. Access management
to large amounts of data that are usually queried by
many users simultaneously is required. Inconsistencies
due to parallel accesses to data must be avoided by
S.VillarroyaandP.Baumann
concurrency control systems. Specifically for arrays,
granularity of access control must allow access manage-
ment of arbitrary areas within datacubes.
6.2.1Storage
The Euclidean neighborhood of array cells is a key concept
when implementing access patterns on arrays. Any storage
engine must implement some spatial clustering strategy to
preserve proximity on persistent storage. It is a commom
practice to partition n-D arrays into n-D subarrays, also
called tiles [ 59] or chunks [ 60], to form the unit of access
to persistent storage. Given the wide variety of possible
workloads, the challenge here is to find the partitioning
technique that minimizes the number of partitions fetched
from persistent storage and, hence, the query performance.
Although this principle is generally accepted, there are
multiple variations for partitioning techniques.
Spatial indexes (e.g., R-Tree) have proven advantageous
to determine the required partitions much faster. Most spa-
tial indexes show good performance because the targeted
objects, which have a box structure, partition a space of
known extent. Contrarily, spatial databases manage poly-
gons which lack that regular structure. Additionally, another
advantageous technique is compression of tiles.
6.2.2Processing
In general, relational query processing is tipically I/O
bound. On the contrary, query evaluation in array operations
is heavily CPU bound. Many array operations are trivially
parallelizable and, thus, easily distributed both in local
nodes (general-purpose GPUs and multicore CPUs) and
remote nodes (clusters, cloud networks). Some other
operations require to be transformed, and sometimes
rewritten (Fig. 3) in a new set of operations, before
being parallelized, e.g., histogram generators, joins of
differently partitioned arrays. Next, some optimizations
proven effective in array DBMSs are briefly explained.
• Parallelization. Sometimes array operations have been
considered “embarrassingly parallel” due to (1) array
operations apply the same operation over a large num-
ber of values, and (2) tiles map naturally to CPU
cores. This is only true for simple operations, but not
true in general. Even simple binary operations like
a + b may be challenging, e.g., both operands are in
different nodes and have incompatible tiling. Addi-
tional complexity may be found in Linear Algebra
operations. Several criteria for splitting queries across
multiple systems may be followed, e.g., data loca-
tion, intermediate results transfer costs, current resource
availability.
(a) avg (a+b) (b) avg(a) + avg(b)
Fig. 3 Equivalence rule for array query rewriting: “avg(a + b) ≡
avg(a) + avg(b)”
• Mixed Hardware. Although processing time can be
highly speeded up by compiling queries into code for
CPUs, GPUs and FPGAs, some non-trivial problems
on mixed hardware evaluation are still under active
research.
• Approximate Caching. Similarly to RDBMSs, caching
final or intermediate array results can improve effi-
ciency when same or similar queries are frequently
executed. For example, mitigation forces and general
public will issue lots of queries on a disaster region.
With array data we may found that these queries do
not hit exactly the same area, but they may share par-
tially the requested region. So, the query engine can also
reuse partially matching areas in arrays to provide faster
results [61].
• Cost-based Optimization. Similarly to RDMSs, this
optimization techniques attempts to find the most effi-
cient execution plan among all possible plans for a
given query. Contrarily to query rewriting, knowledge
of the processing costs is required. Many parameters
can influence such costs, e.g., location of tiles (dis-
tributed environments), number of tiles to be read from
persistent storage, number and complexity of opera-
tions.
6.3Classiﬁcation
Due to the blooming research and development experienced
in this area, further systems not referenced in this survey
may emerge soon. Systems encountered in literature may be
grouped into four main categories.
• Array Database Systems. Relevant features include
multi-user operation, access control mechanisms, stor-
age management and query language. These systems
may be subdivided into two groups.
– Full-stack Array Databases. Systems imple-
mented from the scratch, e.g., rasdaman [ 51],
SciDB [52].
ASurveyonmachinelearninginarraydatabases
– Add-ons to existing database systems. May
be implemented by adding extra layers to
existing DBMSs (e.g., EXTASCID [62]),
performing direct DBMS kernel coding (e.g.,
SciQL [ 63]), or providing object-relational
extensions (e.g., PostGIS Raster [64], Teradata
Arrays [65], OracleGeoRaster [66]).
• Array T ools. Systems encompassing libraries and
command-line tools that do not constitute a complete
service tool but may be components of larger array
services. Since data scientists generally prefer high-
level languages, these solutions are preferably used in
data centers where data experts are also experienced
full-stack developers.
• MapReduce Array Engines. Systems providing pro-
cessing capabilities for multi-dimensional arrays on top
of Hadoop or Spark frameworks.
6.4Full-stackarrayDBMSs
6.4.1Rasdaman
Since its first publication in 1992, rasdaman [ 51] has pio-
neered the field of array databases. Extraction, retrieval,
fusion and aggregation on distributed arrays is performed in
the server through parallelization, effective optimization and
heterogeneous hardware. Single point of failure is avoided
in rasdaman by using a parallelizing peer federation archi-
tecture. Arrays can be stored either in the optimized array
store or in standard databases. Moreover, any pre-existing
archive structure can be used. A SQL-based query language,
called rasQL, is provided in its query interface. Rasdaman is
also a pattern for several Big Data stardards, such as the ISO
SQL/MDA (Multi-Dimensional Arrays) [67] and the OGC
Web Coverage Service (WCS) with its geo datacube query
language, Web Coverage Processing Service (WCPS) [68].
6.4.2SciDB
Similarly to rasdaman, SciDB [ 52] is an array DBMS that
provides an Array Query Language (AQL) and an Array
Functional Language (AFL). A modified Postgres kernel is
the basis for its architecture, plus UDFs implementing array
functionality.
6.5ExtensionstoexistingDBMSs
6.5.1SciQL
SciQL [63] was born as an extension of the column-store
DBMS MonetDB with array-specific operators. There is
no dedicated storage and processing engine in SciQL, n-D
arrays are internally sequentialized and stored into column-
store tables.
6.5.2EXTASCID
Built as a complete and extensible system for scientific
data processing for both arrays and relational data,
EXTASCID [62] considers the execution of arbitrary user
code as a central part of its design. As a result, supports
in-database processing of full scientific workflows over
both raw and derived data. EXTASCID is built around the
massively parallel GLADE architecture for data aggregation
and enhances the original GLA interface implemented in
GLADE with functions specific to scientific processing.
6.5.3PostGISRaster
Geo raster data can be stored and analyzed using the
PostGIS type “Raster” [ 64]. While the implementation
of this type heavily relies on the extension capabilities
of PostgreSQL, GDAL is used for processing purposes.
Although raster expressions are allowed, they are not
integrated with the PostgreSQL query language. Instead,
they are written in a separate Map Algebra language and
passed to a raster object as a string. Large objects are
not partitioned automatically, users have to perform such
partition. Then, partitions are distributed over tuples in
raster columns. Hence, queries have to be written in such
a way that a proper recombination of large rasters from
partitions stored in one tuple each can be achieved.
6.5.4OracleGeoRaster
Both spatial data types and an object-relational schema are
provided by Oracle GeoRaster [66]. Digital images and multi-
dimensional grid layers referencing either a local coordinate
system or locations on the Earth’s surface can be stored by
using these elements. Neither a specific raster query language
nor a specific array-centric architecture are provided.
6.5.5Teradataarrays
Similarly to Oracle GeoRaster, Teradata has recently pro-
posed an object-relational solution to add arrays as a new
datatype [65]. Each array is mapped to 64kB blobs including
the metadata, providing a size array of 40 kB approximately.
6.6Arraytools
6.6.1wendelin.core
Wendelin.core [ 69] allows for processing of big arrays,
bigger than RAM and local disk, which can be processed
S.VillarroyaandP.Baumann
with transactions and persisted in storage. The virtual
address-space is the limit for the big array size.
6.6.2TensorFlow
TensorFlow [70] is a well known tool for machine learning,
mainly focused on deep neural networks, that provides a
wide range of functionalities
6.6.3xtensor
The Python array programming library NumPy is used in
xtensor [71] to build a C++ library for numerical analysis.
Although multi-dimensional array expressions can be
defined, an extensible expression system is provided. Thus,
lazy broadcasting, tools for array expression manipulation
and an API following the C++ stardard library are enabled.
6.6.4OPeN-DAP
The Open-source Project for a Network Data Access Pro-
tocol (OPeN-DAP) [72] is both a transport protocol and an
data architecture for earth scientists. Standards for encapsu-
lation of structured data, addition of semantics describing
such data, and annotation of data with attributes are defined.
Arrays are one-dimensional. Multi-dimensional arrays are
defined as arrays of arrays. Constraint expressions allow
clients to request variables or parts of variables.
6.6.5Ophidia
A full software stack enabling data analytics and manage-
ment of big datasets is provided by Ophidia [ 73]. Server-
side approach, hierarchically distributed storage, and par-
allel and in-memory computation are main features in this
system. Processing of multi-dimensional array-based data is
supported by the implementation of the datacube abstrac-
tion in its data model. A wide set of operations is provided
for data analytics.
6.6.6Googleearthengine
Google Earth Engine [74] relies on grid systems with files.
With a functional programming language in its heart, code
can be submitted to Google Earth Engine by users and
executed in Google’s own distributed environment.
6.6.7TileDB
A library for management of sparse and dense arrays is
provided by TileDB [ 75]. Any number of attributes of
different data types can be stored in each multi-dimensional
array in TileDB. High I/O on several data persistence
backends, compression and integration with well known
data science ecosystems are relevant features on this library.
6.6.8boost::geometry
Primitives and algorithms for solving geometry problems
are defined in this boost.Geometry [ 76]. A generic def-
inition of a multi-dimensional array is provided by the
interface Boost.MultiArray interface. Some common imple-
mentations of this interface are also defined.
6.6.9OpenDataCube
A free and accessible exploitation architecture is provided
by the Open Data Cube (ODC) initiative [ 77] to boost the
impact of satellite Earth observation data.
6.6.10Xarray
Xarray [78] is an open source project which aims to provide
a pandas-like [ 79] toolkit to support analytics on multi-
dimensional arrays.
7MLinArrayDBMSs
7.1Systemcomparison
The importance of several database concepts, such as par-
titioning, caching and compression, applied to the efficient
processing of algorithms in ML systems was stated in
Section 4. This section provides an overview of the imple-
mentation state of those concepts in the above databases.
Tables3 and 4 summarize the information detailed below.
7.2Partitioning
When size of arrays is larger than server RAM or disk
partitions, partitioning becomes indispensable for handling
them. SciQL, Teradata Arrays, TensorFlow, Ophidia, Google
Earth Engine, boost::geometry, Open Data Cube, and xarray
do not support array partitioning. Rasdaman and EXTAS-
CID provide full support for multi-dimensional arrays with
any tiling. Some solutions provide limited support, Post-
GIS Raster supports partitioning only for small arrays,
Oracle GeoRaster only perfoms partitioning during raster
creation, SciDB only implements partitioniong for regular
multi-dimensional arrays, OPen-DAP provides partitioning
per NetCDF file, and TileDB supports regular tiling.
ASurveyonmachinelearninginarraydatabases
Table 3 Database features in ML
Array DBMS
Full-stack Array DBMS Add-on array support
Features Rasdaman SciDB SciQL EXTASCID PostGIS Raster Oracle GeoRaster Teradata Arrays
[51][ 52][ 63][ 62][ 64][ 66][ 65]
Partitioning any nD tiling regular nD no any nD chunking small arrays
(100x100)
yes (during
raster creation)
no
Compression lossy and lossless (zlib,
RLE, wavelets,...)
RLE no no no yes (JPEG, DEFLATE) no
Distribution automatic query distri-
bution, peer federation(shared nothing)
shared nothing no shared memory,
shared disk servers,shared nothing
no yes no
Caching yes (can reuse approxi-
mate caching)
yes (persistent chunk
caching, temporaryresult caching)
?n o n o y e s n o
Query rewriting yes yes yes no no no no
Common subexpression
elimination
yes ? ? no no no no
Cost-based optimization yes ? ? no no no no
Implementation on array DBMSs
S.VillarroyaandP.Baumann
Table 4 Database features in ML
Array Tools
Features wendelin.core TensorFlow OPen-DAP Ophidia Google Earth
Engine
TileDB boost::
geometry
Open Data
Cube
xarray
[69][ 70][ 72][ 73][ 74][ 75][ 76][ 77][ 79]
Partitioning maybe indi-
rectly
no per NetCDF no no regular tiling no no no
Compression no sparse tensor per NetCDF zlib no per tile no no no
Distribution maybe indi-
rectly
Cloud ML no yes no only if VFS
supports it
no no no
Caching yes yes no ? yes yes no yes no
Query rewriting no no no no no no no no no
Common subexpres-
sion elimination
no no no no yes no no no no
Cost-based
optimization
no no no no yes no no no no
Implementation on array Tools
7.3Compression
Both lossy and lossless compression techniques are included.
The impact of lossless compression is highly dependent on
the data properties, e.g., natural images can compress to
about 80% whereas thematic map layers can compress to
about 5%. Lossy compression is also used in some systems
but it may introduce inaccuracies in tile boundaries. Both
lossy and lossless compression schemas are offered by Ras-
daman, e.g., Zlib, RLE, wavelets. Some systems do not pro-
vide compression at all, e.g., SciQL, EXTASCID, PostGIS
Raster, Teradata Arrays, wendelin.core, Google Earth Engine,
boost::geometry, Open Data Cube, and xarray. JPEG and
DEFLATE schemas are provided by Oracle GeoRaster, whereas
SciDB only supports run lenght encoding (RLE). TensorFlow
provide compression support for sparse tensors. OPeN-DAP
also provides compression per NetCDF file. Zlib is supported
in Ophidia. TileDB supports compression per tile.
7.4Distribution
Horizontal scaling is enabled when either complete arrays or
tiles of an array can be partitioned. Of course, this requires
from dynamic reassembly which have to be performed
carefully to ensure a satisfying performance in computation-
ally expensive operations. SciQL, PostGIS Raster, Teradata
Arrays OPeN-DAP, Google Earth Engine, boost::geometry,
Open Data Cube, and xarray do not provide support for
array distribution. A shared nothing strategy is provided by
SciDB. Rasdaman offers automatic query distribution in a
peer federation environment with a shared nothing strat-
egy as well. EXTASCID supports differents options:shared
nothing, shared memory and shared disk. TensorFlow pro-
vide support for distribution on Cloud ML. TileDB relies on
the underlying VFS to support distribution.
7.5Caching
A significant speed-up can be accomplished by caching.
Some relevant questions arise when implementing caching
strategies, e.g., should intermediate results be cached or
only complete results?, can we reuse approximate cache
hits or exact cache content is required?. Rasdaman, SciDB,
Oracle Raster, wendelin.core, TensorFlow, Google Earth
Engine, TileDB, and Open Data Cube provide caching
capabilities. While Rasdaman also allows for the reuse
of approximate cached results, SciDB implements both
persistent chunk caching and temporary chunk caching.
7.6Queryrewriting
This technique has two main advantages: (1) replacing
query expressions by more efficient versions can have a
significant impact, as shown in Section 6, and (2) users are
free from providing the most efficient formulation. Both
a query language and runtime analysis of incoming code
are required to implement this feature. Query rewriting is
supported by Rasdaman, SciDB and SciQL.
7.7Commonsubexpressionelimination
Query engines implementing this technique are able to
identify identical parts within a query, appearing several
times in the query execution tree, and evaluate them only
ASurveyonmachinelearninginarraydatabases
once. Similarly to query rewriting, this frees users from
write their queries in the most efficient way. Support for
common subexpression elimination has been documented
only for Rasdaman and Google Earth Engine.
7.8Cost-basedoptimization
As already stated in Section6, cost-based optimizacion tries
to find the most efficient expression taking into account
information about the execution costs of operators, rather
than following pre-defined rules. For illustration purposes,
let us think in an join operation between array A and array B,
stored in execution nodes A and B respectively. Depending
on whether array A is copied to the node B, or array B is
copied to the node A, or a shared approach is implemented,
a significant difference in the execution time may occur.
Many impact factors (e.g., the actual tiling of both arrays)
can be taken into account in the decision making. Only
Rasdaman and Google Earth Engine have documented this
optimization feature.
7.9MLimplementations
To the best of these authors knowledge, only Rasdaman
and SciDB are able to implement ML algorithms. This
section explores these two different approaches, providing a
depeer view of ML implementation in both systems, through
two examples of ML algorithms implemented in both
systems.
7.10Rasdaman
Since its first publication in 1992, rasdaman has pioneered
the field of array databases. Extraction, retrieval, fusion and
aggregation on distributed arrays is performed in the server
through parallelization, effective optimization and hetero-
geneous hardware. Arrays can be stored either in the opti-
mized array store or in standard databases. A SQL-based
query language, called rasQL, is also provided. Rasdaman is
also a pattern for several Big Data stardards, such as the ISO
SQL/MDA (Multi-Dimensional Arrays) [67] and the OGC
Web Coverage Service (WCS) with its geo datacube query
language, Web Coverage Processing Service (WCPS).
UDTs and UDFs allow the use of linear algebra and
machine learning specific libraries. However, this could lead
to a decrease in the efficiency of machine learning algo-
rithms since different database optimization strategies can
not be applied. On the contrary, these optimization strate-
gies might be applied in the case of the implementation of
those algorithms in the database. Currently, the implemen-
tation of several machine learning algorithms can be done
in Rasdaman through UDFs implementing the underlying
linear algebra operations directly over the arrays.
Since machine learning algorithms can be implemented
as UDFs within the system, they can be called in rasQL
queries. Thus, users only need to call them with appropriate
parameters in a rasQL query. An example rasQL query to
train a simple univariate gradient descent linear regression
algorithm with regularization is shown below.
Fig.4 Training (red) and
predicted (blue) wind speed
values for the linear regression
algorithm
where c is a stored coverage, the number of examples in the
training set is 37, the number of iterations of the gradient
descent is 1500, and parameters alpha and lambda are
0.01 and 0.0 respectively.
S.VillarroyaandP.Baumann
The code of the user-defined function linear
regression, called by the above rasQL query, is shown
below.
Note that computations in this algoritm are applied
directly on the input training arrays x and y.A ni n t e r m e -
diate pre-processing step obtains the input arrays x and y
from the coverage c provided by the user rasQL query.
By using the Web Coverage Processing Service (WCPS) [80]
exposed by Rasdaman we can send queries to the Rasdaman
database. An example WCPS query that predicts the wind
speed at a specific georeferenced point for the requested dates
using a linear regression algorithm trained with wind speeds
of previous dates selected by the user is shown below.
where
• c iterates over all the coverages stored in the array
datacube S1 windspeed w.
• "2018-11-08":"2019-03-27" is the set of pre-
vious dates selected to train the model.
• (53.866278,8.304296) and "2019-03-28,
2019-04-05, 2019-05-03" are, respectively, the
georeferenced point and the dates for which the user
want to predict the wind speed.
Figure 4 depicts the predicted values for this example
together with training values. Predicted values are colored
in blue and training values are colored in red.
7.11SciDB
Similarly to rasdaman, SciDB is an array DBMS that
provides an Array Query Language (AQL) and an Array
Functional Language (AFL). A modified Postgres kernel
is the basis for its architecture, plus UDFs implementing
array functionality. Users need to use the provided linear
algebra operators as building blocks to implement the
ML algorithms. One of the most basic and used ML
algorithm for classification is the logistic regression
algorithm shown in Algorithm 6. In this case, the iterative
optimization algorithm is the Batch Gradient Descent
(BGD).
ASurveyonmachinelearninginarraydatabases
The example code to implement the above logistic regres-
sion algorithm in SciDB, available for download1,i ss h o w n
below.
1https://github.com/ADALabUCSD/SLAB/blob/master/tests/
MLAlgorithms%20(Single%20Node%20Dense)/src/scidb algs.py
The outer loop of the BGD algorithm is implemented in
Python because SciDB lacks form iterative constructs. For
each iteration, a SciDB query is first executed to calculate
the partial resulting model and the final resulting model is
updated. Notice that the code below must be implemented
by users. SciDB only provides the basic linear algebra
operand for matrix multiplication (gemm).
S.VillarroyaandP.Baumann
8Conclusions
Different strategies towards the integration of ML and
database systems have been surveyed in this paper. Special
emphasis has been devoted to the integration of ML
algorithms and arrays databases.
We have seen how linear algebra operations can be imple-
mented in relational databases. From simple straightforward
SQL implementations to more elaborated approaches, e.g,
by using user-defined functions. Iterative algorithms can
also be implemented in current relational databases by using
simple user-defined aggregates.
More sophisticated solutions go beyond the simple SQL
implementation and aim to leverage the underlying relational
data model. Two main approaches were overviewed.Learn-
ing over join leverages database dependencies to accelerate
ML algorithms. Statistical relational learning uses the struc-
ture information in the underlying data model to learn over
multiple-table datasets without making the IID asumption.
The list of linear algebra operations that array databases
should efficiently implement has been provided. An
exhaustive study of the linear algebra foundations of four
core ML learning algorithms has been undertaken.
Previous approaches are devoted to relational databases,
but many ML algorithms use different types of data. Thus,
several systems have been developed to process large amounts
of non-relational data. Specifically, those devoted to large
arrays of data, which are the main focus of this paper.
Then, a brief description of the most relevant DB-based
optimization techniques used in ML systems was provided.
Both logical and physical optimization rewrites were
exposed, e.g., common subexpression elimination, loop
vectorization, caching, partitioning. Moreover, data access
techniques in databases have been also adapted to be used in
ML systems to improve efficiency in ML algorithms. Both
lossy and lossless compression techniques were discussed.
Beginning with the array data model, current features of
array databases have been described. Depending on their
architecture, array databases may be grouped into four cate-
gories. A brief description of the most relevant frameworks
for array data processing in each category was provided. For
each category, a detailed discussion on the implementation
of the aforementioned optimization techniques was pointed
out. A summary is provided below.
• Except for TensorFlow, Array Tools do not provide
ML capabilities at all. Although TensorFlow is widely
used in ML, it has some drawbacks, e.g., it does not
provide Partitioning, Query rewriting and Cost-based
Optimization.
• Xtensor, Ophidia and Xarray are the Array Tools that pro-
vide the underlying mathematical operations required
to implement ML algorithms. However, none of them
provide built-in ML functionality. Moreover, Xarray do
not provided any of the database features in ML, and
Ophidia only provides Compression and Distribution.
• None of the Array Add-ons provide ML capability.
• Only EXTASCID and PostGIS Raster provide the
required mathematical operations for ML implemen-
tation. However, PostGIS Raster only provides Parti-
tioning for small arrays, while EXTASCID provides
Partitioning and Distribution.
• Regarding full-stack databases, both Rasdaman and
SciDB provide the underlying mathematical operators
but none of them provide built-in implementation of
ML algorithms. Rasdaman provides full implementa-
tion of the required database features in ML, while
SciDB provide limited functionality of some features,
e.g., Partitioning, Compression and Distribution.
Finally, two examples of ML algorithm implementation
have been described. As stated above, only those systems
categorized as full-stack array databases provide full
support to implement ML algorithms but do not provide
built-in ML algorithms. Thus, the implementation of more
complex machine learning algorithms, such as deep learning
methods, requires a large implementation effort from the
users of such databases.
Final judgments on machine learning in array databases
are stated below.
• Much research effort has to be undertaken to raise
the ML support in array databases to the level of ML
support in relational databases.
• TensorFlow is the only array tool currently used to
implement ML algorithms. However, the lack of some
database features makes the execution of models with
very large matrices (data matrix and/or model matrix)
very slow and sometimes unfeasible.
• None of the remainder array tools nor the array add-ons
provide the database features required for the efficient
implementation of ML algorithms.
• In our opinion, the most appropriate tools to provide
built-in implementation of complex ML algorithms
are those categorized as full-stack array databases,
since only they provide all the necessary features
and required mathematical operations. However, as
shown in Section7.9, none of them currently provide
such functionality and users have to implement ML
algorithms from the scratch.
9Futurework
Several issues arise when designing and implementing ML
algorithms in array databases. A key feature that allows
ASurveyonmachinelearninginarraydatabases
array databases to achieve good performance is related to
the efficient implementation of linear algebra operations on
distributed array data. Current ML algorithms may have to
perform computations on large matrices because both the
training dataset matrix and the model matrix may be very
large. When these matrices do not fit in main memory, both
data and model need to be distributed. Fortunately, array
databases can model these matrices as distributed arrays
and provide efficient storage techniques. However, the
efficient implementation of linear algebra operations (e.g,
matrix multiplication) involving such distributed arrays still
remains as a challenging problem which deserves research
efforts.
One of the most important research topics is related to the
declarative specification of iterative algorithms. Currently,
many array databases provide SQL-based query languages
but do not provide constructs for implementing For loops.
But even if they were provided, their execution would not
be efficient. Thus, constructs for declarative specification
of such iteration algorithms in provided query languages is
required.
Related to the above, declarative specification of iterative
algorithms give rise to very large execution plans. How
array databases can compile and execute these large and
complex computations is also a challenging research topic.
Acknowledgements The work in this paper is part of the DeepRain
project, funded by the German ministry of education and research
(BMBF) under grant number 01 IS 18047.
Funding Open Access funding provided thanks to the CRUE-CSIC
agreement with Springer Nature.
Open Access This article is licensed under a Creative Commons
Attribution 4.0 International License, which permits use, sharing,
adaptation, distribution and reproduction in any medium or format, as
long as you give appropriate credit to the original author(s) and the
source, provide a link to the Creative Commons licence, and indicate
if changes were made. The images or other third party material in this
article are included in the article’s Creative Commons licence, unless
indicated otherwise in a credit line to the material. If material is not
included in the article’s Creative Commons licence and your intended
use is not permitted by statutory regulation or exceeds the permitted
use, you will need to obtain permission directly from the copyright
holder. To view a copy of this licence, visithttp://creativecommons.
org/licenses/by/4.0/.
References
1. Kim M, Candan KS (2014) TensorDB: In-database tensor
manipulation with tensor-relational query plans. In: Proceedings
of the 23rd ACM International conference on conference on
information and knowledge management. CIKM ’14, pp 2039–
2041. ACM. https://doi.org/10.1145/2661829.2661842
2. Cohen J, Dolan B, Dunlap M, Hellerstein JM, Welton C (2009)
MAD skills: New analysis practices for big data. Proc VLDB Endow
2(2):1481–1492.https://doi.org/10.14778/1687553.1687576
3. Feng X, Kumar A, Recht B, R ´e C (2012) Towards a unified
architecture for in-RDBMS analytics. In: Proceedings of the
2012 ACM SIGMOD International conference on management of
data. SIGMOD ’12, pp 325–336. ACM. https://doi.org/10.1145/
2213836.2213874
4. Zhang Y, Zhang W, Yang J (2010) I/O-efficient statistical
computing with RIOT. 2010 IEEE 26th International Conference
on Data Engineering (ICDE 2010), pp 1157–1160
5. Luo S, Gao ZJ, Gubanov M, Perez LL, Jermaine C (2018) Scalable
linear algebra on a relational database system. SIGMOD Rec
47(1):24–31.https://doi.org/10.1145/3277006.3277013
6. Hellerstein JM, R ´e C, Schoppmann F, Wang DZ, Fratkin E,
G o r a j e kA ,N gK S ,W e l t o nC ,F e n gX ,L iK ,K u m a rA
(2012) The MADlib analytics library: Or MAD skills, the SQL.
Proc VLDB Endow 5(12):1700–1711. https://doi.org/10.14778/
2367502.2367510
7. Cheng Y, Qin C, Rusu F (2012) GLADE: Big data analytics made
easy. In: Proceedings of the 2012 ACM SIGMOD International
conference on management of data. SIGMOD ’12, pp 697–700.
ACM,.https://doi.org/10.1145/2213836.2213936
8. D’silva JV, De Moor F, Kemme B (2018) AIDA: Abstraction for
advanced in-database analytics. Proc VLDB Endow 11(1):1400–
1413. https://doi.org/10.14778/3236187.3236194
9. Deshpande A, Madden S (2006) MauveDB: Supporting model-
based user views in database systems. In: Proceedings of the 2006
ACM SIGMOD International conference on management of data.
SIGMOD ’06, pp 73–84. ACM.https://doi.org/10.1145/1142473.
1142483
10. Schelter S, Palumbo A, Quinn S, Marthi S, Musselman A (2016)
Samsara: Declarative machine learning on distributed dataflow
systems. In: NIPS MLSYs workshop, pp 1–8
11. Sujeeth AK, Lee H, Brown KJ, Chafi H, Wu M, Atreya AR,
Olukotun K, Rompf T, Odersky M (2011) OptiML: An implicitly
parallel domain-specific language for machine learning. In: Proceed-
ings of the 28th International conference on international con-
ference on machine learning. ICML’11, pp 609–616. Omnipress.
http://dl.acm.org/citation.cfm?id=3104482.3104559. Accessed 12
Oct 2019
12. Abadi M, Barham P, Chen J, Chen Z, Davis A, Dean J, Devin
M, Ghemawat S, Irving G, Isard M, Kudlur M, Levenberg J,
Monga R, Moore S, Murray DG, Steiner B, Tucker P, Vasudevan
V, Warden P, Wicke M, Yu Y, Zheng X (2016) Tensorflow:
A system for large-scale machine learning. In: Proceedings of
the 12th USENIX Conference on operating systems design and
implementation. OSDI’16, pp 265–283. USENIX Association.
http://dl.acm.org/citation.cfm?id=3026877.3026899. Accessed 12
Oct 2019
13. Boehm M, Dusenberry MW, Eriksson D, Evfimievski A V,
Manshadi FM, Pansare N, Reinwald B, Reiss FR, Sen P,
Surve AC, Tatikonda S (2016) SystemML: Declarative machine
learning on Spark. Proc VLDB Endow 9(13):1425–1436.
https://doi.org/10.14778/3007263.3007279
14. Park Y, Qing J, Shen X, Mozafari B (2019) BlinkML:
Efficient maximum likelihood estimation with probabilistic
guarantees. In: Proceedings of the 2019 International conference
on management of data. SIGMOD ’19, pp 1135–1152. ACM.
https://doi.org/10.1145/3299869.3300077
15. Yu Y, Tang M, Aref WG, Malluhi QM, Abbas MM, Ouz-
zani M (2017) In-memory distributed matrix computation
processing and optimization. In: 2017 IEEE 33rd Interna-
tional conference on data engineering (ICDE), pp 1047–1058.
https://doi.org/10.1109/ICDE.2017.150
16. Bosagh Zadeh R, Meng X, Ulanov A, Yavuz B, Pu L,
Venkataraman S, Sparks E, Staple A, Zaharia M (2016)
Matrix computations and optimization in Apache Spark. In:
Proceedings of the 22Nd ACM SIGKDD International conference
S. VillarroyaandP. Baumann
on knowledge discovery and data mining. KDD ’16, pp 31–38.
ACM. https://doi.org/10.1145/2939672.2939675
17. Villarroya S, Baumann P (2020) On the integration of machine
learning and array databases. In: 2020 IEEE 36th International
conference on data engineering (ICDE), pp 1786–1789. IEEE
Computer Society.https://doi.org/10.1109/ICDE48307.2020.00170
18. Rodriges Zalipynis RA (2021) Towards machine learning in
distributed array DBMS : Networking considerations. In: Renault,
´e., Boumerdassi, S, M¨uhlethaler, P. (eds.) Machine Learning for
Networking, pp 284–304
19. Ordo ˜nez C, Zhang Y, Johnsson SL (2019) Scalable machine
learning computing a data summarization matrix with a parallel
array DBMS. Distributed and Parallel Databases 37:329–350.
https://doi.org/10.1007/s10619-018-7229-1
20. Baxter J (2000) A model of inductive bias learning. J Artif Int Res
12( 1):149–198
21. Caruana R (1993) Multitask learning: a knowledge-based source
of inductive bias. In: Proceedings of the 10th International
conference on international conference on machine learning.
ICML’93, pp 41–48. Morgan Kaufmann Publishers Inc.http://
dl.acm.org/citation.cfm?id=3091529.3091535. Accessed 12 Oct
2019
22. Faghmous JH, Le M, Uluyol M, Kumar V, Chatterjee
S (2013) A parameter-free spatio-temporal pattern mining
model to catalog global ocean dynamics. In: 2013 IEEE
13th International conference on data mining, pp 151–160.
https://doi.org/10.1109/ICDM.2013.162
23. Liu Y, Bahadori MT, Li H (2012) Sparse-GEV: Sparse latent
space model for multivariate extreme value time series modeling.
In: Proceedings of the 29th international coference on interna-
tional conference on machine learning. ICML’12, pp 1195–1202.
Omnipress. http://dl.acm.org/citation.cfm?id=3042573.3042727.
Accessed 12 Oct 2019
24. Becker AS, Marcon M, Ghafoor S, Wurnig MC, Frauenfelder
T, Boss A (2017) Deep learning in mammography: Diagnostic
accuracy of a multipurpose image analysis software in the
detection of breast cancer. Invest Radiol 52(7):434–440
25. Liu S, Liu S, Cai W, Pujol S, Kikinis R, Feng D (2014) Early diag-
nosis of Alzheimer’s disease with deep learning. In: 2014 IEEE
11th International symposium on biomedical imaging (ISBI),
pp 1015–1018.https://doi.org/10.1109/ISBI.2014.6868045
26. Lee H, Tajmir S, Lee J, Zissen M, Yeshiwas BA, Alkasab TK,
Choy G, Do S (2017) Fully automated deep learning system
for bone age assessment. J Digital Imaging 30(4):427–441.
https://doi.org/10.1007/s10278-017-9955-8
27. Kallenberg M, Petersen K, Nielsen M, Ng AY, Diao P, Igel C,
Vachon CM, Holland K, Winkel RR, Karssemeijer N, Lillholm
M (2016) Unsupervised deep learning applied to breast density
segmentation and mammographic risk scoring. IEEE Trans
Med Imaging 35(5):1322–1331.https://doi.org/10.1109/TMI.2016.
2532122
28. Boehm M, Kumar A, Yang J (2019) Data management in machine
learning systems. Synthesis Lectures on Data Management
14(1):1–173.https://doi.org/10.2200/S00895ED1V01Y201901D
TM057
29. Jankov D, Luo S, Yuan B, Cai Z, Zou J, Jermaine C,
Gao ZJ (2019) Declarative recursive computation on an
RDBMS: Or, why you should use a database for dis-
tributed machine learning. Proc VLDB Endow 12(7):822–835.
https://doi.org/10.14778/3317315.3317323
30. Kumar A, Naughton J, Patel JM (2015) Learning gener-
alized linear models over normalized data. In: Proceedings
of the 2015 ACM SIGMOD International conference on
management of data. SIGMOD ’15, pp 1969–1984. ACM.
https://doi.org/10.1145/2723372.2723713
31. Schleich M, Olteanu D, Ciucanu R (2016) Learning lin-
ear regression models over factorized joins. In: Proceed-
ings of the 2016 International conference on management of
data. SIGMOD ’16, pp 3–18. ACM, New York, NY , USA.
https://doi.org/10.1145/2882903.2882939
32. Nikolic M, Olteanu D (2018) Incremental view maintenance with
triple lock factorization benefits. In: Proceedings of the 2018
International conference on management of data. SIGMOD ’18,
pp 365–380. ACM.https://doi.org/10.1145/3183713.3183758
33. Rendle S (2013) Scaling factorization machines to relational
data. Proc VLDB Endow 6(5):337–348. https://doi.org/10.14778/
2535573.2488340
34. Kumar A, Jalal M, Yan B, Naughton J, Patel JM (2015)
Demonstration of santoku: Optimizing machine learning
over normalized data. Proc VLDB Endow 8(12):1864–1867.
https://doi.org/10.14778/2824032.2824087
35. Chen L, Kumar A, Naughton J, Patel JM (2017) Towards linear
algebra over normalized data. Proc VLDB Endow 10(11):1214–
1225. https://doi.org/10.14778/3137628.3137633
36. Ghoting A, Krishnamurthy R, Pednault E, Reinwald B, Sindhwani
V, Tatikonda S, Tian Y, Vaithyanathan S (2011) SystemML:
Declarative machine learning on MapReduce. In: 2011 IEEE
27th International conference on data engineering, pp 231–242.
https://doi.org/10.1109/ICDE.2011.5767930
37. Li S, Chen L, Kumar A (2019) Enabling and optimiz-
ing non-linear feature interactions in factorized linear alge-
bra. In: Proceedings of the 2019 International conference on
management of data. SIGMOD ’19, pp 1571–1588. ACM.
https://doi.org/10.1145/3299869.3319878
38. Abo Khamis M, Ngo HQ, Nguyen X, Olteanu D, Schleich M
(2018) In-database learning with sparse tensors. In: Proceedings
of the 37th ACM SIGMOD-SIGACT-SIGAI Symposium on
Principles of Database Systems. SIGMOD/PODS ’18, pp 325–
340. ACM. https://doi.org/10.1145/3196959.3196960
39. Richardson M, Domingos P (2006) Markov logic networks. Mach
Learn 62(1):107–136. https://doi.org/10.1007/s10994-006-5833-1
40. Getoor L (2013) Probabilistic soft logic: A scalable
approach for markov random fields over continuous-valued
variables. In: Proceedings of the 7th International confer-
ence on theory, practice, and applications of rules on the
Web. RuleML’13, pp 1–1. Springer, Berlin, Heidelberg.
https://doi.org/10.1007/978-3-642-39617-51
41. Niu F, R ´e C, Doan A, Shavlik J (2011) Tuffy: Scal-
ing up statistical inference in markov logic networks
using an RDBMS. Proc VLDB Endow 4(6):373–384.
https://doi.org/10.14778/1978665.1978669
42. Niu F, Zhang C, Re C, Shavlik J (2012) Scaling inference
for markov logic via dual decomposition. In: Proceedings
of the 2012 IEEE 12th International conference on data
mining. ICDM ’12, pp 1032–1037. IEEE Computer Society.
https://doi.org/10.1109/ICDM.2012.96
43. Zhang C, R ´e C (2013) Towards high-throughput gibbs sampling
at scale: A study across storage managers. In: Proceedings
of the 2013 ACM SIGMOD International conference on
management of data. SIGMOD ’13, pp 397–408. ACM, New
York. https://doi.org/10.1145/2463676.2463702
44. Zhang C, R ´e C, Sadeghian A, Shan Z, Shin J, Wang F, Wu
S (2014) Feature engineering for knowledge base construction.
IEEE Data Eng Bull
45. Lu Y, Chowdhery A, Kandula S (2016) Optasia: A rela-
tional platform for efficient large-scale video analytics. In: Pro-
ceedings of the Seventh ACM Symposium on Cloud Com-
puting. SoCC ’16, pp 57–70. ACM, New York, NY , USA.
https://doi.org/10.1145/2987550.2987564
ASurveyonmachinelearninginarraydatabases
46. Zhang H, Ananthanarayanan G, Bodik P, Philipose M, Bahl
P, Freedman MJ (2017) Live video analytics at scale with
approximation and delay-tolerance. In: Proceedings of the
14th USENIX conference on networked systems design and
implementation. NSDI’17, pp 377–392. USENIX Association,.
http://dl.acm.org/citation.cfm?id=3154630.3154661. Accessed 13
Oct 2019
47. Watcharapichat P, Morales VL, Fernandez RC, Pietzuch P
(2016) Ako: Decentralised deep learning with partial gradi-
ent exchange. In: Proceedings of the Seventh ACM sym-
posium on cloud computing. SoCC ’16, pp 84–97. ACM.
https://doi.org/10.1145/2987550.2987586
48. Duan S, Babu S (2007) Processing forecasting queries. In:
Proceedings of the 33rd international conference on very large
data bases. VLDB ’07, pp 711–722. VLDB Endowment.http://
dl.acm.org/citation.cfm?id=1325851.1325933. Accessed 13 Oct
2019
49. Fischer U (2015) Forecasting in database systems. In: Seidl,
T, Ritter, N, Sch ¨oning, H, Sattler, K-U, H ¨arder, T, Friedrich,
S, Wingerath, W (eds.) Datenbanksysteme F¨ ur Business, Tech-
nologie und Web (BTW 2015), pp 483–492. Gesellschaft f¨ ur
Informatik e.V .
50. Low Y, Bickson D, Gonzalez J, Guestrin C, Kyrola A, Hellerstein
J (2010) Graphlab: A new framework for parallel machine
learning. In: UAI
51. Baumann P, Dehmel A, Furtado P, Ritsch R, Widmann N
(1998) The multidimensional database system RasDaMan. In:
Proceedings of the 1998 ACM SIGMOD International conference
on management of data. SIGMOD ’98, pp 575–577. ACM.
https://doi.org/10.1145/276304.276386
52. Stonebraker M, Brown P, Poliakov A, Raman S (2011) The
architecture of sciDB. In: Proceedings of the 23rd international
conference on scientific and statistical database management.
SSDBM’11, pp 1–16. Springer.http://dl.acm.org/citation.cfm?
id=2032397.2032399. Accessed 13 Oct 2019
53. Huang B, Babu S, Yang J (2013) Cumulon: Optimizing
statistical data analysis in the cloud. In: Proceedings of the
2013 ACM SIGMOD International conference on management
of data. SIGMOD ’13, pp 1–12. ACM, New York, NY , USA.
https://doi.org/10.1145/2463676.2465273
54. Sparks ER, Talwalkar A, Haas D, Franklin MJ, Jordan MI,
Kraska T (2015) Automating model search for large scale
machine learning. In: Proceedings of the Sixth ACM sym-
posium on cloud computing. SoCC ’15, pp 368–380. ACM.
https://doi.org/10.1145/2806777.2806945
55. Alexandrov A, Katsifodimos A, Krastev G, Markl V (2016)
Implicit parallelism through deep language embedding. SIGMOD
Rec 45(1):51–58. https://doi.org/10.1145/2949741.2949754
56. Russ R (2007) NetCDF-4 : Software implementing an enhanced
data model for the geosciences
57. Baumann P (2016) Array Databases. In: Liu L, ¨Ozsu M (eds)
Encyclopedia of Database Systems. Springer, New York, NY.
https://doi.org/10.1007/978-1-4899-7993- 32061-2
58. Baumann P, Misev D, Merticariu V, Huu BP (2021) Array
databases: concepts, standards, implementations. J Big Data 8:1–
61.https://doi.org/10.1186/s40537-020-00399-2
59. Baumann P (1994) Management of multidimensional discrete
data. VLDB J 3(4):401–444. https://doi.org/10.1007/BF01231603
60. Sarawagi S, Stonebraker M (1994) Efficient organization of
large multidimensional arrays. In: Proceedings of 1994 IEEE
10th International conference on data engineering, pp 328–336.
https://doi.org/10.1109/ICDE.1994.283048
61. Liaukevich V, Mi ˇsev D, Baumann P, Merticariu V (2017) Loca-
tion and processing aware datacube caching. In: Proceedings
of the 29th international conference on scientific and statis-
tical database management. SSDBM ’17, pp 34–1346. ACM.
https://doi.org/10.1145/3085504.3085539
62. Cheng Y, Rusu F (2013) Astronomical data processing in
EXTASCID. In: Proceedings of the 25th international conference
on scientific and statistical database management. SSDBM,
pp. 47–1474. ACM.https://doi.org/10.1145/2484838.2484875
63. Zhang Y, Kersten M, Ivanova M, Nes N (2011) SciQL:
Bridging the gap between science and relational DBMS. In:
Proceedings of the 15th Symposium on International Database
Engineering &#38; Applications. IDEAS ’11, pp 124–133. ACM.
https://doi.org/10.1145/2076623.2076639
64. PostGIS (2019 ) Post GIS Raster Manual. http://postgis.net/docs/
manual-dev/using raster dataman.html. Accessed 14 Oct 2019
65. Teradata (2019) Array Data Type. https://docs.teradata.com/r/
Teradata-Database-SQL-Data-Types-and-Literals/June-2017/
ARRAY/V ARRAY-Data-Type. Accessed 14 Oct 2019
66. GeoServer, Oracle Georaster User Manual (2019). https://docs.
geoserver.org/latest/en/user/data/raster/oraclegeoraster.html.
Accessed 14 Oct 2019
67. Information technology database languages — SQL — Part
15: Multi-dimensional arrays (SQL/MDA) (2019) Standard,
International Organization for Standardization
68. Baumann P (2010) The OGC web coverage processing
service (WCPS) standard. GeoInformatica 14(4):447–479.
https://doi.org/10.1007/s10707-009-0087-2. Accessed 14 Oct
2019
69. Nexedi (2016) Wendelin.core Tutorial. https://www.nexedi.com/
wendelin-Core.Tutorial.2016. Accessed 14 Oct 2019
70. TensorFlow (2019) An end-to-end open source machine learning
platform. https://www.tensorflow.org/. Accessed 15 Oct 2019
71. Xtensor (2019) Multi-dimensional arrays with broadcasting
and lazy computing. https://xtensor.readthedocs.io/en/latest/.
Accessed 15 Oct 2019
72. OPeNDAP (2019) Advanced Software for Remote Data Retrieval.
https://www.opendap.org/. Accessed 15 Oct 2019
73. Ophidia (2019) High Performance Data Mining & Analytics for
eScience. http://ophidia.cmcc.it/. Accessed 15 Oct 2019
74. Google Earth Engine (2019) A planetary-scale platform for
Earth science data & analysis. https://earthengine.google.com/.
Accessed 15 Oct 2019
75. Papadopoulos S, Datta K, Madden S, Mattson T (2016)
The TileDB array data storage manager. Proc VLDB Endow
10(4):349–360.https://doi.org/10.14778/3025111.3025117
76. Boost (2019) C++ Libraries. https://www.boost.org/doc/libs/1 71
0/libs/geometry/doc/html/index.html. Accessed 15 Oct 2019
77. Open Data Cube (2019) An Open Source Geospatial Data
Management & Analysis Platform. https://www.opendatacube.
org/. Accessed 15 Oct 2019
78. xarray (2019) N-D labeled arrays and datasets in Python. http://
xarray.pydata.org/en/stable/. Accessed 15 Oct 2019
79. McKinney W (2010) Data structures for statistical computing
in Python. In: St ´efan van der Walt, Jarrod Millman (eds.)
Proceedings of the 9th python in science conference, pp 56–61.
https://doi.org/10.25080/Majora-92bf1922-00a
80. Baumann P (2010) The OGC web coverage processing
service (WCPS) standard. Geoinformatica 14(4):447–479.
https://doi.org/10.1007/s10707-009-0087-2
Publisher’s noteSpringer Nature remains neutral with regard to
jurisdictional claims in published maps and institutional affiliations.
S.VillarroyaandP.Baumann
Dr. Sebasti´an Villarroya is
assistant profesor at Universi-
dade de Santiago de Compos-
tela postdoctoral. He worked
as research associate at Jacobs
University Bremen and Univer-
sidade de Santiago de Com-
postela. He obtained his PhD
at Universidade de Santiago
de Compostela, researching on
the integrated modeling and
analysis of big raster and vec-
tor data. Beyond distributed
big data analysis, sensor data
acquisition systems and big
spatial data analytics, he is
focused on the integration of machine learning technologies and raster
database management systems.
Prof. Peter Baumann leads
the L-SIS research group at
Jacobs University Bremen. He
pioneered the research field of
Array Databases and is Princi-
pal Architect of the worldwide
first complete and operational
Array DBMS, rasdaman. He
authored and co-authored
160+ book chapters and
papers on array databases and
related fields, and holds inter-
national patents. He received a
series of national and interna-
tional innovation awards. He
is actively shaping Big Data
standards, such as the Open Geospatial Consortium (OGC) Big Geo
Data suite and ISO SQL/MDA. He chairs several Big Data relevant
groups. More info at:www.faculty.jacobs-university.de/pbaumann.

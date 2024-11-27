UNET 3+: A FULL-SCALE CONNECTED UNET  FOR MEDICAL IMAGE SEGMENTATION Huimin Huang 1, *Lanfen Lin1, Ruofeng Tong1, *Hongjie Hu2, Qiaowei Zhang2, Yutaro Iwamoto3, Xianhua Han3, *Yen-Wei Chen3,4,1, Jian Wu1 1 College of Computer Science and Technology, Zhejiang University, China 2 Department of Radiology, Sir Run Run Shaw Hospital, China 3 College of Information Science and Engineering, Ritsumeikan University, Japan 4 Research Center for Healthcare Data Science, Zhejiang Lab, Hangzhou, China *Corresponding Authors: Lanfen Lin (llf@zju.edu.cn), Hongjie Hu (hongjiehu@zju.edu.cn), Yen-Wei Chen (chen@is.ritsumei.ac.jp)  ABSTRACT  Recently, a growing interest has been seen in deep learning-based semantic segmentation. UNet, which is one of deep learning networks with an encoder-decoder architecture, is widely used in medical image segmentation. Combining multi-scale features is one of important factors for accurate segmentation. UNet++ was developed as a modified Unet by designing an architecture with nested and dense skip connec-tions. However, it does not explore sufficient information from full scales and there is still a large room for improve-ment. In this paper, we propose a novel UNet 3+, which takes advantage of full-scale skip connections and deep supervi-sions. The full-scale skip connections incorporate low-level details with high-level semantics from feature maps in differ-ent scales; while the deep supervision learns hierarchical rep-resentations from the full-scale aggregated feature maps. The proposed method is especially benefiting for organs that ap-pear at varying scales. In addition to accuracy improvements, the proposed UNet 3+ can reduce the network parameters to improve the computation efficiency. We further propose a hybrid loss function and devise a classification-guided mod-ule to enhance the organ boundary and reduce the over-seg-mentation in a non-organ image, yielding more accurate seg-mentation results. The effectiveness of the proposed method is demonstrated on two datasets. The code is available at: github.com/ZJUGiveLab/UNet-Version Index Terms—Segmentation, Full-scale skip connection, Deep supervision, Hybrid loss function, Classification.  1. INTRODUCTION  Automatic organ segmentation in medical images is a critical step in many clinical applications. Recently, convolutional neural networks (CNNs) greatly promoted to developed a va-riety of segmentation models, e.g.  fully convolutional neural networks (FCNs) [1], UNet [2], PSPNet [3] and a series of DeepLab version [4-6]. Especially, UNet, which is based on an encoder-decoder architecture, is widely used in medical image segmentation. It uses skip connections to combine the high-level semantic feature maps from the decoder and cor-responding low-level detailed feature maps from the encoder. To recede the fusion of semantically dissimilar feature from plain skip connections in UNet, UNet++ [7] further strength-ened these connections by introducing nested and dense skip 
connections, aiming at reducing the semantic gap between the encoder and decoder. Despite achieving good performance, this type of approach is still incapable of exploring sufficient information from full scales.                                                                      As witnessed in many segmentation studies [1-7], feature maps in different scale explore distinctive information. Low-level detailed feature maps capture rich spatial information, which highlight the boundaries of organs; while high-level semantic feature maps embody position information, which locate where the organs are. Nevertheless, these exquisite sig-nals may be gradually diluted when progressively down- and up-sampling. To make full use of the multi-scale features, we propose a novel U-shape-based architecture, named UNet 3+, in which we re-design the inter-connection between the en-coder and the decoder as well as the intra-connection between the decoders to capture fine-grained details and coarse-grained semantics from full scales. To further learn hierar-chical representations from the full-scale aggregated feature maps, each side output is connected with a hybrid loss func-tion, which contributes to accurate segmentation especially for organs that appear at varying scales in the medical image volume. In addition to accuracy improvements, we also show that the proposed UNet 3+ can reduce the network parameters to improve the computation efficiency. To address the demand for more accurate segmentation in medical image, we further investigate how to effectively re-duce the false positives in non-organ images. Existing meth-ods solve the problem by introducing attention mechanisms [8] or conducting a pre-defined refinement approach such as CRF [4] at inference. Different from these methods, we ex-tend a classification task to predict the input image whether has organ, providing a guidance to the segmentation task.  In summary, our main contributions are four-fold: (i) de-vising a novel UNet 3+ to make full use of the multi-scale features by introducing full-scale skip connections, which incorporate low-level details with high-level semantics from feature maps in full scales, but with fewer parameters; (ii) developing a deep supervision to learn hierarchical represen-tations from the full-scale aggregated feature maps, which optimizes a hybrid loss function to enhance the organ bound-ary; (iii) proposing a classification-guided module to reduce over-segmentation on none-organ image by jointly training with an image-level classification; (iv) conducting extensive  
  Fig .1: Comparison of UNet (a), UNet++(b) and proposed UNet 3+ (c). The depth of each node is presented below the circle.   experiments on liver and spleen datasets, where UNet 3+ yields consistent improvements over a number of baselines.  2. METHODS  Fig.1 gives simplified overviews of UNet, UNet++ and the proposed UNet 3+. Compared with UNet and UNet++, UNet 3+ combines the multi-scale features by re-designing skip connections as well as utilizing a full-scale deep supervision, which provides fewer parameters but yields a more accurate position-aware and boundary-enhanced segmentation map.  2.1. Full-scale Skip Connections  The proposed full-scale skip connections convert the inter-connection between the encoder and decoder as well as intra-connection between the decoder sub-networks. Both UNet with plain connections and UNet++ with nested and dense connections are short of exploring sufficient information from full scales, failing to explicitly learn position and bound-ary of an organ. To remedy the defect in UNet and UNet++, each decoder layer in UNet 3+ incorporates both smaller- and same-scale feature maps from encoder and larger-scale fea-ture maps from decoder, which capturing fine-grained details and coarse-grained semantics in full scales.  As an example, Fig. 2 illustrates how to construct the feature map of 𝑋"#$. Similar to the UNet, the feature map from the same-scale encoder layer 𝑋%&$ are directly received in the decoder. In contrast to the UNet, a set of inter encoder-decode skip connections delivers the low-level detailed information from the smaller-scale encoder layer 𝑋%&' and 𝑋%&(, by apply-ing non-overlapping max pooling operation; while a chain of intra decoder skip connections transmits the high-level se-mantic information from larger-scale decoder layer 𝑋"#) and 𝑋"#*, by utilizing bilinear interpolation. With the five same-resolution feature maps in hand, we need to further unify the number of channels, as well as reduce the superfluous infor-mation. It occurred to us that the convolution with 64 filters of size 3 × 3 can be a satisfying choice. To seamlessly merge the shallow exquisite information with deep semantic infor-mation, we further perform a feature aggregation mechanism 
on the concatenated feature map from five scales, which con-sists of 320 filters of size 3 × 3, a batch normalization and a  ReLU activation function. Formally, we formulate the skip connections as follows: let i indexes the down-sampling layer along the encoder, 𝑁 refers to the total number of the encoder. The stack of feature maps represented by 𝑋"#, is computed as:  𝑋"#,=𝑋%&,,																																																																																				𝑖=𝑁ℋ∁𝒟𝑋%&445',6',∁𝑋%&, ,789:#;:'=>~,=> ∁𝒰𝑋"#445,A'B
789:#;:(,A')=>~B=>	,𝑖=1,⋯,𝑁−1	(1) 
where function ∁(∙) denotes a convolution operation,  ℋ(∙) realizes the feature aggregation mechanism with a convolu-tion followed by a batch normalization and a ReLU activation function. 𝒟(∙) and 𝒰(∙) indicate up- and down-sampling op-eration respectively, and [∙] represents the concatenation.  It is worth mentioning that our proposed UNet 3+ is more efficient with fewer parameters. In the encoder sub-network, UNet, UNet++, and UNet 3+ share the same structure, where 𝑋%&, has 32×	2, channels. As for the decoder, the depth of feature map in UNet is symmetric to the encoder, and thus 𝑋"#, also has 32	×	2, channels. The number of parameters in 𝑖LM decoder stage of UNet (𝑃O6"#, ) can be computed as:   𝑃O6"#, =𝐷Q×𝐷Q×𝑑𝑋"#,A'×𝑑𝑋"#, +𝑑𝑋"#,(																																					+𝑑𝑋%&,+𝑋"#,×𝑑𝑋"#,																																								(2) where 𝐷Q is the convolution kernel size, 𝑑∙ denotes the depth of the nodes. When it comes to UNet++, it makes use of a dense convolution block along each skip pathway, where 𝑃OTT6"#,  can be computed as: 																	𝑃OTT6"#, =𝐷Q×𝐷Q×𝑑𝑋"#,A'×𝑑𝑋"#, 	+𝑑𝑋"#,(+																																																								𝑑𝑋%&,+ 𝑋U#,,4+B6'6,45' 𝑋"#,×𝑑𝑋"#, 			(3)  As can be seen, 𝑃OTT6"#,  is larger than 	𝑃O6"#, . While in UNet 3+, each decoder feature map is derived from 𝑁 scales, yielding 64	×	𝑁 channels. 𝑃OXT6"#,  can be computed as: 𝑃OXT6"#, =𝐷Q×𝐷Q× 𝑑𝑋%&4+,45' 𝑑𝑋"#4B45,A' ×64+𝑑𝑋"#,((4)        For the sake of the channel reduction, the parameters in UNet 3+ is fewer than those in UNet and UNet++. 

 Fig. 2: Illustration of how to construct the full-scale aggre-gated feature map of third decoder layer 𝑋"#$.  2.2. Full-scale Deep Supervision  In order to learn hierarchical representations from the full-scale aggregated feature maps, the full-scale deep supervision is further adopted in the UNet 3+. Compared with the deep supervision performed on generated full-resolution feature map in UNet++, the proposed UNet 3+ yields a side output from each decoder stage, which is supervised by the ground truth. To realize deep supervision, the last layer of each de-coder stage is fed into a plain 3 × 3 convolution layer fol-lowed by a bilinear up-sampling and a sigmoid function. To further enhance the boundary of organs, we propose a multi-scale structural similarity index (MS-SSIM) [9] loss function to assign higher weights to the fuzzy boundary. Ben-efiting from it, the UNet 3+ will keep eye on fuzzy boundary as the greater the regional distribution difference, the higher the MS-SSIM value. Two corresponding N×N sized patches are cropped from the segmentation result P and the ground truth mask G, which can be denoted as 𝑝={𝑝[:𝑗=1,…,𝑁(} and	𝑔=𝑔[:𝑗=1,…,𝑁(, respectively. The MS-SSIM loss function of p and g is defined as: ℓa;6;;,a=1−	 2𝜇c𝜇d+𝐶'𝜇c(+𝜇d(+𝐶'fg2𝜎cd+𝐶(𝜎c(+𝜎d(+𝐶(ig	U
a5' 	(5) where 𝑀 is the total number of the scales, 𝜇c,	𝜇d and 𝜎c, 𝜎c are the mean and standard deviations of p and g, 𝜎cd denotes their covariance.	𝛽a and 𝛾a define the relative importance of the two components in each scale, which are set according to [9]. Two small constants 𝐶'=0.01( and 𝐶(=0.03( are added to avoid the unstable circumstance of dividing by zero. In our experiment, we set the scale to 5 based on [9]. By combining focal loss (ℓp:) [10], MS-SSIM loss (ℓa;6;;,a) and IoU loss (ℓ,qr) [11], we develop a hybrid loss for segmentation in three-level hierarchy – pixel-, patch- and map-level, which is able to capture both large-scale and fine structures with clear boundaries. The hybrid segmentation loss (ℓ;#d)is defined as: ℓ;#d=	ℓp:+	ℓa;6;;,a+ℓ,qr																						(6) 2.3. Classification-guided Module (CGM) In the most medical image segmentations, the appearance of false-positives in a non-organ image is an inevitable circums-  
  Fig. 3: Illustration of classification-guided module (CGM).  tance.  It is, in all probability, caused by noisy information from background remaining in shallower layer, leading to the phenomenon of over-segmentation. To achieve more accu-rate segmentation, we attempt to solve this problem by adding an extra classification task, which is designed for predicting the input image whether has organ or not. As depicted in Fig. 3, after passing a series of operations including dropout, convolution, maxpooling and sigmoid, a 2-dimensional tensor is produced from the deepest-level 𝑋%&*, each of which represents the probability of with/without or-gans. Benefiting from the richest semantic information, the classification result can further guide each segmentation side-output in two steps. First, with the help of the argmax func-tion, 2-dimensional tensor is transferred into a single output of {0,1}, which denotes with/without organs. Subsequently, we multiply the single classification output with the side seg-mentation output. Due to the simpleness of binary classifica-tion task, the module effortlessly achieves accurate classifi-cation results under the optimization of Binary Cross Entropy loss function [12], which realizes the guidance for remedying the drawback of over-segmentation on none-organ image.  3. EXPERIMENTS AND RESULTS  3.1. Datasets and Implementation  The method was validated on two organs: the liver and spleen. The dataset for liver segmentation is obtained from the ISBI LiTS 2017 Challenge. It contains 131 contrast-enhanced 3D abdominal CT scans, of which 103 and 28 volumes are used for training and testing, respectively. The spleen dataset from the hospital passed the ethic approvals, containing 40 and 9 CT volumes for training and testing. In order to speed up training, the input image had three channels, including the slice to be segmented and the upper and lower slices, which was cropped to 320×320. We utilized the stochastic gradient descent to optimize our network and its hyper parameters were set to the default values. Dice coefficient was used as the evaluation metric for each case. 3.2. Comparison with UNet and UNet++  In this section, we first compared the proposed UNet 3+ with UNet and UNet++. The loss function used in each method is the focal loss.   

Table 1: Comparison of UNet, UNet++, the proposed UNet 3+ without deep supervision (DS) and UNet 3+ on liver and spleen datasets in terms of Dice metrics. The best results are highlighted in bold. The loss function used in each method is focal loss. 
 
 Fig. 4: Qualitative comparisons of ResNet-101-based UNet, UNet++, and proposed UNet 3+ on liver dataset. Purple ar-eas: true positive (TP); Yellow areas: false negative (FN); Green areas: the false positive (FP).   (i) Quantitative comparison: Based on the backbone of Vgg-16 and ResNet-101, Table 1 compares UNet, UNet++ and the proposed UNet 3+ architecture in terms of the number of parameters and segmentation accuracy on both liver and spleen datasets. As seen, UNet 3+ without deep supervision achieves a surpassing performance over UNet and UNet++, obtaining average improvement of 2.7 and 1.6 point between two backbones performed on two datasets. Taking account of both liver and spleen appearing at varying scales in CT slices, UNet 3+ combined with full-scale deep supervision further improved 0.4 point.  (ii) Qualitative comparison: Figure 2 exhibits the segmenta-tion results of ResNet-101-based UNet, UNet++ and UNet 3+ with full-scale deep supervision on liver datasets. It can be observed that our proposed method not only accurately local-izes organs but also produces coherent boundaries, even in small object circumstances.  3.3. Comparison with the State of the Art  We quantitatively compare our ResNet-101-based UNet 3+ with several recent state-of-the-art methods: PSPNet [3], De-epLabV2 [4], DeepLabV3 [5], DeepLabV3+ [6] and Atten-tion UNet [8]. It is worth mentioning that all results are di-rectly from single-model test without relying on any post-pro-cessing tools. Moreover, all networks were optimized by the loss function proposed in their own paper. Table 2 summarizes the quantitative comparison results. As seen, the proposed hybrid loss function greatly improves the performance by taking pixel-, patch-, map-level optimi-zation in to consideration. Especially, the patch-level MS-SSIM loss function contributes to assign higher weights to the  
 Table 2: Comparison of UNet 3+ and other 5 state-of-the-art methods. The best results are highlighted in bold. 
 fuzzy boundary, yielding more enhance boundary-aware seg-mentation map. Moreover, taking advantages of the classifi-cation-guidance module, UNet 3+ skillfully avoids the over-segmentation in complex background. As it can be seen, this approach is outstanding compared to all other previous ap-proaches. It is also worth noting that the proposed method outperforms the second best result on the liver (0.9675 against 0.9341) and spleen (0.9620 against 0.9324).   4. CONCLUSIONS  In this paper, we proposed a full-scale connected UNet, named UNet 3+ with deep supervision in order to make max-imum use of feature maps in full scales for accurate segmen-tation and efficient network architecture with fewer parame-ters. The classification-guided module and a hybrid loss func-tion is further introduced to yielding more accurate position-aware and boundary-aware segmentation map. Experimental results on both liver and spleen datasets demonstrated that UNet 3+ surpasses all previous state-of-the-art approaches, highlighting the organs and producing coherent boundaries.  5. ACKNOWLEDGMENTS This work was supported in part by Major Scientific Research Project of Zhejiang Lab under the Grant No.2018DG0ZX01, in part by the Key Science and Technology Innovation Sup-port Program of Hangzhou under the Grant No.20172011A038, and in part by the Grant-in Aid for Sci-entific Research from the Japanese Ministry for Education, Science, Culture and Sports (MEXT) under the Grant No. 18H03267 and No.17H00754.   
Architecture Vgg-16 ResNet-101 𝐷𝑖𝑐𝑒9u#v9d# Params 𝐷𝑖𝑐𝑒:,u#v 𝐷𝑖𝑐𝑒;c:##& Params 𝐷𝑖𝑐𝑒:,u#v 𝐷𝑖𝑐𝑒;c:##& UNet 39.39M 0.9206 0.9023 55.90M 0.9387 0.9332 0.9237 UNet++ 47.18M 0.9278 0.9230 63.76M 0.9475 0.9423 0.9352 UNet 3+ w/o DS 26.97M 0.9489 0.9437 43.55M 0.9580 0.9539 0.9511 UNet 3+  26.97M 0.9550 0.9496 43.55M 0.9601 0.9560 0.9552 
Method 𝐷𝑖𝑐𝑒:,u#v 𝐷𝑖𝑐𝑒;c:##& PSPNet [3] 0.9242 0.9240 DeepLabV2 [4] 0.9021 0.9097 DeepLabV3 [5] 0.9217 0.9217 DeepLabV3+ [6] 0.9186 0.9290 Attention UNet [8] 0.9341 0.9324 UNet 3+ (focal loss) 0.9601 0.9560 UNet 3+ (Hybrid loss) 0.9643 0.9588 UNet 3+ (Hybrid loss + CGM) 0.9675 0.9620 
6. REFERENCES  [1] J. Long, E. Shelhamer and T. Darrell, “Fully Convolutional Networks for Semantic Segmentation,” The IEEE Conference on Computer Vision and Pattern Recognition, pp. 3431-3440, 2015. [2] O. Ronneberger, P. Fischer and T. Brox, “U-Net: Convolutional Networks for Biomedical Image Segmentation,” Medical Im-age Computing and Computer-Assisted Intervention, pp.234-241, 2015. [3] H.S. Zhao, J.P. Shi, X.J. Qi, X.G. Wang and J.Y. Jia, “Pyramid scene parsing network,” The IEEE Conference on Computer Vi-sion and Pattern Recognition, pp. 2881-2890, 2017. [4] L-C. Chen, G. Papandreou, I. Kokkinos, K. Murphy and A.L. Yuille, “Semantic image segmentation with deep convolutional nets, atrous convolution, and fully connected crfs,” IEEE trans-actions on pattern analysis and machine intelligence, vol.40, no.4, pp: 834-848, 2018. [5] L-C. Chen, G. Papandreou, F. Schroff and H. Adam, “Rethink-ing atrous convolution for semantic image segmentation”, arXiv preprint arXiv:1706.05587, 2017. [6] L-C. Chen, Y.K. Zhu, G. Papandreou and H. Adam, “Encoder-decoder with atrous separable convolution for semantic image segmentation,” Proceedings of the European Conference on Computer Vision, 2018.   [7] Z.W. Zhou, M.M.R. Siddiquee, N. Tajbakhsh and J.M. Liang, “UNet++: A Nested U-Net Architecture for Medical Image Segmentation,” Deep Learning in Medical Image Anylysis and Multimodal Learning for Clinical Decision Support, pp: 3-11, 2018. [8] O.O et al., “Attention u-net: Learning where to look for the pan-creas,” Medical Imaging with Deep Learning, 2018. [9] Z. Wang, E.P. Simoncelli and A.C. Bovik, “Multiscale struc-tural similarity for image quality assessment,” The Thrity-Sev-enth Asilomar Conference on Signals, Systems & Computers, 2003. [10] T.-Y. Lin, P. Goyal, R. Girshick, K.M. He and P. Dollar. “Focal loss for dense object detection,” The IEEE international con-ference on computer vision, pp. 2980-2988, 2017.   [11] G. Mattyus, W.J. Luo, and R. Urtasun, “Deep-roadmapper: Ex-tracting road topology from aerial images”, The IEEE interna-tional conference on computer vision, 2017.  [12] P.-T. Boer et al., “A tutorial on the cross-entropy method,” An-nals of Operations Research, vol.134, no.1, pp. 19–67, 2005.   
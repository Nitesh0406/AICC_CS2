<h1 style="text-align:center;font-size:30px;" > Forge Signature Detection </h1>

<img src='signature.png'/>

# List of Contents :
* Description
* Problem Statement
* Source of Data
* Business Objectives and Constrains
* Data Overview
* Mapping Real World Problem to Machine Learning Problem
* Performance Metric

# Description
In present forensic authentication is performed by visual examination of biometric signature by the expert without relying on a computer system. Manual examination of authentication depends on experts physical and mental condition also availability of experts is not possible all the time. Think about the situation where thousands of the documents to be examined by the experts, Such a case could not be handled manually in a given time.


Not only in forensic authentication, Forge signature is a big problem in many areas like unauthorized cash withdrawal from banks, property takeover, illegal money transfer etc. Automatic forge detection technique will improve the security system and authentication of individuals.

# Problem Statement
The task is to predict whether the given signature is genuine or forge signature.

# Source of data
To perform forge signature detection, This case study refers this paper

http://www.iapr-tc11.org/archive/icdar2009/papers/3725b403.pdf

and the dataset is available in this link 

 http://www.iapr-tc11.org/mediawiki/index.php/ICDAR_2009_Signature_Verification_Competition_(SigComp2009)

# Objectives and Contrains
* <b>Latency requirement : </b> We do not need to predict whether the given signature is genuine or forge in milliseconds but it should not exceed for more than few minutes. 
* <b>Effect of misclassification : </b> Signature Forgery involves very sensitive cases like authentication of criminal, case withdrawal in banks and property overtaking. These are very sensitive cases where cost of misclassification will be very high.
* <b>Performance Objective : </b> In this task we want that every authentic signature should be predicted as genuine and every fake signature should be predicted as forge signature with high precision and recall.
* <b>Output : </b> Our model output would tell the similarity score between actual signature and questioned signature. Instead of giving binary output we want our model to give probalistic output such that if the questioned signature is genuine then the output should be close to 1 and if the questioned signature is fake then the output should be close to 0.

# Data Overview
The dataset we have consists two different dataset for training and evaluation called SigComp2009-training and SigComp2009-evaluation.
### SigComp2009-training
The name of this signature dataset is ‘NISDCC’ which is derived from the acronyms from both the NISlab (NIS) and Donders Centre for Cognition (DCC). They collected the signature by both online and offline fashion.

<b>Offline Data : </b> The offline training data contains static information only. This dataset is composed of 1920 images from 12 authentic writers (5 authentic signatures per writer) and 31 forging writers (5 forgeries per authentic signature). The static information is nothing but a .png Image file.

<b>Online Data : </b> Online Training data contains dynamic information. This data is available in form of text and stored in .hwr format. To understand dynamic information let’s look at the below figure.
<img src='sig1.png'  width="400" height="400" >
The left one is the original signature and the image at right side is segments of that original image. While recording online information they captured five features for each segment of the signature. Those five features are
* x = x-position on tablet (raw tablet values)
* y = y-position on tablet (raw tablet values), (0,0) is bottom-left (!)
* p = pen pressure in raw tablet values, between [0-1023]
* a = azimuth angle of the pen (yaw) between [0-3600] (10*degrees)
* e = elevation angle of the pen (pitch) between [0-900] (10*degrees)

<b>File Naming : </b> 
    In training data we do not have separate files for genuine signature and forge signature. The original and forge signature is identified by the file name. Each writer is identified via a three digit identifier. The 12 authentic writers are identified as <aaa>={001,002,...012}. The 31 forgers are identified as <fff>={021,022,...,051}. Each signature is contained in a separate file, "NISDCC-<www>_<aaa>_<iii>" with extension ".hwr". Note that the writer identification <www> can refer to an authentic writer or forger. The index <idx> represents the position of the signature on the A4 sheet of paper. So, the file NISDCC-001_001_001.hwr contains the first signature written by the authentic writer <aaa>=001. Similarly, the fileNISDCC-051_012_005.hwr contains the last forged signature from <aaa>=012 written by forger <fff>=051.
    
### SigComp2009-evaluation
This dataset is used to calculate the performance of the model. For the evaluation dataset we have separate files for genuines and forgeries. The dataset collected at the Netherlands Forensic Insti- tute (NFI) was not provided to the participants before the evaluation of the systems and consists of authentic signatures from 100 newly introduced writers (each writer wrote his signature 12 times) and forged signatures from 33 writ- ers (6 forgeries per signature). Each authentic signature was forged by 4 writers. It has 1953 signatures for both the on- line and the offline dataset.

# Mapping Real World Problem to Machine Learning Problem
The Task is to predict whether the given signature is Genuine or Forge signature so the problem statement can be mapped into binary class classification task. 

# Performance Metric
Saying good or bad is not enough for the model, we need numerical metrics to evaluate the performance of the model. For this task we will use EER (Equal error rate) as performance metric of the model. Let’s first understand two terminologies which are FRR, FAR.

<b>FRR : </b> FRR stands for Flase Rejection Rate. In simple words FRR is high when genuine signature is predicted as forge signature.

<b>FAR : </b> FAR stands for False Acceptance Rate. FAR is high when forge signature is predicted as genuine signature.

<b>EER : </b> If we reduce FAR then FRR will be high and vise-versa. Either of the situation is undesirable. What we try to achieve with such systems is a balance between the two error types,  EER can be easily understand by below figure
<img src='EER.png'  width="400" height="400" >

https://www.sciencedirect.com/topics/computer-science/false-acceptance-rate#:~:text=For%20example%2C%20if%20a%20false,overall%20measure%20of%20system%20performance

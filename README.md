# End-to-End MR Fingerprinting Reconstruction
Tissue parameter map reconstruction from the 3D spatial-temporal magnetic resonance fingerprinting (MRF) signal data using a single end-to-end deep learning model. It addresses the fundamental 2D image aliasing problem in MRF as well as the time-series pattern matching. Detailed neural network structure will not be disclosed at this moment.

### Introduction
Without getting into details and causing too much confusion, MRI can be analogous to a clear picture taken with a tripod whereas MRF is like a short film taken by a shaky camera and you want a clear mapping of the underlying properties about the subject. 

MRF signal data is 3D spatial-temporal data (2D spatial + 1D time). The pixel-wise time-series signal, depending on the tissue parameters and experimental setups, forms a unique pattern or a 'Fingerprint' that later is used for pattern matching. However, the MRF signal is spatially aliased at each time frame due to highly undersampled Fourier sapce (k-space). And the aliasing artifacts contaminate into the time domain and cause errors in the pattern matching. For example, the MRF signal data is shown in the gif along side with the corresponding ground truth T1 map.

The objective of this work is to achieve a robust reconstruction that addresses not only the 2D aliasing problem but also the time-series pattern matching, all using a single end-to-end deep learning model.

<p align="center">
<img src="https://github.com/mxf293/End-to-End_MR_Fingerprinting_Reconstruction/blob/master/pics/MRF_Signal.gif" width="300" height="300">
<img src="https://github.com/mxf293/End-to-End_MR_Fingerprinting_Reconstruction/blob/master/pics/Ground%20Truth%20T1%20Map.png" width="320" height="320">
</p>

### Data Source
The dataset used in this work is acquired by our collaborator Brendan Eck for his study [Increasing the Value of Legacy MRI Scanners with Magnetic Resonance Fingerprinting](https://www.ismrm.org/19/program_files/Th07.htm). It consists of 38 MRF vivo scans on 5 subjects and 2 different MRI machines with the same experimental setup. 

### Neural Network Model and Training
In this work, the neural network model inputs gridded 3D spatial-temporal MRF signal data in the x-space and outputs the tissue parameter T1 map. The MRF signal data is synthesized using the experimental T1 and T2 maps and the aliasing artifact at each time frame is caused by a randomly undersampled k-space (ten-fold). The MRF signal has 25 time frames in with each TRs about 7ms in this example. 
The dataset with 38 examples is split into 30 for training and 8 for test. Early stopping is used to prevent overfitting. 

### Results
The end-to-end reconstruction has several huge advantages over the conventional reconstruction method. 
1. The aliasing artifact is removed in the final reconstructed parameter map. It is free from the fundamental problem of undersampling k-space in MRF.
2. Pixel-wise time-series pattern matching is no longer needed and the reconstruction time is reduced astronomically. Once the deep learning model is trained, the model prediction takes less a second to run for a given input. 
3. There's no need for a pre-generated dictionary. Since the dictionary is generated by discrete numbers of T1 and T2, the final pattern-matched T1 T2 maps have to be discrete as well. Moveover, the more precise one wants to achieve, the bigger the dictionary becomes in size. On the contrary, the deep learning model only needs to store the weights at different layers and outputs contineous values.
4. The robustness of reconstruction has made possible a much shorter MRF scan time. Merely 25 time frames were used in the example and it was able to achieve high-resolution artifact-free T1 map. In this imaging/reconstruction scheme, the scan time for a single slice can be well under a second and the image can be visualized instantly.

<p align="center">
<img src="https://github.com/mxf293/End-to-End_MR_Fingerprinting_Reconstruction/blob/master/pics/Recon%20T1%20-%20Ground%20Truth%20T1.png" width="600" height="300">
</p>




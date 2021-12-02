+++
title = "Data Science"
weight = 1
date = "2021-01-12"
[extra]
math = true
+++


# Data science and machine learning
During four years of work in data science and astrophysics, I was involved a number of different projects which I managed with limited supervision. The first project analysed gravitational-wave data produced by numerical-relativity simulations of neutron star mergers along with associated equation of state parameters. The aim was to generate the gravitational-wave signal given the equation of state information.


# Hierarchical machine learning model
Following on from the analysis performed with Principal Component Analysis and the Random Forest and Lasso regressors, we developed a multi-layered machine learning algorithm that was trained to produce gravitational-wave spectra. We had a relatively small data set with 35 time-domain signals. Each of these waveforms were associated with system parameters on which we based our model. Of the thirteen system parameters, we found that we could achieve good results using Leave-One-Out Cross-Validation by using only three system parameters. The results were published in <a href="https://journals.aps.org/prd/abstract/10.1103/PhysRevD.100.043005" target="_blank">Physics Review D</a>, <a href="https://arxiv.org/abs/1811.11183" target="_blank">arXiv</a> and Chapter two of my <a href="https://github.com/pauleaster/PhDThesis/blob/71e87f2a70aab06d60ea0c9e65dc7666961d33c6/PaulEasterPhDThesis.pdf" target="_blank">PhD Thesis</a>. The following plot shows how well I was able to reconstruct the cross-validated spectrum for a given signal. 

<!-- <img1> -->
<div class="row">
    <div class="column">
    <a href="/NewAllMatches35withCoRe.pdf" target="_blank">
    <img src="/NewAllMatches35withCoRe.pdf" alt="Cross-Validated predicted signals" id="img1" style="width:100%">
    </a></div>
    <div class="column">
    <a href="/newFittingFactorHistogram35.pdf" target="_blank">
    <img src="/newFittingFactorHistogram35.pdf" alt="Cross-Validated fits"  id="img2" style="width:100%">
    </a></div>
</div>
<!-- ![Cross-Validated predicted signals](/NewAllMatches35withCoRe.pdf)! -->
<!-- </img1> -->


The model parameters are $(C, K, M)$ and the gravitational-wave signal is $H(f)$. The multiple layers in the algorithm consist of: 

    1. Preprocess all gravitational-wave signals by converting to the frequency domain and performing a frequency shift to align the peak frequencies. The resulting signals are designated as H(f).

    2. Remove signal under test and associated parameters from the training set.

    3. Train analytical relationship between parameters in the training set, this allows C to be determined given (K, M).

    4. Train the main model that relates the system parameters (C, K, M) to the signals H(f) where C is calculated from step 3. After training H(f) can be predicted given (K, M). 

    5. The inferred spectra is frequency shifted by an analytical relationship that relates K and fpeak.
    
    6. Compare the predicted H(f) to the value in the test set. This is what is shown in the plot above.
    
    
    

# Principal Components Analysis and Random Forest Regression
In these projects, I analysed gravitational-wave time-domain signals in both the time domain and frequency (Fourier) domain. Summaries of these results are shown below, for more details refer to my <a href="https://github.com/pauleaster/HonoursThesis/blob/30d1b50cdb4f6c229e37b5b5f58c7bebdcbe81df/BNSHonoursReport.pdf" target="_blank">honours thesis</a>. The main methods used were principal components analysis, random forest regression, and lasso regression. 
## Principal Component Analysis 
For unsupervised learning, I used principal component analysis to investigate the grouping of signals in respect to the principal components. The data was naturally separated into five different categories which are shown by colour on the plots below. The first plot shows the principal components when reduced to the three most significant components, and the second plot shows the projection of this plot onto the first two principal components. As can be seen from both of these plots, the APR4 (red) and SLy(pink) have similar principal components and are clustered closely in these projections. The other signals are loosely clustered together in another part of the parameter space.

<div class="row">
    <div class="column">
    <a href="/PCAUnscaled3Damp.pdf" target="_blank">
    <img src="/PCAUnscaled3Damp.pdf" alt="PCA in 3D" id="img1" style="width:100%">
    </a></div>
    <div class="column">
    <a href="/PCAUnscaled2Damp.pdf" target="_blank">
    <img src="/PCAUnscaled2Damp.pdf" alt="PCA in 2D"  id="img2" style="width:100%">
    </a></div>
</div>



## Random Forest
For supervised learning, I implemented random forest regression to predict the time domain signal given a number of neutron star parameters. This was performed in the frequency domain with the intention of predicting a time domain signal. The following plot shows the amplitude and phase of the original signal in black, followed by the leave-one-out cross-validated prediction in blue. The bottom trace shows the time domain signals for both. The right hand plot shows the relative importance of each parameter determined from the random forest decision trees. At face value, it appears that the first four parameters: $\bar{R}, \bar{k}\_{2}, J, M\_{b}$, account for around 50% of the variance due to parameters, however this is not the whole story, as there are cross-correlations between a number of parameters which has not been shown here. 

<div class="row">
    <div class="column">
    <a href="/RFRReImALF2-q10-M1225-cv.pdf" target="_blank">
    <img src="/RFRReImALF2-q10-M1225-cv.pdf" alt="Random Forest ALF Time Domain" id="img1" style="width:100%">
    </a></div>
    <div class="column">
    <a href="/RFRelativeLabelImportance.pdf" target="_blank">
    <img src="/RFRelativeLabelImportance.pdf" alt="Random Forest Relative Importance"  id="img2" style="width:100%">
    </a></div>
</div>


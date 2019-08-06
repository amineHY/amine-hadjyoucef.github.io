---
layout: "archive"
title: Here is a list of my scientific publications
permalink: /publications/
subtitle: Scientific journals, international conferences, seminars
icon: fa-book
author_profile: true
header:
  image: assets/images/banner_publications.png
---

- [International journals with peer review](#international-journals-with-peer-review)

		- [Convolution kernels for multi-wavelength imaging](#convolution-kernels-for-multi-wavelength-imaging)

- [International conferences with peer review](#international-conferences-with-peer-review)

		- [Spatio-Spectral Multichannel Reconstruction from few Low-Resolution multi-spectral Data](#spatio-spectral-multichannel-reconstruction-from-few-low-resolution-multi-spectral-data)

		- [Restoration from multi-spectral blurred data with non-stationary instrument response](#restoration-from-multi-spectral-blurred-data-with-non-stationary-instrument-response)

		- [Detection of epileptics during seizure free periods](#detection-of-epileptics-during-seizure-free-periods)  

- [National conferences with peer review](#national-conferences-with-peer-review)  

		- [Restauration d’objets astrophysiques à partir de données multi-spectrales floues et d’une réponse instrument non-stationnaire](#restauration-dobjets-astrophysiques-partir-de-donnes-multi-spectrales-floues-et-dune-rponse-instrument-non-stationnaire)

- [Seminears](#seminars)

- [Talks](#talks)


# International journals with peer review  

### Convolution kernels for multi-wavelength imaging  
**Authors**: Alexandre Boucaud, Marco Bocchio, Alain Abergel, François Orieux, Hervé Dole, M. A Hadj-Youcef  
**Publication date**: 2016/12/1  
**Journal**: Astronomy & Astrophysics  
**Volume**: 596  
**Pages**: A63  
**Publisher**: EDP Sciences  
**Link**: https://www.aanda.org/articles/aa/full_html/2016/12/aa29080-16/aa29080-16.html  

**Abstract**
Astrophysical images issued from different instruments and/or spectral bands often require to be processed together, either for fitting or comparison purposes. However each image is affected by an instrumental response, also known as point-spread function (PSF), that depends on the characteristics of the instrument as well as the wavelength and the observing strategy. Given the knowledge of the PSF in each band, a straightforward way of processing images is to homogenise them all to a target PSF using convolution kernels, so that they appear as if they had been acquired by the same instrument. We propose an algorithm that generates such PSF-matching kernels, based on Wiener filtering with a tunable regularisation parameter. This method ensures all anisotropic features in the PSFs to be taken into account. We compare our method to existing procedures using measured Herschel/PACS and SPIRE PSFs and simulated JWST/MIRI PSFs. Significant gains up to two orders of magnitude are obtained with respect to the use of kernels computed assuming Gaussian or circularised PSFs. A software to compute these kernels is available in [GitHub](https://github.com/aboucaud/pypher).

Read the full paper [here](/assets/pdf/A_&_A.pdf).

<br/><br/>
<br/><br/>

# International conferences with peer review

### Spatio-Spectral Multichannel Reconstruction from few Low-Resolution multi-spectral Data
**Authors**: MA Hadj-Youcef, François Orieux, Aurélia Fraysse, Alain Abergel  
**Publication date**: 2018/9/3  
**Conference**: EUSIPCO 2018, The 26th European Signal Processing Conference (EUSIPCO)  
**Location**: Rome, Italy.
**Pages**: 1980-1984  
**Publisher**: IEEE  
**Link**: https://ieeexplore.ieee.org/abstract/document/8553166  

**Abstract**
This paper deals with the reconstruction of a 3-D spatio-spectral object observed by a multi-spectral imaging system, where the original object is blurred with a spectral-variant PSF (Point Spread Function) and integrated over few broad spectral bands. In order to tackle this ill-posed problem, we propose a linear forward model that accounts for direct (or auto) channels and between (or cross) channels degradation, by modeling the imaging system response and the spectral distribution of the object with a piecewise linear function. Reconstruction based on regularization method is proposed, by enforcing spatial and spectral smoothness of the object. We test our approach on simulated data of the Mid-InfraRed Instrument (MIRI) Imager of the James Webb Space Telescope (JWST). Results on simulated multi-spectral data show a significant improvement over the conventional multichannel method.  

Read the full paper [here](/assets/pdf/eusipco_2018.pdf).  

<br/><br/>

### Restoration from multi-spectral blurred data with non-stationary instrument response  
**Authors**: M. A. Hadj-Youcef, François Orieux, Aurélia Fraysse, Alain Abergel  
**Publication date**: 2017/8/28  
**Conference**: EUSIPCO 2017, The 25th European Signal Processing Conference (EUSIPCO)  
**Location**: Kos, Greece.
**Pages**: 503-507  
**Publisher**: IEEE  
**Link**: https://ieeexplore.ieee.org/abstract/document/8081258  

**Abstract**
In this paper we propose an approach of image restoration from multi-spectral data provided by an imaging system. We specifically address two topics: (i) Development of a multi-wavelength direct model for non-stationary instrument response that includes a spatial convolution and a spectral integration, (ii) Implementation of multi-spectral image restoration using a regularized least-square, based on a quadratic criterion and minimized by a gradient algorithm. We test our approach on simulated data of the Mid-InfraRed Instrument IMager (MIRIM) of the James Webb Space Telescope (JWST). Our method shows a clear increase of spatial resolution compare to conventional methods.

Read the full paper [here](/assets/pdf/eusipco-20171.pdf).

<br/><br/>

### Detection of epileptics during seizure free periods
**Authors**: M.A. Hadj-Youcef, M. Adnane, A Bousbia-Salah  
**Publication date**: 2013/12/5  
**Conference**: WOSSPA 2013 : The 9th International Workshop on Systems, Signal Processing and their applications
**Location**: Algiers, Algeria
**Pages**: 209 - 213  
**Publisher**: IEEE  
**Link**: https://ieeexplore.ieee.org/abstract/document/6602363  

**Abstract**  
In this paper the problematic of epileptic detection is treated. An algorithm of EEG signal classification into two classes: Healthy and Epileptics is developed. The difference with conventional methods is the use of free seizure epileptic records. A good classification accuracy means that it is possible to detect an epileptic in normal state or at an early stage of epilepsy. The raw EEG signal is decomposed using discrete wavelet transform (DWT). Then, principal component analysis (PCA) allows dimensionality reduction and better representation of the data. Several features are extracted and used in support vector machine (SVM) classifier. Results show satisfactory classification accuracy comparable or better than those reported in literature.

Read the full paper [here](/assets/pdf/WOSSPA_2013.pdf).

<br/><br/>
<br/><br/>

# National conferences with peer review  

### Restauration d’objets astrophysiques à partir de données multi-spectrales floues et d’une réponse instrument non-stationnaire  

**Authors**: M. A. Hadj-Youcef, François Orieux, Aurélia Fraysse, Alain Abergel  
**Publication date**: 2017/9/5  
**Conference**: 26eme Colloque GRETSI Traitement du Signal & des Images, GRETSI 2017  
**Location**: Juan Les Pins, France.  
**Link**: https://jeannicod.ccsd.cnrs.fr/SUP_LSS/hal-01596257v1  

**Absract (in French)**  
In this paper we deal with the restoration of astrophysical objects from multi-spectral data acquired by an imager on board of a space telescope. The problems we address are the spatial resolution limitation caused by the spectral varying optical response and the broadband spectral integration of the object. Indeed, we develop an instrument model that takes into account these effects, and we propose a direct model by exploiting all the data from different spectral band jointly. The restoration of the object is implemented using the regularized least squares method, and the solution is computed by the conjugate gradient algorithm. We test our approach on simulated imagery data from the Mid-InfraRed Instrument (MIRI) on board the future James Webb Space Telescope (JWST). Our method shows a significant increase in spatial and spectral resolution compared to conventional methods.

  Read the full paper [here](/assets/pdf/GRETSI_2017.pdf).

<br/><br/>
<br/><br/>

# Seminars
in progress

<br/><br/>
<br/><br/>

# Talks
in progress

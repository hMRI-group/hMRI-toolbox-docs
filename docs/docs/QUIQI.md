# QUIQI

QUIQI is a method that accounts for the degradation of data quality due to motion in the analysis of MRI data. This
method is described [Lutti2022](references.md#lutti2022). QUIQI was originally implemented using custom-made matlab
scripts and functionalities provided by the SPM software (https://www.fil.ion.ucl.ac.uk/spm/). These scripts are
available here: (https://github.com/LREN-physics/hMRI-toolbox/tree/quiqi_v1.0). A subset of the data analysed in the
original article can be found here: [doi:10.5281/zenodo.4647081](https://doi.org/10.5281/zenodo.4647081).
In order to help users implement this method for their own analysis, we provide here a GUI-based implementation of QUIQI
built into the hMRI Toolbox, dedicated to the analysis of neuroimaging data.

## Requirements

A running version of Matlab (https://mathworks.com/products/matlab.html) and SPM (https://www.fil.ion.ucl.ac.uk/spm/)
are pre-requisites for running these analyses. Please refer to [this page](getStarted.md) for help in installing and using
the hMRI Toolbox.

## Content

The implementation of QUIQI within the hMRI Toolbox is based on two modules: "QUIQI BUILD" and "QUIQI CHECK".

### QUIQI BUILD

The "QUIQI BUILD" module lies between the usual ‘Factorial design’ and ‘Model estimation’ steps of an SPM image
analysis (see screenshot below). The corresponding fields are:

- SPM.mat: SPM matrix associated with the analysis
- MDI powers: powers of the Motion Degradation Index used to compute the basis functions for the ReML estimation of the
  noise covariance matrix.
- MDI type: type of the object that contains the values of the Motion Degradation Index. If ‘MDI matrix’ is selected,
  the MDI values may be pasted into the GUI from e.g. an Matlab variable or a table. The MDI values can also be read-in
  directly from a json file (‘MDIjsonfile).

![QUIQI_build](/assets/images/docs/QUIQI_build.png)

### QUIQI CHECK

QUIQI CHECK lies at the very end of image analysis (see below). While this module is not necessary to correct for motion
degradation effects in the MRI data, it is useful to assess the degree of remaining motion degradation after correction.
On the model of Lutti et al., this module plots a histogram of the distribution of the image residual samples against
the results of fitting these residuals with a polynomial function of the MDI. A high R<sup>2</sup> value highlights
heteroscedasticity of the noise distribution (see example below, with and without the use of QUIQI). Note that when
multiple sets of MDI values are used for QUIQI (i.e. for the analysis of MRI data computed across multiple raw image
volumes) all sets of MDI values are used for the polynomial fitting. Also, the maximum power of the polynomial fit can
be set from the user interface (field ‘Fit power’).

![QUIQI check 1](/assets/images/docs/QUIQI_check1.png)

Without QUIQI:

![QUIQI check 2](/assets/images/docs/QUIQI_check2.png)

With QUIQI:

![QUIQI check 3](/assets/images/docs/QUIQI_check3.png)

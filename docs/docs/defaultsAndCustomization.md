# Default Parameters and Customization of the hMRI Toolbox

The toolbox provides the user with a series of standard default acquisition and processing parameters.
These parameters will allow the user to run the toolbox for the most standard acquisition protocols without any further
customization. However, customization is possible and necessary to open the toolbox usability to a wide range of
protocols. Therefore, the default parameters can be customised to match the user's own site- or protocol-specific setup.
The parameters are described [below](#list-of-parameters) and can be found in
the [`config`]({{config.repo_url}}/blob/master/config) directory.
Some parameters are meant to be easily and safely modified by any user, while others should only be modified by advanced
users.

## Organization of the defaults

Defaults of the parameters that can be used to modify the behaviour of the toolbox are stored in
the [`config`]({{config.repo_url}}/blob/master/config) (root) directory of the toolbox.
These **standard default parameters** should never be modified.
**Customized** default parameter files should be stored in
the [`config/local`]({{config.repo_url}}/blob/master/config/local) directory.
Template files to define these local default parameters can be found
in [`config/local`]({{config.repo_url}}/blob/master/config/local).
These template files can be modified appropriately and saved under a meaningful name to be loaded in
the `Configure toolbox` module and used in the `Create hMRI maps` and `Process hMRI maps` modules.
Example files to turn on useful features of the toolbox can be found in
the  [`examples`]({{config.repo_url}}/blob/master/examples) directory.

Unlike the processing parameters, the acquisition parameters are retrieved from the input images
if [metadata](MetadataLibrary) are available. Acquisition parameters specified in the defaults are only provided as a
fallback solution when no metadata are available.
Since acquisition parameters are likely to vary from protocol to protocol, the use of metadata is strongly recommended (
see [JSON metadata and DICOM import](MetadataLibrary)).
For some non-product sequences, though, metadata might not be retrievable and default acquisition parameters must be
customized (see below).

During data processing, default parameters are stored in a global variable `hmri_def` and default values can be
retrieved using [`hmri_get_defaults.m`]({{config.repo_url}}/blob/master/hmri_get_defaults.m).

## Customization of the defaults

The default parameters can be customised to match the user's own site- or protocol-specific setup.
Templates for customised default files are stored in the [`config/local`]({{config.repo_url}}/blob/master/config/local)
directory and can be modified and saved with meaningful names by the user.
Note that some parameters should only be modified by advanced users who are aware of the underlying theory and
implementation of the toolbox.
The user can refer to the [`hmri_local_defaults.m`]({{config.repo_url}}/blob/master/config/local/hmri_local_defaults.m)
help where parameters are divided between basic parameters and advanced parameters, and to
the [list](#list-of-parameters) below.

### Customize global parameters

The standard default parameters are defined in
[`config/hmri_defaults.m`]({{config.repo_url}}/blob/master/config/hmri_defaults.m) and the corresponding template
file for customization
is [`config/local/hmri_local_defaults.m`]({{config.repo_url}}/blob/master/config/local/hmri_local_defaults.m).
Parameters defined in the
template [`config/local/hmri_local_defaults.m`]({{config.repo_url}}/blob/master/config/local/hmri_local_defaults.m) are
identical to the ones defined in [`config/hmri_defaults.m`]({{config.repo_url}}/blob/master/config/hmri_defaults.m).

Make a copy of the template with a suffix that is meaningful (e.g. `config/local/hmri_local_defaults_mystudy.m`) and
modify it according to your needs and objectives. It is recommended to delete all unchanged entries and keep the
modified parameters only, to improve readability (one can more easily see what parameters are specific to a given study
or data processing).

In the SPM Batch GUI, select the `Configure toolbox` module first. It must be in first position before any other module
from the hMRI toolbox. Select your customised default file. Standard defaults from `hmri_defaults.m` will be used except
for the ones you modified, which will overwrite the standard ones. Then select `Create hMRI maps`
or `Process hMRI maps`. A different default file can be used for map creation and for map processing.
The `Configure toolbox` module can be inserted both before the `Create hMRI maps` and `Process hMRI maps` modules, with
a different customised default file for each module.

### Customize B1 mapping acquisition and processing parameters

Most of the time, no customization is required for B1 mapping parameters. However, when required, the customization
comes handy e.g. to handle data without available metadata (this includes data from a B1 mapping protocol unknown from
the toolbox). Such an [example](#examples) is provided at the bottom of this page.

The standard default parameters for B1 mapping acquisition are defined in
[`config/hmri_b1_standard_defaults.m`]({{config.repo_url}}/blob/master/config/hmri_b1_standard_defaults.m) and the
corresponding template file for customization
is [`config/local/hmri_b1_local_defaults.m`]({{config.repo_url}}/blob/master/config/local/hmri_b1_local_defaults.m).
Parameters defined in the
template [`config/local/hmri_b1_local_defaults.m`]({{config.repo_url}}/blob/master/config/local/hmri_b1_local_defaults.m)
are identical to the ones defined
in [`config/hmri_b1_standard_defaults.m`]({{config.repo_url}}/blob/master/config/hmri_b1_standard_defaults.m).

Make a copy of the template with a suffix that is meaningful (e.g. `config/local/hmri_b1_local_defaults_myB1protocol.m`)
and modify it according to your needs and objectives. It is recommended to delete all unchanged entries and keep the
modified parameters only, to improve readability (one can more easily see what parameters are specific to the customized
B1 protocol).

In the `Create hMRI maps` module of the SPM Batch GUI, select the `B1 type`. If you choose 3D\_AFI, 3D\_EPI or UNICORT,
you can additionally select a customised default file. Standard defaults from `hmri_b1_standard_defaults.m` will be used
except for the ones you modified, which will overwrite the standard ones.

## List of parameters

All parameters are stored in a global variable `hmri_def` which is a structure containing the fields described below.
Default values can easily be retrieved
using [`hmri_get_defaults.m`]({{config.repo_url}}/blob/master/hmri_get_defaults.m). Most fields are listed below as *
*name** [ *default value* ] - description.

[Global parameters](#global-parameters)    
[B1 mapping processing parameters](#b1-mapping-processing-parameters)     
[Create hMRI maps parameters](#create-hmri-maps-parameters)     
[Process hMRI maps parameters](#process-hmri-maps-parameters)

### Global parameters

These parameters are generally used across all the hMRI modules.

- **centre** [ *centre* ] - to specify the research centre - to be customized in the local configuration file. Not
  mandatory but saved with the defaults used for a given data processing. Therefore comes handy to trace the origin of a
  given set of generated maps.

- **local_defaults** [ *hMRI/config/local/hmri_local_defaults.m* ] - customised defaults file location. Not mandatory.

- **cleanup** [ *true* ] - clean up temporary directories created during processing (
  e.g. `B1Calc`, `RFSens`, `MapCreation`) to keep the only `Results` directory and `Supplementary` subdirectory with
  relevant results. Set it to *false* if you wish to inspect intermediate steps e.g. for debugging purpose or to track
  unexpected results back to the original data.

- **json** - format for JSON metadata, defined as a structure:
    - `extended` [ *false* ]: true if metadata stored as extended header field in the nifti image.
    - `separate` [ *true* ]: true if metadata stored as separate JSON file with same name as corresponding nifti image
    - `anonym` [ *basic* ]: during DICOM to NIFTI conversion, patient ID (presumably not containing patient confidential
      data), age (years at the time of the data acquisition), sex, size and weight are kept; patient name, date of birth
      and DICOM filename (often containing the patient name) are removed (
      see [`anonymise_metadata.m`]({{config.repo_url}}/blob/master/spm12/metadata/anonymise_metadata.m) for details). *
      *IMPORTANT WARNING: EFFECTIVE ANONYMISATION IS NOT GUARANTEED !!!** *The anonymisation implemented depends on the
      structure and content of the DICOM header and might not be effective in many cases.*
    - `overwrite` [ *true* ]: existing metadata will be overwritten (
      see [`set_metadata.m`]({{config.repo_url}}/blob/master/spm12/metadata/set_metadata.m) for details).
    - `indent` [ *'\t'* ]: the way JSON text is formatted (tab separated)

- **TPM** [ *hMRI/tpm/eTPM.nii* ] - recommended set of Tissue Probability Maps for segmentation and spatial processing.

- **autoreorient_template** - default template for [auto-reorientation](AutoReorient).

- **segment** - default parameters for segmentation. This field is effectively the job to be handed to
  spm\_preproc\_run.    
  By default, parameters are set to
    - create tissue class images (`c*`) in the native space of the source image (`tissue(*).native = [1 0]`) for tissue
      classes 1-5
    - save both BiasField and BiasCorrected volumes (`channel.write = [1 1]`)
    - recommended values from SPM12 (October 2017)

### B1 mapping processing parameters

Relevant default parameters are listed below for each type of B1 mapping input. Please refer
to [`hMRI/config/hmri_b1_standard_defaults.m`]({{config.repo_url}}/blob/master/config/hmri_b1_standard_defaults.m) for a
complete list. See further below for [examples](#examples) of B1 local defaults customization based on
the [`hmri_b1_local_defaults.m`]({{config.repo_url}}/blob/master/config/local/hmri_b1_local_defaults.m) template.

NOTE: For acquisition parameters, default values are a fallback solution for B1 data processing when no metadata are
available. Use of metadata is recommended to retrieve site- & protocol-specific parameters and ensure appropriate data
handling and processing.

#### i3D\_EPI - EPI SPIN-ECHO (SE)/STIMULATED-ECHO (STE) METHOD [[Lutti *et al.* 2010 & 2012](References)].

- B0 & B1 processing parameters - **hmri_def.b1map.i3D_EPI.b1proc**
    - **T1** [ *1192* ]: average brain T1 (ms). Default value is strictly valid only for in vivo brain imaging at 3T.
      The T1 value is used to account for T1 recovery during the mixing time between the SE and the STE.
    - **eps** [ *0.0001* ]: regularisation value added to the denominator of the STE/SE ration to avoid division by
      small numbers giving very large values.
    - **Nonominalvalues** [ *5* ]: number of SE/STE pairs to used for parameter estimation. The `Nonominalvalues` number
      of pairs giving the B1 estimate with the lowest standard deviation over the pairs will be used.
    - **nAmbiguousAngles** [ *2* ]: how many `[90*(n-1),90+90*(n-1)]` degree ranges to test to overcome acos ambiguity.
      Minimum of 2, more than 3 probably not needed for reasonable SE/STE flip angle pairs.
    - **HZTHRESH** [ *110* ]: threshold in Hz. Areas of high potential distortions due to B0 field inhomogeneities are
      masked out in the final B1 map, as strong B0 inhomogeneities can give rise to bias in the B1 map. The threshold
      may need to be increased on scanners with reduced field homogeneity, but bear in mind that the larger the field
      deviation, the larger the potential bias in the B1 map.
    - **SDTHRESH** [ *5* ]: B1 is estimated by choosing the combination of angles which gives the smallest standard
      deviation. This threshold masks out voxels where the optimal standard deviation over the angles is still large,
      even for the optimal angles. The appropriate threshold will vary with `Nonominalvalues`.
    - **ERODEB1** [ *1* ]: the B1 mask is eroded to avoid edge artefacts and then padded to fill in holes and cover the
      whole brain. This parameter gives the erosion kernel size.
    - **PADB1** [ *3* ]: padding kernel size.
    - **B1FWHM** [ *8* ]: smoothing kernel size in mm.
    - **match_vdm** [ *1* ]: whether to use the FieldMap toolbox to write the forward warped magnitude image (i.e.
      magnitude image from fieldmap acquisition, coregistered & resliced to anat image).
    - **b0maskbrain** [ *1* ]: whether to generate a brain mask for distortion correction.

- B1 validation options for BIDS-input data - **hmri_def.b1map.i3D_EPI.b1validation**
    - **checkTEs** [ *false* ]: whether to do input validation using image TEs. Assumes SE has shorter TE than STE in
      metadata ([qMRI-BIDS assumption](https://bids-specification.readthedocs.io/en/latest/appendices/qmri.html#tb1epi-specific-notes)).
      Disabled by default as this assumption is not valid for the metadata in the DICOMs from some sequences.
    - **useBidsFlipAngleField** [ *false* ]: read nominal flip angles from
      qMRI-BIDS [FlipAngle](https://bids-specification.readthedocs.io/en/latest/appendices/qmri.html#tb1epi-specific-notes)
      metadata. Disabled by default as this metadata is often not set appropriately.

- B1 acquisition parameters - **hmri_def.b1map.i3D_EPI.b1acq**
    - **beta** [ *115:-5:65* ]: (degrees) range of nominal flip angles used. *Note: the SE/STE B1 mapping sequence uses
      a set of RF pulses with nominal flip angles beta, 2 × beta and beta to create pairs of spin echo and stimulated
      echo images.*
    - **TM** [ *31.2* ]: mixing time (stimulated echo).
    - **tert** [ *540e-3\*24* ]: total EPI readout time (EchoSpacing × numberPElines).
    - **blipDIR** [ *+1* ]: sign of blip-encoded k-space sampling (phase encoding direction).

- B0 acquisition parameters - **hmri_def.b1map.i3D_EPI.b0acq**
    - **shortTE** [ *10* ]: shortest echo time in ms
    - **longTE** [ *12.46* ]: longest echo time in ms

#### i3D\_AFI - ACTUAL FLIP ANGLE IMAGING (AFI) METHOD [[Yarnykh *et al.* 2007](References)]

- Acquisition parameters **hmri_def.b1map.i3D_AFI.b1acq**
    - **TR2TR1ratio** [ *0.2* ]: TR of second input volume divided by TR of first input volume (acquisition parameter).
      This will be ignored if the two separate TRs are individually specified in the respective metadata field of each
      volume or an `alTR` array of TR values is found in the metadata.
    - **alphanom** [ *60* ]: (degrees) nominal flip angle (acquisition parameter). This is normally read from the
      metadata.

- Masking parameters (disabled by default) **hmri_def.b1map.i3D_AFI.b1mask**
    - **domask** [ *false* ]: whether to mask
      using [`hmri_create_pm_brain_mask`]({{config.repo_url}}/blob/master/hmri_create_pm_brain_mask.m)
    - **fwhm** [ *5* ]: option for `hmri_create_pm_brain_mask`
    - **nerode** [ *2* ]: option for `hmri_create_pm_brain_mask`
    - **ndilate** [ *4* ]: option for `hmri_create_pm_brain_mask`
    - **thresh** [ *0.5* ]: % option for `hmri_create_pm_brain_mask`

- Smoothing parameters **hmri_def.b1map.i3D_AFI.b1proc**
    - **B1FWHM** [ *8* ]: (mm) full-width half maximum of smoothing kernel for B1 map. Set to 0 to disable smoothing. If
      **domask** is `true`, then smoothing will only be done within the mask.

#### DAM - DOUBLE ANGLE MAPPING

- Acquisition parameters **hmri_def.b1map.DAM.b1acq**
    - **alphanom** [ *60* ]: (degrees) nominal flip angle (acquisition parameter). This is normally read from the
      metadata.

- Masking parameters (disabled by default) **hmri_def.b1map.DAM.b1mask**
    - **domask** [ *false* ]: whether to mask
      using [`hmri_create_pm_brain_mask`]({{config.repo_url}}/blob/master/hmri_create_pm_brain_mask.m)
    - **fwhm** [ *5* ]: option for `hmri_create_pm_brain_mask`
    - **nerode** [ *2* ]: option for `hmri_create_pm_brain_mask`
    - **ndilate** [ *4* ]: option for `hmri_create_pm_brain_mask`
    - **thresh** [ *0.5* ]: % option for `hmri_create_pm_brain_mask`

- Smoothing parameters **hmri_def.b1map.DAM.b1proc**
    - **B1FWHM** [ *8* ]: (mm) full-width half maximum of smoothing kernel for B1 map. Set to 0 to disable smoothing. If
      **domask** is `true`, then smoothing will only be done within the mask.

#### TFL\_B1\_MAP [[Chung *et al.* 2010](References)]

- Masking parameters (disabled by default) **hmri_def.b1map.tfl_b1_map.b1mask**
    - **domask** [ *false* ]: whether to mask
      using [`hmri_create_pm_brain_mask`]({{config.repo_url}}/blob/master/hmri_create_pm_brain_mask.m)
    - **fwhm** [ *5* ]: option for `hmri_create_pm_brain_mask`
    - **nerode** [ *2* ]: option for `hmri_create_pm_brain_mask`
    - **ndilate** [ *4* ]: option for `hmri_create_pm_brain_mask`
    - **thresh** [ *0.5* ]: % option for `hmri_create_pm_brain_mask`

- Smoothing parameters **hmri_def.b1map.tfl_b1_map.b1proc**
    - **B1FWHM** [ *8* ]: (mm) full-width half maximum of smoothing kernel for B1 map. Set to 0 to disable smoothing. If
      **domask** is `true`, then smoothing will only be done within the mask.

#### RF\_MAP - SE/STE METHOD FROM A SERVICE SEQUENCE BY SIEMENS

- Masking parameters (disabled by default) **hmri_def.b1map.rf_map.b1mask**
    - **domask** [ *false* ]: whether to mask
      using [`hmri_create_pm_brain_mask`]({{config.repo_url}}/blob/master/hmri_create_pm_brain_mask.m)
    - **fwhm** [ *5* ]: option for `hmri_create_pm_brain_mask`
    - **nerode** [ *2* ]: option for `hmri_create_pm_brain_mask`
    - **ndilate** [ *4* ]: option for `hmri_create_pm_brain_mask`
    - **thresh** [ *0.5* ]: % option for `hmri_create_pm_brain_mask`

- Smoothing parameters **hmri_def.b1map.rf_map.b1proc**
    - **B1FWHM** [ *8* ]: (mm) full-width half maximum of smoothing kernel for B1 map. Set to 0 to disable smoothing. If
      **domask** is `true`, then smoothing will only be done within the mask.

#### UNICORT approach [[Weiskopf *et al.* 2011](References)]

- Processing parameters - **hmri_def.b1map.UNICORT.procpar**
    - **reg** = 10^-3
    - **FWHM** = 60
    - **thr** = 5

#### PRE-PROCESSED B1

- Any B1 map pre-calculated using one of the above methods or another method can be used as pre-processed B1 input.
- A scaling factor can be entered in the batch to convert the pre-processed B1 map into appropriate units (the toolbox
  expects B1 maps in [p.u.] of the nominal flip angle).
- Masking can optionally be applied (disabled by default) **hmri_def.b1map.pre_processed_B1.b1mask**
    - **domask** [ *false* ]: whether to mask
      using [`hmri_create_pm_brain_mask`]({{config.repo_url}}/blob/master/hmri_create_pm_brain_mask.m)
    - **fwhm** [ *5* ]: option for `hmri_create_pm_brain_mask`
    - **nerode** [ *2* ]: option for `hmri_create_pm_brain_mask`
    - **ndilate** [ *4* ]: option for `hmri_create_pm_brain_mask`
    - **thresh** [ *0.5* ]: % option for `hmri_create_pm_brain_mask`

- Smoothing can optionally be performed (disabled by default) **hmri_def.b1map.pre_processed_B1.b1proc**
    - **B1FWHM** [ *0* ]: (mm) full-width half maximum of smoothing kernel for B1 map. Set to 0 to disable smoothing. If
      **domask** is `true`, then smoothing will only be done within the mask.

#### NO B1 BIAS CORRECTION

- No parameters relevant for customization.

### Create hMRI maps parameters

#### GENERAL R1/PD/R2\*/MT MAP CREATION PARAMETERS

- **small_angle_approx** [ *true* ] - whether to use the small angle approximation when computing R1 and PD. `true`:
  classic method from [Helms *et al.* 2008a](References). `false`: method that is also valid for large flip angles
  from [Edwards *et al.* 2021](References). Note that if imperfect spoiling correction is used, then the correction
  parameters must have been calculated using the same approximation level.

- **R2sOLS** [ *true* ] - create a combined R2\* map using all of the contrasts (ESTATICS; [Weiskopf *et
  al.* 2014](References)).
- **R2s_fit_method** [ *'OLS'* ] - choose method of R2\* fitting. This is a trade-off between speed and accuracy;
  see [Edwards *et al.* 2022](References) for more information. `'OLS'`: classic ESTATICS log-linear ordinary least
  squares model; fast but not as accurate as WLS1. `'WLS1'`: log-linear weighted least squares ESTATICS with one
  iteration; slower than OLS but significantly more accurate because it accounts for the heteroskedasticity induced by
  the logarithm. Uses parallelization over voxels to speed up calculation. `'WLS2'` and `'WLS3'`: log-linear weighted
  least squares ESTATICS with 2 or 3 iterations of the weight estimation (which should theoretically improve the
  accuracy); did not show great improvement over WLS1 in test data. `'NLLS_OLS'` and `'NLLS_WLS1'`: nonlinear least
  squares ESTATICS using either OLS or WLS1, respectively, to provide the initial parameter estimates. Not affected by
  heteroskedasticity, but very slow, even with parallelization over voxels. `'OLS'` is default for backwards
  compatibility with older versions of the toolbox, but we recommend trying `'WLS1'` when accuracy is important.

- **neco4R2sfit** [ *4* ] - minimum number of echoes to calculate R2\* map. Strictly speaking, the minimum is 2. For a
  robust estimation, the minimum number of echoes required depends on many factors, including:
    - SNR/resolution
    - distribution/spacing between TEs: note that early echoes are more affected by the specific contrast, violating the
      assumption of a common decay between contrasts.
    - number of contrasts available (fewer echoes per contrast required for 3 (PDw, T1w, MTw) contrasts as compared to 2
      or even 1).

  To be on the safe side, a minimum of 6 echoes is recommended ([Weiskopf *et al.* 2014](References)). Further studies
  are required to come up with more detailed and informed guidelines. Use fewer echoes at your own risk...!

- **fullOLS** [ *true* ] - `true`: then ESTATICS fit at TE=0 is used for the further map calculation steps. This is the
  recommended method to get rid of any R2\* bias in the other quantitative maps. `false`: the average over echoes for
  each contrast is used.

- **interp** [ *3* ] - define a coherent interpolation factor used all through the map creation process. Default is 3,
  but if you want to keep SNR and resolution as close as possible to the original images, it is recommended to use sinc
  interpolation (e.g. [ *-7* ]).

- **qMRI_maps.QA** [ *true* ] - generate quality assurance evaluation data.

- **qMRI_maps.ACPCrealign** [ *false* ] - to realign qMRI maps to MNI. This parameter is kept for backward
  compatibility. It corresponds to the realignment implemented as part of the map calculation (
  see `hmri_create_MTProt.m`). It is recommended to rather reorient all images prior any processing using the
  Auto-Reorient module provided with the toolbox (type `help hmri_autoreorient` for details or open the SPM > Tools >
  hMRI Tools > Auto-Reorient module in the Batch GUI).

- **coreg2PDw** [ *true* ] - whether to perform coregistration of T1w images, MTw images, and B1 bias map to the average
  PDw image. The coregistration step can be disabled using this flag (not recommended, only useful for simulated data or
  data that have already been coregistered by the user before being input into the toolbox). ADVANCED USER ONLY.

- **coreg_flags** - coregistration parameters for registering T1w and MTw weighted images to the average PDw image;
  see `spm_coreg.m` in [SPM](https://github.com/spm/spm12) for details.
    - **sep** [ *[4 2]* ]: For high resolution (< 0.8 mm) data, we have found that adding extra steps to the
      registration can improve map quality, e.g. for 0.5 mm data we have found [4 2 1 0.5] to be needed to avoid
      registration artefacts, but this comes at the expense of slowing down map creation.
    - **cost_fun** [ *'nmi'* ]
    - **fwhm** [ *[7 7]* ]

- **coreg_bias_flags** - coregistration parameters for B1 bias maps to the average (or TE=0 fit) PDw image;
  see `spm_coreg.m` in [SPM](https://github.com/spm/spm12) for details.
    - **sep** [ *[4 2]* ]
    - **cost_fun** [ *'nmi'* ]
    - **fwhm** [ *[7 7]* ]

- **UNICORT.PD** [ *false* ] - if *true*, uses the UNICORT-derived B1 transmit field bias for PD map calculation.
  ADVANCED USER ONLY. WARNING: this method has not been validated for PD calculation!
- **UNICORT.MT** [ *false* ] - if *true*, uses the UNICORT-derived B1 transmit field bias for MT map calculation.
  ADVANCED USER ONLY. WARNING: this method has not been validated for MT calculation!

  **NOTE ON THE ABOVE UNICORT PARAMETERS**: Unified Segmentation correction for transmit (UNICORT) and receive field
  inhomogeneities is currently not applied cumulatively to correct for both sources of bias in the maps. In principle,
  when no transmit nor receive fields are measured, the Unified Segmentation approach for RF sensitivity bias correction
  could be combined with the UNICORT approach for B1 transmit bias estimation to generate the PD maps. This is currently
  not made available by default in the toolbox by lack of a validation study assessing the performance of such an
  approach. Based on the same idea, in the absence of a measured B1 transmit field, the B1 transmit bias field map
  estimated from the R1 map using UNICORT could be used to remove the higher-order transmit field contribution in MT
  maps. Again, without further methodological validation of these methods, it is not recommended to use the
  lower-accuracy UNICORT-derived B1 maps ([Weiskopf *et al.* 2011](References)) for correction of the higher-order B1
  transmit effects on MT saturation.

#### RF SENSITIVITY BIAS CORRECTION

- **RFsens.smooth_kernel** [ *12* ] - smoothing kernel applied to the sensitivity maps used for RF sensitivity
  correction (when RF sensitivity mapping data have been acquired).

#### THRESHOLD VALUES FOR qMRI MAPS

- **qMRI_maps_thresh.R1** [ *2000* ] - for R1 maps. NOTE: threshold given in s-1 **×1000** (i.e. 2 s-1 here).
- **qMRI_maps_thresh.A** [ *10^5* ] - for A maps, i.e. PD maps before BiasField correction and calibration. NOTE: this
  value is acquisition-dependent. On different scanners, the threshold might need to be adapted. Values in A maps must
  be properly thresholded for the map calculation to proceed reliably. As a rule of thumb, a value of 10× the maximum
  brain intensity in the A map can be used.
- **qMRI_maps_thresh.R2s** [ *10* ] - for R2\* maps. NOTE: threshold given in s-1 **/1000** (i.e. 10000 s-1 here).
- **qMRI_maps_thresh.MTR** [ *50* ] - for MTR maps.
- **qMRI_maps_thresh.MTR_synt** [ *50* ] - for MTR_synth maps.
- **qMRI_maps_thresh.MT** [ *5* ] - for MT maps in [p.u.].

#### PD MAP CALCULATION

- **PDproc.calibr** [ *true* ] - whether to calibrate the PD map based on PD(WM) = 69% [Tofts 2003]. Will be
  automatically set to *false* if no B1 map is available
- **PDproc.WBMaskTh** [ *0.1* ] - threshold for calculation of whole-brain mask from TPMs. Used to correct for Receiving
  B1 field inhomogeneities in PD maps.
- **PDproc.WMMaskTh** [ *0.95* ] - threshold for calculation of white matter mask from TPMs. Used to calibrate the PD
  value within the white matter (69%).
- **PDproc.biasreg** [ *10^(-5)* ] - segmentation parameter (see Unified Segmentation in SPM12)
- **PDproc.biasfwhm** [ *50* ] - segmentation parameter (see Unified Segmentation in SPM12)
- **PDproc.nr_echoes_forA** [ *6* ] - number of PD-weighted echoes to be used to calculate the PD map. In order to
  minimize R2\* bias on the PD estimates and gain in robustness for bias-field correction, the number of echoes should
  be minimum ("average" calculated over the first echo only) for PD calculation. However, with T2\*-weighting bias
  correction (see below), a higher number of echoes is preferred in order to provide good SNR [[Balteau *et
  al*. 2018](References)].
- **PDproc.T2scorr** [ *true* ] - to correct the A map for T2\*-weighting bias before PD map calculation. Not necessary
  if the fullOLS option is enabled [[Balteau *et al*. 2018](References)].

#### IMPERFECT RF SPOILING CORRECTION

The correction parameters used in `hmri_create_MTProt.m` to correct for imperfect RF spoiling in R1 map calculation are
based on [[Preibisch and Deichmann, 2009](References)]. The correction requires that a B1+ map be available (in addition
to PD- and T1-weighted images). The correction parameters used in the hMRI toolbox can be calculated using a dedicated
module in the toolbox. See [[Corbin & Callaghan. 2021](References)] for guidance on calculation including the choice of
T2 time and the moment of the spoiler gradients and the diffusion-spoiling effects of the readout.

The correction values P2\_a and P2\_b given in this section have been pre-calculated for a range of acquisition
parameters most commonly found in our MPM protocols (namely the Echo Times (TE), Repetition Times (TR) and Flip Angles (
FA) of the respective multi-echo FLASH acquisitions). In future versions, the calculation will be more generally
extended to other sets of acquisition parameters as an additional module. Currently, no RF spoiling correction will be
applied if no match is found between the MPM acquisition parameters and the parameters sets defined below.
Please [contact](Contacts) us if you wish to correct for imperfect spoiling effects and your acquisition parameters are
not among the standard ones defined below.

If the correction values were computed using the small angle approximation for R1 estimation, then they will need to be
recomputed without the small angle approximation if they are to be applied to an R1 map computed without the small angle
approximation.

Given the possible confusion and resulting mistake (imperfect spoiling correction applied to the wrong sequence), when
TR and FA values match one of the listed cases below, the option is disabled by default. When enabling the imperfect
spoiling correction, make sure the coefficients retrieved were definitely calculated for the protocol used!

- **imperfectSpoilCorr.enabled** [ *false* ]

#### Default MPM acquisition parameters (*v2k protocol*, Prisma)

These values are updated at runtime in `hmri_create_MTProt.m` in order to choose the right RF spoiling correction
parameters. The acquisition parameters include the (first) Echo Times (TE) - assuming equidistant echoes in the
multi-echo acquisition, Repetition Times (TR) and Flip Angles (FA) of the respective multi-echo FLASH acquisitions:

- **MPMacq.TE_mtw** [ *2.34* ] - in ms
- **MPMacq.TE_t1w** [ *2.34* ] - in ms
- **MPMacq.TE_pdw** [ *2.34* ] - in ms
- **MPMacq.TR_mtw** [ *24.5* ] - in ms
- **MPMacq.TR_t1w** [ *24.5* ] - in ms
- **MPMacq.TR_pdw** [ *24.5* ] - in ms
- **MPMacq.fa_mtw** [ *6* ] - in deg
- **MPMacq.fa_t1w** [ *21* ] - in deg
- **MPMacq.fa_pdw** [ *6* ] - in deg
- **MPMacq.tag** [ *v2k* ] - tag for the protocol (default is the v2k protocol)

#### Protocol tags and pre-calculated correction factors

Below, for each quadruplet of \[TR\_pdw TR\_t1w fa\_pdw fa\_t1w\] values, a protocol tag is defined together with the
pre-calculated P2\_a and P2\_b correction factors. NOTE: all tags MUST start with a letter and include only letters,
numbers and underscores (NO space) since they are used to define a structure fieldname for RF spoiling correction.

##### 1. classic FIL protocol [[Weiskopf et al. 2011](References)]:

- **MPMacq_set.tags{1}** [ *ClassicFIL* ]
- **MPMacq_set.vals{1}** [ *23.7 18.7 6 20* ] - [TR_pdw TR_t1w fa_pdw fa_t1w] values
- **imperfectSpoilCorr.ClassicFIL.tag** = 'Classic FIL protocol'
- **imperfectSpoilCorr.ClassicFIL.P2_a** = [78.9228195006542,-101.113338489192,47.8783287525126];
- **imperfectSpoilCorr.ClassicFIL.P2_b** = [-0.147476233142129,0.126487385091045,0.956824374979504];
- **imperfectSpoilCorr.ClassicFIL.small_angle_approx** = `true`;
- **imperfectSpoilCorr.ClassicFIL.enabled** [ *hmri_def.imperfectSpoilCorr.enabled* ]

##### 2. new FIL/Helms protocol

- **MPMacq_set.tags{2}** [ *NewFILHelms* ]
- **MPMacq_set.vals{2}** [ *24.5 24.5 5 29* ] - [TR_pdw TR_t1w fa_pdw fa_t1w] values
- **imperfectSpoilCorr.NewFILHelms.tag** = 'New FIL/Helms protocol';
- **imperfectSpoilCorr.NewFILHelms.P2_a** = [93.455034845930480,-120.5752858196904,55.911077913369060];
- **imperfectSpoilCorr.NewFILHelms.P2_b** = [-0.167301931434861,0.113507432776106,0.961765216743606];
- **imperfectSpoilCorr.NewFILHelms.small_angle_approx** = `true`;
- **imperfectSpoilCorr.NewFILHelms.enabled** [ *hmri_def.imperfectSpoilCorr.enabled* ]

##### 3. Siemens product sequence protocol used in Lausanne (G Krueger)

- **MPMacq_set.tags{3}** [ *SiemPrLausGK* ]
- **MPMacq_set.vals{3}** [ *24.0 19.0 6 20* ] - [TR_pdw TR_t1w fa_pdw fa_t1w] values
- **imperfectSpoilCorr.SiemPrLausGK.tag** = 'Siemens product Lausanne (GK) protocol';
- **imperfectSpoilCorr.SiemPrLausGK.P2_a** = [67.023102027100880,-86.834117103841540,43.815818592349870];
- **imperfectSpoilCorr.SiemPrLausGK.P2_b** = [-0.130876849571103,0.117721807209409,0.959180058389875];
- **imperfectSpoilCorr.SiemPrLausGK.small_angle_approx** = `true`;
- **imperfectSpoilCorr.SiemPrLausGK.enabled** [ *hmri_def.imperfectSpoilCorr.enabled* ]

##### 4. High-res (0.8mm) FIL protocol

- **MPMacq_set.tags{4}** [ *HResFIL* ]
- **MPMacq_set.vals{4}** [ *23.7 23.7 6 28* ] - [TR_pdw TR_t1w fa_pdw fa_t1w] values
- **imperfectSpoilCorr.HResFIL.tag** = 'High-res FIL protocol';
- **imperfectSpoilCorr.HResFIL.P2_a** = [1.317257319014170e+02,-1.699833074433892e+02,73.372595677371650];
- **imperfectSpoilCorr.HResFIL.P2_b** = [-0.218804328507184,0.178745853134922,0.939514554747592];
- **imperfectSpoilCorr.HResFIL.small_angle_approx** = `true`;
- **imperfectSpoilCorr.HResFIL.enabled** [ *hmri_def.imperfectSpoilCorr.enabled* ]

##### 5. NEW  High-res (0.8mm) FIL protocol

- **MPMacq_set.tags{5}** [ *NHResFIL* ]
- **MPMacq_set.vals{5}** [ *25.25 25.25 5 29* ] - [TR_pdw TR_t1w fa_pdw fa_t1w] values
- **imperfectSpoilCorr.NHResFIL.tag** = 'New High-res FIL protocol';
- **imperfectSpoilCorr.NHResFIL.P2_a** = [88.8623036106612,-114.526218941363,53.8168602253166];
- **imperfectSpoilCorr.NHResFIL.P2_b** = [-0.132904017579521,0.113959390779008,0.960799295622202];
- **imperfectSpoilCorr.NHResFIL.small_angle_approx** = `true`;
- **imperfectSpoilCorr.NHResFIL.enabled** [ *hmri_def.imperfectSpoilCorr.enabled* ]

##### 6. NEW  1mm protocol - seq version v2k (default)

- **MPMacq_set.tags{6}** [ *v2k* ]
- **MPMacq_set.vals{6}** [ *24.5 24.5 6 21* ] - [TR_pdw TR_t1w fa_pdw fa_t1w] values
- **imperfectSpoilCorr.v2k.tag** = 'v2k protocol';
- **imperfectSpoilCorr.v2k.P2_a** = [71.2817617982844,-92.2992876164017,45.8278193851731];
- **imperfectSpoilCorr.v2k.P2_b** = [-0.137859046784839,0.122423212397157,0.957642744668469];
- **imperfectSpoilCorr.v2k.small_angle_approx** = `true`;
- **imperfectSpoilCorr.v2k.enabled** [ *hmri_def.imperfectSpoilCorr.enabled* ]

##### 7. 800um protocol - seq version v3* released used by MEG group

NOTE: Correction parameters below were determined via Bloch-Torrey simulations but end result agrees well with
EPG-derived correction for this RF spoiling increment of 137
degrees [[Callaghan et al. ISMRM, 2015, #1694](References)].

- **MPMacq_set.tags{6}** [ *v3star* ]
- **MPMacq_set.vals{6}** [ *25 25 6 21* ] - [TR_pdw TR_t1w fa_pdw fa_t1w] values
- **imperfectSpoilCorr.v3star.tag** = 'v3star protocol';
- **imperfectSpoilCorr.v3star.P2_a** = [57.427573706259864,-79.300742898810441,39.218584751863879];
- **imperfectSpoilCorr.v3star.P2_b** = [-0.121114060111119,0.121684347499374,0.955987357483519];
- **imperfectSpoilCorr.v3star.small_angle_approx** = `true`;
- **imperfectSpoilCorr.v3star.enabled** [ *hmri_def.imperfectSpoilCorr.enabled* ]

##### Unknown protocol

If the acquisition parameters are recognised as none of the above predefined sets, no RF spoiling correction will be
applied.

- **imperfectSpoilCorr.Unknown.tag** = 'Unknown protocol. No spoiling correction defined.'
- **imperfectSpoilCorr.Unknown.enabled** [ *false* ]

### Process hMRI maps parameters

#### UNIFIED SEGMENTATION PARAMETERS

The recommended Tissue Probability Maps (TPM) for segmentation in the Process hMRI maps module are - by default - the
same as the global ones, but one could (want to) use another set of TPM at some point. The map creation works with "
standard" weighted-MR images to build the parametric maps. These parametric maps taken together for a
multichannel-segmentation could show more details (for example subcortical nuclei) and would therefore require a
specific TPM - still to be built...

- **proc.TPM** [ *hmri_def.TPM* ] - recommended TPM
- **proc.w_native** = `[[1 1];[1 1];[1 1];[0 0];[0 0];[0 0]]`; - write native and native+dartelImp for GM/WM/CSF only (
  tissue classes 1-3).
- **proc.w_warped** = `[[1 1];[1 1];[1 1];[0 0];[0 0];[0 0]]`; - write warped and mod+unmod for GM/WM/CSF only (tissue
  classes 1-3).
- **proc.nGauss** = `[2 2 2 3 4 2]`; - Number of Gaussians per tissue class, originally `[1 1 2 3 4 2]` in SPM.

## Examples

Below are provided examples illustrating how the toolbox can be customized, concretely. They are based on the hMRI
sample dataset, available [here][hMRI-dataset-zip-to-download]. All examples provided in the Wiki are available in
the [`hMRI/examples`]({{config.repo_url}}/blob/master/examples) directory.

### Global parameters customization

Global parameters, generally used across all the toolbox modules, allow you to select specific processing parameters and
options. In general, standard default parameters are considered a reference and do not require any modification for the
toolbox to run properly.

In the following example, we show how some parameters from `config/local/hmri_local_defaults.m` can be customized and
used during processing. The customized (and
commented) [`hmri_local_default_example_1.m`]({{config.repo_url}}/blob/master/examples/hmri_local_defaults_example_1.m)
has been added to the [`hMRI/examples`]({{config.repo_url}}/blob/master/examples) directory.

The parameters in the example file can be modified and tried out together or separately. Comments in the example file
will guide you to chose and compare the effect of various parameters. The following steps describe how to make the
customized default values effective.

- in the SPM12 batch GUI, select SPM > Tools > hMRI Tools > Configure toolbox.
- under `Defaults parameters`, select `Customised`.
- under `Customised`, select the example file (modified or not).
- select SPM > Tools > hMRI Tools > Create hMRI maps.
- see the [map creation step-by-step example](MapCreation#example) to fill in the fields for this module.
- save and run the batch.

For comparison, the same batch can be run:

- with modified default values in the customized local
  file [`hmri_local_default_example_1.m`]({{config.repo_url}}/blob/master/examples/hmri_local_defaults_example_1.m);
- with `Standard` instead of `Customised` under `Defaults parameters` in the `Configure toolbox` module.

### B1 mapping parameters customization

B1 mapping parameters, especially for the 3D-EPI SE/STE ([Lutti *et al.* 2009 & 2012](References)) and 3D-AFI ([Yarnykh
*et al.* 2007](References)) types of data acquisition, are more likely to require customization in order to account for
local parameters and generate the B1 map output correctly. Many different sequences and protocols are available to
generate such B1 mapping data, and acquisition parameters cannot be retrieved in a consistent way across sequences. As a
result, even if metadata are available, the toolbox might not be able to retrieve the required parameters. In that case,
as well as when no metadata are available at all, the map creation will rely on default parameters to process the data
and a customised B1 mapping default file will be needed. If you are unsure of the rightness of the parameters used to
process B1 mapping data, please refer to the history of messages and warnings output in the Matlab command window during
processing. In particular, pay attention to messages warning you about "parameters not found" and the usage of default
parameters instead.

To illustrate such case, in this second example, we customize `config/local/hmri_b1_local_defaults.m` to match the
acquisition parameters corresponding to the 3D-EPI SE/STE B1 mapping protocol used in
the [hMRI sample dataset][hMRI-dataset-zip-to-download]. The customized (and
commented) [`hmri_b1_local_defaults_example_1.m`]({{config.repo_url}}/blob/master/examples/hmri_b1_local_defaults_example_1.m)
has been added to the [`hMRI/examples`]({{config.repo_url}}/blob/master/examples) directory.

To run the **B1 customization example**:

- Make a fresh copy of the hMRI sample dataset.
- Delete all the *.json files associated to the B1 and B0 mapping files (in
  the `mfc_seste_b1map_v1e_0004`, `gre_field_mapping_1acq_rl_0005` and `gre_field_mapping_1acq_rl_0006` directories).
  That way, no metadata are available to process B1 mapping data. Default values will be used.
- In the SPM12 batch GUI, select SPM > Tools > hMRI Tools > Create hMRI maps.
- See the [map creation step-by-step example](MapCreation#example) to fill in the fields for this module.
- In `B1 bias correction` > `Processing parameters`, select `Customised B1 default file`, then select the
  customised [`hmri_b1_local_defaults_example_1.m`]({{config.repo_url}}/blob/master/examples/hmri_b1_local_defaults_example_1.m).
- Save the batch and run the map creation module.

For comparison, you can:

- **variant 1**: Run the same batch but change `B1 bias correction` > `Processing parameters` back
  to `Use metadata or standard defaults`.
- **variant 2**: Run `Create hMRI maps` exactly as described in
  the [map creation step-by-step example](MapCreation#tutorial) (including all metadata *.json files).

Relevant aspects to compare:

- B1 processing parameters displayed in the Matlab command window (or in the
  output `Results/Supplementary/MPM_map_creation_b1map_params.json` file). Parameters should be identical for **variant
  2** and for the above **B1 customization example**. For **variant 1**, parameters should differ. Moreover, warnings in
  the Matlab command window tell you about the metadata not found for the B1 protocol and the fact that defaults will be
  used. Since the standard defaults are use this time instead of the customised ones, incorrect parameters have been
  used to generate the B1 map.
- B1 maps saved in `Results/Supplementary`
- other quantitative maps (MT, PD, R1) will also slightly differ in **variant 1** as compared to **variant 2** and the *
  *B1 customization example**.

[hMRI-dataset-zip-to-download]: HmriDemoDataset

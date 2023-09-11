###### [Back to hMRI home page](Home)

# Tips & tricks for processing and debugging

Description of the tools available to automatise data processing and control results quality.
Guidelines and good practices to make sure your data are processed as expected, with the help of information stored in the metadata associated to the images & maps and in the parameters and messages log files. 

## Content
    
[SCRIPTING AND BATCHING](#scripting-and-batching)    
[VISUAL INSPECTION](#visual-inspection-of-the-created-maps)    
[METADATA](#metadata)    
[PROCESSING PARAMETER LOG FILES](#processing-parameter-log-files)     
[PROCESSING MESSAGE LOG FILES](#processing-message-log-files)          
[KNOWN ISSUES](#known-issues)     


## Scripting and batching

General documentation about writing batch script in SPM can be found in the SPM documentation on the [SPM Home page](https://www.fil.ion.ucl.ac.uk/spm/doc/manual.pdf).

The hMRI Toolbox benefits from the the `matlabbatch` system integrated in SPM. 
A batch describes which modules should be run on what kind of data and how these modules depend on each other.
This allows you to easily handle input data and define the desired processing steps.
Compared to running each processing step interactively, batches have a number of advantages:

- Tracking: Each batch can be saved as a Matlab *.mat file or as a Matlab *.m script. 
All parameters (including default settings) are included in this script. 
Thus, a saved batch contains a full description of the sequence of processing steps and the parameter settings used.
- Reproducibility: Batches can be saved, even if not all parameters have been set. 
For a multi-subject study, this allows you to create template batches. 
These templates contain all settings which do not vary across subjects. 
For each subject, they can be loaded and only subject-specific parts need to be completed.
- Dependencies: Instead of waiting for a processing step to complete before entering the
results in the next one, all processing steps can be run in the specified order without any
user interaction, retrieving output from one step to be fed into the next step using `Dependencies`.
- Scripting: Template batches can be edited to loop over a number of datasets automatically.

When piloting the processing of your data, we recommend you to:

- define the processing steps using the SPM Batch GUI 
- limit the processing to a single subject's data set
- systematically save the whole batch (including all steps for one subject) as a *.mat file (for every set of options you may consider - if several)
- once the results have been evaluated and the processing steps are established, save the corresponding batch as Matlab *.m script.

To process multiple data sets (from multiple subjects and possibly multiple sessions), we recommend you to use the saved Matlab *.m script as a template. 
Edit it to loop over subjects and save it with a meaningful name. 
Such a script will help you tracking the processing steps for later review, just as *.mat batch files did for a single subject. 
It will also guarantee that all your data are processed in a consistent manner for every subject and every group of subjects. 
Finally, it also allows you to repeat the processing or extend it to more subjects or sessions, 
identically or with slightly different parameters, with as little additional user input as possible - which is likely to be an important source of error otherwise ;)!

## Visual inspection of the created maps

A simple visual inspection of the maps will help you to spot quickly a few basic problems:    
- erroneous reorientation of the images (Auto-reorient module)
- excessive B0 field variation thresholding leading to holes in the images (3D EPI SE/STE B1 transmit bias correction)
- ...

To visualise the maps, it is important to use appropriate windowing range. Typically, the following intensity ranges will provide good rendering:    
- PD maps: [50 120] p.u.
- MT saturation maps: [0 2] p.u.
- R2* maps: [0 70] s-1
- R1 maps: [0 1.4] s-1
- B1 maps: [75 125] p.u.
- RF sensitivity maps: [0 2] a.u.

The `Quality tools` module can be used for visual inspection of the results, as a first step. 
The selected images are displayed with predefined windowing and saved as PNG files for quick visual inspection. 
See the help available in the SPM Batch GUI of the `Quality tools` module.

*WARNING - The current version of the `Quality tools` is not meant to automatically flag anomalies. 
The assessment still relies on the expertise of the person visualizing the data! 
Moreover, the PNG files saved don't show everything, and anomalies can still remain unnoticed.*

The following example illustrates typical rendering of the maps (processed from the [MPM example dataset](HmriDemoDataset), 
see [tutorial for map creation](MapCreation#example)) as rendered using SPM CheckReg and manual adjustment of the intensity ranges:    
[[/images/DebugMetadataAndLogFiles_MPM_example_dataset_display_MT_PD_R1_R2s.png|Example display maps with appropriate scaling]]

The next example shows the same data displayed using the `Quality tools > Visual check` module:      
[[/images/MapCreation_tutorial_check_maps_run03_RFsensmapscorr.png|Visual check of the results]]

## Metadata

If your images have been converted from DICOM to NIfTI using either the SPM12 `DICOM Import` tool with the `Export metadata` option enabled (available since SPM12.4 r7219) or the `DICOM Import` tool from the hMRI toolbox, the whole DICOM header is stored as a JSON file along the nifti volume (same file name, `*.json` and `*.nii` extensions respectively). NOTE: the SPM12.4 implementation of the additional DICOM header storage to a JSON file is based on the hMRI toolbox.

Several tools and scripts are available to handle the metadata in order to set, get and search the parameters and information stored. Refer to the [Metadata Library](MetadataLibrary) page for a full description of the metadata, and in particular to the section dedicated to [metadata handling](MetadataLibrary#how-to-retrieve-specific-parameters-from-the-json-metadata).

The metadata can be used for several purposes. A few examples are given below, illustrating the usefulness of the metadata for a much broader use than the hMRI toolbox alone:     
- *Protocol consistency*: when reviewing the data before processing, check all data sets have been acquired with consistent acquisition parameters (i.e. critical parameters are identical, while e.g. slice orientation and position will differ from one subject to the next). No fully integrated tool is provided for that purpose (yet), but all required tools are available in the metadata library for a user familiar with Matlab to do so.
- *Batching and data processing*: when preparing a batch to process your data, metadata can conveniently be retrieved directly from the metadata (e.g. RepetitionTime, diffusion directions and b-values) rather than hard coded in the script. Can be useful for standard SPM spatial processing, quality assurance, DWI processing *and the hMRI toolbox, obviously*...
- *In the hMRI toolbox*: metadata are used all along, from the DICOM header (to retrieve the appropriate acquisition parameters required to process the data) to information logged into the JSON metadata file of the quantitative maps. The metadata structure of the output maps and images generated by the hMRI toolbox includes processing parameters, dependencies of the result on other input images as well as a summary description of each output image (convenient if you want to double-check e.g. what bias correction has been applied to generate a given map). 

Below we provide an example of metadata including a summary description of the R1 map output. The metadata structure is retrieved using the following SPM/Matlab command:
```matlab
metadata = spm_jsonread(<name of the JSON file>);
```
Then the summary description can be displayed:
```matlab
fprintf(1,'\nSummary description of the output:\n%s', metadata.history.output.imtype);
```
All three examples below correspond to R1 maps generated with per-contrast [RF sensitivity bias correction](MapCreation#rf-sensitivity-bias-correction) and different [B1 transmit bias corrections](MapCreation#b1-transmit-bias-correction).

Case 1: B1 transmit correction using 3D EPI SE/STE mapping:
```
R1 map [s-1]
- B1+ bias correction using provided B1 map (i3D_EPI)
- RF sensitivity bias correction based on a per-contrast sensitivity measurement
```
Case 2: B1 transmit correction using UNICORT:
```
R1 map [s-1]
- B1+ bias correction using UNICORT
- RF sensitivity bias correction based on a per-contrast sensitivity measurement
```
Case 3: no B1 transmit correction:
```
R1 map [s-1]
- no B1+ bias correction applied
- RF sensitivity bias correction based on a per-contrast sensitivity measurement
```

Note that the same information can be read directly from the JSON file using a text editor - easy :)!

## Processing parameter log files

Several JSON files are logged during the [`Create hMRI maps`](MapCreation) module, containing mainly processing parameters. Below is the list of the files generated, stored in the `Results/Supplementary` directory. Depending on the processing options selected, some of them may not be generated.

- `hMRI_map_creation_rfsens_params.json`: RF sensitivity bias correction parameters (measured sensitivity maps).
- `hMRI_map_creation_b1map_params.json`: B1 transmit map estimation: acquisition and processing parameters.
- `hMRI_map_creation_job_create_maps.json`: Create hMRI maps: acquisition and processing parameters.
- `hMRI_map_creation_mpm_params.json`: Acquisition and processing parameters used for the current *Create hMRI maps* job.
- `hMRI_map_creation_quality_assessment.json`: Quality assessement results.

Review these files (you can read them directly using a text editor, or retrieve the Matlab structure therein with the `spm_jsonread` function) to check the parameters used are as expected.


## Processing message log files

*NOTE: The LogMsg option is available from version [v0.1.2-beta2](https://github.com/hMRI-group/hMRI-toolbox/releases/tag/release-v0.1.2-beta2).*

### General purpose and default settings

In order to review and keep track of info, warnings and other messages coming up during data processing, all messages are logged using the `hmri_log.m` script. 

By default, all messages are logged both to the Matlab Command Window and into a `hMRI_LogFile.log` (saved in the current directory). The latter can be more specifically customized (see `help hmri_log`). During the [`Create hMRI maps`](MapCreation) module execution, messages are logged into a `hMRI_map_creation_logfile.log` file into the `Results/Supplementary` directory.

Additionally, any information or warning considered potentially critical for the processing of your data will be displayed in a pop-up window, blocking the execution of the code. This is obviously rather annoying if you intend to process a bunch of data overnight ;)! It can be disabled (see the Batch option "Pop-up warnings") for that purpose. 

### Tips & tricks

For piloting data processing though (e.g. processing a single-subject dataset to check everything is generated as expected), it is recommended to keep the pop-up option enabled, so you can read through and acknowledge every message and make sure everything is set up properly before running the processing on a whole group.

Always carefully review the warning messages. Some of them may be expected. Others may be more relevant and point you towards some errors in the processing parameters used. For example, if no metadata are available, this will be flagged up as a warning. If this is the case, you'd better check whether the processing parameters listed (either in the pop-up warning, processing message log files or [processing parameter log files](#processing-parameter-log-files) correspond to the data and acquisition parameters given as input.

### List of messages

Warning messages are only meant to make you aware of some of the processing steps and parameters used to generate the maps. And to give you a chance to decide whether these processing steps and parameters are definitely the right ones for the data you have at hand. Most warning and information messages are listed below, with a description of potential implications and actions to be taken.

```
WARNING: the (strict) minimum number of echoes required 
to calculate R2* map is 2. The default value (...) has 
been modified and is now neco4R2sfit=2.
```

This warning is related to the minimum number of echoes required to calculate a R2s map. This number is set via the global parameter `hmri_def.neco4R2sfit`. Strictly speaking, the minimum is 2. If you accidentally changed it to anything <2, it will be set back to 2 (although the default is 4). For a robust estimation, the minimum number of echoes required depends on many factors, amongst which:     

- SNR/resolution
- distribution/spacing between TEs: note that early echoes are more affected by the specific contrast, violating the assumption of a common decay between contrasts. 
- number of contrasts available (fewer echoes per contrast required for 3 (PDw, T1w, MTw) contrasts as compared to 2 or even 1)

To be on the safe side, a minimum of 6 echoes is recommended ([ESTATICS](References) paper). Further studies are required to come up with more detailed and informed guidelines. Use fewer echoes at your own risk...! 

```
WARNING: UNICORT B1+ estimate (if available) will be 
used to correct the PD/MT map for B1 transmit bias. This method has 
not been validated. Use with care!
```

This warning relates to the usage of the B1+ transmit field estimated using UNICORT and the apparent (biased) R1 map, to correct for B1 transmit inhomogeneities in PD and/or MT maps. The corresponding parameters are `hmri_def.UNICORT.PD` and `hmri_def.UNICORT.MT`. By default, these options are disabled. The UNICORT-estimated B1 maps usually agree with the measured B1 maps (see the [UNICORT](References) paper), and could possibly be used when no B1 transmit field has been measured. However, the method has not been validated for PD and MT map calculation.

```
WARNING: no UNICORT B1 estimate available for PD/MT calculation.
B1 bias correction must be defined as "UNICORT" (not the case).
UNICORT.PD/MT is disabled!
```

Obviously, if no UNICORT-estimated B1 map is available, it cannot be used to correct PD and MT maps for B1 transmit biases! Again related to the global parameters `hmri_def.UNICORT.PD` and `hmri_def.UNICORT.MT`.

```
INFO: FLASH echoes loaded for each contrast are:
- WARNING: no MT-weighted FLASH echoes available! / MT-weighted: ... echoes
- WARNING: no PD-weighted FLASH echoes available! / PD-weighted: ... echoes
- WARNING: no T1-weighted FLASH echoes available! / T1-weighted: ... echoes
```

This message is only confirming the number of echo(es) available for each contrast. You don't need multi-echo data for each contrast. The toolbox will generate the maps for which the required inputs are available only. For example, if only 8 echoes of PDw images are available, a R2s map alone will be generated. If only one echo is available for each contrast, the toolbox will generate PD, R1 and MT maps (provided B1+ bias correction is somehow enabled for R1 and PD calculation). Note that the efficiency and robustness of the various bias corrections will depend on the contrasts and number of echoes available.

```
WARNING: No TE/TR/FA values found for PDw/T1w/MTw 
images. Fallback to defaults.
```

These parameters can be easily retrieved from the NIfTI description field and/or metadata when images have been imported from DICOM to NIfTI using the hMRI DICOM Import tool, or the standard SPM DICOM Import tool either with or without enabling metadata storage. This message is therefore more likely to come up if your data have been converted to NIfTI using another conversion tool. If so, you will have to define these parameters manually in your local default file and load them using the [`Configure toolbox`](DefaultsAndCustomization) module. The default parameters to be customised are stored in `hmri_def.MPMacq`.

```
ERROR: Echo times do not match between contrasts! Aborting.
```

When creating maps from multi-echo series, one can either average the images from a given contrast (e.g. average over all PDw echoes) or extrapolate the series of echoes to TE=0. In the former case, the processing assumes matching TEs for all contrasts, and the average will only consider the echoes common to every contrast (matching T2* weighting). For example, if 8 PDw echoes, 8 T1w echoes and 6 MTw echoes are available, with identical TEs except for the last two missing echoes in the MTw series, the average will be done over the first 6 echoes for each contrast. In the latter case (extrapolation to TE=0), the mismatch between TEs does not matter, since each extrapolated image is free of any T2* weighting. If you have multi-echo data with non-matching echoes between contrasts, it is therefore necessary to enable the TE=0 extrapolation. This is done by setting `hmri_def.fullOLS = true` (which is the default value).

```
INFO: averaged PDw/T1w/MTw will be calculated based on the first ... echoes.
```

This message is related to the previous ERROR message. It only applies when averaged rather than TE=0 extrapolated images are used. The number of echoes correspond to the maximum number of echoes available in every contrast, to ensure a uniform T2* weighting effect across the contrasts. Does not apply and can be entirely ignored for TE=0 extrapolation case (default `hmri_def.fullOLS = true` case).

```
INFO: MPM acquisition protocol = ...
The coefficients corresponding to this protocol will be applied
to correct for imperfect spoiling. Please check carefully that
the protocol used is definitely the one for which the
coefficients have been calculated.
```

If imperfect spoiling correction is enabled (i.e. `hmri_def.spoilcorr.enabled = true` while default is `hmri_def.spoilcorr.enabled = false`), and if correction factors are available, the method from [Preibisch *et al.*](References) is applied to correct for deviations from the Ernst equation due to remaining transverse magnetisation at the end of each TR (imperfect spoiling). The correction coefficients must be calculated specifically (using simulations) for a given sequence, and depend on the RF spoiling phase increment, gradient spoiling moment, TR and FA. These coefficients have been calculated for a series of customized sequences (standard MPM protocols as used in e.g. [Callaghan *et al.* 2014, Weiskopf *et al.* 2013, Draganski *et al.* 2011](References)) only. Although some other sequences may use the same TRs and FAs, it is unlikely that the other parameters be identical, therefore the correction coefficients estimated for one protocol/sequence must not be used for any other protocol/sequence. If imperfect spoiling correction is enabled, you must therefore make sure that the coefficients used are the right ones for your protocol. To date, the code used to derive these coefficients is not made available. More work required...

```
INFO (PD calculation):
	mean White Matter intensity: ...
	SD White Matter intensity ...

WARNING: Error estimate is high (...%) for calculated PD map:
Error higher than 6% may indicate motion.
```

When a PD map is generated (requires PDw, T1w, B1 transmit and RF receive bias corrections of some sort, and calibration), the average and standard deviation of the values in the white matter before calibration is calculated. If the standard deviation is higher than 6% of the mean value, a warning is displayed. The cutoff threshold of 6% is a good motion indicator for standard MPM protocol with 1 mm isotropic resolution and RF sensitivity bias correction using the Unified Segmentation method. In other cases, and error higher than 6% may originate from various sources (including motion but not only), e.g. residual RF sensitivity modulation from the body coil when measure RF sensitivity maps have been used, residual T2*-weighting when not accounted for, low SNR due to lower number of echoes and/or higher spatial resolution. This warning is rather informative...

```
WARNING: both RF sensitivity and B1 transmit bias corrections 
are required to generate a quantitative (calibrated) PD map.
Either or both of these is missing. Therefore an amplitude 
"A" map will be output instead of a quantitative 
PD map. PD map calibration has been disabled.
```

Consistency check: `hmri_def.PDproc.calibr = 1` (calibration of white matter PD value to 69%) while either (or both) B1 transmis or RF sensitivity bias were disabled or unavaible. In that case, it does not make sense to calibrate the resulting biased A map. Therefore: `hmri_def.PDproc.calibr = 0`.

```
WARNING: if TE=0 fit is enabled (fullOLS option), 
no T2*-weighting bias correction is required. 
T2scorr option disabled.
```

The `hmri_def.T2scorr` option (correction of PD maps for T2*-weighting bias, [Balteau *et al.*](References)) is enabled by default, to be applied if `hmri_def.fullOLS` (interpolation to TE=0) were disabled. By default, interpolation to TE=0 is applied preferencially (removes T2* weighting from all maps). If only single-echo data are available though, the warning will still be displayed although no TE=0 extrapolation nor T2* estimate for T2*-weighting bias correction can be derived. In that case, a residual T2*-weighting modulation will affect the PD map (small effect for short TE).

```
WARNING: both TE=0 fit (fullOLS option) and T2*-weighting 
bias correction (T2scorr option) are disabled. The T2* bias won't
be corrected for and impact the resulting PD map. It is strongly 
recommended to have either of these options enabled!
NOTE: this recommendation does not apply to single-echo datasets.
```

The message is self-explanatory (see [Balteau *et al.* 2018](References)) for details. In case of a single-echo dataset, the T2* bias will remain, as a small effect for small TEs. 

```
WARNING: not enough echoes available (minimum is ...) to 
calculate R2* map. No T2*-weighting bias correction can be 
applied (T2scorr option). T2scorr disabled.
```

The minimum of echoes required to estimate R2* is defined by `hmri_def.neco4R2sfit`. The default value is 4 (although a number of 6 has been used in the literature and considered as the only validated option to date). If no R2* map can be evaluated (number of available echoes < neco4R2sfit for all constrasts), the correction implemented as "T2*-weighting bias correction" (enabled with parameter `hmri_def.T2scorr`) cannot be applied and the option is disabled.

```
WARNING: number of T1w echoes to be averaged for PD calculation (...)
is bigger than the available number of echoes (...). Setting nr_echoes_forA' ...
to ..., the maximum number of echoes available.
```

This messages only applies if `hmri_def.fullOLS = false` (i.e. no interpolation to TE=0). In that case, maps are calculated based on averaged echoes for each contrast. If no T2*-weighting bias correction is applied (`hmri_def.T2scorr = false`, not recommended), it is preferable to calculate the PD map (and preliminary A map) based on the first echo only (`hmri_def.PDproc.nr_echoes_forA = 1`) to reduce the impact of T2*-weighting bias. If `hmri_def.PDproc.T2scorr = true`, the average is calcuated by default over the first 6 echoes (excluding the longer, lower-SNR echoes) or fewer if echo train shorter than 6. The message can be ignored if `hmri_def.fullOLS = true`.

```
-------- R2* map calculation (basic exponential decay) --------
INFO: minimum number of echoes required for R2* map calculation is ...
Number of PDw echoes available: ...
```

Just FYI, related to the basic exponential fit on PDw echoes only. Based on the global value `hmri_def.neco4R2sfit` defining the number of echoes considered safe enough to derive R2* from the PDw echo train. NOTE: 6 is the current minimum published in various [papers](References). If the number of echoes available is lower than the minimum required, the following message comes up:

```
No (basic) R2* map will be calculated.
```

If the number of echoes available is equal or higher than the minimum required, but is lower than 6, the following message comes up:

```
GENERAL WARNING: deriving R2* map from an echo train including 
fewer than 6 echoes has not been validated nor investigated. 
For a robust estimation, the minimum number of echoes required 
depends on many factors, amongst which: 
- SNR/resolution
- distribution/spacing between TEs: note that early echoes are more
  affected by the PDw contrast.
Interpret results carefully, with in mind a possible lack of robustness
and reliability in the R2* estimation.
```



```
-------- OLS R2* map calculation (ESTATICS) --------
INFO: minimum number of echoes required for R2* map calculation is ...
Number of echoes available:  MTw = ... PDw = ... T1w = ...
```

Just FYI, related to the R2* estimate based on the ESTATICS model (fit all echoes). Based on the global value `hmri_def.neco4R2sfit` defining the number of echoes considered safe enough to derive R2*. NOTE: 6 is the current minimum published in the [ESTATICS paper](References). If the number of echoes available is lower than the minimum required for all contrasts, the following message comes up:

```
No R2* map will be calculated using the ESTATICS model.
```

If the number of echoes available is equal or higher than the minimum required at least one of the contrasts, but is lower than 6, the following message comes up:

```
GENERAL WARNING: deriving R2* map from an echo train including 
fewer than 6 echoes has not been validated nor investigated. 
For a robust estimation, the minimum number of echoes required 
depends on many factors, amongst which: 
- SNR/resolution
- distribution/spacing between TEs: note that early echoes are more
  affected by the specific contrast, violating the ESTATICS assumption 
  of a common decay between contrasts.
- number of contrasts available (fewer echoes per contrast required 
  for 3 (PDw, T1w, MTw) contrasts as compared to 2 or even 1) 
Interpret results carefully, with in mind a possible lack of robustness 
and reliability in the R2* estimation.
```

```
WARNING: ACPC Realign was enabled, but no MT map was available 
to proceed. ACPC realignment must be done separately, e.g. you can
run [hMRI tools > Auto-reorient] before calculating the maps.
NOTE: segmentation might crash if no initial reorientation.
```

An AC/PC realignment of all images can be applied *during* the `Create hMRI maps` module (`hmri_def.qMRI_maps.ACPCrealign = 1`). It is based on the already calculated MT map. If not available, the realignment cannot proceed. This option is kept for backward compatibility but is not recommended (`hmri_def.qMRI_maps.ACPCrealign = 0` by default). Realignement using the `Auto-reorient` module is preferred.


## Known issues

Below are listed common issues experienced by users when installing and first running the toolbox. If you face issues that are not listed here, please report it. About issue tracking and support, refer to the [Develop and Contribute](Contribute) section.

**Invalid MEX file under MacOS**    
The compiled ```spm_jsonread.mexmaci64``` file is unfortunately version-dependent (including Matlab, compiler and Mac OS versions)! Therefore, you might need to recompile the mex file before using it.     
Here is how to proceed:     
- in Matlab, change directory to ```/hMRI-Toolbox/spm12/src```
- run ```mex spm_jsonread.c jsmn.c -DJSMN_PARENT_LINKS```    
- copy the created spm_jsonread.mexmaci64 file from ```/hMRI-Toolbox/spm12/src``` to ```/hMRI-Toolbox/spm12``` (overwriting the existing one).


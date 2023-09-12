###### [Back to hMRI home page](Home)

# Help

On top of this Wiki, help information can be found in the Help section of each matlab script (or by typing ```help <function>``` in Matlab) and in the SPM Batch GUI in the Help box at the bottom of the Batch window. Both sources are collected here for convenience. 

*NOTE: the information below can already give you an overview of the toolbox but needs to be updated - version November 2017! Refer to the help available directly in the scripts and Batch GUI.*

## Content

[HELP ON INDIVIDUAL SCRIPTS](HelpScripts#help-on-individual-scripts)    
- [hmri_autoreorient.m](HelpScripts#hmri_autoreorientm)    
- [hmri_create_B1Map_process.m](HelpScripts#hmri_create_B1Map_processm)    
- [hmri_create_B1Map_unwarp.m](HelpScripts#hmri_create_B1Map_unwarpm)    
- [hmri_create_FieldMap.m](HelpScripts#hmri_create_FieldMapm)    
- [hmri_create_MTProt.m](HelpScripts#hmri_create_MTProtm)    
- [hmri_create_RFsens.m](HelpScripts#hmri_create_RFsensm)    
- [hmri_create_b1map.m](HelpScripts#hmri_create_b1mapm)    
- [hmri_create_comm_adjust.m](HelpScripts#hmri_create_comm_adjustm)    
- [hmri_create_pm_brain_mask.m](HelpScripts#hmri_create_pm_brain_maskm)    
- [hmri_create_pm_segment.m](HelpScripts#hmri_create_pm_segmentm)    
- [hmri_create_unicort.m](HelpScripts#hmri_create_unicortm)    
- [hmri_get_defaults.m](HelpScripts#hmri_get_defaultsm)    
- [hmri_get_version.m](HelpScripts#hmri_get_versionm)    
- [hmri_proc_MPMsmooth.m](HelpScripts#hmri_proc_MPMsmoothm)    
- [hmri_run_create.m](HelpScripts#hmri_run_createm)    
- [hmri_run_proc_US.m](HelpScripts#hmri_run_proc_USm)    
- [hmri_run_proc_dartel_norm.m](HelpScripts#hmri_run_proc_dartel_normm)    
- [hmri_run_proc_pipeline.m](HelpScripts#hmri_run_proc_pipelinem)    
- [hmri_run_proc_smooth.m](HelpScripts#hmri_run_proc_smoothm)    
- [config\hmri_b1_standard_defaults.m](HelpScripts#confighmri_b1_standard_defaultsm)    
- [config\hmri_defaults.m](HelpScripts#confighmri_defaultsm)    
- [config\local\hmri_b1_local_defaults.m](HelpScripts#configlocalhmri_b1_local_defaultsm)    
- [config\local\hmri_local_defaults.m](HelpScripts#configlocalhmri_local_defaultsm)    
- [spm12\spm_dicom_convert.m](HelpScripts#spm12spm_dicom_convertm)    
- [spm12\spm_jsonwrite.m](HelpScripts#spm12spm_jsonwritem)    
- [spm12\config\spm_cfg_dicom.m](HelpScripts#spm12configspm_cfg_dicomm)    
- [spm12\config\spm_run_dicom.m](HelpScripts#spm12configspm_run_dicomm)    
- [spm12\metadata\anonymise_metadata.m](HelpScripts#spm12metadataanonymise_metadatam)    
- [spm12\metadata\find_field_name.m](HelpScripts#spm12metadatafind_field_namem)    
- [spm12\metadata\get_jhdr_and_offset.m](HelpScripts#spm12metadataget_jhdr_and_offsetm)    
- [spm12\metadata\get_metadata.m](HelpScripts#spm12metadataget_metadatam)    
- [spm12\metadata\get_metadata_val.m](HelpScripts#spm12metadataget_metadata_valm)    
- [spm12\metadata\has_extended_header.m](HelpScripts#spm12metadatahas_extended_headerm)    
- [spm12\metadata\init_metadata.m](HelpScripts#spm12metadatainit_metadatam)    
- [spm12\metadata\init_output_metadata_structure.m](HelpScripts#spm12metadatainit_output_metadata_structurem)    
- [spm12\metadata\read_ASCII.m](HelpScripts#spm12metadataread_ASCIIm)    
- [spm12\metadata\reformat_spm_dicom_header.m](HelpScripts#spm12metadatareformat_spm_dicom_headerm)    
- [spm12\metadata\set_metadata.m](HelpScripts#spm12metadataset_metadatam)    
- [spm12\metadata\tidy_CSA.m](HelpScripts#spm12metadatatidy_CSAm)    
- [spm12\metadata\write_extended_header.m](HelpScripts#spm12metadatawrite_extended_headerm)    

[HELP FOR THE BATCH OPTIONS](HelpScripts#help-for-the-batch-options)    
- [Configure Toolbox](HelpScripts#configure-toolbox)
- [Auto-Reorient](HelpScripts#auto-reorient)
- [DICOM Import](HelpScripts#dicom-import)
- [Create hMRI maps](HelpScripts#create-hmri-maps)
- [Process hMRI maps](HelpScripts#process-hmri-maps)

## Help on individual scripts

#### hmri_autoreorient.m
```
 FORMAT out = hmri_autoreorient(ref, template, other) 
 
 PURPOSE: Reorientation of the images towards the MNI space is a standard 
 step in neuroimage processing, and often a prerequisite for successful 
 segmentation. The Unified Segmentation process is indeed rather sensitive 
 to the initial orientation of the image. We provide you with a simple 
 tool for reorientation of all images prior to any further processing 
 (including multiparameter map calculation).  
 
 METHODS: Reorientation is based on rigid-body coregistration of a 
 suitable image (i.e. contrast must be well enough defined to allow for 
 reliable coregistration) to the MNI space (i.e. mainly set the AC 
 location and correct for head rotation) and application of the 
 coregistration matrix to all images acquired during the same session 
 (specified as "other images"). The code makes use of spm_affreg and 
 templates available in SPM.  
 
 IN: 
 - ref      : filename of the reference image to reorient, 
 - template : a template image, already in the MNI space, to which the 
              reference image is reoriented, 
 - other    : filenames of other images to be reoriented along with the 
              reference image. 
 
 OUT: 
 - out, a structure with fields: 
       - files  : the list (cell array of strings) of reoriented images  
                  listed in the same order as the input (ref, then other). 
       - M      : the rigid-body transformation matrix  
       - invM   : the rigid-body transformation matrix inverted 
```

#### hmri_create_B1Map_process.m
```
 PURPOSE 
 To mask, pad and smooth the unwrapped B1 map. 
 Part of the hMRI toolbox, B1+ map calculation, EPI SE/STE protocol. 
```

#### hmri_create_B1Map_unwarp.m
```
 PURPOSE 
 For B0 undistortion of EPI-based B1 maps, part of the hMRI toolbox. 
```

#### hmri_create_FieldMap.m
```
 This is a fraction of the FieldMap script (case 'createfieldmap') 
 rewritten for the hMRI toolbox in order to make use of the original 
 SPM12's FieldMap script wherever no modification is required. The 
 modification implies the use of new segmentation tools to create the 
 brain mask for unwrapping. 
```

#### hmri_create_MTProt.m
```
 This is hmri_create_MTProt, part of the hMRI-Toolbox. 
```

#### hmri_create_RFsens.m
```
 RF sensitivity calculation as part of the hmri toolbox 
 Based on a script by Daniel Papp 
 Wellcome Trust Centre for Neuroimaging (WTCN), London, UK. 
 daniel.papp.13@ucl.ac.uk 
 Adapted by Dr. Tobias Leutritz 
```

#### hmri_create_b1map.m
```
 FORMAT P_trans = hmri_create_b1map(jobsubj) 
    jobsubj - are parameters for one subject out of the job list. 
    NB: ONE SINGLE DATA SET FROM ONE SINGLE SUBJECT IS PROCESSED HERE, 
    LOOP OVER SUBJECTS DONE AT HIGHER LEVEL. 
    P_trans - a vector of file names with P_trans(1,:) = anatomical volume 
        for coregistration and P_trans(2,:) = B1 map in percent units. 
```

#### hmri_create_comm_adjust.m
```
 This function is a version of comm_adjust.m customized for T1 like 
 images, i.e. MT images. Now it is necessary to define as input parameter 
 the Template to be used for the commisure adjustment. 
```

#### hmri_create_pm_brain_mask.m
```
 Calculate a brain mask in hMRI Toolbox 
 This is pm_brain_mask (SPM12/toolbox/FieldMap) modified for the hMRI 
 toolbox. Calls hmri_create_pm_segment instead of pm_segment. This is the 
 only modification, syntax is unchanged otherwise. 
 
 FORMAT bmask = hmri_create_pm_brain_mask(P,flags) 
 
 P - is a single pointer to a single image 
 
 flags - structure containing various options 
         template - which template for segmentation 
         fwhm     - fwhm of smoothing kernel for generating mask 
         nerode   - number of erosions 
         thresh   - threshold for smoothed mask  
         ndilate  - number of dilations 
 
__________________________________________________________________________ 
 
 Inputs 
 A single *.img conforming to SPM data format (see 'Data Format'). 
 
 Outputs 
 Brain mask in a matrix 
__________________________________________________________________________ 
 
 The brain mask is generated by segmenting the image into GM, WM and CSF,  
 adding these components together then thresholding above zero. 
 A morphological opening is performed to get rid of stuff left outside of 
 the brain. Any leftover holes are filled.  
```

#### hmri_create_pm_segment.m
```
 To segment the brain and extract a brain mask in the hMRI Toolbox 
 This function replaces pm_segment used in the FieldMap toolbox 
 (SPM12/toolbox/FieldMap). Used in hmri_create_pm_brain_mask. 
```

#### hmri_create_unicort.m
```
 function P = hmri_create_unicort(P_PDw, P_R1, jobsubj) 
 P_PDw: proton density weighted FLASH image (small flip angle image) for 
 masking 
 P_R1: R1 (=1/T1) map estimated from dual flip angle FLASH experiment 
```

#### hmri_get_defaults.m
```
 Get/set the defaults values associated with an identifier 
 FORMAT defaults = hmri_get_defaults 
 Return the global "defaults" variable defined in hmri_defaults.m. 
 
 FORMAT defval = hmri_get_defaults(defstr) 
 Return the defaults value associated with identifier "defstr". 
 Currently, this is a '.' subscript reference into the global 
 "hmri_def" variable defined in hmri_defaults.m. 
 
 FORMAT hmri_get_defaults(defstr, defval) 
 Sets the defaults value associated with identifier "defstr". The new 
 defaults value applies immediately to: 
 * new modules in batch jobs 
 * modules in batch jobs that have not been saved yet 
 This value will not be saved for future sessions of hMRI. To make 
 persistent changes, edit hmri_defaults.m. 
 
 NOTE, specific to hMRI tool (might become obsolete in future versions): 
 In order to allow centre specific defaults, *without* editing/commenting 
 the default file itself, these centre specific values are placed in a 
 centre specific substructure, named with 'fil', 'lren' or 'crc'(see the  
 hmri_defaults.m file). 
 Then if the required field is not found in the standard default  
 structure, the centre specific field, e.g. 'fil', 'lren' or crc', is  
 included *automatically* in the call. Therefore you can access a centre 
 specific parameter by specifying the centre properly in the defaults 
 file AND calling for the parameter. 
  
 Example: 
 -------- 
 Definition of hmri_def 
 hmri_def.centre = 'crc' ;  
 hmri_def.param1 = 123 ; 
 hmri_def.crc.TR  = 3;  % in sec 
 hmri_def.fil.TR  = 2;  % in sec 
 hmri_def.lren.TR  = 2.5;  % in sec 
  
 v = hmri_get_defaults('param1') 
 returns the value 123 into v 
 v = hmri_get_defaults('TR') 
 returns the value 3 into v 
 If you edit the file and set hmri_def.centre = 'fil' ; 
 then v = hmri_get_defaults('TR') 
 returns the value 2 into v 
 
 A few constraints for this trick to work: 
 - the default structure has a field called 'centre' defining the current 
   centre values to use ('fil', crc', 'lren' to begin with) 
 - all centre substructures should have the same organisation! 
 - centre specific fields and global fields should have *different* names, 
   e.g. do NOT define "def.TE = 20" and "def.fil.TE = 30" fields as the  
   latter would NEVER be used. 
```

#### hmri_get_version.m
```
 To retrieve the SHA1, author, date and message of the last commit of the 
 current branch of the repository and return the information as a string. 
 This script MUST be located in the root directory of the repository. 
 If the Toolbox has been copied whitout version tracking, the version can 
 only be retrieved if a version.txt file is already present in the root 
 directory of the Toolbox. 
```

#### hmri_proc_MPMsmooth.m
```
 Applying tissue specific smoothing, aka. weighted averaging, in order to  
 limit partial volume effect.  
  
 FORMAT 
   fn_out = hmri_proc_MPMsmooth(fn_wMPM, fn_mwTC, fn_TPM, fwhm) 
  
 INPUT 
 - fn_wMPM : filenames (char array) of the warped MPM, i.e. the 
             w*MT/R1/R2s/A.nii files 
 - fn_mwTC : filenames (char array) of the modulated warped tissue 
             classes, i.e. the mwc1/2*.nii files 
 - fn_TPM  : filenames (char array) of the a priori tissue probability  
             maps to use, matching those in fn_mwTC 
 - fwhm    : width of smoothing kernel in mm [6 by def.] 
 - l_TC    : explicit list of tissue classes used [1:nTC by def.] 
  
 OUTPUT 
 - fn_out  : cell array (one cell per MPM) of filenames (char array) of  
             the "smoothed tissue specific MPMs". 
  
 REFERENCE 
 Draganski et al, 2011, doi:10.1016/j.neuroimage.2011.01.052 
```

#### hmri_run_create.m
```
 PURPOSE 
 Calculation of multiparameter maps using B1 maps for B1 bias correction. 
 If no B1 maps available, one can choose not to correct for B1 bias or 
 apply UNICORT. 
```

#### hmri_run_proc_US.m
```
 Deal with the spatial preprocessing, 1 subject at a time: segmentation of 
 the MT and T1 images 
```

#### hmri_run_proc_dartel_norm.m
```
 function out = hmri_run_proc_dartel_norm(job) 
 derived from spm_dartel_norm_fun_local 
  
 It applies the "Dartel - Normalize to MNI" onto the tissue classes and 
 parametric maps, from the job created in the batch. 
```

#### hmri_run_proc_pipeline.m
```
 Deal with the preprocessing pipelines. There are 2 options 
 1/ US+Smooth 
 2/ US+Dartel+Smooth 
 
 Input include only some parametric maps, the structural maps (for 
 segmentation), the required smoothing and which pipeline to use. All 
 other options are hard-coded! 
 By default, these pipelines focus only on the first 2 tissue classes, 
 i.e. GM and WM only. 
  
 For more flexibility, individual modules can be combined. :-) 
  
```

#### hmri_run_proc_smooth.m
```
 Function to run the smoothing/weighted averaging over a bunch of 
 subjects, as defined in the batch interface. 
 Data are selected in a 'many subject' style, i.e. all the images of one 
 type are selected from many subjects at once! 
  
 The 'out' structure is organized as a structure out.tc where 
 - tc is a cell-array of size {n_TCs x n_pams} 
 - each element tc{ii,jj} is a cell array {n_subj x 1} with each subject's 
   smoothed data for the ii^th TC and jj^th MPM 
```

#### config\hmri_b1_standard_defaults.m
```
 Sets the defaults for B1 bias correction, part of the hMRI toolbox. 
```

#### config\hmri_defaults.m
```
 Sets the defaults parameters which are used by the hMRI toolbox. 
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! 
 DON'T MODIFY THIS FILE, IT CONTAINS THE REFERENCE DEFAULTS PARAMETERS. 
 Please refer to hMRI-Toolbox\config\local\hmri_local_defaults.m to 
 customise defaults parameters.   
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! 
 
 FORMAT hmri_defaults 
__________________________________________________________________________ 
 
 THIS FILE SHOULD NOT BE MODIFIED.  
 To customize the hMRI-Toolbox defaults parameters so they match your own 
 site- or protocol-specific setup, please refer to the defaults files in 
 hMRI-Toolbox\config\local. In particular, use "hmri_local_defaults.m". 
 Make a copy with meaningful name, modify as desired and select as general 
 defaults file in the "Configure toolbox" branch of the hMRI-Toolbox. 
 
 The structure and content of this file are largely inspired by the 
 equivalent file in SPM. 
```

#### config\local\hmri_b1_local_defaults.m
```
 Sets the defaults for B1 bias correction, part of the hMRI toolbox. 
 Consider this file as a template for local settings specifications.  
 Please read below for details. 
```

#### config\local\hmri_local_defaults.m
```
 PURPOSE 
 To set user-defined (site- or protocol-specific) defaults parameters 
 which are used by the hMRI toolbox. Customized processing parameters can 
 be defined, overwriting defaults from hmri_defaults. Acquisition 
 protocols can be specified here as a fallback solution when no metadata 
 are available. Note that the use of metadata is strongly recommended.  
```

#### spm12\spm_dicom_convert.m
```
 Convert DICOM images into something that SPM can use (e.g. NIfTI) 
 FORMAT spm_dicom_convert(hdr,opts,root_dir,format,out_dir) 
 Inputs: 
 hdr      - a cell array of DICOM headers from spm_dicom_headers 
 opts     - options: 
              'all'      - all DICOM files [default] 
              'mosaic'   - the mosaic images 
              'standard' - standard DICOM files 
              'spect'    - SIEMENS Spectroscopy DICOMs (some formats only) 
                           This will write out a 5D NIFTI containing real 
                           and imaginary part of the spectroscopy time  
                           points at the position of spectroscopy voxel(s). 
              'raw'      - convert raw FIDs (not implemented) 
 root_dir - 'flat'       - do not produce file tree [default] 
              With all other options, files will be sorted into 
              directories according to their sequence/protocol names: 
            'date_time'  - Place files under ./<StudyDate-StudyTime> 
            'patid'      - Place files under ./<PatID> 
            'patid_date' - Place files under ./<PatID-StudyDate> 
            'patname'    - Place files under ./<PatName> 
            'series'     - Place files in series folders, without 
                           creating patient folders 
 format   - output format: 
              'nii'      - Single file NIfTI format [default] 
              'img'      - Two file (hdr+img) NIfTI format 
            All images will contain a single 3D dataset, 4D images will 
            not be created. 
 out_dir  - output directory name. 
 
 json     - a structure with fields:  
               extended -> metadata stored as extended nii header included 
                           in the nii image [default: false] 
               separate -> metadata stored in separate json file [default: 
                           false] 
               Note: extended and separate fields can be simultaneously  
                           true or false. 
               anonym   -> 'basic' (default), 'full', 'none'  
               Important note: anonymisation depends on input headers and 
                           cannot be guaranteed. 
               
 Output: 
 out      - a struct with a single field .files. out.files contains a 
            cellstring with filenames of created files. If no files are 
            created, a cell with an empty string {''} is returned. 
```

#### spm12\spm_jsonwrite.m
```
 Serialize a JSON (JavaScript Object Notation) structure 
 FORMAT spm_jsonwrite(filename,json) 
 filename - JSON filename 
 json     - JSON structure 
 
 FORMAT S = spm_jsonwrite(json) 
 json     - JSON structure 
 S        - serialized JSON structure (string) 
 
 FORMAT [...] = spm_jsonwrite(...,opts) 
 opts     - structure of optional parameters: 
              indent: string to use for indentation [Default: ''] 
              replacementStyle: string to control how non-alphanumeric 
                characters are replaced [Default: 'underscore'] 
  
 References: 
   JSON Standard: http://www.json.org/ 
```

#### spm12\config\spm_cfg_dicom.m
```
 SPM Configuration file for DICOM Import 
```

#### spm12\config\spm_run_dicom.m
```
 SPM job execution function 
 takes a harvested job data structure and call SPM functions to perform 
 computations on the data. 
 Input: 
 job    - harvested job data structure (see matlabbatch help) 
 Output: 
 out    - computation results, usually a struct variable. 
```

#### spm12\metadata\anonymise_metadata.m
```
 USAGE: hdrout = anonymise_metadata(hdr, opts) 
 hdr is a DICOM header read with spm_dicom_header 
 opts are anonymisation options: 
       opts.anonym = 'none': no anonymisation, all patient data kept in 
                       the metadata. 
                     'full': no patient information is kept at all 
                     'basic': patient ID (presumably not containing his 
                       name), age (years at the time of the data 
                       acquisition), sex, size and weight are kept, 
                       patient name, date of birth and DICOM filename 
                       (often containing the patient name) are removed. 
                      
 !!! IMPORTANT WARNING: EFFEVTIVE ANONYMISATION IS NOT GUARANTEED !!! 
 The anonymisation implemented here depends on the structure and content 
 of the DICOM header and might not be effective in many cases.  
```

#### spm12\metadata\find_field_name.m
```
 PURPOSE 
 To search a structure recursively to retrieve fields with a 
 specific name (or part of that name). Returns the number of fields found 
 with that name (nFieldFound) and a cell array containing the path of 
 each occurrence of that name (each line in fieldList gives the list of 
 nested fields down to the expected one, e.g. 
 inStruct.(fieldList{1,1}).(fieldList{1,2}). ... 
 .(fieldList{1,last-non-empty-one})). If unsuccessful, the  
 search returns [0,{}].   
```

#### spm12\metadata\get_jhdr_and_offset.m
```
 FORMAT 
 [jhdr, offset] = get_jhdr_and_offset(hdr) 
 hdr       a Matlab structure 
 jhdr      the JSONified Matlab structure 
 offset    the offset at which image data start in the nifti file 
```

#### spm12\metadata\get_metadata.m
```
 PURPOSE 
 To retrieve JSON-encoded metadata from extended nifti images or 
 associated JSON file. 
```

#### spm12\metadata\get_metadata_val.m
```
 This is get_metadata_val, part of the metadata library 
```

#### spm12\metadata\has_extended_header.m
```
 Must strip the ',1' (at the end of the file extension '.nii,1')  
 if the name has been collected using e.g. spm_select: 
```

#### spm12\metadata\init_metadata.m
```
 To create JSON-encoded metadata during DICOM to nifti conversion, 
 including all acquisition parameters. The metadata can either (or both) 
 be stored in the extended header of the nifti image or saved as a 
 separate file. This function is called by spm_dicom_convert. In case of 
 an extended nii header, the size of the JSON header is used to define a 
 new offset (N.dat.offset) for the image data in the nifti file and the 
 JSON header is written into the nifti file. The modified nifti object N 
 is returned so the data can be written in it according to the new offset 
 (in spm_dicom_convert). In case of a separate JSON file, N is returned 
 unchanged and the JSON metadata are written in a separate file (same file 
 name as the nifti image, with .json extension). 
```

#### spm12\metadata\init_output_metadata_structure.m
```
 PURPOSE: To create a metadata structure to be used for newly created 
 output images. 
```

#### spm12\metadata\read_ASCII.m
```
 function ascout = read_ASCII(ascin) 
```

#### spm12\metadata\reformat_spm_dicom_header.m
```
 To tidy up and rearrange CSA fields in the header, including formatting 
 the ASCII part into a proper Matlab structure (Note: this is specific to 
 Siemens DICOM format but could be extended to other cases where needed)  
```

#### spm12\metadata\set_metadata.m
```
 To insert or update JSON-encoded metadata into nifti images (for extended 
 nifti format) or JSON files associated to standard nifti format.  
```

#### spm12\metadata\tidy_CSA.m
```
 function tdycsa = tidy_CSA(csahdr) 
 DESCRIPTION: 
 To rearrange CSAImageHeaderInfo, CSASeriesHeaderInfo, ... structures 
 and make it a simplified, easier to browse structure (Siemens specific). 
 USAGE: 
 tdycsa = tidy_CSA(csahdr) 
 where csahdr is a structure, content of the CSA field. 
 WRITTEN BY: Evelyne Balteau - Cyclotron Research Centre 
```

#### spm12\metadata\write_extended_header.m
```
 FORMAT 
 write_extended_header(fnam, jsonhdr) 
 fnam      the name of a nifti file 
 jsonhdr   the JSONified Matlab structure to be written as extended header 
```


## Help for the BATCH options 


## Configure toolbox

  Customised default parameters can be set here by selecting a customised [hmri_local_defaults_*.m] file. Type [help hmri_local_defaults] for more details.

- **Defaults parameters**    
  You can either stick with standard defaults parameters from [hmri_defaults.m] or select your own customised defaults file.

  - **Standard**    
    

  - **Customised**    
    Select the [hmri_local_defaults_*.m] file containing the specific defaults to process your data. Note that all other defaults values will be reinitialised to their standard values.


## DICOM Import

  DICOM Conversion.
  Most scanners produce data in DICOM format. This routine attempts to convert DICOM files into SPM compatible image volumes, which are written into the current directory by default. Note that not all flavours of DICOM can be handled, as DICOM is a very complicated format, and some scanner manufacturers use their own fields, which are not in the official documentation at http://medical.nema.org/

- **DICOM files**    
  Select the DICOM files to convert.

- **Directory structure for converted files**    
  Choose root directory of converted file tree. The options are:
  
  * Output directory: ./StudyDate-StudyTime/ProtocolName
  * Output directory: ./PatientID/ProtocolName
  * Output directory: ./PatientID/StudyDate-StudyTime/ProtocolName
  * Output directory: ./ProtocolName
  * No directory hierarchy: Convert all files into the output directory, without sequence/series subdirectories

- **Output directory**    
  Select a directory where files are written.

- **Protocol name filter**    
  A regular expression to filter protocol names. DICOM images whose protocol names do not match this filter will not be converted.

- **Conversion options**    
  

  - **Output image format**    
    DICOM conversion can create separate img and hdr files or combine them in one file. The single file option will help you save space on your hard disk, but may be incompatible with programs that are not NIfTI-aware.
    In any case, only 3D image files will be produced.

  - **Use ICEDims in filename**    
    If image sorting fails, one can try using the additional SIEMENS ICEDims information to create unique filenames. Use this only if there would be multiple volumes with exactly the same file names.

  - **JSON metadata**    
    

    - **JSON metadata format**    
      Metadata (acquisition parameters, processing steps, ...) 
      can be stored using JSON format. The JSON metadata can 
      either (or both) be stored as a separate JSON file 
      or as an extended header (included in the image file, 
      for nii output format only).


## Auto-Reorient

  Function to automatically (but approximately) rigid-body reorient a T1 image (or any other usual image modality) in the MNI space, i.e. mainly set the AC location and correct for head rotation, in order to further proceed with the segmentation/normalisation of the image. This is useful since the Unified Segmentation process is rather sensitive to the initial orientation of the image.
  
  A set of other images can be reoriented along the 1st one. They should be specified as "Other Images".
  
  The following outputs are available as dependencies for further processing:
  - reoriented images (provided as individual images or as a group of images),
  - transformation matrix M (if needed to be applied to other images at a later stage using SPM > Util > Reorient Images),
  - inverted transformation matrix M (to go back to the initial orientation using SPM > Util > Reorient Images).
  
  NOTE: the job structure and the output structure are saved as JSON data along with the reoriented images.

- **Image**    
  Select the image that is best suited for rigid-body coregistration to MNI space. Ideally, the GM/WM contrast in that image should be well defined.

- **Template**    
  Auto-Reorient needs to know which modality of image is selected and which template to use for the rigid-body coregistration.
  You can select the template image from SPM12/canonical, SPM12/toolbox/OldNorm or any suitable source of your choice.
  By defaults, SPM12/canonical/avg152T1.nii is used.
  NOTE: The template must already be in the MNI orientation to make the reorientation effective!

- **Other Images**    
  Select all the other images that need to remain in alignment with the reference image that is reoriented. Typically, all the images acquired during a single MRI session should be reoriented together. For convenience, select all these images here, including the reference image.

- **Output choice**    
  Output directory can be the same as the input directory or user selected.

  - **Input directory**    
    Auto-reorientation is applied to the input files directly.

  - **Output directory**    
    Input files are first copied to the selected directory before auto-reorientation.

- **Dependencies**    
  Dependencies can be made available as individual output files or as a group of images, as is most convenient for the next processing steps...


## Create hMRI maps

  hMRI map creation based on multi-echo FLASH sequences including optional receive/transmit bias correction.

- **Few Subjects**    
  Specify the number of subjects.

  - **Subject**    
    Specify a subject for maps calculation.

    - **Output choice**    
      Output directory can be the same as the input directory for each input file or user selected

      - **Input directory**    
        Output files will be written to the same folder as each corresponding input file.

      - **Output directory**    
        Select a directory where output files will be written to.

    - **RF sensitivity bias correction**    
      Specify whether RF sensitivity maps have been acquired. You can select either:
      - None: no RF sensitivity map has been acquired,
      - Single: single set of RF sensitivity maps acquired for all contrasts,
      - Per contrast: one set of RF sensitivity maps acquired for each contrast.

      - **None**    
        No RF sensitivity map was acquired.

      - **Single**    
        Single set of RF sensitivity maps acquired for all contrasts.
        Select low resolution RF sensitivity maps acquired with the head and body coils respectively, in that order.

      - **Per contrast**    
        One set of RF sensitivity maps is acquired for each contrast i.e. for each of the PD-, T1- and MT-weighted multi-echo FLASH acquisitions.

        - **RF sensitivity maps for MTw images**    
          Select low resolution RF sensitivity maps acquired with the head and body coils respectively, in that order.

        - **RF sensitivity maps for PDw images**    
          Select low resolution RF sensitivity maps acquired with the head and body coils respectively, in that order.

        - **RF sensitivity maps for T1w images**    
          Select low resolution RF sensitivity maps acquired with the head and body coils respectively, in that order.

    - **B1 bias correction**    
      Choose the methods for B1 bias correction.
      Various types of B1 mapping protocols can be handled by the hMRI toolbox when creating the multiparameter maps. See list below for a brief description of each type. Note that all types may not be available at your site.
       - 3D EPI: B1map obtained from spin echo (SE) and stimulated echo (STE) images recorded with a 3D EPI scheme [Lutti A et al., PLoS One 2012;7(3):e32379].
       - 3D AFI: 3D actual flip angle imaging (AFI) method based on [Yarnykh VL, Magn Reson Med 2007;57:192-200].
       - tfl_b1_map: Siemens product sequence for B1 mapping based on turbo FLASH.
       - rf_map: Siemens product sequence for B1 mapping based on SE/STE.
       - no B1 correction: if selected no B1 bias correction will be applied.
       - pre-processed B1: B1 map pre-calculated outside the hMRI toolbox, must be expressed in percent units of the nominal flip angle value (percent bias).
       - UNICORT: Use this option when B1 maps not available. Bias field estimation and correction will be performed using the approach described in [Weiskopf et al., NeuroImage 2011; 54:2116-2124]. WARNING: the correction only applies to R1 maps.

      - **3D EPI**    
        Input B0/B1 data for 3D EPI protocol
        As B1 input, please select all pairs of SE/STE 3D EPI images.
        For this EPI protocol, it is recommended to acquire B0 field map data for distortion correction. If no B0 map available, the script will proceed with distorted images.
        Please enter the two magnitude images and the presubtracted phase image from the B0 mapping acquisition, in that order.
        Regarding processing parameters, you can either stick with metadata and standard defaults parameters (recommended) or select your own [hmri_b1_local_defaults_*.m] customised defaults file (fallback for situations where no metadata are available).

        - **B1 input**    
          Select B1 input images according to the type of B1 bias correction.

        - **B0 input**    
          Select B0 field map input images.
          Only required for distortion correction of EPI-based B1 maps.
          Select both magnitude images and the presubtracted phase image, in that order.

        - **Processing parameters**    
          You can either stick with metadata and standard defaults parameters (recommended) or select your own customised defaults file (fallback for situations where no metadata are available).

          - **Use metadata or standard defaults**    
            

          - **Customised B1 defaults file**    
            Select the [hmri_b1_local_defaults_*.m] file containing the parameters to process the B1 map data. By default, parameters will be collected from metadata when available. Defaults parameters are provided as fallback solution when metadata are not available and/or uncomplete.
            Please make sure that the parameters defined in the defaults file are correct for your data. To create your own customised defaults file, edit the distributed version and save it with a meaningful name such as [hmri_b1_local_defaults_*myprotocol*.m].

      - **3D AFI**    
        3D Actual Flip Angle Imaging (AFI) protocol.
        As B1 input, please select a TR2/TR1 pair of magnitude images.
        Regarding processing parameters, you can either stick with metadata and standard defaults parameters (recommended) or select your own [hmri_b1_local_defaults_*.m] customised defaults file (fallback for situations where no metadata are available).

        - **B1 input**    
          Select B1 input images according to the type of B1 bias correction.

        - **Processing parameters**    
          You can either stick with metadata and standard defaults parameters (recommended) or select your own customised defaults file (fallback for situations where no metadata are available).

          - **Use metadata or standard defaults**    
            

          - **Customised B1 defaults file**    
            Select the [hmri_b1_local_defaults_*.m] file containing the parameters to process the B1 map data. By default, parameters will be collected from metadata when available. Defaults parameters are provided as fallback solution when metadata are not available and/or uncomplete.
            Please make sure that the parameters defined in the defaults file are correct for your data. To create your own customised defaults file, edit the distributed version and save it with a meaningful name such as [hmri_b1_local_defaults_*myprotocol*.m].

      - **tfl_b1_map**    
        Input B1 images for TFL B1 map protocol.
        As B1 input, please select the pair of anatomical and precalculated B1 map, in that order.

        - **B1 input**    
          Select B1 input images according to the type of B1 bias correction.

      - **rf_map**    
        Input B1 images for rf_map B1 map protocol.
        As B1 input, please select the pair of anatomical and precalculated B1 map, in that order.

        - **B1 input**    
          Select B1 input images according to the type of B1 bias correction.

      - **pre-processed B1**    
        Input pre-calculated B1 bias map.
        Please select one unprocessed magnitude image from the B1map data set (for coregistration with the multiparameter maps) and the preprocessed B1map (in percent units), in that order.

        - **B1 input**    
          Select B1 input images according to the type of B1 bias correction.

      - **UNICORT**    
        UNICORT will be applied for B1 bias correction.
        No B1 input data required.
        Customized processing parameters may be introduced by loading customized defaults from a selected [hmri_b1_local_defaults_*.m] file.

        - **Processing parameters**    
          You can either stick with metadata and standard defaults parameters (recommended) or select your own customised defaults file (fallback for situations where no metadata are available).

          - **Use metadata or standard defaults**    
            

          - **Customised B1 defaults file**    
            Select the [hmri_b1_local_defaults_*.m] file containing the parameters to process the B1 map data. By default, parameters will be collected from metadata when available. Defaults parameters are provided as fallback solution when metadata are not available and/or uncomplete.
            Please make sure that the parameters defined in the defaults file are correct for your data. To create your own customised defaults file, edit the distributed version and save it with a meaningful name such as [hmri_b1_local_defaults_*myprotocol*.m].

      - **no B1 correction**    
        No B1 bias correction will be applied.
        NOTE: when no B1 map is available, UNICORT might be a better solution than no B1 bias correction at all.

    - **Multiparameter input images**    
      Input all the MT/PD/T1-weighted images.

      - **MT images**    
        Input MT-weighted images.

      - **PD images**    
        Input PD-weighted images.

      - **T1 images**    
        Input T1-weighted images.


## Process hMRI maps

  Parameter maps are spatially processed and brought into standard spacefor furhter statistical analysis.
  This include 3 main processing steps:
  - Unified Segmentation (US) - produces individual tissue maps+ warping into MNI space
  - DARTEL - improves the warping into a common space of multiplesubjects, based on their tissue maps
  - Smoothing - tissue specific weighted averaging
   
  For simplicity, 2 standard pipelines are also set up:
  - US+Smooth - applies US, warps into MNI, then smoothes (weighted-average)
  US+Dartel+Smooth - applies US, builds Dartel template and warpsinto MNI, then smoothes (weighted-average)

- **Proc. hMRI -> Pipelines**    
  Parameter maps are spatially processed and brought into standard spacefor furhter statistical analysis.
   
  For simplicity, 2 standard pipelines are also set up:
  - US+Smooth - applies US, warps into MNI, then smoothes (weighted-average)
  US+Dartel+Smooth - applies US, builds Dartel template and warpsinto MNI, then smoothes (weighted-average)

  - **Structural images (T1w or MT) for segmentation**    
    Select structural images, i.e. T1w or MT, for "unified segmentation". They are used to create the individuam tissue class maps, e.g. GM and WM posterior probability maps

  - **Parametric maps**    
    Select whole brain parameter maps (e.g. MT, R2\*, FA, etc.) from all subjects for processing, one type at a time.

    - **Parametric maps (single type)**    
      Select whole brain parameter maps (e.g. MT, R2\*, FA, etc.) from all subjects for processing.

  - **Gaussian FWHM**    
    Specify the full-width at half maximum (FWHM) of the Gaussian blurring kernel in mm. Three values should be entereddenoting the FWHM in the x, y and z directions.

  - **Pipeline**    
    Chose the predefined pipeline that you prefer:
    - US+Smooth - applies US, warps into MNI, then smoothes (weighted-average)
    - US+Dartel+Smooth - applies US, builds Dartel template and warps intoMNI, then smoothes (weighted-average)

- **Proc. hMRI -> Individual modules**    
  Parameter maps are spatially processed and brought into standard spacefor furhter statistical analysis.
  This include 3 main processing steps:
  - Unified Segmentation (US) - produces individual tissue maps+ warping into MNI space
  - DARTEL - improves the warping into a common space of multiplesubjects, based on their tissue maps
  - Smoothing - tissue specific weighted averaging

  - **Proc. hMRI -> Segmentation**    
    Segmentation, bias correction and spatially normalisation - all in the same model.
    
    This procedure is an extension of the old unified segmentation algorithm (and was known as "New Segment" in SPM8). The algorithm is essentially the same as that described in the Unified Segmentation paper /* \cite{ashburner05}*/, except for (i) a slightly different treatment of the mixing proportions, (ii) the use of an improved registration model, (iii) the ability to use multi-spectral data, (iv) an extended set of tissue probability maps, which allows a different treatment of voxels outside the brain. Some of the options in the toolbox do not yet work, and it has not yet been seamlessly integrated into the SPM8 software.  Also, the extended tissue probability maps need further refinement. The current versions were crudely generated (by JA) using data that was kindly provided by Cynthia Jongen of the Imaging Sciences Institute at Utrecht, NL.
    
    This function segments, bias corrects and spatially normalises - all in the same model/* \cite{ashburner05}*/. Many investigators use tools within older versions of SPM for a technique that has become known as "optimised" voxel-based morphometry (VBM). VBM performs region-wise volumetric comparisons among populations of subjects. It requires the images to be spatially normalised, segmented into different tissue classes, and smoothed, prior to performing statistical tests/* \cite{wright_vbm,am_vbmreview,ashburner00b,john_should}*/. The "optimised" pre-processing strategy involved spatially normalising subjects" brain images to a standard space, by matching grey matter in these images, to a grey matter reference.  The historical motivation behind this approach was to reduce the confounding effects of non-brain (e.g. scalp) structural variability on the registration. Tissue classification in older versions of SPM required the images to be registered with tissue probability maps. After registration, these maps represented the prior probability of different tissue classes being found at each location in an image.  Bayes rule can then be used to combine these priors with tissue type probabilities derived from voxel intensities, to provide the posterior probability.
    
    This procedure was inherently circular, because the registration required an initial tissue classification, and the tissue classification requires an initial registration.  This circularity is resolved here by combining both components into a single generative model. This model also includes parameters that account for image intensity non-uniformity. Estimating the model parameters (for a maximum a posteriori solution) involves alternating among classification, bias correction and registration steps. This approach provides better results than simple serial applications of each component.

    - **Data & options**    
      Specify images for many subjects

      - **Output choice**    
        Output directory can be the same as the input directory for each input file or user selected

        - **Input directory**    
          Output files will be written to the same folder as each corresponding input file.

        - **Output directory**    
          Select a directory where output files will be written to.

      - **Parametric maps**    
        Select whole brain parameter maps (e.g. MT, R2\*, FA, etc.) from all subjects for processing, one type at a time.

        - **Parametric maps**    
          Select whole brain parameter maps (e.g. MT, R2\*, FA, etc.) from all subjects for processing.

      - **Ref. structurals**    
        Specify a channel for processing.
        If multiple channels are used (eg PD & T2), then the same order of subjects must be specified for each channel and they must be in register (same position, size, voxel dims etc..). The different channels can be treated differently in terms of inhomogeneity correction etc. You may wish to correct some channels and save the corrected images, whereas you may wish not to do this for other channels.

        - **Structural images (T1w or MT) for segmentation**    
          Select structural images, i.e. T1w or MT, for "unified segmentation". They are used to create the individuam tissue class maps, e.g. GM and WM posterior probability maps

        - **Bias regularisation**    
          MR images are usually corrupted by a smooth, spatially varying artifact that modulates the intensity of the image (bias). These artifacts, although not usually a problem for visual inspection, can impede automated processing of the images.
          
          An important issue relates to the distinction between intensity variations that arise because of bias artifact due to the physics of MR scanning, and those that arise due to different tissue properties.  The objective is to model the latter by different tissue classes, while modelling the former with a bias field. We know a priori that intensity variations due to MR physics tend to be spatially smooth, whereas those due to different tissue types tend to contain more high frequency information. A more accurate estimate of a bias field can be obtained by including prior knowledge about the distribution of the fields likely to be encountered by the correction algorithm. For example, if it is known that there is little or no intensity non-uniformity, then it would be wise to penalise large values for the intensity non-uniformity parameters. This regularisation can be placed within a Bayesian context, whereby the penalty incurred is the negative logarithm of a prior probability for any particular pattern of non-uniformity.
          Knowing what works best should be a matter of empirical exploration.  For example, if your data has very little intensity non-uniformity artifact, then the bias regularisation should be increased.  This effectively tells the algorithm that there is very little bias in your data, so it does not try to model it.

        - **Bias FWHM**    
          FWHM of Gaussian smoothness of bias.
          If your intensity non-uniformity is very smooth, then choose a large FWHM. This will prevent the algorithm from trying to model out intensity variation due to different tissue types. The model for intensity non-uniformity is one of i.i.d. Gaussian noise that has been smoothed by some amount, before taking the exponential. Note also that smoother bias fields need fewer parameters to describe them. This means that the algorithm is faster for smoother intensity non-uniformities.

        - **Save Bias Corrected**    
          Option to save a bias corrected version of your images from this channel, or/and the estimated bias field.
          MR images are usually corrupted by a smooth, spatially varying artifact that modulates the intensity of the image (bias). These artifacts, although not usually a problem for visual inspection, can impede automated processing of the images.  The bias corrected version should have more uniform intensities within the different types of tissues.

    - **Tissues**    
      The data for each subject are classified into a number of different tissue types.  The tissue types are defined according to tissue probability maps, which define the prior probability of finding a tissue type at a particular location. Typically, the order of tissues is grey matter, white matter, CSF, bone, soft tissue and air/background (if using tpm/TPM.nii).

      - **Tissue**    
        A number of options are available for each of the tissues.  You may wish to save images of some tissues, but not others. If planning to use Dartel, then make sure you generate "imported"" tissue class images of grey and white matter (and possibly others).  Different numbers of Gaussians may be needed to model the intensity distributions of the various tissues.

        - **Tissue probability map**    
          Select the tissue probability image for this class.
          These should be maps of eg grey matter, white matter or cerebro-spinal fluid probability. A nonlinear deformation field is estimated that best overlays the tissue probability maps on the individual subjects" image.
          
          Rather than assuming stationary prior probabilities based upon mixing proportions, additional information is used, based on other subjects" brain images.  Priors are usually generated by registering a large number of subjects together, assigning voxels to different tissue types and averaging tissue classes over subjects. Three tissue classes are used: grey matter, white matter and cerebro-spinal fluid. A fourth class is also used, which is simply one minus the sum of the first three. These maps give the prior probability of any voxel in a registered image being of any of the tissue classes - irrespective of its intensity.
          
          The model is refined further by allowing the tissue probability maps to be deformed according to a set of estimated parameters. This allows spatial normalisation and segmentation to be combined into the same model.

        - **Num. Gaussians**    
          The number of Gaussians used to represent the intensity distribution for each tissue class can be greater than one.
          In other words, a tissue probability map may be shared by several clusters. The assumption of a single Gaussian distribution for each class does not hold for a number of reasons. In particular, a voxel may not be purely of one tissue type, and instead contain signal from a number of different tissues (partial volume effects). Some partial volume voxels could fall at the interface between different classes, or they may fall in the middle of structures such as the thalamus, which may be considered as being either grey or white matter. Various other image segmentation approaches use additional clusters to model such partial volume effects. These generally assume that a pure tissue class has a Gaussian intensity distribution, whereas intensity distributions for partial volume voxels are broader, falling between the intensities of the pure classes. Unlike these partial volume segmentation approaches, the model adopted here simply assumes that the intensity distribution of each class may not be Gaussian, and assigns belonging probabilities according to these non-Gaussian distributions. Typical numbers of Gaussians could be two for grey matter, two for white matter, two for CSF, three for bone, four for other soft tissues and two for air (background).
          Note that if any of the Num. Gaussians is set to non-parametric, then a non-parametric approach will be used to model the tissue intensities. This may work for some images (eg CT), but not others - and it has not been optimised for multi-channel data. Note that it is likely to be especially problematic for images with poorly behaved intensity histograms due to aliasing effects that arise from having discrete values on the images.

        - **Native Tissue**    
          The native space option allows you to produce a tissue class image (c*) that is in alignment with the original/* (see Figure \ref{seg1})*/. It can also be used for "importing"" into a form that can be used with the Dartel toolbox (rc*).

        - **Warped Tissue**    
          You can produce spatially normalised versions of the tissue class - both with (mwc*) and without (wc*) modulation (see below). These can be used for voxel-based morphometry. All you need to do is smooth them and do the stats.
          
          "Modulation"" is to compensate for the effect of spatial normalisation.  When warping a series of images to match a template, it is inevitable that volumetric differences will be introduced into the warped images.  For example, if one subject"s temporal lobe has half the volume of that of the template, then its volume will be doubled during spatial normalisation. This will also result in a doubling of the voxels labelled grey matter.  In order to remove this confound, the spatially normalised grey matter (or other tissue class) is adjusted by multiplying by its relative volume before and after warping.  If warping results in a region doubling its volume, then the correction will halve the intensity of the tissue label. This whole procedure has the effect of preserving the total amount of grey matter signal in the normalised partitions.  Actually, in this version of SPM the warped data are not scaled by the Jacobian determinants when generating the "modulated" data.  Instead, the original voxels are projected into their new location in the warped images.  This exactly preserves the tissue count, but has the effect of introducing aliasing artifacts - especially if the original data are at a lower resolution than the warped images.  Smoothing should reduce this artifact though.
          Note also that the "unmodulated" data are generated slightly differently in this version of SPM. In this version, the projected data are corrected using a kind of smoothing procedure. This is not done exactly as it should be done (to save computational time), but it does a reasonable job. It also has the effect of extrapolating the warped tissue class images beyond the range of the original data.  This extrapolation is not perfect, as it is only an estimate, but it may still be a good thing to do.

    - **Warping & MRF**    
      A number of warping options.
      The main one that you could consider changing is the one for specifying whether deformation fields or inverse deformation fields should be generated.

      - **MRF Parameter**    
        When tissue class images are written out, a few iterations of a simple Markov Random Field (MRF) cleanup procedure are run.  This parameter controls the strength of the MRF. Setting the value to zero will disable the cleanup.

      - **Clean Up**    
        This uses a crude routine for extracting the brain from segmented images.
        It begins by taking the white matter, and eroding it a couple of times to get rid of any odd voxels.  The algorithm continues on to do conditional dilations for several iterations, where the condition is based upon gray or white matter being present.This identified region is then used to clean up the grey and white matter partitions.  Note that the fluid class will also be cleaned, such that aqueous and vitreous humour in the eyeballs, as well as other assorted fluid regions (except CSF) will be removed.
        
        If you find pieces of brain being chopped out in your data, then you may wish to disable or tone down the cleanup procedure. Note that the procedure uses a number of assumptions about what each tissue class refers to.  If a different set of tissue priors are used, then this routine should be disabled.

      - **Warping Regularisation**    
        Registration involves simultaneously minimising two terms.  One of these is a measure of similarity between the images (mean-squared difference in the current situation), whereas the other is a measure of the roughness of the deformations.  This measure of roughness involves the sum of the following terms:
        * Absolute displacements need to be penalised by a tiny amount.  The first element encodes the amount of penalty on these.  Ideally, absolute displacements should not be penalised, but it is necessary for technical reasons.
        * The `membrane energy" of the deformation is penalised (2nd element), usually by a relatively small amount. This penalises the sum of squares of the derivatives of the velocity field (ie the sum of squares of the elements of the Jacobian tensors).
        * The `bending energy" is penalised (3rd element). This penalises the sum of squares of the 2nd derivatives of the velocity.
        * Linear elasticity regularisation is also included (4th and 5th elements).  The first parameter (mu) is similar to that for linear elasticity, except it penalises the sum of squares of the Jacobian tensors after they have been made symmetric (by averaging with the transpose).  This term essentially penalises length changes, without penalising rotations.
        * The final term also relates to linear elasticity, and is the weight that denotes how much to penalise changes to the divergence of the velocities (lambda).  This divergence is a measure of the rate of volumetric expansion or contraction.
        The amount of regularisation determines the tradeoff between the terms. More regularisation gives smoother deformations, where the smoothness measure is determined by the bending energy of the deformations.

      - **Affine Regularisation**    
        The procedure is a local optimisation, so it needs reasonable initial starting estimates. Images should be placed in approximate alignment using the Display function of SPM before beginning. A Mutual Information affine registration with the tissue probability maps (D"Agostino et al, 2004) is used to achieve approximate alignment. Note that this step does not include any model for intensity non-uniformity. This means that if the procedure is to be initialised with the affine registration, then the data should not be too corrupted with this artifact.If there is a lot of intensity non-uniformity, then manually position your image in order to achieve closer starting estimates, and turn off the affine registration.
        
        Affine registration into a standard space can be made more robust by regularisation (penalising excessive stretching or shrinking).  The best solutions can be obtained by knowing the approximate amount of stretching that is needed (e.g. ICBM templates are slightly bigger than typical brains, so greater zooms are likely to be needed). For example, if registering to an image in ICBM/MNI space, then choose this option.  If registering to a template that is close in size, then select the appropriate option for this.

      - **Smoothness**    
        This is used to derive a fudge factor to account for correlations between neighbouring voxels.
        Smoother data have more spatial correlations, rendering the assumptions of the model inaccurate.
        For PET or SPECT, set this value to about 5 mm, or more if the images have smoother noise.  For MRI, you can usually use a value of 0 mm.

      - **Sampling distance**    
        This encodes the approximate distance between sampled points when estimating the model parameters.
        Smaller values use more of the data, but the procedure is slower and needs more memory. Determining the "best"" setting involves a compromise between speed and accuracy.

      - **Deformation Fields**    
        Deformation fields can be saved to disk, and used by the Deformations Utility.
        For spatially normalising images to MNI space, you will need the forward deformation, whereas for spatially normalising (eg) GIFTI surface files, you"ll need the inverse. It is also possible to transform data in MNI space on to the individual subject, which also requires the inverse transform. Deformations are saved as .nii files, which contain three volumes to encode the x, y and z coordinates.

  - **Proc. hMRI -> Dartel**    
    This toolbox is based around the "A Fast Diffeomorphic Registration Algorithm"" paper/* \cite{ashburner07} */. The idea is to register images by computing a "flow field"", which can then be "exponentiated"" to generate both forward and backward deformations. Currently, the software only works with images that have isotropic voxels, identical dimensions and which are in approximate alignment with each other. One of the reasons for this is that the approach assumes circulant boundary conditions, which makes modelling global rotations impossible. Another reason why the images should be approximately aligned is because there are interactions among the transformations that are minimised by beginning with images that are already almost in register. This problem could be alleviated by a time varying flow field, but this is currently computationally impractical.
    Because of these limitations, images should first be imported. This involves taking the "*_seg_sn.mat"" files produced by the segmentation code of SPM12, and writing out rigidly transformed versions of the tissue class images, such that they are in as close alignment as possible with the tissue probability maps. Rigidly transformed original images can also be generated, with the option to have skull-stripped versions.
    The next step is the registration itself.  This can involve matching single images together, or it can involve the simultaneous registration of e.g. GM with GM, WM with WM and 1-(GM+WM) with 1-(GM+WM) (when needed, the 1-(GM+WM) class is generated implicitly, so there is no need to include this class yourself). This procedure begins by creating a mean of all the images, which is used as an initial template. Deformations from this template to each of the individual images are computed, and the template is then re-generated by applying the inverses of the deformations to the images and averaging. This procedure is repeated a number of times.Finally, warped versions of the images (or other images that are in alignment with them) can be generated. 
    
    This toolbox is not yet seamlessly integrated into the SPM package. Eventually, the plan is to use many of the ideas here as the default strategy for spatial normalisation. The toolbox may change with future updates.  There will also be a number of other (as yet unspecified) extensions, which may include a variable velocity version (related to LDDMM). Note that the Fast Diffeomorphism paper only describes a sum of squares objective function. The multinomial objective function is an extension, based on a more appropriate model for aligning binary data to a template.

    - **Run Dartel (create Templates)**    
      Run the Dartel nonlinear image registration procedure. This involves iteratively matching all the selected images to a template generated from their own mean. A series of Template*.nii files are generated, which become increasingly crisp as the registration proceeds.

      - **Images**    
        Select the images to be warped together. Multiple sets of images can be simultaneously registered. For example, the first set may be a bunch of grey matter images, and the second set may be the white matter images of the same subjects.

        - **Images**    
          Select a set of imported images of the same type to be registered by minimising a measure of difference from the template.

      - **Settings**    
        Various settings for the optimisation. The default values should work reasonably well for aligning tissue class images together.

        - **Template basename**    
          Enter the base for the template name.  Templates generated at each outer iteration of the procedure will be basename_1.nii, basename_2.nii etc.  If empty, then no template will be saved. Similarly, the estimated flow-fields will have the basename appended to them.

        - **Regularisation Form**    
          The registration is penalised by some "energy"" term.  Here, the form of this energy term is specified. Three different forms of regularisation can currently be used.

        - **Outer Iterations**    
          The images are averaged, and each individual image is warped to match this average.  This is repeated a number of times.

          - **Outer Iteration**    
            Different parameters can be specified for each outer iteration. Each of them warps the images to the template, and then regenerates the template from the average of the warped images. Multiple outer iterations should be used for more accurate results, beginning with a more coarse registration (more regularisation) then ending with the more detailed registration (less regularisation).

            - **Inner Iterations**    
              The number of Gauss-Newton iterations to be done within this outer iteration. After this, new average(s) are created, which the individual images are warped to match.

            - **Reg params**    
              For linear elasticity, the parameters are mu, lambda and id. For membrane energy, the parameters are lambda, unused and id.id is a term for penalising absolute displacements, and should therefore be small.  For bending energy, the parameters are lambda, id1 and id2, and the regularisation is by (-lambda*Laplacian + id1)^2 + id2.
              Use more regularisation for the early iterations so that the deformations are smooth, and then use less for the later ones so that the details can be better matched.

            - **Time Steps**    
              The number of time points used for solving the partial differential equations.  A single time point would be equivalent to a small deformation model. Smaller values allow faster computations, but are less accurate in terms of inverse consistency and may result in the one-to-one mapping breaking down.  Earlier iteration could use fewer time points, but later ones should use about 64 (or fewer if the deformations are very smooth).

            - **Smoothing Parameter**    
              A LogOdds parameterisation of the template is smoothed using a multi-grid scheme.  The amount of smoothing is determined by this parameter.

        - **Optimisation Settings**    
          Settings for the optimisation.  If you are unsure about them, then leave them at the default values.  Optimisation is by repeating a number of Levenberg-Marquardt iterations, in which the equations are solved using a full multi-grid (FMG) scheme. FMG and Levenberg-Marquardt are both described in Numerical Recipes (2nd edition).

          - **LM Regularisation**    
            Levenberg-Marquardt regularisation.  Larger values increase the the stability of the optimisation, but slow it down.  A value of zero results in a Gauss-Newton strategy, but this is not recommended as it may result in instabilities in the FMG.

          - **Cycles**    
            Number of cycles used by the full multi-grid matrix solver. More cycles result in higher accuracy, but slow down the algorithm. See Numerical Recipes for more information on multi-grid methods.

          - **Iterations**    
            Number of relaxation iterations performed in each multi-grid cycle. More iterations are needed if using "bending energy"" regularisation, because the relaxation scheme only runs very slowly. See the chapter on solving partial differential equations in Numerical Recipes for more information about relaxation methods.

    - **Run Dartel (existing Templates)**    
      Run the Dartel nonlinear image registration procedure to match individual images to pre-existing template data. Start out with smooth templates, and select crisp templates for the later iterations.

      - **Images**    
        Select the images to be warped together. Multiple sets of images can be simultaneously registered. For example, the first set may be a bunch of grey matter images, and the second set may be the white matter images of the same subjects.

        - **Images**    
          Select a set of imported images of the same type to be registered by minimising a measure of difference from the template.

      - **Settings**    
        Various settings for the optimisation. The default values should work reasonably well for aligning tissue class images together.

        - **Regularisation Form**    
          The registration is penalised by some "energy"" term.  Here, the form of this energy term is specified. Three different forms of regularisation can currently be used.

        - **Outer Iterations**    
          The images are warped to match a sequence of templates. Early iterations should ideally use smoother templates and more regularisation than later iterations.

          - **Outer Iteration**    
            Different parameters and templates can be specified for each outer iteration.

            - **Inner Iterations**    
              The number of Gauss-Newton iterations to be done within this outer iteration.

            - **Reg params**    
              For linear elasticity, the parameters are mu, lambda and id. For membrane energy, the parameters are lambda, unused and id.id is a term for penalising absolute displacements, and should therefore be small.  For bending energy, the parameters are lambda, id1 and id2, and the regularisation is by (-lambda*Laplacian + id1)^2 + id2.
              Use more regularisation for the early iterations so that the deformations are smooth, and then use less for the later ones so that the details can be better matched.

            - **Time Steps**    
              The number of time points used for solving the partial differential equations.  A single time point would be equivalent to a small deformation model. Smaller values allow faster computations, but are less accurate in terms of inverse consistency and may result in the one-to-one mapping breaking down.  Earlier iteration could use fewer time points, but later ones should use about 64 (or fewer if the deformations are very smooth).

            - **Template**    
              Select template. Smoother templates should be used for the early iterations. Note that the template should be a 4D file, with the 4th dimension equal to the number of sets of images.

        - **Optimisation Settings**    
          Settings for the optimisation.  If you are unsure about them, then leave them at the default values.  Optimisation is by repeating a number of Levenberg-Marquardt iterations, in which the equations are solved using a full multi-grid (FMG) scheme. FMG and Levenberg-Marquardt are both described in Numerical Recipes (2nd edition).

          - **LM Regularisation**    
            Levenberg-Marquardt regularisation.  Larger values increase the the stability of the optimisation, but slow it down.  A value of zero results in a Gauss-Newton strategy, but this is not recommended as it may result in instabilities in the FMG.

          - **Cycles**    
            Number of cycles used by the full multi-grid matrix solver. More cycles result in higher accuracy, but slow down the algorithm. See Numerical Recipes for more information on multi-grid methods.

          - **Iterations**    
            Number of relaxation iterations performed in each multi-grid cycle. More iterations are needed if using "bending energy"" regularisation, because the relaxation scheme only runs very slowly. See the chapter on solving partial differential equations in Numerical Recipes for more information about relaxation methods.

    - **Normalise to MNI Space**    
      Normally, Dartel generates warped images that align with the average-shaped template. This routine includes an initial affine regisration of the template (the final one generated by Dartel), with the TPM data released with SPM.
      "Smoothed"" (blurred) spatially normalised images are generated in such a way that the original signal is preserved. Normalised images are generated by a "pushing"" rather than a "pulling"" (the usual) procedure. Note that a procedure related to trilinear interpolation is used, and no masking is done.  It is therefore recommended that the images are realigned and resliced before they are spatially normalised, in order to benefit from motion correction using higher order interpolation.  Alternatively, contrast images generated from unsmoothed native-space fMRI/PET data can be spatially normalised for a 2nd level analysis.
      Two "preserve"" options are provided.  One of them should do the equavalent of generating smoothed "modulated"" spatially normalised images.  The other does the equivalent of smoothing the modulated normalised fMRI/PET, and dividing by the smoothed Jacobian determinants.

      - **Dartel Template**    
        Select the final Template file generated by Dartel. This will be affine registered with a TPM file, such that the resulting spatially normalised images are closer aligned to MNI space. Leave empty if you do not wish to incorporate a transform to MNI space (ie just click "done" on the file selector, without selecting any images).

      - **Data**    

        - **Segemented tissue class**    
          Select the tissue class (TC) imagesof interest from all subjects. This should typically be the c1* and c2* images for GM and WM.

          - **c* images**    
            Select the tissue classes images (c*)

        - **Parameter maps**    
          Select whole brain parameter maps (e.g. MT, R2\*, FA etc).

          - **Volumes**    
            Select whole brain parameter maps (e.g. MT, R2\*, FA etc).

        - **Flow fields**    
          Flow fields.

      - **Voxel sizes**    
        Specify the voxel sizes of the deformation field to be produced. Non-finite values will default to the voxel sizes of the template imagethat was originally used to estimate the deformation.

      - **Bounding box**    
        Specify the bounding box of the deformation field to be produced. Non-finite values will default to the bounding box of the template imagethat was originally used to estimate the deformation.

  - **Proc. hMRI -> Smoothing**    
    Applying tissue specific smoothing, aka. weighted averaging, 
    in order to limit partial volume effect.

    - **Warped parameter maps**    
      Select whole brain parameter maps (e.g. MT, R2\*, FA etc) warped into MNI space.

      - **Volumes**    
        Select whole brain parameter maps (e.g. MT, R2\*, FA etc) warped into MNI space.

    - **Modulated warped tissue class**    
      Select the modulated warped tissue classes (TC) of interest from all subjects. This ould typically be the mwc1* and mwc2* images for GM and WM.

      - **mwTC images**    
        Select the modulated warped tissue classes (mwc*)

    - **Tissue probability maps**    
      Select the TPM used for the segmentation.

    - **Gaussian FWHM**    
      Specify the full-width at half maximum (FWHM) of the Gaussian blurring kernel in mm. Three values should be entered denoting the FWHM in the x, y and z directions.

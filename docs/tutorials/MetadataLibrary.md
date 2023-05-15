###### [Back to hMRI home page](Home)

# JSON metadata and DICOM to NIfTI conversion 

*Responsible developer: Evelyne Balteau*      
*Developed as part of the hMRI toolbox, including the following developers: Martina Callaghan, Tobias Leutritz, Antoine Lutti, Siawoosh Mohammadi, Enrico Reimer, Karsten Tabelow, Nikolaus Weiskopf.*

## Content    

[Introduction](#introduction)     
[DICOM headers – ```spm_dicom_header```](#dicom-headers--spm_dicom_header)     
[DICOM to NIfTI conversion – ```spm_dicom_convert```](#dicom-to-nifti-conversion--spm_dicom_convert)     
[DICOM to NIfTI conversion – SPM Batch](#dicom-to-nifti-conversion--spm-batch)     
[The metadata structure](#the-metadata-structure)     
[How to retrieve specific parameters from the JSON metadata](#how-to-retrieve-specific-parameters-from-the-json-metadata)     
 
## Introduction

To support the flexibility of the hMRI-Toolbox processing pipeline and the traceability of the data at every 
step, number of parameters from acquisition to statistical results must be retrieved and stored. 

Therefore, DICOM import and JSON metadata handling functionalities have been implemented with the following
objectives:

1. retrieving the full DICOM header as a readable and searchable Matlab structure (metadata),
2. during DICOM to NIfTI conversion, storing the metadata structure as separate JSON file 
(alongside the NIfTI image file) and/or as extended header into the NIfTI image file,
3. handling metadata in order to set, get and search parameters.

With the current implementation, these objectives are achieved: the hMRI-toolbox handles and stores data acquisition and processing parameters as JSON-encoded metadata alongside brain imaging data sets. Outside the hMRI-toolbox, metadata can prove useful for many purposes. For quality control, metadata can help checking for protocol consistency by comparing essential parameters across sessions, subjects and sites. When processing data acquired with several protocols on several scanners from different vendors (multicentric studies), metadata provide the information to automatically extract acquisition parameters to be used as regressors of no interest in a statistical analysis. Metadata also facilitate the sorting of the data between different data types (e.g. functional MRI, structural MRI, diffusion images, etc.) together with retrieving BIDS essential parameters (e.g. session and series numbers), therefore providing all the information required to make the data archiving and storage fully BIDS-compatible ([Gorgolewski *et al.* 2016](References)). 

A **BIDS extension proposal (BEP)** is currently under development, that will define the structure and parameters necessary to describe ***structural acquisitions with multiple contrasts*** and more generally the multiparameter mapping protocols. With such BIDS extension, the hMRI-toolbox will be adapted and made fully BIDS-compliant, including the structure of the JSON-stored metadata and the structure of the output data. The required adaptation will be achieved smoothly thanks to the metadata handling functionalities, without affecting the processing part of the hMRI-toolbox.

In the next sections, detailed syntax and many examples using the `metadata library` are provided. Good housekeeping suggestions are provided to make use of the metadata structure to track acquisition and processing steps, including processing parameters, tools and software versions.

The current implementation rests on the SPM12 implementation of DICOM tools (```spm_dicom_header(s)```, ```spm_dicom_essentials```, ```spm_dicom_convert```) and relies on ```spm_jsonread``` and ```spm_jsonwrite``` for handling JSON-encoded metadata. 

From **SPM12 version r7219** (released 16 November 2017) and in later versions, functionalities described in points 1. and 2. above (excluding the extended header option though) have been integrated to the DICOM import module in SPM12 (implementation from the hMRI toolbox).

## DICOM headers – ```spm_dicom_header``` 

The following scripts are used, unchanged from the SPM12 implementation: ```spm_dicom_header```, ```spm_dicom_headers``` and ```spm_dicom_essentials```.

## DICOM to NIfTI conversion – ```spm_dicom_convert```

An additional input argument has been added to ```spm_dicom_convert``` to deal with the new JSON metadata options. If JSON metadata storage is enabled, DICOM header information is stored as a JSON-formatted structure either as a separate JSON file or as extended NIfTI header (or both). If omitted or disabled, the DICOM to NIfTI conversion proceeds using exactly the same implementation as before. The output NIfTI images, extended or not, have a valid NIfTI-1 format compatible with standard NIfTI readers (**MRICron**, **SPM**, **FreeSurfer**, **MRtrix**). Note that in FSL version 5.0.9 and older, the offset defining the size of the NIfTI header (extended or not) is not read, and default (352 bytes) offset is assumed. As a result, NIfTI files containing extended headers are misread in FSL 5.0.9, and separate JSON metadata files are recommended if you plan to process your data with that version. With **FSL 5.0.10**, extended NIfTI files are read properly :).

### Usage and examples  - [hMRI toolbox]

WARNING: The usage and examples given below relate to the hMRI implementation of ```spm_dicom_convert```. For usage and examples for the current SPM12 implementation (SPM12.4 - r7219), either refer to the SPM12 documentation, or to the usage and examples given a bit further [below](#usage-and-examples---spm124---r7219). The two implementations should merge at some point but are kept side-by-side for now.

USAGE: 
```matlab
spm_dicom_convert(hdr, opts, root_dir, format, out_dir, json) 
Inputs are as in SPM12 except for the last json input: 
    json - a structure with fields: 
        extended -> metadata stored as extended nii header included in the nii image [default: false] 
        separate -> metadata stored in separate json file [default: false]  
        anonym -> 'basic' [default], 'full', 'none' (see anonymise_metadata.m)
```

EXAMPLES: 
```matlab
files2convert = spm_select(Inf,'^*.IMA'); 
outpth = spm_select(1,'dir','Select output directory'); 
hdr = spm_dicom_headers(files2convert); 
% for conversion output identical to previous SPM12 versions (no 
% metadata stored during conversion): 
files_converted = spm_dicom_convert(hdr,'all','flat','nii',outpth); 
% for conversion with creation of a separate JSON metadata file: 
json = struct('extended',false,'separate',true); 
files_converted = spm_dicom_convert(hdr,'all','flat','nii',outpth,json); 
% for conversion with insertion of the JSON metadata as NIfTI 
% extended header: 
json = struct('extended',true,'separate',false); 
files_converted = spm_dicom_convert(hdr,'all','flat','nii',outpth,json); 
% metadata can be stored simultaneously as a separate JSON file and 
% extended NIfTI header: 
json = struct('extended',true,'separate',true); 
files_converted = spm_dicom_convert(hdr,'all','flat','nii',outpth,json); 
```

###### [Back to top](#json-metadata-and-dicom-to-nifti-conversion)

### Usage and examples - [SPM12.4 - r7219]

The SPM12 implementation is limited to the usage of a separate JSON file to store the metadata. The extra argument in ```spm_dicom_convert``` is therefore a simple binary flag enabling (```meta = true```) or disabling (```meta = false```) the storage of the metadata. 

USAGE:     
Type ```help spm_dicom_convert``` for a detailed description of every parameter.     
```matlab
spm_dicom_convert(hdr, opts, root_dir, format, out_dir, meta) 
    meta - save metadata as sidecar JSON file [default: false]
```

EXAMPLES: 
```matlab
files2convert = spm_select(Inf,'^*.IMA'); 
outpth = spm_select(1,'dir','Select output directory'); 
hdr = spm_dicom_headers(files2convert); 
% for conversion output identical to previous SPM12 versions (no 
% metadata stored during conversion): 
files_converted = spm_dicom_convert(hdr,'all','flat','nii',outpth); 
% for conversion with creation of a separate JSON metadata file: 
meta = true;
files_converted = spm_dicom_convert(hdr,'all','flat','nii',outpth,meta); 
```

###### [Back to top](#json-metadata-and-dicom-to-nifti-conversion)

## DICOM to NIfTI conversion – SPM Batch

The above modifications have been reflected in the SPM Batch GUI (Batch > SPM > Tools > DICOM Import, or directly from the DICOM Import button) by adding the following option: 

**JSON metadata format**      

Metadata (acquisition parameters, processing steps, …) can be stored in JSON format. The metadata can be stored as a separate JSON file and/or as an extended header (included in the image file, for nii output format only).     

One of the following options must be selected:     

```matlab
    - None 
    - Separate JSON file 
    - Extended nii header 
    - Both 
```

###### [Back to top](#json-metadata-and-dicom-to-nifti-conversion)

## The metadata structure

**FORMAT OF THE METADATA STRUCTURE**

- Metadata are stored as simple Matlab structures – hence quite flexible and modular.      
- The Matlab structure is converted into JSON (a text format transcription of the Matlab structure) to be stored in the NIfTI header or as a separate JSON file.     
- The JSON transcription is easily readable and searchable by opening the NIfTI or JSON file with a text editor.    
- JSON string <> Matlab structure conversion easily done using ```spm_jsonread/write```.   
- NIfTI files with extended headers can be read without any trouble with SPM and MRIcron (existing bug with FSL though).

**CONTENT AND STRUCTURE OF THE METADATA**

The structure of the metadata is divided into the following two main fields: 

- **Acquisition parameters** (```hdr.acqpar```): this field contains the original DICOM header. It is created during DICOM to NIfTI conversion and may be dropped if the processed image relies on several input images (e.g. when creating a map from mtflash and b1mapping acquisitions). Note that the patient name, date of birth and DICOM file name (often containing the patient name) fields do not appear in the metadata, while patient age (in years), sex, size and weight are kept. Confidentiality is not guaranteed though, since confidential data may have been encoded in other fields. 

- **History** (```hdr.history```): is a nested structure containing    

  - ```procstep```: a structure describing the current processing step and containing    

    - ```descrip```: a brief description of the processing applied (e.g. DICOM to NIfTI conversion, realign, create map, …)    
    - ```version```: version number of the processing applied (traceability!)     
    - ```procpar```: processing parameters (relevant parameters used to process the data – e.g. for fieldmap-based EPI undistortion: EPI readout duration, TEs of the field mapping acquisition, ...). Basically all the parameters included into the matlab batch "job" except for the input images. 

  - ```input```: an array listing the input images used for the processing. Each input (hdr.history.input(i)) is a structure containing

    - ```filename```: the filename of the input image     
    - ```history```: the history structure from the input image     

  - ```output```: a structure containing 

    - ```imtype```: image type of the current output (e.g. R1, FA, MT, ADC, …)     
    - ```units```: either physical units (sec, 1/sec, …), standardized units (e.g. percent units = p.u.) or arbitrary units (a.u.)… 

###### [Back to top](#json-metadata-and-dicom-to-nifti-conversion)

## Metadata handling

The JSON transcription of the Matlab structure is easily readable and searchable by opening the extended NIfTI or separate JSON file with a text editor. Furthermore, metadata set, get and search tools have been implemented. These tools have been added into the ```spm12/metadata```directory. Type ```help <function>``` in Matlab for detailed syntax and help. 

Here is the list of most useful scripts and their summary descriptions:

- [```get_metadata```]: returns the Matlab structure described above.
- [```set_metadata```]: to write (insert, modify, overwrite) metadata in JSON files and extended NIfTI headers. In general, metadata are initialized during the DICOM to NIfTI conversion and further modified as the processing progresses. See also [```init_output_metadata_structure```].
- [```init_output_metadata_structure```]: to create a metadata structure to be used for newly created output images.
- [```get_metadata_val```]: returns pairs of structure fields and values agreeing with search criteria.
- [```find_field_name```]: recursively searches a Matlab structure for a specific field name according to search criteria. *Can be applied to any Matlab structure - not only the hMRI metadata!*

*Note that other scripts in ```spm12/metadata``` are private scripts, called from other functions withing the metadata & dicom import library and are not expected to be used otherwise.*

[```get_metadata```]: ../blob/master/spm12/metadata/get_metadata.m
[```set_metadata```]: ../blob/master/spm12/metadata/set_metadata.m
[```init_output_metadata_structure```]: ../blob/master/spm12/metadata/init_output_metadata_structure.m
[```get_metadata_val```]: ../blob/master/spm12/metadata/get_metadata_val.m
[```find_field_name```]: ../blob/master/spm12/metadata/find_field_name.m

**EXAMPLE 1**: modify existing metadata     
*NB: a very basic example (NIfTI file with extended header, no JSON file associated), not the most common usage of ```set_metadata```, but should help getting familiar with handling the metadata...*
```matlab 
% select nii file with extended header 
niifile = spm_select(1,'^*.nii'); 
% read the extended header 
hdr = get_metadata(niifile);
% define json format (since working with NIfTI file, we can choose 
% to limit set_metadata to updating the extended header without 
% caring about updating the JSON file associated). 
json = struct('extended',true,'separate',false,'overwrite',true);
% modify the header of the existing nii file 
modif_hdr = hdr{1}; 
modif_hdr.history.procstep.descrip = 'original file with modified header'; 
modif_hdr.history.procstep.version = 'v0.1.2'; 
modif_hdr.history.procstep.procpar = []; 
modif_hdr.history.input(1).filename = niifile; 
modif_hdr.history.input(1).history = hdr{1}.history; 
modif_hdr.history.output.imtype = hdr{1}.history.output.imtype; 
modif_hdr.history.output.units = hdr{1}.history.output.units; 
% write the modified metadata 
set_metadata(niifile, modif_hdr, json);
```

**EXAMPLE 2**: save a newly computed R1 map with extended header      
*NB: for the example, we simply read an existing NIfTI volume and copy it into another NIfTI file to which we add an extended header.* 
```matlab
% Select nifti file to copy 
P = spm_select(1,'^*.nii'); 
V = spm_vol(P); 
Y = spm_read_vols(V);
% initialize new nifti object 
N = nifti; 
% fill up the fields and data section of the nifti object as usual... 
N.dat  = V(1).private.dat; 
N.dat  = file_array('R1map.nii',V.dim,V.dt,0,V.pinfo(1),V.pinfo(2)); 
N.mat  = V.mat; 
N.mat0 = V.mat; 
N.mat_intent  = 'Scanner'; 
N.mat0_intent = 'Scanner'; 
N.descrip = 'test creating a new NIfTI file with extended JSON header'; 
create(N); 
% write the data N.dat(:,:,:) = Y;    
% create a new structure for the extended header 
outhdr = struct('history', struct('procstep',[],'input',[],'output',[])); 
% NB: since it is no longer an original image from the scanner, the field 'acqpar' is dropped. Input 
% files can be retrieved with their full acquisition parameters from the history.input field. 
outhdr.history.procstep.descrip = 'map creation'; 
outhdr.history.procstep.version = hmri_get_version; 
% insert job parameters here: 
outhdr.history.procstep.procpar = struct('par1',[1 2],'par2','test'); 
outhdr.history.input{1}.filename = V.fname; 
inhdr = get_metadata(V.fname); 
if ~isempty(inhdr)    
    outhdr.history.input{1}.history = inhdr{1}.history; 
else
    outhdr.history.input{1}.history = 'No history available'; 
end 
outhdr.history.output.imtype = 'R1 map';
outhdr.history.output.units = '1/sec'; 
% Write the metadata as both extended NIfTI header and separate JSON file 
% NB: if no json structure provided, set_metadata will check for existing 
% JSON and NIfTI files and update the existing ones. 
json = struct('extended',true,'separate',true,'overwrite',true); 
set_metadata('R1map.nii', outhdr, json);
```

For an easier setting of the metadata, it is recommended to use ```init_output_metadata_structure```:

```matlab
input_files = {V.fname};
proc.descrip = 'map creation';
proc.version = hmri_get_version;
proc.params = struct('par1',[1 2],'par2','test');
output.imtype = 'R1 map';
output.units = '1/sec';
outhdr = init_output_metadata_structure(input_files, proc, output);
json = struct('extended',true,'separate',true,'overwrite',true); 
set_metadata('R1map.nii', outhdr, json);
```

###### [Back to top](#json-metadata-and-dicom-to-nifti-conversion)

## How to retrieve specific parameters from the JSON metadata 

This section deals mainly with the ```acqpar``` subfield of the JSON metadata, i.e. the parameters retrieved from the DICOM header, but the tools implemented to search the metadata structure can be used more generally to search any Matlab structure.

Since metadata are stored as a Matlab structure, searching for a specific parameter is often equivalent to searching for the corresponding field name. For that reason, the function ```find_field_name``` has been implemented to recursively search for a specific field name (exact or partial match, case sensitive or not). The ```get_metadata_val``` script makes use of ```find_field_name``` to sort out the parameters and return the desired value. More refined versions of ```get_metadata_val``` will be developed with time to deal with hardware/software/sequences variations. However the front end of ```get_metadata_val``` should stay unchanged for backwards compatibility. 

Below are listed a series of common parameters and how they can be retrieved from the JSON metadata. If these examples don't work with your own version of MRI hardware, software and sequences, please report it as an [issue][reporting-issues] on the [hMRI-Toolbox] repository. Other arbitrary parameters can easily be searched using the same ```get_metadata_val``` script. See the examples at the end of this section. 

- Retrieve the whole header (type ```help get_metadata``` for details on the syntax and input files)     
  ```matlab 
  hdr = get_metadata('my_extended_nifti_file.nii'); 
  ```    
- Excitation flip angle [deg]     
  ```matlab
  fa = get_metadata_val(hdr{1},'FlipAngle');
  ```    
- Repetition time(s) [ms] 
  ```matlab
  TR = get_metadata_val(hdr{1},'RepetitionTime'); % the repetition time of the image
  TR = get_metadata_val(hdr{1},'RepetitionTimes'); % an array of TRs if multi-TR sequence (Siemens-specific)
  ```
- Echo time(s) [ms]
  ```matlab
  TE = get_metadata_val(hdr{1},'EchoTime'); % the echo time of the image
  TE = get_metadata_val(hdr{1},'EchoTimes'); % an array of TEs if multi-echo sequence (Siemens-specific)
  ```
- Magnetization transfer (MT) pulse [1/0 according to pulse switched on/off]
  ```matlab
  MT = get_metadata_val(hdr{1},'MT'); 
  ```
- Field strength B0 [T] 
  ```matlab
  B0 = get_metadata_val(hdr{1},'FieldStrength'); 
  ```
- Center frequency [Hz] 
  ```matlab
  freq = get_metadata_val(hdr{1},'Frequency');
  ```
- Protocol Name 
  ```matlab
  ProtName = get_metadata_val(hdr{1},'ProtocolName'); 
  ```
- Sequence Name 
  ```matlab
  SeqName = get_metadata_val(hdr{1},'SequenceName'); 
  ```
- RF spoiling phase increment (warning: sequence dependent, see comments in the code) 
  ```matlab
  RFSpoilIncr = get_metadata_val(hdr{1},'RFSpoilingPhaseIncrement'); 
  ```
- Bandwidth per pixel (in the RO direction) in Hz/Px 
  ```matlab
  BWPPRO = get_metadata_val(hdr{1},'BandwidthPerPixelRO'); 
  ```
- Bandwidth per pixel (in the PE direction) in Hz/Px 
  ```matlab
  BWPPPE = get_metadata_val(hdr{1},'BandwidthPerPixelPE'); 
  ```
- PAT acceleration (structure containing all parameters) 
  ```matlab
  PAT = get_metadata_val(hdr{1},'PATparameters'); 
  ```
- PAT acceleration factor in PE direction (in-plane) 
  ```matlab
  RPE = get_metadata_val(hdr{1},'AccelFactorPE'); 
  ```
- PAT acceleration factor in 3D direction (through-slice direction for 3D acquisitions) 
  ```matlab
  R3D = get_metadata_val(hdr{1},'AccelFactor3D'); 
  ```
- All WIP parameters (Siemens ASCII field: returns a structure with two fields alFree and adFree containing arrays of long and double values respectively) 
  ```matlab
  WIP = get_metadata_val(hdr{1},'WipParameters'); 
  ```
- EPI parameters (including distortion correction)

  - Short and long echo times from dual-echo field mapping: 
    ```matlab
    TEshort = get_metadata_val(hdr_gre_field_mapping_magn{1},'EchoTime'); 
    TElong = get_metadata_val(hdr_gre_field_mapping_magn{2},'EchoTime'); 
    ```    
  - EPI phase encoding direction (ROW/COL) 
    ```matlab
    PEDir = get_metadata_val(hdr_epi{1},'PhaseEncodingDirection'); 
    ```
  - EPI phase encoding direction positive (e.g. A>>P is positive (1), P>>A is negative (-1)) 
    ```matlab
    PEDirPos =get_metadata_val(hdr_epi{1},'PhaseEncodingDirectionSign');
    ```
  - Total EPI readout duration (warning: tricky for home-made sequences with uncompleted headers) 
    ```matlab
    epiROdur = get_metadata_val(hdr{1},'epiReadoutDuration');
    ```
  - EPI echo spacing (warning: tricky for home-made sequences with uncompleted headers) 
    ```matlab
    epiEchoSpacing = get_metadata_val(hdr{1},'EchoSpacing'); 
    ```

- Parameters for processing B1 maps (al_b1mapping) 

  - Nominal FA values (betas) 
    ```matlab
    beta = get_metadata_val(hdr{1},'B1mapNominalFAValues');
    ```
  - Mixing time (TM) 
    ```matlab
    TM = get_metadata_val(hdr{1},'B1mapMixingTime'); 
    ```

- DWI parameters

  This is specific to Siemens DICOM metadata. In that case, the diffusion parameters can be retrieved following two distinct ways.

  1. Reading all the directions and b-values at once from CSASeriesHeaderInfo subfield      
    *NB: the current implementation assumes that Free diffusion mode is used, with a userdefined set of diffusion directions.*

      - Diffusion directions: list of directions as defined in the *.dvs file (normalised or not) 
        ```matlab
        DiffDir = get_metadata_val(hdr{1},'AllDiffusionDirections'); 
        ```
      - b-values: list of b-values as defined in the Diff tab of the UI 
        ```matlab
        bVal = get_metadata_val(hdr{1},'AllBValues'); 
        ```

  2. Reading the individual directions and b-values for each image from CSAImageHeaderInfo subfield       
    *NB: this implementation is expected to work more generally. However, the directions are converted according to the volume orientation and possibly with reference to different axes. Use with care...*

      - Diffusion direction: normalised vector 
        ```matlab
        DiffDir = get_metadata_val(hdr{1},'DiffusionDirection'); 
        ```
      - b-values: corresponding b-value 
        ```matlab
        bVal = get_metadata_val(hdr{1},'BValue'); 
        ```

### General examples searching for arbitrary parameters 

The recursive search of the field names of the Matlab structure follows simple pattern matching rules. Any pattern, partial or full, case sensitive or not, can be used. One can either use the ```get_metadata_val``` script with any desired pattern (see EXAMPLES 1 & 2 below), or use the ```find_field_name``` script with more specific matching rules (see EXAMPLES 3 & 4). In the former case, the search is not case sensitive and will retrieve any field that contains the pattern. In the latter case, one can specify whether to be case sensitive or not, and whether to allow partial matches or not.

In EXAMPLES 1 & 2, we search for any field containing “diff” because we want to identify potential diffusion parameters. The search is performed on a DWI image and on a t1-mprage image.

**EXAMPLE 1** – search in an (extended) NIfTI file containing a t1-mprage image: 
```matlab
[parVal, parFieldNam] = get_metadata_val('t1mprage.nii','diff'); 
```
The output is: 
```
parVal = 
    ulMode: 1 
parFieldNam =
    acqpar.CSASeriesHeaderInfo.MrPhoenixProtocol.sDiffusion
```
A single field name is found containing “diff” and its value is a structure, with a single field ulMode and value 1. 

**EXAMPLE 2** – search an (extended) NIfTI file containing a DWI image: 
```matlab
[parVal, parFieldNam] = get_metadata_val('dwi.nii','diff'); 
```
The output is: 
```
parVal = {    [3x1 double],    
              'DIRECTIONAL',    
              'GAAXkVNRAAD\/\/\/…',       
              [1x1 struct],    
              [2],    
              [72],
              [1x1 struct],
              [72],    
              [71x1 struct] } 

parFieldNam = {  acqpar.CSAImageHeaderInfo.DiffusionGradientDirection  
                 acqpar.CSAImageHeaderInfo.DiffusionDirectionality  
                 acqpar.CSAImageHeaderInfo.MRDiffusion  
                 acqpar.CSASeriesHeaderInfo.MrPhoenixProtocol.sDiffusion  
                 acqpar.CSASeriesHeaderInfo.MrPhoenixProtocol.sDiffusion.lDiffWeightings  
                 acqpar.CSASeriesHeaderInfo.MrPhoenixProtocol.sDiffusion.lDiffDirections  
                 acqpar.CSASeriesHeaderInfo.MrPhoenixProtocol.sDiffusion.sFreeDiffusionData  
                 acqpar.CSASeriesHeaderInfo.MrPhoenixProtocol.sDiffusion.sFreeDiffusionData.lDiffDirections  
                 acqpar.CSASeriesHeaderInfo.MrPhoenixProtocol.sDiffusion.sFreeDiffusionData.asDiffDirVector } 
```

In the following examples, we search the same DWI metadata as above using ```find_field_name```. Type ```help find_field_name``` in Matlab to know more about ```find_field_name```'s syntax. 

**EXAMPLE 3** – search for “Diff” (case sensitive and partial match): `
```matlab
hdr = get_metadata('dwi.nii'); 
[nFieldFound, fieldList] = find_field_name(hdr{1},'Diff','caseSens', 'sensitive','matchType','partial'); 
```
The output is: 
```
nFieldFound = 
    9 
fieldList =     
'acqpar'  'CSAImageHeaderInfo'   'DiffusionGradient…'             []                    []                 []    
'acqpar'  'CSAImageHeaderInfo'   'DiffusionDirectio…'             []                    []                 []    
'acqpar'  'CSAImageHeaderInfo'   'MRDiffusion'                    []                    []                 []    
'acqpar'  'CSASeriesHeaderInfo'  'MrPhoenixProtocol'    'sDiffusion'                    []                 []    
'acqpar'  'CSASeriesHeaderInfo'  'MrPhoenixProtocol'    'sDiffusion'  'lDiffWeightings'                    []    
'acqpar'  'CSASeriesHeaderInfo'  'MrPhoenixProtocol'    'sDiffusion'  'lDiffDirections'                    []    
'acqpar'  'CSASeriesHeaderInfo'  'MrPhoenixProtocol'    'sDiffusion'  'sFreeDiffusionData'                 []    
'acqpar'  'CSASeriesHeaderInfo'  'MrPhoenixProtocol'    'sDiffusion'  'sFreeDiffusionData'  'lDiffDirections'    
'acqpar'  'CSASeriesHeaderInfo'  'MrPhoenixProtocol'    'sDiffusion'  'sFreeDiffusionData'  'asDiffDirVector'
```

**EXAMPLE 4** – search for “sfreediffusiondata” (case insensitive and exact match):
```matlab
hdr = get_metadata('dwi.nii'); 
[nFieldFound, fieldList] = find_field_name(hdr{1},'sfreediffusiondata','caseSens', 'insensitive','matchType','exact'); 
```
The output is: 
```
nFieldFound =     
    1 
fieldList =     
'acqpar'    'CSASeriesHeaderInfo'    'MrPhoenixProtocol'    'sDiffusion'    'sFreeDiffusionData'
```

###### [Back to top](#json-metadata-and-dicom-to-nifti-conversion)

[hMRI-Toolbox]: ../
[reporting-issues]: ../issues
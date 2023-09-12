###### [Back to hMRI home page](Home)

# Spatial processing for quantitative maps

## Content
    
[INTRODUCTION](#introduction)    
[SEGMENTATION](#segmentation)    
[DIFFEOMORPHIC DEFORMATION](#diffeomorphic-deformation)    
[TISSUE-WEIGHTED SMOOTHING](#tissue-weighted-smoothing)    
[INTEGRATED PIPELINE](#integrated-pipeline)     
[BATCH OPTIONS](#batch-options)      
[EXAMPLE](#example)  

## Introduction

The spatial processing implementation of the Toolbox relies on the voxel-based quantification (VBQ) approach developed by [Draganski et al., 2011](references.md#draganski-et-al-2011) to preserve the quantitative nature of qMRI maps during processing. The spatial processing part of the toolbox can be applied to any set of rotationally-invariant qMRI maps, including diffusion MRI scalar parameter maps as well as the standard (R1, PD, MT and R2\*) MPM maps.

The spatial processing pipeline for quantitative data relies on three main operational steps: 
[segmentation](#segmentation), [diffeomorphic deformation](#diffeomorphic-deformation) and [tissue-weighted smoothing](#tissue-weighted-smoothing). 
They are organized in separate sub-modules. Except for the latter, these 
operations rely on SPM12 functionalities tailored to the specific requirements of qMRI. 
Furthermore a standard [fully integrated processing pipeline](#integrated-pipeline) is provided as a separate sub-module
to facilitate standard data processing without the need to combine the individual modules.

The processing tools and user interface were developed with in mind the user-friendly and easy processing of 
parametric maps generated from a series of subjects, assuming each subject has the same small number of images (for example the four (MT, PD, R1, R2\*) maps). 
Therefore each module takes as input several series of images sorted per image type considered (for example the MT maps from all the subjects). The images in each series must follow the same subject-based ordering. In practice one such series of images can easily be selected, manually or through a script, by using the spm_select function features that allow the recursive search of files in all sub-folders and name-filtering according to a specific pattern.

The three main steps and the integrated pipeline are described hereafter.

## Segmentation

This step relies on the latest Unified Segmentation (US) algorithm as available 
in SPM12 ([Ashburner2005](References)) and is interfaced through a single module. 
Unlike the *SPM12-Segment* module (single-subject processing), *hMRI-Segment* module can handle and process successively a whole series of subjects. 

The segmentation itself relies on one structural image per subject, typically a MT map, R1 map or T1w image, and a series of tissue probability maps (TPMs). 
The TPMs used by default in the hMRI Toolbox were specifically derived 
from multi-parametric maps ([Lorio2014](References)). 
By default (adjustable in the batch interface) the gray matter (GM), 
white matter (WM) and CSF tissue class images will be generated in native space (DARTEL imported version) and standard MNI space, with and without modulation. 
Moreover, the parametric maps are also warped into MNI space (without modulation and using a more basic non-linear registration than in the following section). Notably, in analogy to SPM processing, these 'simply normalized' qMRI maps might be analyzed without generating sample specific templates (next section) after smoothing. However, to generally improve spatial normalization results, the usage of DARTEL is recommended. 

## Diffeomorphic deformation

This step includes three sub-modules derived from the DARTEL toolbox ([Ashburner2007](references.md#ashburner-et-al-2007)). 
The key idea of DARTEL is to iteratively align the tissue class images, e.g. the gray and 
white matter, from a series of subjects to their own average by successively generating average shaped images with increasing  overlap and detail (called "Templates"). 
This only depends on the DARTEL-imported tissue class images from each subject.

The three sub-modules included are:      
- *Run DARTEL, create Templates*: This module warps together tissue class images from the series of subjects. A set of of Templates (population average tissue class images, with increasing sharpness) as well as, for each subject, the deformation field to the final template are generated.    
- *Run DARTEL, existing Templates*: If Templates already exist, this module can be used to estimate only the deformation fields for another set of subjects (relying on the same tissue class images as those used to generate the Templates).
- *Normalize to MNI*: This module applies the estimated deformation field to the parametric and tissue class images and brings them all in alignment with MNI space through an additional affine transformation. Contrary to the DARTEL toolbox, no smoothing is applied on the normalized images. A specific smoothing operation is applied at the next \textit{Tissue-weighted smoothing} step. 

The first two modules are identical to those in the DARTEL toolbox and are also made available in the hMRI toolbox for convenience. The *Normalize to MNI* module is also based on the equivalent DARTEL toolbox module but was extended to efficiently warp the few parametric maps of each subject.

## Tissue-weighted smoothing

As proposed in [Draganski *et al.* 2011](References), the parametric maps should be smoothed while accounting for the partial volume contribution of the tissue density in each voxel in subject space. One *tissue-weighted smoothed* parameter map is generated per tissue class considered. These images can then be statistically analyzed, per parameter map and tissue class type.

## Integrated pipeline

The standard spatial processing pipeline of parameter maps would combine four modules (*Segmentation*, *Run DARTEL, create Templates*, *Normalize to MNI* and finally *Tissue-weighted smoothing*) into a single batch. The options of each module can be adjusted and the images passed from one module to the next via virtual dependencies, as available in the batch-GUI, forming a complete *processing pipeline*. A simpler (but less spatially accurate) approach would skip the diffeomorphotic warping steps and only combine the *Segmentation* and *Tissue-weighted smoothing* modules.

To help novice user, an *Integrated pipeline* module directly implements the whole procedure with minimal input from the user. The module only requires a few input:     
- the series of structural reference images and parametric maps (one per subject),
- the width of the *Tissue-weighted smoothing* kernel desired,
- the type of processing pipeline, i.e. with or without diffeomorphic registration.

All the other options from each of the intermediate steps are set by defaults (e.g. only GM and WM tissue 
classes are of interest and the TPMs are those from the hMRI toolbox).

## Batch options

*Work in progress...*

## Example

*Work in progress...*
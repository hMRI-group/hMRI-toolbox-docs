---
title: Automatic Reorientation
---

The reorientation of the images towards the MNI space is a standard step in neuroimage processing.
It increases the consistency in individual head positions prior to normalisation, and often is a prerequisite for
successful segmentation.
The Unified Segmentation ([Ashburner2005](references.md#ashburner2005)) process is indeed rather sensitive to the initial
orientation of the image. Auto-reorient provides you with a simple tool for reorientation of all images prior to any
further processing including multiparameter map calculation.

Reorientation is based on rigid-body coregistration of a reference image to the MNI space (i.e. mainly set the AC
location and correct for head rotation) and application of the coregistration matrix to all images acquired during the
same session (specified as "other images"). The code makes use of spm affreg and templates available in SPM12.

The user must provide one reference image and a template for coregistration, and a series of other images that need to
keep in alignment with the reference image (typically, all the images acquired during a given imaging session). The
reference image is the one that will be coregistered to the template. It must have as good a signal-to-noise ratio (SNR)
and a white matter/grey matter contrast-to-noise ratio (CNR) as possible to ensure a robust and reliable coregistration.
The template can be choosen e.g. amongst SPM canonical templates (from the SPM/canonical directory) or any other
userdefined template (e.g. atypical population-driven template), provided it is already oriented according to MNI. It is
mandatory that the contrast of the reference image is close to the template image.

The following list is provided as guidelines:

```
SOURCE IMAGE > RECOMMENDED TEMPLATE    
First T1w echo > SPM/canonical/avg152T1.nii    
First PDw echo > SPM/canonical/avg152PD.nii    
First MTw echo > SPM/canonical/avg152PD.nii     
```

If no specific output directory is selected, then the original orientation of the images will be overwritten. If an
output directory is selected then the images are copied to the ouput directory in a AutoReorient folder before being
reoriented, therefore leaving the original data untouched.

The reoriented images can be collected as dependencies for further processing, either all at once (Dependencies >
Grouped) or as individual output images (Dependencies > Individual) which is more convenient to connect the
Auto-reorient module to the next Create hMRI maps module.

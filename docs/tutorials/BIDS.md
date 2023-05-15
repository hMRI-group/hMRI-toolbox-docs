###### [Back to hMRI home page](Home)

# BIDS compatibility
The [Brain Imaging Data Structure (BIDS)](https://bids-specification.readthedocs.io/en/latest/) is a set of standards for representing brain imaging data.
The [quantitative MRI (qMRI) BIDS extension](https://bids-specification.readthedocs.io/en/latest/appendices/qmri.html) describes how datasets should be arranged and which metadata must be provided in order for qMRI maps to be calculated from the data. 
The hMRI toolbox predates the qMRI BIDS standards, and so the implementation of full BIDS compatibility is still under development.
However, the toolbox can already read nearly all of the necessary metadata from BIDS sidecar files (exceptions are listed [below](#limitations)), and there are [Matlab tools](#tools) which can filter BIDS-arranged data to provide them as input to the toolbox and can move and rename the outputs to arrange them in a BIDS-compatible way.

## Tools 
The following tools can be used to parse BIDS datasets to feed them into the hMRI toolbox, and then move and rename the hMRI toolbox outputs into a BIDS-compatible structure: 
- [bids-matlab](https://github.com/bids-standard/bids-matlab/)
- [bids-processing-tools](https://github.com/nbeliy/bids-processing-tools)
- [hmri-bids-pipeline](https://github.com/nbeliy/hmri-bids-pipeline)

## Limitations
- Output filenames are not qMRI-BIDS compatible; this means scripting must be used to rename the outputs (see [Tools](#tools)).
- For reasons of maintaining backwards compatibility with old 3D-EPI SE/STE B1-mapping datasets where these parameters were not set in a BIDS-compatible way, reading the `EchoTime` and `FlipAngle` metadata from 3D-EPI SE/STE B1-mapping datasets must be enabled by the user; see [here](DefaultsAndCustomization#b1-mapping-processing-parameters) or [this example B1-defaults file](../blob/master/examples/hmri_b1_local_defaults_bids.m) for how to do this.

## BIDS-ified example dataset
A conversion of the [hMRI toolbox example dataset](https://doi.org/10.1016/j.dib.2019.104132) to a BIDS-compatible format can be found as `ds-mpm` [here](https://doi.org/10.17605/OSF.IO/K4BS5).
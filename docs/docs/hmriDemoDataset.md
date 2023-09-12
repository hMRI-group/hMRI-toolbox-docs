# hMRI Toolbox Demo Dataset

The hMRI demo dataset is used and referred to across the whole toolbox, for tutorial and illustration purposes. A
description of the data, acquisition parameters and experiment is provided below and in the Data in Brief
article: [Callaghan2019](references.md#callaghan2019).

## Download

!!! note

    The dataset and protocol examples (detailed list of sequences and acquisition parameters) are currently
    available from the [hMRI Toolbox](http://hmri.info) home page, section "hMRI Example Data and MRI Protocols".

- Demo dataset ([ZIP archive][download-link]).
- Corresponding protocol `hmri_sample_dataset_protocol_800um_64ch.pdf` ([PDF printout][download-link])
- Other MPM protocol examples, including Siemens and Philips scanners.

## Description of the Experiment

An example multiparameter mapping (MPM) dataset was acquired on a whole body 3T Prisma system (Siemens Healthcare,
Erlangen, Germany) to illustrate the hMRI Toolbox features and provide the users with a full MPM dataset used in the
tutorial and examples (see for example the [step-by-step tutorial for map creation](mapCreation#example) and
the [toolbox customization examples](defaultsAndCustomization#examples)). The study was approved by the local ethics
committee and informed written consent was obtained from the participant prior to scanning.

Rapid calibration data were acquired at the outset of each session to map and consequently correct for inhomogeneities
in the B1 transmit field ([Lutti2010](references.md#lutti2010), [Lutti2012](references.md#lutti2012)).
Eleven spin-echo and stimulated-echo pairs were acquired with the nominal flip angles varying from 115° to 65° in 5° decrements.

These were followed by acquisition of 3D multi-echo RF-spoiled gradient echo acquisitions
with predominantly PD, T1 or MT weighting similar to the MPM protocol described in
[Weiskopf2013](references.md#weiskopf2013). The flip angle was 6° for the PDw and MTw images and
21° for the T1w images. MT-weighting was achieved through the application of
a Gaussian RF pulse 2 kHz off-resonance with 4 ms duration and a nominal flip angle of 220°.
The data were acquired with whole-brain coverage at an isotropic resolution of 800 µm using a
FoV of 256 mm head-foot, 224 mm anterior-posterior (AP) and 179 mm right-left (RL). Gradient
echoes were acquired with alternating readout gradient polarity at eight equidistant echo times
ranging from 2.30 to 18.40 ms in steps of 2.30 ms using a readout bandwidth of 488Hz/pixel.
Only six echoes were acquired for the MTw acquisition in order to maintain a
TR of 25 ms for all RF-spoiled gradient echo volumes. To accelerate the data acquisition, partially parallel imaging
using
the GRAPPA algorithm was employed in each phase-encoded direction (AP and RL) with forty reference
lines and a speed-up factor of two. The acquisition time for each RF-spoiled gradient echo volume was 7'08''.
The data were acquired using the body coil for signal transmission and a 64 channel head and
neck array for signal reception. Each dataset was acquired with a 30° rotation of the sagittal plane
so that any eye-related motion artifact propagated to the neck/inferior cerebellum rather than the cortex.

Prior to the acquisition of each RF-spoiled gradient echo volume, two additional 3D rapid, unaccelerated, low
resolution (8 mm isotropic, same FoV) volumes were acquired one with the 64 channel head and neck array coil
and the other with the body coil. A single echo, with a TE of 2.20 ms, was
acquired in each case using a 6° flip angle and a TR of 6.00 ms. The acquisition
time for each of these calibration volumes was 5.90 s.

These data were used to correct for the position-specific sensitivity modulation of the
array coil by approximately removing the receive field sensitivity bias.
To test the performance of this correction, the participant performed a yaw
rotation (i.e. about the z-axis) within the confines of the 64 channel coil prior
to the acquisition of the MT-weighted dataset before returning to the original position
centered within the head coil for the PD-weighted and T1-weighted acquisitions.

## Acquisition Parameters

The ZIP archive contains the following data:

- `mfc_seste_b1map_v1e_0004`: B1 mapping data (3D EPI SE/SESTE protocol)
- `gre_field_mapping_1acq_rl_0005`: B0 mapping data (2 magnitude images with different TE)
- `gre_field_mapping_1acq_rl_0006`: B0 mapping data (presubtracted phase image)
- `mfc_smaps_v1a_Array_0007`: RF sensitivity mapping data (64 channel head coil)
- `mfc_smaps_v1a_QBC_0008`: RF sensitivity mapping data (body coil)
- `pdw_mfc_3dflash_v1i_R4_0009`: 8 echoes with PD-weighting

Large yaw rotation of the volunteer's head (i.e. about the z-axis) within the confines of the 64 channel coil.

- `mfc_smaps_v1a_Array_0010`: RF sensitivity mapping data (64 channel head coil)
- `mfc_smaps_v1a_QBC_0011`: RF sensitivity mapping data (body coil)
- `mtw_mfc_3dflash_v1i_R4_0012`: 6 echoes with MT-weighting

Back to initial (straight) position.

- `mfc_smaps_v1a_Array_0013`: RF sensitivity mapping data (64 channel head coil)
- `mfc_smaps_v1a_QBC_0014`: RF sensitivity mapping data (body coil)
- `t1w_mfc_3dflash_v1i_R4_0015`: 8 echoes with T1-weighting

The full protocol is available and can be downloaded as a [PDF printout][download-link].

[download-link]: https://owncloud.gwdg.de/index.php/s/iv2TOQwGy4FGDDZ
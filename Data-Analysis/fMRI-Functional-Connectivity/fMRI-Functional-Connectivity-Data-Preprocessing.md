PREAMBLE.

This wiki is prepared by Josh Goh (2021/08/26) based on the excellent materials in [Yi-Chuan Lin's quick step-by-step SOP for functional connectivity](https://docs.google.com/document/d/1YLlCRCp3QKHgIMoRkhyInhMnq42tQFPBooxj4y4rKIU/edit#heading=h.8sya4qho1b74). You should refer there if you already have basic understanding and want to skip to the bare essential steps using [SPM](https://en.wikibooks.org/wiki/SPM). This wiki goes through functional connectivity analysis in more detail for the purpose of semi-layman understanding. For deeper technical understanding, which is not the purpose here, refer to other sources for MR physics or image processing formalism. Still, reading through this present comprehensive document is not trivial and is estimated to take at least 60 min with no pauses. If you are working through this document the first time, the assumption is that you are either using your lab account on the servers with a remote desktop instance or you have Matlab, [SPM12](https://en.wikibooks.org/wiki/SPM), and all other necessary tools (e.g. dcm2nii, etc.) installed on your local system.

Most of the matlab scripts quoted here can also be found in the repo /[BML Data Analysis Tools/BML SPM](https://gitlab.com/brain-and-mind-lab/bml-data-analysis-tools/bml-spm).

# Table of Contents

1. [Purpose & Issue](#1-purpose-issue-difference-between-functional-connectivity-and-functional-activity-analysis)
2. [Preprocessing functional connectivity data](#2-preprocessing-functional-connectivity-data)
   1. [Principles](#21-principles-to-remove-artifacts-and-yield-residual-images)
   2. [SOP](#22-sop-for-preprocessing-using-spm)
      1. [Prepare image data](#221-prepare-image-data)
         1. [Convert DICOM to NIFTI](#2211-convert-dicom-to-nifti)
         2. [Reorient](#2212-reorient-images)
         3. [Coregister T1 to EPI](#2213-coregister-t1-to-epi)
      2. [T1 spatial normalization using DARTEL to create a SST](#222-t1-spatial-normalization-using-dartel-to-create-a-sst)
         1. [Segment T1](#2221-segment-t1)
         2. [Make SST](#2222-make-sst)
         3. [Spatially normalize SST to MNI space](#2223-spatially-normalize-sst-to-mni-space)
      3. [EPI preprocessing](#223-epi-preprocessing)
         1. [Optionals](#2231-optionals)
            1. [EPI Inhomogeneity correction](#22311-epi-inhomogeneity-correction)
            2. [Apply gradient field map](#22312-apply-gradient-field-map)
         2. [Motion correction](#2232-motion-correction)
         3. [Slice-time correction](#2233-slice-time-correction)
         4. [1st-level modeling to generate residual images](#2234-1st-level-modeling-to-generate-residual-images)
         5. [Temporal filtering of residual images](#2235-temporal-filtering-of-residual-images)

## **1. Purpose & Issue: Difference between functional connectivity and functional activity analysis.**

In fMRI functional connectivity analysis, the key goal is to evaluate the degree of association between a source MRI functional time-series (X) and a target time-series (Y). This primary goal is what distinguishes functional _connectivity_ analysis from functional _activity_ analysis. In functional activity analysis, the response of interest is the magnitude of the signal in each voxel, which is theoretically primarily treated as independent from the signals of other voxels in different spatial locations, albeit this is not the case in practice. In functional connectivity, the response of interest is the covariation of signals between voxels. In this case, the goal is to evaluate signals that are non-independent between voxels at different spatial locations.

![Screen_Shot_2021-08-26_at_3](uploads/3a95fbb37ebfec42e4fe2fbae7f24868/Screen_Shot_2021-08-26_at_3.53.20_PM.png)

Because of this, the preprocessing of functional imaging volumes for functional _connectivity_ analysis seeks to:

1. Keep all signal adjustment and derivation of final parameter estimates from the signal in the native space of the subject's brain image.
2. Minimize the introduction of inter-voxel associations due to post-acquisition adjustments (e.g. spatial smoothing, spatial normalization interpolations, etc.).

Violations of the above two concerns might apply extra covariation in the signals between voxels, which would artificially inflate the non-independence between voxels at different spatial locations. Note, by contrast, inflation of local inter-voxel signal (but not noise) covariance is mostly beneficial in functional _activity_ analysis since this would increase the likelihood of detecting consistent activity in a given spatial location.

![Screen_Shot_2021-08-26_at_5](uploads/b54b2d45af046fc3eff45e52250faac9/Screen_Shot_2021-08-26_at_5.16.26_PM.png)

[Return to TOC](#table-of-contents)

## **2. Preprocessing functional connectivity data**

### **2.1. Principles to remove artifacts and yield residual images.**

The principle of preprocessing fMRI data for functional connectivity analysis is to remove the following artifacts in the imaging data:

1. Head motion.
2. Physiological signals (e.g. heartbeat, respiration).
3. Scanner-related signals (e.g. signal drifts).
4. Unwanted task-related neural responses.

The general preprocessing schematic is shown below. The steps to adjust functional volumes borrow from standard preprocessing of echo-planar imaging (EPI) functional images with some modifications. The same general preprocessing is applied to both resting-state and task-based fMRI data to yield residual images for functional connectivity analysis. The key difference is that with resting-state fMRI, task-related regressors are not used in the 1st-level model.

![Screen_Shot_2021-08-26_at_6](uploads/2a2989cb0bb8dcc6761a693f73cb4bc9/Screen_Shot_2021-08-26_at_6.23.14_PM.png)

[Return to TOC](#table-of-contents)

### 2.2. SOP for Preprocessing Using SPM

Click [\[here\]](https://docs.google.com/document/d/1YLlCRCp3QKHgIMoRkhyInhMnq42tQFPBooxj4y4rKIU/edit#heading=h.8sya4qho1b74) to skip to the quick step-by-step coverage in a nutshell in Yi-Chuang's SOP Google Doc on functional connectivity analysis. Below, we go through each step in slightly deeper detail for understanding.

[Return to TOC](#table-of-contents)

### 2.2.1. Prepare Image Data

Ensure that your brain image data directory are in the [Brain Imaging Data Structure (BIDS) format](https://bids.neuroimaging.io/) in your system. In general, the image data files should be organized in this manner:

![Screen_Shot_2021-08-27_at_6](uploads/7b32ea2688ffd1e0b5c1d16475893e9f/Screen_Shot_2021-08-27_at_6.19.42_PM.png)

Note that DICOM files downloaded from the MRI center servers can either end with the \*.dcm or \*.ima extensions, or have no extensions at all. In addition, the DICOM files might be organized in directories indicating the type of image file (i.e., MPRAGE, T2, EPI, DTI, etc.) or not organized at all. In general, as long as the downloaded DICOM data are stored in the same subject folder (e.g. /path/to/mydataset/sourcedata/sub-01/), the above details do not really matter for the next DICOM conversion step.

[Return to TOC](#table-of-contents)

#### 2.2.1.1. Convert DICOM to NIFTI

Brain and Mind Lab uses [dcm2niix](https://github.com/rordenlab/dcm2niix) in the [mricrogl](https://www.nitrc.org/projects/mricrogl/) package. You can download and install these in your local system. Or if you are using the lab servers, they should have dcm2niix already installed. In the terminal, just type:

```plaintext
user$ dcm2niix -g n -d n /path/to/mydataset/sourcedata
```

The option -g n tells dcm2niix not to zip the output (i.e., convert to \*.nii and not \*.nii.gz). The option -d n tells dcm2niix not to apply the date in the output filename.

The converted \*.nii files will be in the sourcedata/sub-\* directory. You will need to move them to the rawdata/sub-\* directory accordingly. For example, if we were working on sub-01, you can move files using the mv command in the server terminal. NOTE!! Be careful of the mv command, it is equivalent to the rename command and it will overwrite files with the same name without warning.

```plaintext
user$ mv /path/to/mydataset/sourcedata/sub-01/*.nii /path/to/mydataset/rawdata/sub-01/
```

[Return to TOC](#table-of-contents)

#### 2.2.1.2. Reorient Images

It is good practice to always first view the brain image data to check for any artifacts or other issues. To do so, we can use the display tool in SPM as follows.

1. Start the Matlab environment and load the SPM toolbox.

```plaintext
   user$ matlab
   >> addpath /usr/local/spm12
   >> spm fmri
```

1. Clicking on the "Display" button in the resulting SPM menu when graphics are fully loaded.

   ![Screen_Shot_2021-08-27_at_9](uploads/16616a0abffe578117afa360f83d6b31/Screen_Shot_2021-08-27_at_9.24.52_PM.png)
2. Selecting the \*.nii image we want to display or reorient.\
   ![Screen_Shot_2021-08-27_at_8](uploads/b553b5c4b02634f1212b4a51df329b54/Screen_Shot_2021-08-27_at_8.21.53_PM_copy.png)

Once the \*.nii image is selected, click "Done" and the image will be displayed on the SPM Graphics window (the Display tool only allows selecting one image at a time). Alternatively, you can enter the following in the Matlab command window, which uses the example T1 image file with the name "SampleT1filename.nii":

```plaintext
   >> T1fn = 'SampleT1filename.nii';
   >> spm_image('Display',T1fn);
```

The aim here is to ensure that head placement is roughly in the AC-PC plane orientation and central origin position at the time of acquisition at the scanner. SPM (and most software) expects that typical heads should be somewhere in the center of the image space and show minimal pitch, roll, and yaw (see below figure).

![Screen_Shot_2021-08-28_at_11](uploads/897c00034890a8c03cbcc42cacedba3a/Screen_Shot_2021-08-28_at_11.48.24_AM.png)

If head placement is too far off (see below figure), SPM detection of where the head is in the image space would be compromised. Older adult head images especially might show more instances of non-adherence to this ideal because of stiff neck during head positioning at the scanner. This is usually less of an issue for young adult images, but its always good to perform visual inspection of all data as much as possible.

![Screen_Shot_2021-08-27_at_9](uploads/ebfe6edc31cf40088fb05b37ba275f82/Screen_Shot_2021-08-27_at_9.14.54_PM_copy.png)

To reorient the brain image, adjustment values are entered into the fields on the bottom left panel of the SPM Graphics window, as follows:

1. x, y, z translations in + or - mm shift the image accordingly: left/right (lateral/medial), forward/backward (anterior/posterior), up/down (superior/inferior) in the panel.
2. pitch, roll, yaw rotations in radians rotate the image about the x, y, and z axes, respectively.

You also see the resize (x,y,z) fields in the bottom left panel. Those are for re-scaling the size of each voxel. Adjusting these expands or shrinks the size of the image accordingly.

Once adjustment values are entered, hitting enter will reorient the image. Inspect the outcome in the axial, sagittal, coronal views above the SPM Graphics window. If the position and orientation is acceptable, click the "Reorient" button to apply the entered parameters to the image(s). A file selection window will appear and you should select the \*.nii file(s) for which you want to apply the reorientation adjustments you specified. Remember if you are applying the parameters to EPI images, you should also expand the number of frames and select all frames of the 4D EPI \*.nii volume images to apply the parameters to all volumes. After the file(s) is(are) selected, click "Done".

A second window will appear to ask whether you want to same the parameters into a _.mat file. This is a separate header file containing the reorientation parameters you specified and will be useful for iterative application (see below), so you should save it. A following window will then ask what filename you want to use for this reorientation parameter file with the default as the image file you are displaying with the affix "_reorient" appended at the end of the_ .mat file. Typically these defaults are accepted. Reorientation parameters are then applied to the selected image file and the reorientation parameter file also generated in the same directory as the selected image file.

IMPORTANT: Reorientation will apply the adjustment parameters to the image headers, the file modification date and time will also be updated, but it will not change the filename of the \*.nii file.

Alternatively, you might use the following matlab script methods if you are confident to adapt them for your purposes.

This code applies a pre-specified reorientation parameter vector with 12 values for x,y,z translation, x,y,z rotation, x,y,z affine, accordingly.

```plaintext
   matlabbatch{1}.spm.util.reorient.srcfiles = {[T1fn ',1']};
   matlabbatch{1}.spm.util.reorient.transform.transprm = [0 0 0 0 0 0 0 0 0 0 0 0];
   matlabbatch{1}.spm.util.reorient.prefix = '';
   spm_jobman('run',matlabbatch);
```

This code applies the pre-specified reorientation parameters in the saved orientation parameter .mat file. The extra fileparts step is to extract the T1 image filename from the .nii extension to associate with the "_reorient.mat' affix.

```plaintext
   [p n e] = fileparts(T1fn);
   matlabbatch{1}.spm.util.reorient.srcfiles = {[T1fn ',1']};
   matlabbatch{1}.spm.util.reorient.transform.transF = {[n '_reorient.mat,1']};
   matlabbatch{1}.spm.util.reorient.prefix = '';
   spm_jobman('run',matlabbatch);
```

[Return to TOC](#table-of-contents)

#### 2.2.1.3. Coregister T1 to EPI

In SPM, coregistration is a semi-rigid-body spatial transformation method to bring different brain images to the same orientation and placement location. This can be done for brain image data acquired using different sequences. For instance, we can coregister T1 to EPI, T2 to EPI, T1 to T2, DTI to T1, etc. The key principle is that this is a semi-rigid-body transformation with only rotations, translations, and affine transforms. As such, the brain image shape are preserved. This is distinct from complete rigid-body spatial transforms with only rotations and translations, and no affine transforms, and deformations in which shapes can be altered.

\[insert image of 6d transform, 12d affine transform, and deform\]

The main aim of coregistration in preprocessing is to match the orientations of different brain image data for the same subject which were acquired using different sequences or at different times. For instance, T2 images acquired on first, coregistered to EPI images later on during the same scan session, or T2 images each acquired on separate days, or a T1 and T2 image each acquire on separate days, etc. We generally do not coregister brain images of different subjects together since coregistration is a semi-rigid-body spatial transformation method and not a deformation method, which is required to match different brain shapes together.

When brain images are coregistered spatial geometries estimated from one image can be applied to the other coregistered images. This cannot be done if brain images are not coregistered, since spatial geometries estimated from the spatial prior of one image would not apply to the other non-coregistered images, which thus have different spatial priors.

The recommendation is to plan ahead and coregister all required images of the subject to the same orientation right from the start before estimating any further modifications in spatial geometries, such as in computing spatial normalization to a template space. However, if this is not possible, such as when access to image data are only available for some preprocessed form, coregistration can always be performed post-hoc, then corresponding spatial geometries applied. A good policy is to first graph out which image data need to be (or have been) coregistered to which, which images the geometry modification parameters were estimated from, which to which images they need to be applied. Once this is clear, the coregistration procedure as outlined below is actually a very straightforward one.

\[insert image of deform parameters applied to coregistered vs. non-coregistered shapes\]

In BML, we usually acquire an axial T2 image that has co-planar orientations with the EPI images. Thus, we usually coregister the T2 to the EPI images so use the higher spatial resolution of the T2 for subsequent anatomical matching. The T1 image is then coregistered to the EPI-coregistered T2 image, leverage on the higher anatomical detail in the T2 and T1 images. However, if the T2 is not available, it is also possible to directly coregister the T1 to the EPI image (or vice-versa).

In practice, it is more efficient to coregister by moving one T2 (or T1) volume to the reference volume of the EPI, which is one volume. Alternatively, you may coregister by moving the reference volume of the EPI image to the one T2 (or T1) volume, but you would also have to move all other volumes of the EPI image jointly, which can be many and therefore take up more time. The reference volume of the EPI image is the one to which all other EPI images are matched to during motion realignment. This can either be the first EPI volume, the mean EPI volume, or whichever was the reference volume specified during motion realignment.

In SPM, coregistration can be done by clicking on the "Batch" button of the SPM Menu:

![Screen_Shot_2021-08-28_at_12](uploads/8875952a04bd8a6d577785e6f25739cb/Screen_Shot_2021-08-28_at_12.33.12_PM_copy.png)

This opens the Batch Editor window (below). To invoke the Coregister tool, select the following in the window Menu: SPM > Spatial > Coregister > Coregister:Estimate

![Screen_Shot_2021-08-28_at_12](uploads/995eff13c7fcff620de6ee7c89258cce/Screen_Shot_2021-08-28_at_12.31.59_PM_copy.png)

As mentioned, in BML, we usually use the first volume of the EPI as the reference volume (or the mean volume) and enter that in the Reference Image field of the Coregistration setup (above). The above graphic depicts the usage of frame 1 of the EPI .nii image, which can be changed to the mean EPI .nii image frame 1 or whichever frame was set to be the reference frame. Importantly, this is the image that is kept stationary.

The Source Image is the T2 image (if available), which is moved to match the position or the Reference Image.

Once the Reference and Source images are supplied, if there are no Other images (in sync with the Source Image) that should also be moved to the position of the Reference Image, the batch setting can be saved as a .mat file if needed and the green play button can be clicked. The coregistration will then begin. When done, the SPM graphics window will show:

![Screen_Shot_2021-08-28_at_12](uploads/a2701c5dc72df3715aa11c5314e5ad6c/Screen_Shot_2021-08-28_at_12.35.40_PM_copy.png)

The top two panels show the joint voxel intensity histograms before and after coregistration. They usually do not differ too much if the Source and Reference images were generally in the same position to begin with, which is typical. Importantly, the bottom two panels show the Reference (left) and Source (right) images after coregistration. You can click around the borders of the brain images as well as key anatomical structures like the ventricles to move the crosshair jointly in the two panels. In this way, you can verify that the two images are coregistered correctly (i.e., the crosshair position is always on the same anatomical structure in the two images).

IMPORTANT: Because we use the coregister estimation tool in SPM, the resulting image coregistration orientation parameters are written into the header of the Source Image (and Other Images, if used). The data part of the image file are not altered. The filenames of the Source Image (and Other Images) also remained unchanged. Thus, apart from the file modification date and time, there are no other external indications whether a brain image file has been coregistered or not. SPM will always read the header orientation information when opening brain image files so the coregisteration information will be used in this manner.

The above example shows coregistration of the T2 to the T1. In BML, we then proceed to coregister again but moving the T1 to the EPI-coregistered T2 position. In this way, we coregister the T1 to the EPI. If no T2 is available, coregistration is direct from T1 to the EPI reference volume. If the T1 was not coregistered to the EPI, but already has some spatial geometries pre-estimated that need to be applied to the EPI, then coregistration can be done from the EPI reference (determined in motion realignment) volume set in the Source Image field, the other EPI non-reference volumes set in the Other Image field, and the T1 image set in the Reference Image field.

Alternatively, you might use the following matlab script methods if you are confident to adapt them for your purposes.

```plaintext
   EPIfn = 'SampleEPIfilename.nii';
   T2fn  = 'SampleT2filename.nii';
   matlabbatch{1}.spm.spatial.coreg.estimate.ref = {[EPIfn ',1']};
   matlabbatch{1}.spm.spatial.coreg.estimate.source = {[T2fn ',1']};
   matlabbatch{1}.spm.spatial.coreg.estimate.other = {''};
   matlabbatch{1}.spm.spatial.coreg.estimate.eoptions.cost_fun = 'nmi';
   matlabbatch{1}.spm.spatial.coreg.estimate.eoptions.sep = [4 2];
   matlabbatch{1}.spm.spatial.coreg.estimate.eoptions.tol = [0.0200 0.0200 0.0200 1.0000e-03 1.0000e-03 1.0000e-03 0.0100 0.0100 0.0100 1.0000e-03 1.0000e-03 1.0000e-03];
   matlabbatch{1}.spm.spatial.coreg.estimate.eoptions.fwhm = [7 7];
   spm_jobman('run',matlabbatch);
```

[Return to TOC](#table-of-contents)

#### 2.2.2. T1 Spatial Normalization Using DARTEL To Create a SST

[DARTEL](https://www.fil.ion.ucl.ac.uk/\~john/misc/dartel.ppt) stands for Diffeomorphic Anatomical Registration Through Exponentiated Lie algebra. If you use DARTEL, be sure to cite: Ashburner, J. (2007). A fast diffeomorphic image registration algorithm. _Neuroimage_, _38_(1), 95–113. [https://doi.org/10.1016/j.neuroimage.2007.07.007](https://doi.org/10.1016/j.neuroimage.2007.07.007). However, do note a more recent development using [LDDM](https://www.sciencedirect.com/science/article/pii/S1053811910016496?via%3Dihub) (Large Deformation Diffeomorphic Metric mapping) or geodesic shooting. Also note [Freesurfer](https://surfer.nmr.mgh.harvard.edu/), which is a completely different spatial normalization approach using projection to spherical coordinates.

When approaching the brain using a 3D cartesian coordinate system, BML uses DARTEL as the de facto spatial normalization algorithm in SPM, for now. Do refer to the references above for more technical details on how DARTEL works. Essentially, unlike a 12D affine transform, DARTEL optimizes local deformations of shapes so that a more accurate geometric transformation can be derived between different brain shapes.

Critically, the DARTEL procedure is used to generate a Study Specific Template (SST), which avoids biases when spatially normalizing subject brain anatomy to the standard template space, which is usually generated from healthy, young adult brains (e.g. the [Montral Neurological Institute, MNI, template](https://www.bic.mni.mcgill.ca/ServicesAtlases/ICBM152Lin), and many [other template coordinate systems](https://bids-specification.readthedocs.io/en/latest/99-appendices/08-coordinate-systems.html)). As such, if a study involves older adults, direct spatial normalization to the template brain would introduce age-related biases in the imaging voxel values due to structural anatomical differences between young and older adult groups. Specifically, older adult brains systematically deviate from young adult brains in having larger ventricles, thinner gray manner, and enlarged sulci. Spatial normalization to a young adult template would thus involve larger magnitudes of projection (and interpolation) to make older adult brains "fit" younger adult templates, especially if the study is evaluating differences relative to young adult brain data (which would have lower magnitudes of projection)

An SST avoids the above spatial projection bias by providing a mean point for spatial normalization that would reduce bias magnitudes before general projection to the template space.

\[insert figure to show how SST avoids distance biases to a target location\]

In BML, the T1 DARTEL step is an independent pipeline and can be applied in parallel to other EPI or diffusion imaging data preprocessing. See [example schematic for functional connectivity analysis](#21-principles-to-remove-artifacts-and-yield-residual-images). It is recommended to plan ahead to establish a more efficient pipeline before proceeding. In general, the [Segment T1](#2221-segment-t1) step can be applied progressively as data collection progresses one subject at a time. However, the [Make SST](#2222-make-sst) step requires that all study subject data have been collected and segmented.

[Return to TOC](#table-of-contents)

#### 2.2.2.1. <span dir="">Segment T1</span>

Segment

[Return to TOC](#table-of-contents)

#### 2.2.2.2. Make SST

[Return to TOC](#table-of-contents)

#### 2.2.2.3. Spatially normalize SST to MNI space

[Return to TOC](#table-of-contents)

#### 2.2.3. EPI Preprocessing

[Return to TOC](#table-of-contents)

#### 2.2.3.1. Optionals

[Return to TOC](#table-of-contents)

#### 2.2.3.1.1. EPI Inhomogeneity correction

[Return to TOC](#table-of-contents)

#### 2.2.3.1.2. Apply gradient field map

[Return to TOC](#table-of-contents)

#### 2.2.3.2. Motion correction

[Return to TOC](#table-of-contents)

#### 2.2.3.3. Slice-time correction

[Return to TOC](#table-of-contents)

#### 2.2.3.4. 1st-level modeling to generate residual images

[Return to TOC](#table-of-contents)

#### 2.2.3.5. Temporal filtering of residual images

[Return to TOC](#table-of-contents)
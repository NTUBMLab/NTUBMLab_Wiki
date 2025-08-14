# SPM Cookbook

## Catalog

- [How to launch SPM](#how-to-launch-spm)
- [Preprocessing](#preprocessing)
  - [Example data descriptions](#example-data-descriptions)
  - [Convert DCM to NII file](#convert-dcm-to-nii-file)
  - [Preprocessing parameters and steps](#preprocessing-parameters-and-steps)
    - [Slice Timing](#slice-timing)
    - [Realignment for EPI data](#realignment-for-epi-data)
    - [Coregistration: from T2 to EPI](#coregistration-from-t2-to-epi)
    - [Coregistration: from EPI & T2 to T1](#coregistration-from-epi-t2-to-t1)
    - [Normalise the T1 image](#normalise-the-t1-image)
    - [Normalise the T2 and EPI images](#normalise-the-t2-and-epi-images)
    - [Smooth the normalised EPI images](#smooth-the-normalised-epi-images)
- [FAQ](#faq)

## How to launch SPM

- Steps:
  - Launch MATLAB first
  - add path:
 `addpath('/usr/local/spm12/') # Note that the path could be different depends on where the SPM12 was installed`
  - type `spm` in the MATLAB console
  - If you are going to analyze fMRI data, you can also type `spm fmri`


## Preprocessing

### Example data descriptions
  - I will use the  [SVS-ADM](https://gitlab.com/brain-and-mind-lab/research-projects/adm-personal-core-value) data set as example to demonstrate the preprocessing process.
  - File names:
    - EPI files: Each participant has 5 sessions/runs of EPI data, and each session has 218 volumes. The file name for the raw NII of the 1st run is `dm1.nii`. The 56th volume in the 1st run could be referred by `dm1.nii,56`.
    - T1 file: `T1.nii`
    - T2 file: `T1.nii`
  - Data directories:
    - The raw DCM files of the subject S023 could be found in: `/home/.bml/Data/Bank6/ADM-YunShiuan/adm-personal-core-value/analysis_on_server/IMG_RAW/S023`
    - The raw unpreprocessed NII files of the subject S023 could be found in: `/home/.bml/Data/Bank6/ADM-YunShiuan/adm-personal-core-value/analysis_on_server/IMG_nii/S023`
    - The preprocessed NII files of the subject S023 could be found in: `/home/.bml/Data/Bank6/ADM-YunShiuan/adm-personal-core-value/analysis_on_server/IMG_nii/S023/normalise_3x3x3/`

### Convert DCM to NII file

- For different types of images:
  - EPI functional images (note there are 218 volumes in a run):
      - For each run, input a series of DCM files (number of DCM files = 218), each file is a volume, and output one 4-dimensional NII file (4 dimensions = number voxels in the x-axis, number voxels in the y-axis, number voxels in the z-axis , number of volumes in the time dimension). Each NII file contains 218 volumes of the brain. The length of the time dimension = 218.
  - T1 & T2 structural images:
    - Also convert the images from DCM file to 4-dimensional NII file. Although for structural images, the length of the time dimension = 1. Note that number of DCM files of T1/T2 image is NOT 1. In the case for [ADM-SVS](https://gitlab.com/brain-and-mind-lab/research-projects/adm-personal-core-value) data, the number of DCM files is 192. This is different from EPI files, where the number of files equal to the number of the DCM files.
- How to convert:
  - Use [dcm2nii](https://people.cas.sc.edu/rorden/mricron/dcm2nii.html) to convert.
    - Take T1 image for example, drag the whole T1 folder containing all the T1 DCM files to the **dcm2nii** program.
    - Output format = 4D NII file. After conversion, there will be three NII files in the T1 folder:
      - (1) "s...a1001"(the raw image)
      - (2) "os...a1001"(corrected)
      - (3)"cos...a1001"(cropped image). What we want is the raw image.
    - Note that the dcm2nii process could be automized by batch script. See [this file](https://gitlab.com/brain-and-mind-lab/research-projects/adm-personal-core-value/blob/master/analysis_on_server/sh_files/transRAWdata2.sh) for example.
  - Use SPM to convert:
    - From SPM Menu -> Batch to launch the Batch Editor-> SPM -> Util -> Import -> DICOM import
    - This could be automized by SPM batch scripting. That is, once you completed a SPM script, you could view the .m code (From Batch Editor -> View -> Show .m code) to construct the batch.

###	Preprocessing parameters and steps

I recommend loading in the batch file while reading the following instruction for parameters so that you know which part I am referring to.

- Example Batch file
  - The batch file containing the whole set preprocessing steps for the subject S023 is `/home/.bml/Data/Bank6/ADM-YunShiuan/adm-personal-core-value/analysis_on_server/pre_mat_3x3x3/pre2_23.mat`.
  - You could read in the batch and review the content by (1) launch the Batch Editor from SPM Menu (press the button `Batch`) (2) `File` -> `Load Batch` -> Choose the `pre2_23.mat` file as detailed above.

The following preprocessing operations are introduced in the order that the SVS-ADM used. Note that there could be some deviation in the order of preprocessing steps between different projects.

#### Slice Timing

Step 1: Slice Timing for EPI data (called `Slice Timing` in the SPM Batch Editor)
  - How to add this operation to your batch
    - In SPM batch editor, `SPM`->`Temporal`->`Slice Timing`
  - Specify parameters:
    - Data:
      - `New: Session`: It will create a session. I created 5 sessions because my task has 5 runs.
      -  `Specify`: Specify the files for each session. It will launch a session window.
      - From the session window, type the filter criteria in the filter search bar: `dm1.nii` and `1:218` in the bar below. This will filter in the 218 NII files for the 1st run (`dm1.nii, 1, ..., dcm1.nii, 218`).
    - Number of slices = 38
      - this corresponds to number voxels in the z-axis
    - TR = 2
      - this is the repetition time (TR) of the EPI sequence
    - TA = 2 - (2/38)
      - TA(acquisition time) = TR - (TR/nslices) = 2 - (2/38)
    - Slice order = [2:2:38 1:2:38]
      - Because the slice order used in the project is 'interleaved (bottom->up)', so the slice order for the 38 slices is `2   4   6   8  10  12  14  16  18  20  22  24  26  28  30  32  34  36  38   1   3   5   7   9  11  13  15  17  19  21  23  25  27  29  31  33  35  37`.
    - Reference slice = 1
      - Because  the slice order is `2   4  ... 36  38  1   3 ...  35  37`, so the 1st slice is the 'middle slice'. Choosing the middle slice as the reference for slice timing could minimize the slicing error.
      - Note that some argue that one should use the first acquired slice (which is the 2nd slice in the project) because it corresponds to the onset time.
    - Filename prefix = a
      - the output image will have the prefix 'a'. That is, the input raw image `dm1.nii` will output post-slice-timing image `adm.nii`.

#### Realignment for EPI data

Step 2: Realignment for EPI data (called `Realign: Estimate` in the SPM Batch Editor)
  - Note
    - the `realign:estimate` operation does not correct the input image or output a realigned image. The input image is intact after the operation. The operation only estimates the 6 parameters of head motion (3 translations and 3 rotations) and output the estimates in txt files. The estimates will be used as covariates in the first-level analysis to partial out the effect from head motions.
    - Alternatively, some people use `realign:estimate and reslice` to directly correct the image. (I tried this but this somehow produced weird results when it follows the slice-timing operation.)
  - How to add this operation to your batch
    - In SPM batch editor, `SPM`->`Spatial`->`Realign`->`Realign: Estimate`
  - Specify parameters:
    - Data:
      - `New: Session`: It will create a session. I created 5 sessions because my task has 5 runs.
      -  For simplicity, I recommend to use `Dependency` to set the input data of Realignment as the output data from the slice-timing. Alternatively, you could still use `Specify` to manually select the post-slice-timing files (`adm.nii`) as above.
    - I leave other parameters as default. Note that `Num Passes = Registered to first` means that the first volume of each run is regarded as the reference when estimating the coregistration head motion parameters.

#### Coregistration: from T2 to EPI

Step 3: Coregistration: from T2 to EPI (called `Coregister: Estimate` in the SPM Batch Editor)
  - Note
    - I need to coregister twice: one for coregistering T2 to EPI images (T2 header is changed while EPI remains intact), another for coregistering EPI & T2 to T1 (T2 and EPI headers are changed while T1 remains intact).
    - Coregistration does no change the value of each voxel. Instead, it changes the 'header', or the coordinates, of each voxel. When coregistering T2 to EPI images, it makes sure the voxel of the same part of the brain shares the same coordinate value across two images.
  - How to add this operation to your batch
    - In SPM batch editor, `SPM`->`Spatial`->`Coregister`->`Coregister: Estimate`
  - Specify parameters:
    - reference image = `Realign: Estimate: Realigned Images (Sess1)`
      - Use `Dependency` to specify to EPI image (because EPI image is the reference for T2 to match)
      - Use the  irst session because all functional images are already realigned to the first volume of each run in the previous step(`Num Passes = Registered to first`). By setting the reference image to `Realign: Estimate: Realigned Images (Sess1)`, this ensures all functional images to be coregistered to the first functional volume of all runs in the next step (Coregistration: from T2 & EPI to T1)
    - Source image = the raw T2 image (`T2.nii,1`)
      - This ensures that the T2 image will use the coordinate value of the EPI images.

#### Coregistration: from EPI & T2 to T1

Step 4: Coregistration: from EPI & T2 to T1 (called `Coregister: Estimate` in the SPM Batch Editor)
  - Note
    - The headers of T2 and EPI images are adjusted to match the coordinates of the T1 image, while the T1 image remains intact.
  - How to add this operation to your batch
    - In SPM batch editor, `SPM`->`Spatial`->`Coregister`->`Coregister: Estimate`
  - Specify parameters:
    - Reference image = the raw T1 image (`T1.nii,1`)
    - Source image = the raw T2 image (`T2.nii,1`)
    - Other image = the five EPI run images (`Dep (5 outputs)`)
      - Use `Dependency` to specify to EPI image of 5 runs.
      - The headers of these 'other images', along with the source image (T2 image), will be adjusted to match the reference image (T1 image).

#### Normalise the T1 image

Step 5: T1 is being normalised and used to generate the deformation field (called `Normalise: Estimate & Write` in the SPM Batch Editor)
  - How to add this operation to your batch
    - In SPM batch editor, `SPM`->`Spatial`->`Normalise`->`Normalise: Estimate & Write`
  - Specify parameters:
    - Data
      -  Choose `New: Subject`
      - Image to Align: the raw T1 image (`T1.nii,1`)
        - The image that is going to be normalised to the template brain space.
        - This will generate the **'deformation field'** (`y_T1.nii`), which is a map of normalisation parameters that will be used to normalised the EPI images in the later step (Normalise the T2 and EPI image).
      - Images to Write: the raw T1 image (`T1.nii,1`)
        - The image that is going to be normalised to the template brain space.
        - This will generate the normalised T1 image with the filename prefix `wadm.nii`.
    - Estimation options
      - Tissue probability map = `specify` > `spm12`> open the `tpm` folder > select 'TPM.nii'
      - Affine Regularisation = `ICBM space template - East Asian Brains`
        - Depends on the race of the subjects. `ICBM space template - European Brains` will be better for Caucasian subjects.
    - Writing options
      - Voxel sizes = `[3 3 3]`
        - Because the raw EPI voxel size is 3.75 x 3.75 x 3 mm, the target voxel size should not be larger than the raw voxel size, otherwise the resolution will be wasted. Likewise, the target voxel size should not be much smaller than the raw voxel size, otherwise the number of voxels will be way too many.
    - Filename prefix = w
      - the output image will have the prefix 'w'. That is, the input raw T1 image `T1.nii` will output normalised T1 image `wT1.nii`.
  - Notes for advanced users, feel free to skip this:
      - The new segmentation operation does generate deformation fields, but not in a form that is easy to use via the `Normalise->Write` option. You would need to use the Deformations Utility (with the `y_T1.nii` file that is optionally generated), which is not so straightforward.

#### Normalise the T2 and EPI images

Step 6: normalise:write (T2 and EPI images are being normalised using the deformation field generated in the previous step) (called `Normalise: Write` in the SPM Batch Editor)
- How to add this operation to your batch
  - In SPM batch editor, `SPM`->`Spatial`->`Normalise`->`Normalise: Write`
- Specify parameters:
  - Data
    -  Choose `New: Subject`
    -  Deformation Field = Estimate&Write: `Deformation (Subj 1)`
        - the deformation field that recorded the normalisation information from subject T1 brain to the template
    - Images to Write = the coregistered T2 and EPI images (`Coregister: Estimate: Coregistered Images`)
      - Use `Dependency` to specify the T2 and EPI images. There are two `Coregister: Estimate: Coregistered Images`,the first one is the T2 image coregistered to EPI image and the second one is the T2 and EPI images coregistered to the T1 image.
  - Writing options
    - Voxel sizes = `[3 3 3]`    
    - Filename prefix = w
      - the output image will have the prefix 'w'. That is, the input raw T2 image `T2.nii` will output normalised T1 image `wT2.nii` and the input time-sliced EPI images `adm.nii` will output normalised EPI images `wadm.nii`.

#### Smooth the normalised EPI images

Step 7: smooth the normalised EPI images
- How to add this operation to your batch
  - In SPM batch editor, `SPM`->`Spatial`->`Smooth`
- Specify parameters:
  - Images to smooth = the normalised EPI images (`Normalise: Write: Normalised Images (Subj 1)`)
  - FWHM = `[8 8 8]`
    - Full-Width Half-Maximum Gaussian Blur。 It defines the weighting parameters for the voxel values. The closer to the Gaussian central, the larger the weight is. The definition of HWHM is the width corresponding to 50% area below the Gaussian function.
    - The default and the convention is 8 x 8 x 8 mm. If questioned, you could refer to the references that use the same FWHM.

## FAQ   
### Q1. how to deform from normalized brain to native space?
deformation -> composition -> iy*.nii, use "pullback" option


[Ref](https://www.jiscmail.ac.uk/cgi-bin/webadmin?A2=ind1904&L=SPM&P=334108)

### Q2. How to skull-striped brain image in SPM? How to do BET in SPM?
Step01: Segment to get GM, WM, CSF

Step02: Use ImCalc, input your images(i1), GM(i2), WM(i3), CSF(i4). And calculate by (i1.*(i2+i3+i4>0.5)).

[Ref](https://www.jiscmail.ac.uk/cgi-bin/webadmin?A2=ind1904&L=SPM&P=332664)

### Q3. How to do slice timing correction on multiband functional images?
A1. specify "slice order" and "reference slice" in unit of time (milliseconds), and set "TA" to 0.

[Ref](https://www.jiscmail.ac.uk/cgi-bin/webadmin?A2=ind1407&L=SPM&D=0&P=795113)

A2. If the TR is very short, it is OK to not do slice timing correction. But when you specify first order, set the microtime onset to middle of time to reduce slice time bias.

[Ref](https://wiki.humanconnectome.org/display/PublicData/HCP+fMRI+slice-timing+acquisition+parameters)

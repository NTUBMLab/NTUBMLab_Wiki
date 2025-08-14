### Single subject data processing tutorial

## Table of Content
   *  [Loading Data in EEGLAB](#loading-data-in-eeglab)
   *  [Preprocessing](#preprocessing)
      *   [Re-referencing](#re-referencing)
      *   [Artifact rejection](#artifact-rejection)
      *   [Filter data](#filter-data)
      *   [Elist](#elist)
      *   [Assign bins](#assign-bins)
      *   [Extract bins](#extract-bins)
      *   [Compute ERP](#compute-erp)

## Loading Data in EEGLAB ##

### Install EEGLAB

*  Downlaod [EEGLAB](https://sccn.ucsd.edu/eeglab/downloadtoolbox.php)
*  Start {+ MATLAB +} > Click {+ Set Path +} > Click {+ Add folder +} > Select the EEGLAB folder you downloaded > Click {+ Save +} > Click {+ Close +}
![Matlab_set_path_gui](uploads/62eb853f5309cbd8d283a5e9cbf25a75/Matlab_set_path_gui.png)
*  In your MATLAB command window, just type **eeglab** and hit enter. 
*  The blue main EEGLAB window below should pop up. Ya!!

### Load EEG datasets

*  If you want to load the EEG datasets from the BrainVision, you need to download the plugins:

   *  Download [BVA data import/export](https://sourceforge.net/projects/bva-io/).
   *  Download [ERPLab](https://erpinfo.org/erplab)
   *  Put the folder you download (bvaio and ERPlab) into the EEGLAB's plugin
   ![Screen_Shot_2019-03-08_at_5.58.21_PM](uploads/1d7bc7a38dc230788a26e2017feb13be/Screen_Shot_2019-03-08_at_5.58.21_PM.png)
   *  Start MATLAB and Open EEGLAB by typing **eeglab** in the command window and hit enter
   *  {+File+} > {+Import Data+} > {+Using EEGLAB functions and plugins+} > {+From Brain Vis. Rec. .vhdr file+} > Select the vhdr file you recorded 
   ![Screen_Shot_2019-03-08_at_9.51.11_PM](uploads/5559fcd0fa9ae04412061e01c30deb46/Screen_Shot_2019-03-08_at_9.51.11_PM.png)

*  Scrolling through the data:
   * Plot > Channel Data (scroll)- You will see the plots
![Screen_Shot_2019-03-08_at_9.55.01_PM](uploads/70c24572a7dba99948f5344527c3ae0e/Screen_Shot_2019-03-08_at_9.55.01_PM.png)
   * Display > Remove DC offsets
![Screen_Shot_2019-03-08_at_9.55.24_PM](uploads/85fe9e2b1d74b013b65a74b170f838b7/Screen_Shot_2019-03-08_at_9.55.24_PM.png)

   * Then, you will see the EEG data
![Screen_Shot_2019-03-10_at_2.26.31_PM](uploads/df133b0fff9c3c1ca8a3745054c57c2d/Screen_Shot_2019-03-10_at_2.26.31_PM.png)


## Preprocessing ##

Preprocessing procedures depend on experimental designs and labs. Here, I use the DMERP as a demonstration of preprocessing.

### Re-referencing
*  {+Tools+} > {+Re-referencing+}
![Screen_Shot_2019-03-10_at_2.35.31_PM](uploads/8ed6ed4d1a01941d8b8a81798553162f/Screen_Shot_2019-03-10_at_2.35.31_PM.png)
*  How to re-reference depends on the experimental degisn. Some people prefer **Compute average reference**, and some like **average of M1 and M2**. Here, we use M2 as our reference: Re-reference data to channels > M2 > OK.
*  Name it: S005_run1_re

### Artifact rejection 
*  Manually reject: {+Tools+} > {+Reject Continuous Data by eye+} > Select the time windown you want to reject.
![Screen_Shot_2019-03-10_at_2.43.16_PM](uploads/9177608f981c031f508df8d4e5e0fba4/Screen_Shot_2019-03-10_at_2.43.16_PM.png)
*  Run ICA: If you don't want to reject data manually, you can use ICA to help you get the "poor" component. 


### Filter data


*  Select {+Tools+} > {+Filter the data+} > {+Basic FIR filter+}
   *  Lower 0.5 Hz
   *  Higher 50 Hz
   *  The lower and higher threshold depends on the task and analysis.
   *  Press OK
   *  Name it: S005_run1_re_filt
![Screen_Shot_2019-03-10_at_2.45.59_PM](uploads/b619cb7403decaefc1a7ebcd82853fcf/Screen_Shot_2019-03-10_at_2.45.59_PM.png)
*  ERPLab: You can also use filtering tools in the ERPlab.
   *  {+ERPLAB+} > {+Filter & Frequency Tools+} > {+Filter EEG data+} > {+IIR+} (with remove DC bias)
   *  Lower 0.5 Hz
   *  Higher 50 Hz
   *  The lower and higher threshold depends on the task and analysis.
   *  Press OK
   *  Name it: S005_run1_re_filt
### Elist

*  {+ERPLab+} > {+Elist+} > {+Create EEG EVENTLIST+}
   *  Name it: S005_run1_re_elist

### Assign bins

*  {+ERPLab+} > {+Assign bins+}
*  Click {+Browse+} > Select bin files (e.g. bin_DM_ERP_prob-locked.txt) > Click {+Run+}
   *  Name it: S005_run1_re_elist_bins

### Extract bins

*  {+ERPLab+} > {+Extract bin-based epoch+} >  Type the time window you want to study (None baseline)
   *  Name it: S005_run1_re_elist_bins_be

### Compute ERP
*  {+ERPLab+} > {+Compute averaged ERPs+}
You can {+Plot+} > {+Channel data scolls+}
![Screen_Shot_2019-03-10_at_5.16.16_PM](uploads/cb5580ca22b79d50129b3bce9e02b6a3/Screen_Shot_2019-03-10_at_5.16.16_PM.png)

You can plot ERPs by:
*  {+ERPLab+} > {+Plot ERPs+}
![Screen_Shot_2019-03-10_at_5.15.08_PM](uploads/a6a34ba8df21d011a138a95145a5cadf/Screen_Shot_2019-03-10_at_5.15.08_PM.png)
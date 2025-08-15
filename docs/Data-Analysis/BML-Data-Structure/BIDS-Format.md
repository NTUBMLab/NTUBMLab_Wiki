Construct your study directory structure according to the [Brain Imaging Data Structure (BIDS) ](https://bids-specification.readthedocs.io/en/stable/)format. Accordingly, set up the following directory structure in your computer or the server system.

```
~/Study/
|-- README.md
|-- Info
|-- Forms
|-- Experiment
|-- Code
`-- Data
    |-- sourcedata
    |   |-- README.md
    |   `-- sub-y01
    |       `-- ses-01
    |           |-- beh
    |           |-- anat
    |           |-- func
    |           |-- eeg
    |           `-- blood
    |-- rawdata
    |   |-- README.md
    |   `-- sub-y01
    |       `-- ses-01
    |           |-- beh
    |           |-- anat
    |           |-- func
    |           |   |-- sub-01_task_events.tsv
    |           |   `-- sub-01_task_events.json
    |           |-- eeg
    |           |   |-- sub-01_task_events.tsv
    |           |   `-- sub-01_task_events.json
    |           `-- blood
    `-- derivatives
        |-- README.md
        |-- beh
        |   `-- dataset_description.json
        |-- spm-preproc
        |   |-- dataset_description.json
        |   `-- sub-y01
        |       `-- ses-01
        `-- spm-stats
            |-- README.md
            |-- 1stlvl
            |   |-- dataset_description.json
            |   `-- sub-y01
            |       `-- ses-01
            `-- 2ndlvl
                `-- ses-01
                    `-- dataset_description.json
```

In the above code block,

* _Study_ is the study name (use your study name as needed, eg. ADM, DSN, RES, etc.).
  * _README.md_ at this level is the text file containing the verbose description of this study and its contents to help understanding of subsequent directory structure and interpretation of the purpose(s), research question(s), finding(s), and publication(s) pertaining to this study.
  * _Info_ is the directory containing qualifying information to take note of about the study in general.
  * _Experiment_ is the directory containing any stimuli presentation and/or behavioral response acquisition software files (e.g. E-Prime, PyschoPy, Psychtoolbox, Unity, etc.).
  * _Code_ is the directory containing scripts or other data processing pipelines required for data analysis.
  * _Data_ is the directory containing original and derived experimental data.
    * _sourcedata_ is the directory containing the original dicom images, other original data modalities (e.g. eeg, blood-based, etc.) and behavioral data (e.g. neuropsychological test responses, experimental task logs such as e-Prime or PsychoPy outputs).
    * _rawdata_ is the directory containing minimally modified data, e.g. nifti files converted from dicom images. This also includes the \_events.tsv and \_events.json behavioral logs converted from the original experimental task logs that accompany the other neurophysiological data. Note that these event files are placed in the corresponding modality folders and not in the _beh_ folder.
    * _derivatives_ is the directory containing subsequently preprocessed and analysis files.

The hierarchy for _sub_ vs. _ses_ can be flexible according to the study requirement. It might be more meaningful to put _ses_ higher than _sub_ for some studies. For some studies, it might be more meaningful to put _sub_ higher than _ses_. For some studies, _ses_ might also be omitted. Regardless, whether and how _ses_ is placed, or omitted, with respect to _sub_ must be described in README.md.

In principle, it should be possible to delete the "rawdata" and "derivatives" directories and regenerate them again from the source, provided the procedures to transition from source to derivatives are known, and there is no random seeding involved in the transition process.

Formats for,\
[dataset_description.json](https://bids-specification.readthedocs.io/en/stable/03-modality-agnostic-files.html#dataset_descriptionjson) \
[README.md](https://bids-specification.readthedocs.io/en/stable/03-modality-agnostic-files.html#readme)\
[CHANGES.md.](https://bids-specification.readthedocs.io/en/stable/03-modality-agnostic-files.html#changes)\
[participants.tsv, participants.json](https://bids-specification.readthedocs.io/en/stable/03-modality-agnostic-files.html#participants-file)\
[\_events.tsv, \_ events.json](https://bids-specification.readthedocs.io/en/stable/04-modality-specific-files/05-task-events.html)

[heudiconv ](https://github.com/nipy/heudiconv)is a useful tool to automate the directory and data re-structuring process.
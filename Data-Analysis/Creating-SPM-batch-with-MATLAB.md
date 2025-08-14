## Table of content
[[_TOC_]]

## When should I look at this Wiki Page?
-- When you are ready to process a whole batch of subjects using SPM.

In my experience, I like to set up my analysis protocol using the SPM graphical user interface(i.e. All the 'clicking' with your mouse) with my first subject. I'd do a test run, make sure everything works with subject#1. Typically after that, you would probably save the batch, then re-click everything again for the next subject. But are you going to repeat all the clicking a hundred times for the hundred subjects you are using just for changing that subject number?

Of course not. Not only is that time-consuming, but it also creates room for mistakes that you might not even be aware of, such as forgetting to change the data somewhere, or clicking on the wrong subject.
So on this Wiki page, you will be learning how to write codes to create SPM batch files. Please still be aware though, there can still be lots of room for mistakes even if you are writing codes! On this wiki page, some tips that I usually use to check for mistakes shall also be mentioned.

### Stuff you should have before starting:
1. **The SPM batch file** for your first subject. (The batch file can contain only one subroutine or a pipeline containing multiple subroutines.)
2. **A group of subjects that you intend to run (a) specific SPM process(es) on.** They should be named in a serial order. (e.g. MIFC0001, MIFC0002, MIFC0003 etc.) It's also best to have the structure for your data set up properly. Our lab is aiming to have our data setup in the form of [BIDS](https://bids.neuroimaging.io/) (Brain Imaging Data Structure). Check the website or ask a senior to learn more about how to set up your data structure properly.

Now that you've got what we need. Let's start!

## Looking deeper into the essence of SPM batch files
To start with, we have to learn about what SPM batch files are storing essentially.
Usually, we open the SPM batch file through the SPM `batch` function here.

![Opening batch with SPM](uploads/2fad62ae5e04ca294cd0780e969299e0/opening_batch_with_SPM.gif)

But if you directly open the SPM batch file in MATLAB, one variable named `matlabbatch` would be loaded. You can explore the variable by clicking it.

![opening_batch_with_matlab](uploads/4147f6ad090f829226b7cf4754ceed96/opening_batch_with_matlab.gif)

By opening the batch file directly from MATLAB, you can see that the SPM batch file is essentially the `matlabbatch` [structure](https://www.mathworks.com/help/matlab/ref/struct.html) variable. In the example GIF image provided, the subroutine I had was for coregistration, therefore, you can find that in one of the layers, the field was `coreg`.

Knowing this implies that as long as you know the required field and input format for each subroutine, you can write a SPM batch using just the MATLAB script editor. You can even create multiple batches using `for` loops! Isn't that amazing?!

## Skills you will be learning
Simply knowing that SPM batch files works as `matlabbatch` is not enough. Before properly writing batch files with the MATLAB script editor. There are some skills that you will need to learn:

1. Creating `matlabbatch` fields with the desired subroutine
2. Creating subject path strings with `for` loops
3. Saving and running SPM batch files

## Creating `matlabbatch` fields with desired subroutines

As mentioned earlier, the SPM batch editor reads the respective fields in the SPM `matlabbatch` file. But how do you know how to name the fields in the `matlabbatch` structure properly?

### Method 1. Copying the subroutine from the BML SPM project

There are two ways to doing so, the first one is by visiting the [BML SPM project](https://gitlab.com/brain-and-mind-lab/bml-data-analysis-tools/spm).

![copy_subroutine_from_gitlab](uploads/bcec23eb99d8385e5fbe60393e6dbc89/copy_subroutine_from_gitlab.gif)

**Steps:**

1. Find the subroutine that you need, copy and paste it into the MATLAB script editor.
2. Change the strings stored in `EPIfn` and the `T2fn` to the EPI and T2 that you need.
3. If you want SPM to run right after running the batch, keep the last line (` spm_jobman('run',matlabbatch)`). If not, you can delete or comment(we comment with `%` in MATLAB) it so that the line does not run.
4. If you want to save the batch, add the line `save(FileNameYouWantToAaveAs,'matlabbatch')`

### Method 2. Save script with the SPM batch editor

You can also save the script with the SPM batch editor with the batch that you have already created.



**Steps:**

1. In the SPM batch editor, open the batch file that you have created previously
2. Go to `Files>Save Batch and Script`
3. Name the batch file as you want to, in this example, I saved it as `coregister_preprocess`
4. Two files will be created then:
    - `coregister_preprocess.m`
    - `coregister_preprocess_job.m`
5. Open the `coregister_preprocess_job.m` file. You will see a similar format just like the one created in [method 1].
6. You can then change the corresponding stuff that you would want to change instead.

## Creating subject path string with `for` loops

The next thing you would want to learn is probably how to create subject path strings. Typically, you would have all your subjects saved under that same home directory. The only difference between each subject directory is the subject number. How do you create strings using variables so that a `for` loop can create the 100 strings for the 100 subjects for you?

There are several MATLAB functions you can use:

1. [`sprintf`](https://www.mathworks.com/help/matlab/ref/sprintf.html)

`sprintf` is a function that allows storing data as strings. Let's say you want to create a string that points to the T1 file of MIFC subject#1, the following is how it will be written with `sprintf`:

`subjno = 1;`
 `T1path = sprintf('/bml/Data/Bank1/MIFC/derivatives/nifti/MIFC%04d/T1/MIFC%04d_T1.nii', subjno, subjno);`

 The string (note that [strings](https://www.mathworks.com/help/matlab/characters-and-strings.html) are assigned with '' operators in MATLAB.) before the comma would be the path to your subject folder. Put a `%` wherever you want a variable to replace the string. Be sure to put in the same number of varibles as the `%` specifiers!

 In my case, I used `%04d`, the `d` means that the variable I'd like to put is an integer, and the `04` means that I want to pad the integer with zeros so that there are always four digits. I repeated `subjno` twice because there are two `%04d` in the string above. Remember that `subjno` is a variable that stores the double `1`.

 The resulting `T1path` would thus be:

 ![sprintf_example](uploads/d8f46fd55540f814956823e0ebb2971a/sprintf_example.png)

 You can also check how to create a proper [format specification](https://www.mathworks.com/help/matlab/ref/sprintf.html#btf_bfy-1_sep_shared-formatSpec) on the MATLAB site.

2. [`strcat`](https://www.mathworks.com/help/matlab/ref/strcat.html)

`strcat` concatenates the string into the format desired. If I'd like to achieve the same as what I did with `sprintf`, I can do it this way:

`subjno = 1;`

`T1path = strcat('/bml/Data/Bank1/MIFC/derivatives/nifti/MIFC',num2str(subjno,'%04d'),'/T1/MIFC',num2str(subjno,'%04d'),'_T1.nii');`

The resulting `T1path` would thus be:

![strcat_example](uploads/f29ca2c2ae7411c6b3aa13e2b7f4197a/strcat_example.png)

You can also replace `num2str(subjno,'%04d')` with `sprintf('%04d',subjno)`, which will do the same trick for you.

3. [`fullfile`](https://www.mathworks.com/help/matlab/ref/fullfile.html)

`fullfile` creates the full path for you. It adds the slash separator according to the operating system that you are using. I used to prefer `sprintf` but recently I try to work with `fullfile` if I am creating paths. To point to the T1 for MIFC subject#1, I do:

`subjno = 1;`

`T1path = fullfile('/bml','Data','Bank1','MIFC','derivatives','nifti',sprintf('MIFC%04d',subjno),'T1',sprintf('MIFC%04d_T1.nii',subjno));`

The resulting `T1path` would then be:

![fullfile_example](uploads/94f2f1121a9d68bde66df5f32a5b6593/fullfile_example.png)

### Fetching directory names with MATLAB

There are also some cases where your subject have missing numbers so that you cannot simply write a `for` loop to loop through all subjects. In that case, I use the `dir` command to ask MATLAB to fetch subject directories for me. Let's say if I want to fetch all the young subjects in the project, I can do the following:

`allyoung = dir('/bml/Data/Bank1/MIFC/derives/nifti/YA/')`

Since I have 135 young subjects, my `allyoung` variable would then be a 137x1 struct.

![dir_without_wildcard](uploads/485b27d43127ea0737ed2c77a85ddac2/dir_without_wildcard.png)

Note that in the example figure uses a different path with the text example above because I have mounted the `MIFC` directory into my local home directory using [`sshfs`](https://blog.gtwang.org/linux/sshfs-ssh-linux-windows-mac-os-x/).

The two extra struct are the `.`(current) and the `..`(parent) folder. If you don't want the extra two folders to appear in the result, specify the search along with wildcards(\*):

`allyoung = dir('/bml/Data/Bank1/MIFC/derives/nifti/YA/MIFC*')`

Now only search results with the name `MIFC` will show up.

![dir_with_wildcard](uploads/ccc7b3f491cb1270651a736d43ced0c8/dir_with_wildcard.png)

## Saving and running SPM batchfiles

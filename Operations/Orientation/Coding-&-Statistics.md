Ver.210728 by Jing-Yu Chuang

Ver.190309

[Ver.150723](https://www.evernote.com/shard/s604/sh/a128f266-be73-4dcc-819c-4254293fb9b5/df9dafd34e95337bd6cf11d29c26bc65)

# 目錄 Table of Content

[程式與統計 Coding & Statistics](#%E7%A8%8B%E5%BC%8F%E8%88%87%E7%B5%B1%E8%A8%88-coding-&-statistics)

* [程式 Coding](#%E7%A8%8B%E5%BC%8F-coding)
  * [Python](#python)
  * [R](#r)
  * [Matlab](#matlab)
  * [Linux](#linux)
  * [Git](#Git)
  * [Tensorflow](#Tensorflow)
* [統計 Statistics](#%E7%B5%B1%E8%A8%88-statistics)

# 程式與統計 Coding & Statistics

## 程式 Coding

### Python

#### Practice

* 

#### Resources

* Tutorials
  * [Google's Python Class](https://developers.google.com/edu/python)
  * [Real Python](https://realpython.com/tutorials/python/)
  * [Python Official Website](https://docs.python.org/zh-tw/3/tutorial/)
  * [W3Schools](https://www.w3schools.com/python/)
* Debugging
  * [Stackoverflow](https://stackoverflow.com/questions)

### R

#### Resources

* [Coursera-R Programming](https://www.coursera.org/learn/r-programming/home/info)
* [Coursera-Exploratory Data Analysis](https://www.coursera.org/learn/exploratory-data-analysis)
* [Cookbook for R](http://www.cookbook-r.com/)
* [Swirl](https://swirlstats.com/)
* [R語言翻轉教室](http://datascienceandr.org/)

**Coursera-R-Mentoring**

* [Webpage](https://corytu.github.io/CourseraRMentoring/) by Yu-Zhen

**R-Tutirial by Yu-Zhen**

* [R-Tutorial](https://corytu.github.io/RLanguagePresentations/docs/R-Tutorial.html#1)
* [Plotting with R: ggplot2](https://corytu.github.io/RLanguagePresentations/docs/Plotting_with_R_ggplot2.html#1)

### Matlab

實驗室購買的是2012b和2016a，Windows版的ISO檔在伺服器上有。

如果想在自己的電腦上安裝Matlab，可以先寫信申請授權（請註明是台大腦心所吳老師實驗室）。

#### Authorization

#### 

#### Resources

* [Introduction to Matlab](https://drive.google.com/drive/u/2/folders/0BzjcVTDEKxivb2dfcmZXZG5vVUE)
* [Coursera-MATLAB程序開發入門](https://www.coursera.org/learn/matlab)

### Linux

* [Linux command tutorial](https://drive.google.com/drive/u/2/folders/0BzjcVTDEKxivNzRCUTFYbDBOV0E)
* [鳥哥的 Linux 私房菜](http://linux.vbird.org/)
  * 非常好用的網站，請多多查看！
* [Coursera-The Unix Workbench](https://www.coursera.org/learn/unix/home/info)
* Close the terminal but keep Matlab running remotely:

  `nohup matlab -nodisplay -nosplash < XXX.m > output_XXX`
* XXX.m is the Matlab code you want to run
* output_XXX is the text file of the output log when you run XXX.m

  e.g., `nohup matlab -nodisplay -nosplash < test_r.m > output_test_r`
* Close the terminal but keep iPython running remotely:

  `nohup ipython XXX.py &> output_XXX`
* XXX.py is the python code you want to run
* output_XXX is the text file of the output log when you run XXX.py

  e.g., `nohup ipython main_model.py &> output_main_model` or
* [Fixing problems with the tab key with XFCE desktop](https://www.starnet.com/xwin32kb/tab-key-not-working-when-using-xfce-desktop/)
  * 在XQuartz介面裡按右鍵 → Applications → Settings → Windows Manager → 選 Keyboard 標籤 → Clear the Switch window for same application setting

### Git

* [Git notes](https://gitlab.com/brain-and-mind-lab/notes-for-bml/notes-for-git)

### Tensorflow

* [Tensorflow notes](https://gitlab.com/brain-and-mind-lab/notes-for-bml/notes-for-analysis/blob/master/Tensorflow.md)

#### Open Anaconda on server

Type `anaconda-navigator` on Terminal.

#### Install Tensorflow

```plaintext
conda create --name tf_gpu tensorflow-gpu
activate tf_gpu
```

#### Check Tensorflow version

`conda list | grep tensorflow`

#### Use Tensorflow

After you successfully install Tensorflow, you need to activate the tf_gpu environment.

```plaintext
conda activate tf_gpu
```

Then, you are under the tf environment :

<div>

<span dir="">![](/brain-and-mind-lab/notes-for-bml/bmlab-wiki-home/-/wikis/uploads/Screen%20Shot%202019-03-30%20at%202.09.33%20PM.png)</span>

</div>## 統計 Statistics
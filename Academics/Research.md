## 目錄 ##

*  [MRI](#mri)
   *  [MRI 分析方法](#mri-分析方法)
   *  [fMRI Second level analysis](#fmri-second-level-analysis)
   *  [Voxel size re-size](#voxel-size-re-size)
   *  [Conjunction analysis](#conjunction-analysis)
   *  [Masking](#masking)
      *  [Create brain mask from templates](https://gitlab.com/brain-and-mind-lab/notes-for-bml/notes-for-analysis/blob/master/creat_brain_mask.md)
   *  [fMRI brainshots](#fmri-brainshots)
   *  [P value correction](#p-value-correction)
   *  [Small volume correction](#small-volume-correction)
*  [ERP](#erp)
   *  [Theory](#theory)
   *  [Experimental procedure](#experimental-procedure)
   *  [ERP analysis](#erp-analysis)
   *  [Software and training resources](#software-and-training-resources)
*  [Projects](#projects)
*  [International Conferences](#international-conferences)
   * [Conferences](#conferences)
   * [Poster](#poster)
   * [獎學金](#獎學金)

## MRI ##

### MRI 分析方法
[腦影像分析方法](https://drive.google.com/drive/u/2/folders/0BzjcVTDEKxivb3Y3bkt5MDBGNmM)

### fMRI Second level analysis
[fMRI second level analysis](https://gitlab.com/brain-and-mind-lab/notes-for-bml/notes-for-analysis/blob/master/GLM_second_level.md)

### Voxel size re-size
[Resize the voxel size of a nifti image](https://gitlab.com/brain-and-mind-lab/notes-for-bml/notes-for-analysis/blob/master/brian_image_voxel_resize.md)

### Conjunction analysis
[Conjunction analysis](https://gitlab.com/brain-and-mind-lab/notes-for-bml/notes-for-analysis/blob/master/conjunction_analysis.md)


### Masking

[Create brain mask from templates](https://gitlab.com/brain-and-mind-lab/notes-for-bml/notes-for-analysis/blob/master/creat_brain_mask.md)

### fMRI brainshots

[fMRI brainshots processing](https://gitlab.com/brain-and-mind-lab/notes-for-bml/notes-for-analysis/blob/master/fMRI_brain_snapshot_processing.md)

### P value correction
[Correction for fMRI cluster results](https://gitlab.com/brain-and-mind-lab/notes-for-bml/notes-for-analysis/blob/master/fMRI_p_value_correction.md)

### Small volume correction
[Small volume correction](https://gitlab.com/brain-and-mind-lab/notes-for-bml/notes-for-analysis/blob/master/small_volume_correction.md)
## ERP ##

ERPs are measured by means of electroencephalography (EEG). An event-related potential (ERP) is the measured brain response that is the direct result of a specific sensory, cognitive, or motor event.

### Theory

ERPs are the averaged EEG data under the same conditions. In order to obtain
an appropriate signal to noise ratio, we often present the same type of stimuli (stimuli under the same conditions) multiple times, and then averaged those EEG data. They are thought to reflect the summed activity of postsynaptic potentials produced when a large number of similarly oriented cortical pyramidal neurons (in the order of thousands or millions) fire in synchrony while processing information (*Peterson et al., 1995*)

![The-Event-Related-Potential-ERP-technique-2](uploads/c2cf31a5e041ecfda11764586323c679/The-Event-Related-Potential-ERP-technique-2.png)

#### Different ERP waveforms

Please refer to [this paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3016705/) to study the physiological meaning of different waveforms]

### Experimental procedure ##

#### 實驗前注意事項

##### 洗頭券
* 自行列印（[參考](https://drive.google.com/file/d/1rbMEwjY63sPGL1AsB2ajv-H0DuhiP99X/view)）
* 印完後給老師蓋章，再拿給所辦蓋所張，即可使用
* 列印[洗頭券報帳名冊](https://drive.google.com/file/d/1BOmUyzMEjakUbB8mYgEbUhmGnFIxah2p/view)並記錄
* 每月和理髮部結帳一次（後付款，一月結一次）
* 結帳請阿姨開收據

##### ERP log
* 依照實驗需求自行設計，可以參考[NTUAC](https://drive.google.com/file/d/1YJ951phyRzpxzQrwIiuP_fnYzfcw97n7/view)或[DMERP](https://drive.google.com/file/d/1OuyASq9ouVbu-C5_unTcN8HiXdlp4iw-/view)


#### 實驗前準備

1. ERP筆電前一天需要充滿電
2. ERP電池要一直插著充電
3. 準備同意書、ERP log, NPT問卷、收據、洗頭券
4. 確認材料充足（導電膠、電擊貼紙、磨砂膏、酒精棉片、衛生紙、棉棒）
5. 確認電腦實驗流程（eprime）


#### 實驗流程

1. 使用酒精棉片＋磨砂膏，清潔臉部與兩耳後方
2. 量頭圍、用眉筆做記號，並記錄
3. 戴帽子，對齊眉筆記號與耳朵
4. 打膠
5. 插入電擊
6. 進入ERP實驗房間，接好所有線，連接電腦，降電阻
7. 實驗設置完成，可以給受試者看腦波


### ERP analysis
#### [Single subject data processing tutorial](single-subject-data-processing-tutorial)
*   [Re-referencing](single-subject-data-processing-tutorial#re-referencing)
*   [Artifact rejection](single-subject-data-processing-tutorial#artifact-rejection)
*   [Filter data](single-subject-data-processing-tutorial#filter-data)
*   [Elist](single-subject-data-processing-tutorial#elist)
*   [Assign bins](single-subject-data-processing-tutorial#assign-bins)
*   [Extract bins](single-subject-data-processing-tutorial#extract-bins)
*   [Compute ERP](single-subject-data-processing-tutorial#compute-erp)


### Software and training resources

*  [EEGLAB Toolbox](https://sccn.ucsd.edu/eeglab/index.php) – A freely available, open-source, Matlab toolbox for processing and analyzing EEG data
*  [ERPLAB Toolbox](http://erpinfo.org/erplab) – A freely available, open-source, Matlab toolbox for processing and analyzing ERP data


## Projects ##

### 主計畫
主要兩大計畫-ADM & BLO
*  ADM 是老化與決策的長期追蹤計畫
   *  一開始稱為ADMS > 第一次的兩年期追蹤，稱之為 ADMT > 後續追蹤 ADMU, ADMV...
*  BLO 則是抽血與飲食、和老化的關係

### 子計畫
老師鼓勵每個學生自己發想題目，所以我們有很多子計畫：
*  ADME 是以前的 RA 珺崴做的決策（跟 ADM 的 task 不同）與執行功能研究
*  DIET 是第二屆的淑韵在 ADM 下面追蹤的飲食，現在延伸在 BLO 中繼續收
*  TAD 是第三屆孜玲做的 rsfMRI 研究
*  第三屆玉臻的情緒計畫比較複雜，ACT 是拍照當臉孔刺激材料、PRP 是對圖片景象做 rating、FRD 是對臉孔做紙本 rating、最後 AEE 是把臉孔跟圖片一起做電腦 task
*  Ref 是第三屆怡君做的 ERP 與白質研究。跟李佳霖老師共指
*  DEF 是第四屆東衛做的情緒回饋 (feedback) 研究
*  CSR 是跟 Angela 合作跨文化記憶和老化的研究
*  [DMERP](https://gitlab.com/brain-and-mind-lab/research-projects/dmerp) 是柏伃和幸儀做的腦波決策研究
*  XDPX 是第五屆啟權做的non-linear rule的老化研究
*  ADHDDM 是第五屆俊毅做的比較不複雜的過動症決策研究
*  APJ 是助理一慈做的機率研究
*  VI 是第六屆幸儀做的information theory研究
*  PROS 是第六屆哲宇做的prosocial老化研究
*  DSN 是第七屆雅婷及第八屆以莊做的spatial/social navigation研究
*  MDM 是第七屆鈺軒做的道德決策研究
*  LEGO 是第七屆宛儒及第八屆志瑜做的樂高機器人研究
*  [AI Robohon](airobohon) 為老師和心理系團隊和電資團隊一起合作做的social network representation 的 project
*  TIME 是第九屆苡蓁做的時間感研究
*  MIFC 是第九屆芝嘉做的用mutual information分析functional connectivity研究
*  DSN_AGE 是第十屆易修做的空間探索（spatial navigation）與老化的相關研究
*  SIEM 是第十屆景伃做的Seeking Information Equilibrium in Music的研究
*  AXC_RES 是第十屆麟涵做的關於跨文化rt-fMRI 的 functional connectivity 的研究
*  NTUSEC_LEGO 是第十一屆一欣繼續做的樂高機器人研究
*  DSN_PRIOR 是第十一屆之邑做的老化於不確定性下的決策研究 
*  XXXX 是第十二屆力陞還不確定做甚麼
*  TIME 是第十二屆庭萱做的時間感研究

## International Conferences ##

老師鼓勵每位學生都要出國參加國際會議，因此我們實驗室有很大的機會可以在國際會議上呈現你的研究！

### Conferences
*  [SFN - Society for Neuroscience](https://www.sfn.org/):
   *  Abstract大概五月截止
   *  Conference時間大約為十一月
   *  三萬多人的超大型會議，會場很大，posters很多，因此需要事前排好你想看哪些poster、聽哪些演講，不然會迷失在會議中

*  [OHBM - Organization for Human Brain Mapping](https://www.humanbrainmapping.org/i4a/pages/index.cfm?pageid=3267&pageid=1):
   *  以人類大腦為主題的會議
   *  Abstract大概十一、十二月截止
   *  Conference時間大約為六月底左右
   *  會議比較小，但是有很多活動（和食物），比較能夠針對一個主題做更細緻的討論，是個很活潑的會議

*  [CNS - Cognitive Neuroscience Society](https://www.cogneurosociety.org/)
*  [Art and Affect in the Predictive Mind](https://sites.google.com/view/artandaffectinpp/home?authuser=0)
   *  'Art and Affect in the Predictive Mind' is a three-day international and interdisciplinary conference that brought together philosophers, art historians, and cognitive scientists for the first systematic exploration of the interactions between predictive processing and aesthetics.
   *  Organising committee: University of York (York Music Psychology Group, Department of Philosophy, Department of History of Art)
   *  [Recordings](https://sites.google.com/view/artandaffectinpp/recordings?authuser=0)

### Poster

**該如何製作 Poster？用什麼軟體？字體大小？該注意什麼？**

可以參考：[ScientificPosters.pdf](uploads/1b7dc2fa34fa908c52cb027e6db6f6a4/ScientificPosters.pdf)

*  軟體
   *  InDesign 是專門製作文書海報等等的軟體
   *  Adobe Illustrator
   *  PS (Photoshop) 主要是做圖用的，以“海報”來說，InDesign比較適合
*  [BML歷屆poster](https://drive.google.com/drive/u/2/folders/0BzjcVTDEKxivQkhCUnMtd0E1LVk)

### 獎學金
最後編輯時間：20231130
最後編輯人：王力陞

若你已經被accepted，除了做poster以外，獎學金也可以讓你的荷包沒那麼重的負擔。主要可以拿到錢錢的方式有幾個：
1. 計畫主持人所編列之出國考察費用
2. [國科會-補助國內研究生出席國際學術會議](#國科會-補助國內研究生出席國際學術會議)
3. [台大醫學院](#台大醫學院-醫學生出席國際會議及海外實習服務交換獎學金)
4. 其他補助（各個學院都會有自己的補助，所以如果有其他共指在其他學院，也可以申請該學院的）

#### [國科會-補助國內研究生出席國際學術會議](https://www.aca.ntu.edu.tw/w/aca/GAADService_21071211044877383)
**會前一個月要申請，一收到post或者是文章被accept就趕快申請申請吧！不然怕行政單位會處理很久**

**審查文件**

(1) 申請書。

(2) 論文被接受發表之證明文件。

(3) 擬發表之論文摘要。（如果是poster，就直接將投稿時的摘要傳上去就好）

(4) 指導教授推薦函（註明外語能力）。

(5) 其他有助於審查之文件 (如論文全文)。`強烈建議，一定要附上論文全文！這樣申請到獎學金的機率比較大！（可能也比較多錢？）`

(6) 系統中會需要填寫：
1. *會議之性質以及學術地位*
2. *簡述從事之研究以及研究表現*

#### [台大醫學院-醫學生出席國際會議及海外實習服務交換獎學金](https://www.mc.ntu.edu.tw/oia/Fpage.action?muid=1961&fid=1296)

如前面所說，在申請的時候，你必須告知你**已經**有申請科技部獎學金，然後他會問你，科技部給你多少錢，他在決定要給你多少。

通常，科技部獎學金給的比較多，所以建議兩個都申請。因為科技部如果沒有過，可能醫學院還會補貼

**審查文件**

這些文件全部要印出紙本~~非常浪費...

(1)申請表（請至中心首頁相關表單下載）。

(2)國際會議主辦單位致申請者本人之正式邀請函，或論文被接受發表之證明文件（信函或電子郵件）等影本。

(3)擬發表之論文摘要及英文論文全文。（應以首次發表之論文為限。論文全文需以期刊格式撰寫。申請者須為會議摘要第一作者，該論文若係合著者，每一論文以補助一人為限。申請發表的研討會摘要需有指導教授列名，申請人的論文全文需署名本校/院/系科所。）

(4)近年最具代表性之著作抽印本或影印本（五篇以內）。

(5)國際會議日程表、會議有關資料及其他有助審查之資料。


如果你申請到以上的獎學金，非常恭喜你！至少出國參加國際會議有些補貼。除此之外，可以把這些獎學金列入你的**CV**中，對申請學校會有幫助！

#### 回國後報帳

上述獎學金是等你已經參加完會議，他們才會根據你的登機證等等證明，撥錢給你。報帳流程，請參考：[參加國際研討會-報帳事前準備](admin#參加國際研討會-報帳事前準備)

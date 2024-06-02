# Git 

主要是記錄一下Git常用的技巧和觀念，因為工作都用GitHub Desktop，怕忘記所以筆記下來。

先至[官網](https://www.git-scm.com/)進行對應的OS進行安裝

> 以下使用MAC進行，Git指令基本上不變，有些終端機的指令需要自己轉換

---

## 目錄

* [全局配置](#全局配置)
* [專案配置](#專案配置)
* [Git 檔案觀念](#Gitㄦ檔案觀念)
    * [Track](#Track)
    * [Commit](#Commit)
    * [Checkout](#Checkout)
    * [Revert](#Revert)
* [遠端儲存庫](#遠端儲存庫)
* [分支](#分支)

---

## 全局配置
我們先配置全局環境中，Git的`email`和`name`
```bash!
git config --global user.name "yourname" 
git config --global user.email "test@gmail.com" 
```

查看全局環境的Git configuration，通常會放在使用者目錄底下，並且檔案名為`.gitconfig`，也就代表是一個隱藏檔案。

我們可以查看，或是直接修改configuration的內容

```bash!
cat /Users/<username>/.gitconfig
```
or
```bash!
cat ~/.gitconfig 
```

![截圖 2024-06-02 上午9.01.18](https://hackmd.io/_uploads/HJ-C4rYVA.png)

當然，如果你只是要查看資料內容而已也可以使用
```bash!
git config --list
```


就是這麼簡單!!!


---

## 專案配置
```bash!
git init
```

查看所有.git內的檔案
```bash!
ls 
```

查看專案內的git，

```bash!
cat config
```


![截圖 2024-06-02 上午11.21.04](https://hackmd.io/_uploads/rkBVSvKNR.png)

---

## Git 檔案觀念

首先，我們在專案中建立一個markdown檔案，並且插入標題Hello到檔案中

```bash=
touch README.md && echo "# Hello" >> README.md
```

查看目前所有專案中檔案的狀態
```bash=
git status
```

![截圖 2024-06-02 上午11.29.52](https://hackmd.io/_uploads/ByVIDDYNC.png)


可以看到，剛剛建立的README.md檔案尚未被加入追蹤(untracked)

### Track

把檔案加為追蹤中
```bash=
git add <fileName>
```

![截圖 2024-06-02 上午11.33.07](https://hackmd.io/_uploads/SJnr_wK4C.png)


再加兩個檔案，分別是.md和.txt檔案
![截圖 2024-06-02 上午11.41.48](https://hackmd.io/_uploads/ry6b9PtEA.png)


這時候我只想先追蹤`shoppingList.md`，可以使用萬用字元加上副檔名

```bash=
git add *.md
```

![截圖 2024-06-02 上午11.43.11](https://hackmd.io/_uploads/B1WD9DKN0.png)


### Commit
接著我們要把剛剛那些已追蹤的檔案做成一個版本，也可以稱為暫存點

```bash=
git commit -m "description"
```

![截圖 2024-06-02 上午11.47.33](https://hackmd.io/_uploads/ByUPiwYNR.png)

這時候看一下檔案的狀態，可以發現剛剛的README.md和shoppingList.md都已經被存成暫存點了，所以不會出現在檔案暫存區中。

![截圖 2024-06-02 上午11.48.52](https://hackmd.io/_uploads/B1B2oPK4A.png)

查看所有的暫存點
```bash=
git log
```
![截圖 2024-06-02 上午11.50.14](https://hackmd.io/_uploads/SkJdnvYE0.png)

當然，我們不需要看到這麼詳細的資訊像是日期，可以用
```bash=
git log --oneline
```

![截圖 2024-06-02 上午11.53.30](https://hackmd.io/_uploads/r1cTnDYVA.png)

現在我們需要修改shoppingList.md跟README.md的檔案內容，並且存到暫存點`snapshot1`

```bash=
git commit -m "snapshot1"
```

看一下log
![截圖 2024-06-02 上午11.58.26](https://hackmd.io/_uploads/rJwXAwFNA.png)

可以看到現在的主要暫存點(版本)已經指向到snapshot1了

但我們想看一下到底snapshot1中的shoppingList.md跟init的shoppingList.md到底差在哪?

首先，先複製init前面的暫存點ID

```bash=
git diff a620b0e shoppingList.md
```

可以看到差別在，我多加了一個蔬菜到購物清單中
![截圖 2024-06-02 中午12.04.35](https://hackmd.io/_uploads/B1IwyOY4A.png)

### Checkout

這時候我想到，蔬菜其實已經買好了。所以想要把shoppingList.md再回到一開始的版本，但是不能動到README.md裡面的內容該怎麼辦

![截圖 2024-06-02 中午12.08.54](https://hackmd.io/_uploads/rkYDldtNC.png)

```bash=
git checkout <暫存點ID> <fileName>
```

![截圖 2024-06-02 中午12.13.49](https://hackmd.io/_uploads/By7q-dFEA.png)

再查看專案中檔案的狀態，可以看到shoppingList.md已經回到指定版本的狀態，但是這時候需要再存到暫存點，才是可以的喔！

### Revert 

![截圖 2024-06-02 中午12.16.45](https://hackmd.io/_uploads/ByVBzdYN0.png)

這時候又想到蔬菜可以多買放冷藏，所以我想把蔬菜再加回來

```bash!
git revert <暫存點ID>
```

這時候我們只要存檔即可

![截圖 2024-06-02 下午1.35.19](https://hackmd.io/_uploads/ByYoVYKVR.png)

再回來看log，可以看到我們多了一個commit
![截圖 2024-06-02 下午1.36.58](https://hackmd.io/_uploads/B1hbrtFEA.png)

---

## 遠端儲存庫
遠端儲存庫是可以讓我們多人協作的一個雲端平台，常見的有:
1. GitHub
2. Bitbucket
3. GitLab
4. Amazon CodeCommit
5. 等等...

> 接下來會使用GitHub練習


首先先到GitHub中建立一個儲存庫，新增後會如下圖:

![截圖 2024-06-02 下午2.04.06](https://hackmd.io/_uploads/ryAYpYFNR.png)




檢視專案中remote的儲存庫
```
git remote -v
```

添加專案遠端儲存庫的網址，並且使用origin的簡稱代替這段網址
```bash!
//git remove add <nickname> <url>
git remote add origin https://github.com/Zheng-Yan-Zhong/Git-practice.git
```

![截圖 2024-06-02 下午1.56.03](https://hackmd.io/_uploads/ByiFFFtN0.png)

如果想改名origin -> mainRepository
```bash=
git remote rename origin mainRepository
```

![截圖 2024-06-02 下午1.59.31](https://hackmd.io/_uploads/B1mL5tFV0.png)


第一次推到遠端儲存庫的時候，branch名為`master`

```bash!
//git push <nickname> <branch>
git push mainRepository master
```


遠端儲存庫就會有本地的Git的版本和檔案
![截圖 2024-06-02 下午2.04.27](https://hackmd.io/_uploads/H1CdoYtN0.png)



## 分支
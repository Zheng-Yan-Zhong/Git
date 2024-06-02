# Git

主要是記錄一下 Git 常用的技巧和觀念，因為工作都用 GitHub Desktop，怕忘記所以筆記下來。

先至[官網](https://www.git-scm.com/)進行對應的 OS 進行安裝

> 以下使用 MAC 進行，Git 指令基本上不變，有些終端機的指令需要自己轉換

---

## 目錄

- [Git](#git)
  - [目錄](#目錄)
  - [全局配置](#全局配置)
  - [專案配置](#專案配置)
  - [Git 檔案觀念](#git-檔案觀念)
    - [Track](#track)
    - [Commit](#commit)
    - [Checkout](#checkout)
    - [Revert](#revert)
  - [遠端儲存庫](#遠端儲存庫)
  - [分支](#分支)

---

## 全局配置

我們先配置全局環境中，Git 的`email`和`name`

```bash!
git config --global user.name "yourname"
git config --global user.email "test@gmail.com"
```

查看全局環境的 Git configuration，通常會放在使用者目錄底下，並且檔案名為`.gitconfig`，也就代表是一個隱藏檔案。

我們可以查看，或是直接修改 configuration 的內容

```bash!
cat /Users/<username>/.gitconfig
```

or

```bash!
cat ~/.gitconfig
```

![截圖 2024-06-02 上午9.01.18](./images/1-1.png)

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

查看所有.git 內的檔案

```bash!
ls
```

查看專案內的 git，

```bash!
cat config
```

![截圖 2024-06-02 上午11.21.04](./images/1-2.png)

---

## Git 檔案觀念

首先，我們在專案中建立一個 markdown 檔案，並且插入標題 Hello 到檔案中

```bash=
touch README.md && echo "# Hello" >> README.md
```

查看目前所有專案中檔案的狀態

```bash=
git status
```

![截圖 2024-06-02 上午11.29.52](./images/1-3.png)

可以看到，剛剛建立的 README.md 檔案尚未被加入追蹤(untracked)

### Track

把檔案加為追蹤中

```bash=
git add <fileName>
```

![截圖 2024-06-02 上午11.33.07](./images/1-4.png)

再加兩個檔案，分別是.md 和.txt 檔案
![截圖 2024-06-02 上午11.41.48](./images/1-5.png)

這時候我只想先追蹤`shoppingList.md`，可以使用萬用字元加上副檔名

```bash=
git add *.md
```

![截圖 2024-06-02 上午11.43.11](./images/1-6.png)

### Commit

接著我們要把剛剛那些已追蹤的檔案做成一個版本，也可以稱為暫存點

```bash=
git commit -m "description"
```

![截圖 2024-06-02 上午11.47.33](./images/1-7.png)

這時候看一下檔案的狀態，可以發現剛剛的 README.md 和 shoppingList.md 都已經被存成暫存點了，所以不會出現在檔案暫存區中。

![截圖 2024-06-02 上午11.48.52](./images/1-8.png)

查看所有的暫存點

```bash=
git log
```

![截圖 2024-06-02 上午11.50.14](./images/1-9.png)

當然，我們不需要看到這麼詳細的資訊像是日期，可以用

```bash=
git log --oneline
```

![截圖 2024-06-02 上午11.53.30](./images/1-10.png)

現在我們需要修改 shoppingList.md 跟 README.md 的檔案內容，並且存到暫存點`snapshot1`

```bash=
git commit -m "snapshot1"
```

看一下 log
![截圖 2024-06-02 上午11.58.26](./images/1-11.png)

可以看到現在的主要暫存點(版本)已經指向到 snapshot1 了

但我們想看一下到底 snapshot1 中的 shoppingList.md 跟 init 的 shoppingList.md 到底差在哪?

首先，先複製 init 前面的暫存點 ID

```bash=
git diff a620b0e shoppingList.md
```

可以看到差別在，我多加了一個蔬菜到購物清單中
![截圖 2024-06-02 中午12.04.35](./images/1-12.png)

### Checkout

這時候我想到，蔬菜其實已經買好了。所以想要把 shoppingList.md 再回到一開始的版本，但是不能動到 README.md 裡面的內容該怎麼辦

![截圖 2024-06-02 中午12.08.54](./images/1-13.png)

```bash=
git checkout <暫存點ID> <fileName>
```

![截圖 2024-06-02 中午12.13.49](./images/1-14.png)

再查看專案中檔案的狀態，可以看到 shoppingList.md 已經回到指定版本的狀態，但是這時候需要再存到暫存點，才是可以的喔！

### Revert

![截圖 2024-06-02 中午12.16.45](./images/1-15.png)

這時候又想到蔬菜可以多買放冷藏，所以我想把蔬菜再加回來

```bash!
git revert <暫存點ID>
```

這時候我們只要存檔即可

![截圖 2024-06-02 下午1.35.19](./images/1-16.png)

再回來看 log，可以看到我們多了一個 commit
![截圖 2024-06-02 下午1.36.58](./images/1-17.png)

---

## 遠端儲存庫

遠端儲存庫是可以讓我們多人協作的一個雲端平台，常見的有:

1. GitHub
2. Bitbucket
3. GitLab
4. Amazon CodeCommit
5. 等等...

> 接下來會使用 GitHub 練習

首先先到 GitHub 中建立一個儲存庫，新增後會如下圖:

![截圖 2024-06-02 下午2.04.06](./images/1-18.png)

檢視專案中 remote 的儲存庫

```
git remote -v
```

添加專案遠端儲存庫的網址，並且使用 origin 的簡稱代替這段網址

```bash!
//git remove add <nickname> <url>
git remote add origin https://github.com/Zheng-Yan-Zhong/Git-practice.git
```

![截圖 2024-06-02 下午1.56.03](./images/1-19.png)

如果想改名 origin -> mainRepository

```bash=
git remote rename origin mainRepository
```

![截圖 2024-06-02 下午1.59.31](./images/1-20.png)

第一次推到遠端儲存庫的時候，branch 名為`master`

```bash!
//git push <nickname> <branch>
git push mainRepository master
```

遠端儲存庫就會有本地的 Git 的版本和檔案
![截圖 2024-06-02 下午2.04.27](./images/1-21.png)

## 分支

9 分鐘架一個 PttBBS
===

Author: r2 ``<holishing aT ccns.ncku.edu.tw>``

---

## Why?

* 可以從中釐清 Ptt 站長、各級站務、板主具體權限為何，是否可查看使用者個資、刪改文章，履行監督之責
* 協助重現 PttBBS 系統既有問題(Bugs)，實踐開源參與、回饋的精神
* ~~可以架個個人 BBS 站自 high~~ ( [範例](https://www.clam.ml) )

---

## 假設

* 你已經碰過一點 Linux 環境常用的終端機指令了
* 如果沒有也沒關係，只是不保證能 9 分鐘架完而已 (!?

---

## 步驟 0. 安裝作業系統

* 安裝方式1: 開虛擬機 (VMware, VirtualBox) 安裝 Debian/Ubuntu
* 安裝方式2: 在 Windows 10 啟用 [WSL2](https://docs.microsoft.com/zh-tw/windows/wsl/install-win10), 在 Windows 市集安裝 Debian/Ubuntu

---

### 文件提及相關指令權限說明

(root)
```bash
echo hello
```
表示上述指令要用到 `root` 權限

(bbsadm)
```bash
echo hello
```
表示上述指令要用到 `bbsadm` 這個使用者的權限

---

## 步驟 1. 建立 bbs/bbsadm 帳號

(root)
```bash
groupadd --gid 99 bbs \
&& useradd -m -d /home/bbs -g bbs -s /bin/bash --uid 9999 bbsadm
```

* 若需啟用 ssh 登入，請另外利用 `vipw` [編輯使用者帳戶資訊](https://github.com/ptt/pttbbs/wiki/INSTALL#%EF%BC%91-%E5%BB%BA%E7%AB%8B-bbs--bbsadm-%E5%B8%B3%E8%99%9F)

---

## 步驟 2. 安裝套件

(root)
```bash
apt install -y git python bmake gcc clang ccache libevent-dev pkg-config sudo
```

---

## 步驟 3. 取得 PttBBS 原始碼

(root)
```
sudo -iu bbsadm
```
切換至 bbsadm 使用者

(bbsadm)
```
git clone http://github.com/ptt/pttbbs.git
```
抓取原始碼

---

## 步驟 4. 修改設定檔

依據您的需求, 修改 pttbbs.conf (Big5 編碼)
```bash
vim -c 'set fenc=big5 enc=big5 tenc=utf8' -c 'e!' pttbbs.conf
```
若在 WSL2 底下，你也可以使用 VScode 來輔助編輯 Big5 編碼檔案

---

如果是在 64bit 的作業系統編譯安裝 PttBBS，請將以下定義取消註解 (`//`)：

```c
#define TIMET64
```

在大部分 Linux 核心作業系統，請將以下定義取消註解 (`//`)：

```c
#define SHMALIGNEDSIZE (1048576*4)
```

---

## 步驟 5. 編譯 PttBBS

編譯前先記得 
```
    - bbsadm - $ alias make=pmake
```
或是將以下 `make` 指令都改成 `pmake`

---

在 `/home/bbs/pttbbs` 下執行 

```
    - bbsadm - $ make all install clean 
```

---

開始編譯：

![](http://i.imgur.com/s8gRIux.png)

---

編譯完成：

![](http://i.imgur.com/VYVwRPv.png)

---

## 步驟 6. 架新站才要做的相關設定

**要協助將BBS站資料搬家時務必注意。**

**如果您的 BBS中已經有資料了, 請務必不要執行此部分步驟**

---

若確定自己是要架新站，請執行：

(bbsadm)
```bash
cd ~/pttbbs/sample; make install
cd /home/bbs; bin/initbbs -DoIt
```

---

如果 `initbbs` 參數沒打對的話會出現一些訊息：

![](http://i.imgur.com/mlZum4f.png)

---

## 步驟 7. 啟動 BBS

(bbsadm)
```
~/bin/shmctl init
~/bin/mbbsd -d -e utf8 -p 8888
```

* 注意這邊刻意將預設顯示編碼改成 UTF-8，給架站者方便

---

## 步驟 8. 測試是否能連線

```
telnet localhost 8888
```

有正常顯示登入畫面代表成功了！

---

## 步驟 9. 取得 BBS 站長權限

new 一個帳號叫SYSOP, 然後 logout 再 login, 這樣子就會擁有站長權限囉~

---

## 更多資源請參考

https://github.com/ptt/pttbbs/wiki/INSTALL/

---

## 是不是很簡單呢?

---

## 還是覺得很難?

歡迎 email 到我的信箱，我的信箱在哪裡? 不告訴你!
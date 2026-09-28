# 英文讀書計畫助手 — GitHub Pages 設定教學

這份教學會帶你把 `webapp` 這個資料夾發布成一個手機隨時打開的網址，之後每週只要在電腦上跑一個指令，手機就會自動看到最新教材。

整體流程是：

```
電腦：文章 → generate_weekN_pdfs.py（跟以前一樣）
      → publish_week.sh N（新增的一步：匯出 + 上傳到雲端）
手機：打開你的網址 → 按「立即重新整理」→ 看到最新一週教材、可以複習單字
```

---

## 一、第一次設定（大約 15-20 分鐘，只需要做一次）

### 步驟 1：申請 GitHub 帳號

1. 打開 https://github.com/signup
2. 用你的 email 註冊一個免費帳號（不需要信用卡）
3. 記得你的帳號名稱（例如 `ivykuo123`），後面會用到

### 步驟 2：在 GitHub 上建立一個新的 repository（倉庫）

1. 登入後，右上角按 `+` → `New repository`
2. Repository name 填：`english-study-app`（或你喜歡的名字，全部小寫、不要空白）
3. 選 **Public**（GitHub Pages 的免費方案需要 Public repo）
4. 不要勾選 "Add a README file"（保持空的 repo）
5. 按 `Create repository`
6. 建立後，畫面上會有一串網址，例如：
   `https://github.com/ivykuo123/english-study-app.git`
   把這串網址記下來，等一下會用到

### 步驟 3：把 webapp 資料夾變成這個 repo

打開終端機（Mac 的「終端機」App），輸入以下指令（記得把路徑跟網址換成你自己的）：

```bash
cd "你的 webapp 資料夾路徑"
git init
git add -A
git commit -m "first publish"
git branch -M main
git remote add origin https://github.com/你的帳號/english-study-app.git
git push -u origin main
```

第一次 push 時，如果跳出視窗要你登入 GitHub，就照畫面指示登入即可（GitHub 現在通常會開瀏覽器讓你授權，不用輸入密碼）。

### 步驟 4：啟用 GitHub Pages

1. 到你的 repo 頁面（`https://github.com/你的帳號/english-study-app`）
2. 點上方選單的 `Settings`
3. 左側選單找到 `Pages`
4. 在 "Build and deployment" 底下的 "Branch" 選單，選擇 `main`，資料夾選 `/ (root)`
5. 按 `Save`
6. 等 1-2 分鐘，重新整理頁面，畫面上會出現一個網址，長得像：
   `https://你的帳號.github.io/english-study-app/`

這就是你以後要在手機上打開的網址！

### 步驟 5：手機上把網址加到主畫面

1. 用手機瀏覽器（Safari / Chrome）打開上面那個網址
2. 確認可以看到「英文讀書計畫 AI 助手」的頁面
3. 點分享按鈕 → 「加入主畫面」（iPhone）或選單 →「加到主畫面」（Android）
4. 以後就可以像 App 一樣點桌面圖示打開

---

## 二、之後每週的使用流程

1. 跟以前一樣，把新文章丟給 Claude，產生 `weekN/generate_weekN_pdfs.py`
2. 在終端機執行：

   ```bash
   cd "2: 英文讀書計畫 資料夾路徑"
   ./publish_week.sh N
   ```

   （把 `N` 換成週數，例如 `./publish_week.sh 14`）

   這個指令會自動：
   - 把 weekN 的教材匯出成 `webapp/data/weekN.json`
   - 更新 `webapp/data/manifest.json`
   - `git commit` + `git push` 到 GitHub

3. 等 1-2 分鐘讓 GitHub Pages 更新
4. 在手機上打開網址（或主畫面的圖示），按「立即重新整理」
   - 之後每次打開網頁也會自動嘗試重新整理，不一定要手動按

---

## 三、常見問題

**Q: 網頁打開後說「同步失敗」怎麼辦？**
A: 檢查手機是否有網路連線；也可能是 GitHub Pages 還在部署中（剛 push 完等 1-2 分鐘再試）。就算同步失敗，網頁也會顯示上次同步下來、存在手機本機的教材，不會整個打不開。

**Q: `git push` 時要我輸入帳號密碼，但密碼一直錯？**
A: GitHub 從 2021 年起不再接受帳號密碼登入，需要用個人存取權杖（Personal Access Token）或是讓瀏覽器授權登入。如果跳出瀏覽器視窗選「Authorize」通常就可以了；如果一直卡住，把錯誤訊息貼給 Claude，我可以幫你排查。

**Q: 我想要多個裝置（手機 + 平板）都能看，可以嗎？**
A: 可以，只要在每個裝置的瀏覽器打開同一個網址即可，教材是從雲端抓的，跟裝置無關。但「單字複習」的記憶紀錄（記得/不記得、下次複習時間）是存在各裝置的瀏覽器本機裡，不會互相同步。

**Q: 這個網址其他人看得到嗎？**
A: 因為是 Public repo，理論上知道網址的人都看得到內容（就像一般網站一樣），但不會出現在 Google 搜尋結果，也没有任何個資（沒有姓名、帳號、Email）。如果之後想完全私密，需要升級 GitHub 付費方案才能用 Private repo 的 Pages 功能。

**Q: 我不想再用某一週的教材了，怎麼從網頁上移除？**
A: 到 `webapp/data/manifest.json` 手動把那個週數從 `weeks` 陣列移除，然後 `git add -A && git commit -m "remove weekN" && git push`。也可以直接把對應的 `weekN.json` 檔案刪掉。

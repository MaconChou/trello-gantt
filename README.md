# Trello 甘特圖

把 Trello 看板上的卡片畫成甘特圖。可依成員篩選,直接修改卡片日期、查看與新增留言,並匯出 Excel 與 PDF。

純前端網頁,不需要自己架伺服器。可以直接用瀏覽器開啟,也可以掛成 Trello Power-Up,在看板上按一個按鈕開啟。

## 功能

- **選擇看板**:列出你有權限且未封存的看板,可多選。
- **甘特圖**:依看板分組,顯示每張卡片的開始日期、結束日期與成員。
  - 已完成的卡片以淡色顯示,未完成且已逾期的卡片加紅框,並以橘色線標出今天。
  - 只有單一日期的卡片會顯示為 1 天的色條。
  - 兩個日期都沒有的卡片不會出現,頁面會顯示略過的張數。
- **成員篩選**:下拉選單可只看某位成員,或只看「未指派」的卡片。
- **修改日期**:點卡片名稱或色條開啟視窗,直接修改開始與結束日期,儲存後寫回 Trello。
- **留言**:在同一個視窗查看卡片所有留言(含作者與時間),可新增留言,並編輯自己寫的留言。
- **匯出**:
  - Excel(.xlsx):看板、列表、卡片、開始日期、結束日期、天數、已完成、成員、連結。
  - PDF:A3 橫式,圖太長時自動分頁。
  - 兩者都只輸出目前篩選後看到的卡片。
- **區塊收合**:「連線 Trello」與「選擇看板」完成後自動收起,只留一行摘要,點標題可再展開。

## 檔案

| 檔案 | 用途 |
| --- | --- |
| `trello-gantt.html` | 主程式,甘特圖的所有功能都在這個檔案 |
| `index.html` | Power-Up 的 Iframe 連接器,只在掛成 Power-Up 時使用 |
| `README.md` | 本說明 |

## 事前準備:取得 API Key 與 Token

1. 到 <https://trello.com/power-ups/admin> 建立一個 Power-Up,複製 **API 金鑰**(Key)。**密碼(Secret)用不到,請勿外流。**
2. 把下面網址中的 `你的API金鑰` 換成你的 Key,用瀏覽器開啟並按「允許」,頁面顯示的那串文字就是 **Token**:

   ```
   https://trello.com/1/authorize?expiration=never&scope=read,write&response_type=token&name=TrelloGantt&key=你的API金鑰
   ```

   - `scope=read,write`:修改日期與留言需要寫入權限。如果只想看圖,可改成 `scope=read`。
   - `expiration=never`:Token 不會過期。想保守一點可改成 `30days`。

## 使用方式

### 方式一:直接在本機使用

下載 `trello-gantt.html`,用瀏覽器開啟即可。這種方式不需要設定 Power-Up 或任何網址。

### 方式二:掛成 Trello Power-Up

1. 把 `index.html` 與 `trello-gantt.html` 放到任何公開的 HTTPS 空間,例如 GitHub Pages、Netlify、Cloudflare Pages。
2. 到 Power-Up 管理頁面設定:
   - **Iframe connector URL**:`https://你的網域/路徑/index.html`
   - **Allowed Origins**:`https://你的網域`
3. 在看板的 **Power-Ups → Custom** 加入這個 Power-Up,看板右上角會出現「甘特圖」按鈕。

> OAuth 2.0 的「回呼 URL」不需要填,本專案沒有使用 OAuth 流程。

### 操作流程

1. 在「連線 Trello」貼上 API Key 與 Token,按「載入看板」。
2. 勾選要處理的看板,按「產生甘特圖」。
3. 用「顯示成員」篩選。
4. 點卡片開啟視窗,修改日期或處理留言。
5. 按「匯出 Excel」或「匯出 PDF」。

## 注意事項

### 安全

- API Key 與 Token 只存在使用者瀏覽器的 localStorage,不會傳送到 Trello 以外的地方。
- **Token 等同你的 Trello 權限。** 不要寫進程式碼,也不要上傳到公開的 repository。外流時,請到 <https://trello.com/my/account> 的授權應用程式撤銷。
- 在共用電腦上使用後,請清除瀏覽器資料。

### 日期與時間

- 所有日期以台灣時間(UTC+8)顯示與計算。
- 修改日期時會保留卡片原本的時間。原本沒有日期的欄位,預設開始 09:00、結束 18:00。
- 修改日期時,至少要保留一個日期,且開始日期不能晚於結束日期。

### 留言

- 只能編輯自己寫的留言,別人的留言只能查看(Trello 的限制)。
- 目前不支援刪除留言。

### 匯出

- PDF 是把畫面轉成圖片再輸出,中文不會亂碼,但 PDF 內的文字無法選取或搜尋。
- Excel 與 PDF 的函式庫從 cdnjs 載入,使用時需要能連上網路。

### 限制

- 沒有任何日期的卡片不會顯示,因此也無法從這裡替它補日期。
- 目前不支援拖曳色條調整日期。
- 已封存的卡片不會讀取。
- Trello API 有請求次數限制(約每個 Token 每 10 秒 100 次),一次選很多看板時,讀取會比較久,程式已在看板之間加入短暫間隔。

## 疑難排解

| 狀況 | 可能原因與處理 |
| --- | --- |
| 載入看板失敗 | API Key 或 Token 貼錯,頁面會顯示 Trello 回傳的錯誤訊息 |
| 儲存日期或留言時顯示「權限不足」 | Token 只有讀取權限,請用 `scope=read,write` 重新產生 |
| 在 Power-Up 裡無法讀取資料(CORS 錯誤) | 確認 **Allowed Origins** 已加入網站網域 |
| 看板上沒有「甘特圖」按鈕 | 確認 Iframe connector URL 正確,且已在看板加入此 Power-Up |
| GitHub Pages 打開是 404 | 剛部署需要 1 到 2 分鐘,稍後再重新整理 |
| 更新檔案後畫面沒變 | 瀏覽器快取,按 Ctrl+Shift+R 強制重新整理 |

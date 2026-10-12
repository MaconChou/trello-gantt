# trello-gantt

feat: 初版 Trello 甘特圖與 Power-Up 連接器

初版內容:
- trello-gantt.html:Trello 甘特圖頁面
  - 輸入 API Key 與 Token 後,列出未封存的看板供勾選
  - 讀取所選看板卡片的開始日期、結束日期與成員
  - 依看板分組繪製甘特圖,顯示卡片名稱與成員
  - 已完成的卡片以淡色顯示,未完成且逾期的卡片加紅框,並標示今天
  - 日期以台灣時間(UTC+8)計算
  - 只有單一日期的卡片以 1 天的色條顯示,完全沒有日期的卡片略過
  - 匯出 Excel(.xlsx):看板、列表、卡片、開始與結束日期、天數、
    是否完成、成員、連結
  - 匯出 PDF(A3 橫式,過長時自動分頁)
- index.html:Trello Power-Up 的 Iframe 連接器,
  在看板上新增「甘特圖」按鈕,以全螢幕視窗開啟 trello-gantt.html

注意
- API Key 與 Token 僅存於使用者瀏覽器的 localStorage,不寫入程式碼
- Power-Up 設定需將 Iframe connector URL 指向 index.html,
  並在 Allowed Origins 加入此網站網域
- 目前為唯讀版本,尚不支援在頁面內修改卡片

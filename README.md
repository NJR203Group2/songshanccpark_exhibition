# tibame_songshanccpark_exhibition
Python 專案，用於自動爬取松山文創園區網站展覽資訊，透過 EasyOCR 辨識圖片文字，並利用 Gemini 模型進行活動資訊的結構化分析。

## 功能
1. 爬取展覽官網資訊（標題、日期、地點、描述、圖片）
2. 進行圖片文字辨識（EasyOCR）
3. 呼叫 Gemini API 進行結構化資料提取（活動日期、票價、地點、連結）
4. 結果輸出為 csv，儲存於 Google Drive 或本地端

## 系統需求
* **環境**：Google Colab
* **Python**：3.10+
* **套件安裝**
    ```bash
    pip install easyocr google-genai google-api-python-client \
      opencv-python-headless beautifulsoup4 requests
    ```

### Secrets 設定
請在 Colab 左側**「Secrets」**功能中建立以下金鑰：
1.  `GEMINI_API_KEY`：您的 Google Gemini API 金鑰
2.  `USER_AGENT`：自定義 HTTP Header，用於模擬瀏覽器
程式會自動從 Secrets 中讀取設定，不會在程式中顯示或記錄金鑰。

## 專案結構
```tree
.
├── tibame_songshanccpark_exhibition.ipynb    # 主程式 (Colab)
├── /drive/MyDrive/tibame_proj/               # 預設輸出資料夾
│   ├── YYYY_MM_DD_images/                    # 下載圖片
│   └── exhibition_info_YYYY_MM_DD.json       # 結構化資料
└── README.md
```

## 主要流程
1. 抓取展覽列表與詳細頁面
2. 下載文字與圖片內容
3. 使用 EasyOCR 辨識圖片文字
4. 使用 Gemini API 提取活動資訊
5. 匯出 CSV 結果

### 錯誤處理
1. API 重試與延遲機制
2. JSON 格式驗證
3. 圖片異常偵測與跳過

## 安全守則
1. API Key 不得寫入程式碼或 Notebook 輸出。
2. 請勿提交包含 `/content/drive` 絕對路徑的執行紀錄。
3. 若分享 Notebook，請先：
    * 清除所有輸出（Runtime → Restart & Clear Output）。
    * 可另外輸出結果檔供展示。

## CSV 輸出格式

CSV 檔案欄位範例說明如下。
| 欄位名稱 (Header) | 範例值 (第一筆資料) | 範例值 (第二筆資料) |
| :--- | :--- | :--- |
| `name` | 展覽名稱 A | 展覽名稱 B |
| `strt_dt` | 2025-10-01 | 2025-10-15 |
| `end_dt` | 2025-10-31 | 2025-11-05 |
| `loc` | 松山文創園區 | 製菸工廠北向一樓 |
| `wrktime` | 10:00 - 18:00 | 14:00 - 20:00 |
| `price` | 免費入場 | NT$ 300 |
| `note` | 無資訊 | 請提早預約 |
| `pageurl` | https://www.songshanculturalpark.org/... | https://www.songshanculturalpark.org/... |

### 檔案內容範例 (`exhibition_info_YYYY_MM_DD.csv`)

以下是 CSV 檔案的內容範例：

```csv
name,strt_dt,end_dt,loc,wrktime,price,note,pageurl
展覽名稱 A,2025-10-01,2025-10-31,松山文創園區,10:00 - 18:00,免費入場,無資訊,[https://www.songshanculturalpark.org/](https://www.songshanculturalpark.org/)...
展覽名稱 B,2025-10-15,2025-11-05,製菸工廠北向一樓,14:00 - 20:00,NT$ 300,請提早預約,[https://www.songshanculturalpark.org/](https://www.songshanculturalpark.org/)...
... (更多活動資料) ...
```

## 注意事項
1. 本專案僅供學習及研究使用。  
2. 若官方網站結構異動，請更新爬蟲選擇器（Selector）。  
3. Gemini API 回傳結果需人工抽查確認。  

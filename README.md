# 國衛共享圈

國家衛生研究院 2026 Green November 永續創新提案雛形：院內科研物資再利用與跨研究室共享平台。核心理念是「先在院內找，再決定要不要買」。

> 本網站為提案展示用雛形。物資、單位與數字皆為示範資料，不會儲存，重新整理即恢復。

## 線上瀏覽

https://ying-ying11.github.io/nhri-share-circle/

## 功能

| 頁面 | 內容 |
|---|---|
| 首頁 | 請購前查詢、運作流程、近效期物資、最新上架、交換日預告、研究室貢獻榜 |
| 物資 | 找物資（同義詞與語意媒合）、我要上架（拍照辨識標籤）、徵求牆、請購清單比對 |
| 借用 | 未來 14 天借用狀態、預約、逾期提醒 |
| 修好再用 | 送修申請、維修看板、每月工時上限、配件設計圖庫 |
| 技能互助 | 30 分鐘專長預約、求助媒合、排錯經驗卡 |
| 交換日 | 11/18 實體交換日報名、物品預登記、現場流程 |
| 成果 | 四項成效指標、上架到使用的漏斗、流通紀錄、月報 |
| 關於 | 想解決的問題、治理原則、發展藍圖、常見問題 |
| 我的 | 待回報、借用、徵求、申請、綠點 |
| 管理端 | 財產核可、待交付、借用確認、逾期未還、超過 30 天未媒合、送修評估（右上角切換為「管理端視角」） |

## 本機 AI

所有 AI 都在使用者的瀏覽器，或你指定的院內電腦上執行，不使用雲端 API key，資料不會送到外部 AI 服務。網路只用來第一次下載模型檔，之後由瀏覽器快取。

| 功能 | 技術 | 執行位置 |
|---|---|---|
| 語意搜尋：找物資、媒合徵求、請購清單比對 | Transformers.js 3.8.1 與 `Xenova/paraphrase-multilingual-MiniLM-L12-v2`（量化版，約 100 多 MB） | 瀏覽器 |
| 標籤辨識：讀出品名、規格、目錄編號、效期 | Tesseract.js 5.1.1（繁體中文與英文） | 瀏覽器 |
| 經驗卡整理、月報段落、標籤欄位整理（選配） | Ollama 與開源語言模型（預設 `qwen2.5:7b`，可自行更換） | 院內電腦或伺服器 |

使用方式：

- **語意搜尋**：點右上角「AI」→「啟用語意搜尋」。關鍵字找不到時會再用語意比對，結果標示為「AI 語意相近」。相似度門檻可以在同一個視窗調整。
- **標籤辨識**：「物資」→「我要上架」→ 選擇照片 →「辨識標籤」。辨識出的原始文字會一併顯示，方便核對。
- **本機語言模型**：設定好 Ollama 後，「技能互助」的「整理成經驗卡」與「成果」的「用本機 AI 改寫成段落」才會產生草稿；沒接上時，這些功能仍可手動使用。

AI 只提出候選或草稿。規格、相容性、安全，以及經驗卡是否公開，都由人確認。建議使用 Chrome 或 Edge。

### 連接 Ollama（選配）

1. 安裝 [Ollama](https://ollama.com)，下載模型：

   ```bash
   ollama pull qwen2.5:7b
   ```

2. 允許本網站連線，設定環境變數 `OLLAMA_ORIGINS` 後重新啟動 Ollama：

   - macOS
     ```bash
     launchctl setenv OLLAMA_ORIGINS "https://ying-ying11.github.io"
     ```
   - Windows：在「系統內容 → 環境變數」新增使用者變數 `OLLAMA_ORIGINS`，值為 `https://ying-ying11.github.io`
   - Linux（systemd）
     ```bash
     sudo systemctl edit ollama.service
     # 加入以下兩行後存檔
     # [Service]
     # Environment="OLLAMA_ORIGINS=https://ying-ying11.github.io"
     sudo systemctl restart ollama
     ```

3. 在網站右上角「AI」填入 Ollama 位址（預設 `http://localhost:11434`）與模型名稱，按「儲存並測試連線」。
4. 瀏覽器若詢問是否允許存取區域網路，請選擇允許。

注意事項：

- GitHub Pages 使用 HTTPS。瀏覽器允許 HTTPS 網頁連到本機的 `http://localhost`，但若 Ollama 架在院內另一台伺服器（例如 `http://10.x.x.x:11434`），會被當成混合內容擋下。院內伺服器的做法是把本網站也放在同一台伺服器，或替 Ollama 加上 HTTPS 反向代理。
- 讓 Ollama 接受其他電腦連線需設定 `OLLAMA_HOST=0.0.0.0`，並請限制在院內網路使用。

## 在自己的電腦上執行

不需要編譯或安裝套件。下載後在資料夾內執行：

```bash
python3 -m http.server 8000
```

再開啟 `http://localhost:8000`。直接點兩下 `index.html` 也能瀏覽，但本機 AI 需要以網址開啟。若要在這個網址連 Ollama，`OLLAMA_ORIGINS` 也要加入 `http://localhost:8000`（多個網址以逗號分隔）。

## 檔案

- `index.html`：整個網站，HTML、CSS 與 JavaScript 都在同一個檔案

## 參考做法

- [東京大學 ShareWel](https://sharewel.riise.u-tokyo.ac.jp)：工學研究團隊開發的校內物品再利用平台
- [MIT Rheaply](https://sustainability.mit.edu/article/mit-sized-neighborhood-swap)：研究室可刊登剩餘設備，也能張貼需求
- [密西根大學 Lab Swap Shop](https://michigannow.vpcomm.umich.edu/?p=68069)：實體交換空間，不到兩年為研究人員省下超過 100 萬美元

## 聲明

本專案為提案展示用途。網站中的物資、單位、人員與數字皆為示範資料，不代表國家衛生研究院的實際物資、流程或統計。

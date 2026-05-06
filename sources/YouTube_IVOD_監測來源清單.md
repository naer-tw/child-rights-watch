---
source_id: YouTube_IVOD_監測來源
title: YouTube + 立法院 IVOD 監測來源清單
publisher: 國教行動聯盟兒少人權監督平台
last_updated: 2026-04-26
related_issues: [跨議題]
---

# YouTube / IVOD 監測來源清單

> **使用方法**:從下面任一頻道找到關鍵直播後,執行:
> ```bash
> bash ~/clawd/tools/yt-to-text.sh "https://youtube.com/watch?v=XXX" "對應檔名"
> # 或對 IVOD 影片下載 mp4 後跑本平台:
> bash scripts/transcribe_audio.sh /path/to/影片.mp4
> ```
> 結果會自動進 `data/sources/`,然後在對應議題卡片加引用。

## 1. 立法院 IVOD(政府公聽會 / 委員會審查直播)

### 入口
- **IVOD 全文檢索**:[ivod.ly.gov.tw/Search/SearchKeyword](https://ivod.ly.gov.tw/Search/SearchKeyword)(網頁手動搜)
- **教育及文化委員會**:[ivod.ly.gov.tw/Demand/Meeting](https://ivod.ly.gov.tw/Demand/Meeting)
- **社會福利及衛生環境委員會**:同上

### 平台已知關注場次(待抓 IVOD 連結)
| 日期 | 主題 | 對應議題 |
|---|---|---|
| 2026-04-13 | 實驗教育三法修法公聽會 | PI-09 |
| 2026-04-15 | 性別平等教育法修法方向公聽會 | PI-08 |
| 2026-04-02 | 民眾黨團「完善少事法」公聽會 | PI-05 |
| 2026-04-10 | 民眾黨團「教師荒」公聽會 | PI-04 / PI-09 |
| 2026-04-14 | 司法院少事法部分條文修正草案諮詢會議 | PI-05(內部會議,可能無 IVOD)|

## 2. 衛福部 YouTube

- **官方頻道**:[YouTube @MOHW_TW](https://www.youtube.com/@MOHW_TW)
- **重點關注**:
  - 心理健康司年度成果發表
  - 兒少保護記者會
  - 性影像處理中心定期報告
  - 自殺防治成果發表

## 3. 教育部 YouTube

- **官方頻道**:[YouTube @MoeTaiwan](https://www.youtube.com/@MoeTaiwan)
- **重點關注**:
  - 校安通報年度檢討記者會
  - 性平教育成果發表
  - WISER 三級輔導推廣
  - 實驗教育論壇

## 4. 國家人權委員會 YouTube / 線上記者會

- **官方頻道**:[YouTube @NHRCofROCTaiwan](https://www.youtube.com/@NHRCofROCTaiwan)
- **重點關注**:
  - NAP 監督報告發表記者會
  - 兒童權利監測指標集 239 項發表(2026-02 已辦)
  - 校園師對生暴力專案報告 4641 件發表(2025-12 已辦)

## 5. 國教盟 / 合作夥伴 YouTube

- **國教盟 Facebook 直播**:[fb.com/twedumove](https://www.facebook.com/twedumove)(常用 FB 直播,需手動下載)
- **全國家長會長聯盟**(全長聯)
- **台灣兒童權利公約聯盟**
- **人約盟**(人權公約施行監督聯盟)
- **台少盟**

## 6. 抓取工作流

### 場景 A:已知 YouTube URL
```bash
bash ~/clawd/tools/yt-to-text.sh "https://www.youtube.com/watch?v=XXXX" "0415_性平公聽會_IVOD"
# 結果在 ~/clawd/tools/?(yt-to-text 預設輸出位置)
# 然後 mv 到 platform 並寫進對應 advocacy_action 引用
```

### 場景 B:已下載 mp4/m4a/mp3
```bash
bash scripts/transcribe_audio.sh /path/to/影片.mp4
# 自動走 Groq large-v3,放 data/sources/ 並產 .txt + .srt
```

### 場景 C:IVOD 下載
立法院 IVOD 影片可右鍵「儲存影片」下載成 mp4,然後同場景 B。

## 7. 高優先補抓清單(等用戶貼 URL)

| 優先度 | 影片 | 對應檔名建議 |
|---|---|---|
| 🔴 高 | 0413 實驗教育公聽會 IVOD | `0413_公聽會_IVOD_實驗教育三法.txt` |
| 🔴 高 | 0415 性平公聽會 IVOD | `0415_公聽會_IVOD_性平教育.txt` |
| 🟡 中 | NHRC 4641 件報告發表(2025-12) | `NHRC_師對生暴力_2024_記者會.txt` |
| 🟡 中 | NHRC 兒童權利監測 239 指標(2026-02) | `NHRC_兒童權利監測_2026-02.txt` |
| 🟡 中 | 0410 民眾黨教師荒公聽會 IVOD | `0410_公聽會_IVOD_教師荒.txt` |
| 🟢 低 | 0402 民眾黨少事法公聽會 IVOD | `0402_公聽會_IVOD_少事法.txt`(可替代 OCR 0402 PDF)|

> **編輯 SOP**:抓到 mp3/mp4 → 跑 transcribe_audio.sh → txt + srt 進 sources/ → 在對應 advocacy_action 加引用 → md_to_sqlite 重建。

## 8. 注意事項

- **YouTube 抓取依 yt-to-text.sh 邏輯**:可能需要 cookies(若影片需登入)
- **IVOD 下載**:有時段時段限制,建議錄製當週內處理
- **長片自動分段**:`transcribe_audio.sh` 已支援 >25MB 自動 ffmpeg 壓縮 + 20 分鐘段切割
- **Hallucination 清理**:大檔常含「请不吝点赞 订阅 转发」等中國 YouTube 訓練資料污染,腳本後處理已 strip

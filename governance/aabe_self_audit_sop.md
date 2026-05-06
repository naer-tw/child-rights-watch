# AABE 自我問責 SOP — 3 年回溯評估機制(governance/)

> **建立日**:2026-04-28(Wave 99)
> **依據**:用戶 Wave 95 確認「AABE 願意做自我問責」
> **適用對象**:每一份 `data/advocacy_actions/A-*.md`(原倡議 +3 年)

## 一、為什麼 AABE 要自審

平台主要監督政府,但 NGO 自己之倡議若帶來負面後果,**也要負責**。否則平台變成單向打擊工具,失去公信力。

## 二、3 年回溯觸發

每份 advocacy_action 之 `action_date` + 3 年到期時:
- 自動寫入 `advocacy_self_audit` 表(audit_date = today, triggered_at = action_date + 3y)
- 每月 maintenance 檢查未完成之 audit
- 主編收到提醒後啟動 4 人簽名流程

## 三、4 人簽名強制

| 角色 | 任務 | 簽核權 |
|---|---|---|
| 主編 | 撰寫 rationale,引用觀察證據 | 必須 |
| DPO | 確認資料引用無侵犯個資 / 兒少身分 | 必須 |
| 1 名兒少顧問 | 從兒少視角評估「當時主張對我們現在的影響」 | 必須 |
| **❗ 1 名 dissenter** | **必須是不同意原 AABE 立場之外部學者** | **必須(否則整份 audit 不算完成)** |

**dissenter 的角色**:防止 AABE 自我吹捧(「3 年前我們對了!」)。dissenter 必須:
- 是外部學者(非 AABE 成員)
- 對原倡議之核心立場持不同意見
- 必填 `reviewer_dissenter_minority_opinion` 欄位
- 該欄位內容必須在最終 audit 公開頁可見

## 四、評估三選一

| 結果 | 條件 | 行動 |
|---|---|---|
| `reaffirm` | 原立場成立,後續證據支持 | 繼續持有,但須記錄 dissenter 反對理由 |
| `revise` | 原立場大致對,但細節須修 | 公開修正之具體點 |
| `withdraw` | 原立場錯誤,有負面後果證據 | 公開撤回 + 道歉 + 承擔 |

## 五、observed_outcomes 結構

每筆 audit 必須列出 **正面 + 負面**證據:

```json
[
  {
    "type": "aligned_with_claim",
    "evidence": "2028 年 X 統計顯示 Y%,符合我們 2025 年預測",
    "strength": 0.7,
    "source": "https://..."
  },
  {
    "type": "contradicts_claim",
    "evidence": "2028 年 Z 統計顯示我們忽略的反例:...",
    "strength": 0.4,
    "source": "https://..."
  }
]
```

**強制原則**:**至少 1 條 contradicts_claim**(若主編只列 aligned 證據,DPO 必須要求補)。

## 六、公開規則

完成 audit 之 advocacy_action HTML 頁面:
- 加 `<details>` 區塊「3 年回溯評估」
- 預設展開
- 完整列 4 名簽核者(姓名可匿名為「主編 / DPO / 兒少顧問 / dissenter 學者(任職單位)」)
- dissenter 之 minority_opinion 必須完整顯示

## 七、首批待 audit 之倡議(action_date 已超過 3 年者)

當前(2026-04-28)平台之 advocacy_actions 多為 2024-2026 期間,**3 年觸發點將從 2027-Q1 起開始**。本 SOP 屬**前置部署**:平台架構先建,自審週期到來時自動觸發。

## 八、查核清單(每年 Q4 維護)

- ☐ 跑 `scripts/check_audit_due.py`(待開發)列出本年到期清單
- ☐ 主編開 4 人簽名邀請
- ☐ 30 天內完成 audit
- ☐ 寫入 `advocacy_self_audit` 表
- ☐ 重新 build advocacy 之 _public/ 頁面(顯示 3 年評估區塊)

## 九、與既有治理之關聯

- `child_safeguarding.md` §三點五:audit 引用之兒少敘事須去識別化
- `transcript_citation_sop.md`:audit 引用學者意見須遵循訪談 SOP
- `rbac.md`:dissenter 學者列為「外部專家」角色
- `document_governance.md` §3.5 vocab_quotability:audit 結論之引用層級

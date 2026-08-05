# 自動化工作流程設計 — 完成版

**這一包是已經做好、而且真的跑過的成品。** 你不用照著課堂重做一遍，
改一個檔案、貼一把金鑰，它就開始每週一早上幫你工作。

課堂上那顆練習用的 repo 是 [ai-workflow-workshop](https://github.com/young-ai-courses/ai-workflow-workshop) ——
想自己一步一步做一遍走那顆；想直接有東西用，留在這裡。

---

## 它跑出來長這樣

先看成果再決定要不要花五分鐘：**[reports/weekly/2026-W31.md](reports/weekly/2026-W31.md)**

那份不是範例檔，是這支程式真的抓了三個來源、16 篇文章，丟給 AI 寫出來的中文摘要。
你設好之後每週一早上會自己多一份，長得跟它一樣。

---

## 五分鐘讓它變成你的

**① 按右上角綠色 `Use this template` → Create a new repository → 取名字 → 建立**

不要用 Fork。fork 來的 repo，GitHub 預設**不會**跑排程，而且不會告訴你。

**② 改 `config.yaml`** —— 把 `feeds` 換成你要追蹤的來源，就這一個檔案

```yaml
feeds:
  - name: 你想追的東西
    url: https://那個網站的/rss
```

不知道 RSS 網址？多數網站是網域後面加 `/feed` 或 `/rss`。
沒有 RSS 的用 [Google News RSS](https://news.google.com/rss/search?q=關鍵字)。

**③ 拿一把免費金鑰**：[console.groq.com](https://console.groq.com) 註冊（不用信用卡）→ 建 API key

**④ 貼進你的 repo**：Settings → Secrets and variables → Actions → New repository secret
名字**一字不差**打 `GROQ_API_KEY`

**⑤ 到 Actions 分頁按啟用** ← 最多人漏這步。漏了的症狀是：設定全對，什麼都不會發生

**⑥ Actions → Weekly Competitor Digest → Run workflow**，等 15–30 秒

跑完 `reports/weekly/` 會多一份這週的。看到它就是成了。

---

## 確認自己真的做好了

```bash
pip install -r requirements.txt
python3 scripts/check_setup.py
```

它查七件事，包含那些**畫面上看起來都對、實際上永遠不會跑**的地方。
查不到的會標 ❔，不會假裝是綠的。

想在自己電腦上先跑一次看看：

```bash
export GROQ_API_KEY=你的金鑰
python3 scripts/weekly_competitor_digest.py
```

---

## 想改成完全不同的用途

排程、執行、把結果存回 repo 這三件**一行都不用動**，你只要換掉：

| 你要做的事 | 換哪裡 |
|---|---|
| 追別的網站 | `config.yaml` 的 `feeds` |
| 換 AI 講話的口氣 | `scripts/weekly_competitor_digest.py` 裡的 system prompt |
| 改成每天／每月 | `.github/workflows/weekly-digest.yml` 的 `cron` |
| 做完全不同的事 | 整支 `scripts/` 換掉，其他照舊 |

同一套架構可以做：每日 standup（抓 git log）、客戶新聞追蹤、論文追蹤（arXiv RSS）、
SEO 排名監控、專案進度報告（GitHub Issues API）。

**卡住了就問 AI。** 這包附了一位知道課堂內容的助教：
免費 ChatGPT / Gemini 使用者打開 [貼上版助教.md](貼上版助教.md) 複製貼上即可；
有 Claude Code / Codex 的，在資料夾裡打開它直接講話。

---

## 兩個會咬人的地方

**Actions 預設是停用的。** 不會報錯，就是安靜地什麼都不做。

**`weekly-digest.yml` 裡的 `permissions: contents: write` 不要刪。**
GitHub 預設 workflow 只有唯讀權限；刪掉之後程式照跑、Actions 照樣綠燈，
但報告不會出現，錯誤訊息還很難懂。

兩件 `check_setup.py` 都會抓。

---

## 這裡面有什麼

```
├── reports/weekly/2026-W31.md          真的跑出來的成果，先看這個
├── config.yaml                         ← 你唯一要改的檔案
├── scripts/
│   ├── weekly_competitor_digest.py     本體
│   └── check_setup.py                  七項機器驗
├── tests/test_weekly_digest.py         三條回測鎖
├── .github/workflows/weekly-digest.yml 每週一 09:00 (台北) 自動跑
├── 貼上版助教.md                        免費 AI 用這份
└── .claude/                            Claude Code / Codex 的助教
```

---

*2026-08-06 線上課「自動化工作流程設計」— 完成版*
*練習版 → [ai-workflow-workshop](https://github.com/young-ai-courses/ai-workflow-workshop)*

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

它查**八件事**，包含那些**畫面上看起來都對、實際上永遠不會跑**的地方：

| 查什麼 | 為什麼要查 |
|---|---|
| 你在自己的 repo 裡 | 直接 clone 我這顆的話你沒有寫入權限，後面全都是白做 |
| `config.yaml` 換過了 | 沒換就還是在追我的來源，不是你的 |
| workflow 的寫入權限還在 | 刪掉的話程式照跑、Actions 照樣綠燈，但報告不會出現 |
| Actions 啟用了 | 最多人漏這步，症狀是「設定全對，什麼都沒發生」 |
| `GROQ_API_KEY` 設好了 | 沒設不會壞，但摘要那段會是空的 |
| 測試是綠的 | 確認沒改壞地基 |
| 報告真的存在 | 有檔案才算數 |
| workflow 在 GitHub 上真的跑過 | 本機好了 ≠ 線上會跑 |

**查不到的會標 ❔，不會假裝是綠的。**

想在自己電腦上先跑一次看看：

```bash
export GROQ_API_KEY=你的金鑰
python3 scripts/weekly_competitor_digest.py
```

沒有金鑰也可以跑 —— 報告照出，只是 AI 摘要那段會寫「⚠️ 沒有 GROQ_API_KEY」。

---

## 這包附了一位助教（知道那天課上講了什麼）

不用記得課堂內容，讓它帶你走。**看你手上有什麼，三條路都可以：**

| 你有什麼 | 怎麼用 |
|---|---|
| **只有免費的 ChatGPT / Gemini / Claude 網頁版**（多數人） | 打開 **[貼上版助教.md](貼上版助教.md)** → 按複製 → 貼進去送出。什麼都不用安裝，全程在瀏覽器 |
| **Claude Code** | 在這個資料夾裡打開它，直接講話。它會自動讀 `CLAUDE.md` 和 `.claude/` |
| **Codex** | 一樣在資料夾裡打開。它讀 `AGENTS.md`；`.claude/` 裡的檔案對它就是一般 markdown，要用的時候整份讀進來 |

三條路**內容一模一樣**，差別只在後兩條看得到你的檔案、能直接幫你跑檢查。

### 跟它說這三句就好

| 你說 | 它做什麼 |
|---|---|
| **開始教我** | 一次一步帶你把這顆改成解決你自己那件重複工作的東西。說完一步會**停下來等你**，不會一次倒一堆 |
| **檢查一下我做好了沒** | 跑上面那八項機器驗，拿證據回答你，不接受「我覺得好了」 |
| **幫我複習** | 把整條線重新串一遍，你也可以只挑忘記的那段聽 |

（不知道要說哪句，就打「助教」，它會問你。）

第一次用，它可能會先花兩分鐘問你八題（你的程度、想自動化什麼、希望它怎麼跟你講話），
答完寫成 `CLAUDE.local.md`，之後講話就照你的節奏。**不想做就說不用，它不會催。**

裡面實際有什麼：

```
CLAUDE.md                          Claude Code 的入口
AGENTS.md                          Codex 的入口（內容同上，多了「要自己讀檔」的說明）
貼上版助教.md                       免費 AI 用這份，一鍵複製
.claude/agents/teaching-assistant.md   助教本人，帶著整堂課的內容
.claude/agents/student-profiler.md     入學診斷（那八題）
.claude/skills/workshop-guide/         「開始教我」走這個流程
.claude/skills/blind-spot-check/       「檢查一下」走這個流程
.claude/skills/student-intake/         「診斷我」走這個流程
```

⚠️ **它不會跟你要 API 金鑰，你也不要貼給它。** 金鑰只走 GitHub repo 的 secret。

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

**卡住了就跟助教說「開始教我」**（見上面那節），它會一次一步帶你改。

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

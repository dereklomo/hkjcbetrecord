# 賽後熱帳（Agent 預設只讀本檔）

> **角色：** 常駐表 A / A′ + 粗 WR + 分支速查 + 失誤短表 + 路由。  
> **結算：** 半贏=W、半輸=L、走=P（P 不進 WR 分母）。  
> **執行規則：** `analysis-checklist.md`（唯一 checklist）。  
> **硬規則：** `.github/skills/defensive-betting-analysis/SKILL.md`  
> **冷檔：** 批次敘事 → `post-match/batches/` · 索引 → [`post-match/INDEX.md`](post-match/INDEX.md)  
> **Legacy 全文快照（只讀）：** [`post-match/batches/2026-07-archive-legacy.md`](post-match/batches/2026-07-archive-legacy.md)  
> **禁止：** 把整份 legacy 當預設 context；cover 回寫；A′ 併入表 A WR。

## Agent 讀序（強制）

1. `analysis-checklist.md`
2. **本檔**（熱）
3. `post-match/INDEX.md` → **最近 2 批** `post-match/batches/*`
4. 冷 batch / legacy — **僅** formal review、同分支重複失效、用戶點名歷史
5. skill 硬規則

## 給新 Agent 的摘要（熱）

- **主目標：** 表 A Track B 勝率（W1–W6；可多腳；可空）
- **A′：** 表 A 空時 0～1 腳可小注；獨立帳；路徑寬優先；原 Obs 已合併
- **每次賽果：** ① append **本檔**表 A/A′（若有）+ 粗 WR/速查 ② 新建/append **batch 檔** ③ 更新 INDEX ④ checklist §9
- **預設不改** skill 半贏=W 硬結算

## 路由

| 需求 | 去哪 |
|------|------|
| 正式命中 | 下方表 A |
| 準表小注命中 | 下方表 A′ |
| 完整 PASS 逐場 B 帳 | legacy 表 B |
| 最近批次 §9 / 賽果敘事 | INDEX → 最近 2 batch |
| 07-10～早期 narrative | legacy 只讀 |

---

## 常駐表 A · 正式 PLAY 命中帳

> 僅正式事前 `PLAY`。半贏=W、半輸=L、走=P（P 不進 WR 分母）。

| 日期批 | 場次 | 盤口 | 賽果 | Track B | 分支 | 備註 |
|--------|------|------|------|---------|------|------|
| 07-10 前後 | 西班牙 vs 比利時 | 比利時 **+1** | 2-1 西勝 | **P**（輸 1 走） | dog-plus1 | 現金保本；P 剔除 |
| 07-13 | 馬爾多納多 vs 艾比安FC | 艾比安 **+0.25** | 1-1 | **W**（和=半贏） | dog-plus0.25 | 命中 |
| 07-14 | 阿美利加明尼路 vs 隆德里納 | 隆德里納 **+0.25** | 1-1 | **W** | dog-plus0.25 | 命中 |
| 07-15 | 羅奇代爾 vs 南區奇兵 | 羅奇代爾 **+0.25** | 1-2 | **L** | aus-semi-dog-shallow | 失手 |
| 07-15 | 萊卡特 vs 半島電力 | 半島電力 **+1** | 2-1 萊勝 | **P**（客輸 1 走）或依實際結算 | dog-plus1 | 以原盤為準；常見走/半 |
| 07-15（GPT 對照） | 全南天龍 vs 忠南牙山 | 牙山 **平手** | 3-4 客勝 | **W** | level-ball-lean | Grok 原 PASS 過嚴；**不**回寫 Grok 為 PLAY |
| pending | 芝加哥 vs 溫哥華 | 溫哥華 **0**（原 PLAY） | — | pending | level-ball-lean | **待補賽果** |
| ~07-17 | 利昂 vs 阿特拿斯（Atlético Nacional） | 利昂 **-0.25** | **2-3** | **L** | shallow-fav-0.25-light-lean | 跨國；light lean；原 home 檔 |
| 07-18 | Gold Coast Knights vs（NPL） | Knights **-0.5/-1** @1.84 | **1-2** | **L** | npl-aus / home-fav-0.75 | **watchlist**；NPL 正式極稀 |
| 07-18 | Lillestrøm vs KFUM | Lillestrøm **-0.5/-1** @1.81 | **2-1** | **W** | league-fav-0.75（挪超） | Medium + 榜階梯；**勿與 NPL 合併** |
| 07-19 | Hammarby vs Degerfors | Hammarby **-1.5/-2** @1.96 | **4-0** | **W** | league-fav-deep Strong | 深讓需 ≥2 球 |
| 07-19 | Halmstads vs BK Häcken | Häcken **-0.5/-1** @1.82 | **0-2** 客 | **W** | league-fav-0.75 away Med–Strong | 客讓勝 1 即 W |
| 07-23 | Houston Dynamo vs DC United | Houston **-0.75** @1.86 | **1-1** | **L** | league-fav-0.75 MLS | §1 升格；**和 = L** |
| 07-23 補 | LAFC vs Real Salt Lake | LAFC **-0.75** @1.85 | **3-1** | **W** | league-fav-0.75 MLS | §1 升格；勝 2 全贏 |
| 07-24 | Botafogo-RJ vs Vitória-BA | Botafogo **-0.75** @1.91 | **0-0** | **L** | league-fav-0.75 BRA | §1 升格；**和=L**；賽前已寫交手和磁鐵 |
| 07-25 | Västerås vs Örgryte | Örgryte **+1** @1.86 | **2-0** 主 | **L**（客負 2） | league-dog-plus1 | §1d 防漏；Strong 主穿 2 球 |
| 07-25 | Atlante vs CF América | Atlante **+1** @1.80 | **1-1** | **W**（和=全贏 +1） | league-dog-plus1 MX | §1d 防漏；和局護盤命中 |
| 07-27 | Rosenborg vs Fredrikstad | Rosenborg **-0.75** @1.84 | **4-0** | **W** | league-fav-0.75 NOR | §6b P1；挪超 Medium 半一；勝 4 |
| 07-28 | Juventude vs Avaí | Juventude **-0.75** @1.77 | **1-0** | **W** | league-fav-0.75 BRA-B | 巴乙 Medium 半一；勝 1 = W |
| **08-01 卡2** | **Häcken vs Kalmar** | Häcken **-0.5/-1** @1.90 | **1-1** | **L** | league-fav-0.75 SWE W1 | 表 A；**和=L**；**formal：應硬否**（@1.90≥線；見 §1b-2） |

**正式 PLAY 粗 WR（有 W/L；P 剔除；分支勿混算單一 WR）：**  
早期 dog/level：艾比安 W、隆德里納 W、羅奇代爾 L、牙山（GPT）W；  
近期上盤：里昂 L、Knights L、Lillestrøm W、Hammarby W、Hacken W、Houston L、LAFC W、Botafogo L、Rosenborg W、Juventude W、**08-01 Häcken L**；  
**07-25 下盤 +1：** Örgryte **L**、Atlante **W** → 累計約 **10W / 7L**（小樣本；半島/比利時 +1 多為 **P** 剔除）。  
**MLS -0.75 正式：** Houston **L** + LAFC **W** = **1W1L**。  
**歐北穩聯賽 -0.75 正式：** Lillestrøm W、Hacken(07-19 客) W、Rosenborg W、**Häcken(08-01 主) L** = **3W1L**。  
**巴乙 -0.75 正式：** **Juventude W**（n=1；≠ Botafogo 甲級和殺混算）。  
**dog +1 正式（有 W/L）：** Atlante **W** + Örgryte **L** = **1W1L**（Lincoln lean 0-0 為 **W** 不回寫）。  
**聯賽 -0.75 和殺（跨池 n · Grill 08-01）：** Houston + Botafogo + **Häcken 08-01** = **n=3** → formal **已** → checklist **§1b-2 常態閘**（**不改 skill**）。  
**分支 W 帳仍分開：** 歐北 ≠ MLS ≠ 巴甲/巴乙；**僅和殺 n 跨池**；dog-plus1 ≠ thin +0.25；**同隊不同日**仍各計一 pen。

---

---

## 常駐表 A′ · 準表 A 結算（**可小注 · 不進表 A WR**）

> 賽前標 **準表 A′ / Soft PLAY**（**僅表 A 空** 時 0～1 腳）。半贏=W、半輸=L、走=P。  
> **分結構／子帳計 WR**（`league` / `cup` / `euro-2leg` / `women` / `u20` / `semi`）；**禁止**併入表 A 粗 WR。  
> 歷史：原「表 Obs」已遷移至此；舊 §1-max 誤標正式改 A′ 的腳備註寫明。

| 日期批 | 場次 | 盤口 | 賽果 | Track B | 結構標籤 | 備註 |
|--------|------|------|------|---------|----------|------|
| 07-30 | Gornik Zabrze vs Fenerbahçe | Gornik **+1** @1.85 | **1-1** | **W**（和=全贏 +1） | euro-2leg · Strong-blast · path-wide | 賽前 §1-max 誤標正式 → **Obs → A′ 遷移**；負≥2 才 L 未觸發 |
| 07-30/31 | Inter Turku vs Başakşehir | Inter Turku **+0.75** @1.89 | **2-0** | **W**（客勝 2 全贏 dog） | euro-2leg · Strong-blast · path-wide | 賽前 **A′ Soft PLAY**；表 A none；路徑冠軍；Strong 客未爆破 |
| **08-01 卡1** | **Gold Coast Utd vs Gold Coast Knights** | **GCU +1/+1.5** @1.74 | **1-0** | **W**（主勝全贏 dog） | **semi** / npl-aus · path-wide | 賽前 **A′ Soft PLAY**；表 A none；**NPL 子帳**；勿併表 A / 勿與 Knights 表 A L 混算 |
| **08-02 卡1** | **Estudiantes LP vs Defensa** | **Est -0.5/-1** @1.78 | **3-0** | **W**（勝≥2 全贏半一） | **league** · ARG · path-narrow | 賽前 **A′ Soft**；表 A none；勿併表 A |
| **08-02 卡2** | **Sydney Olympic vs APIA** | **Olympic +1/+1.5** @1.83 | **1-3** | **L**（負 2 穿 +1.25） | **semi** / npl-aus · path-wide | 賽前 **A′ Soft**；NPL 子帳；**不**與 GCU W 混洗；勿併表 A |

**表 A′ 粗 WR（有 W/L；分結構）：**  
- **euro-2leg dog +1／+0.75：** Gornik **W** + Inter Turku **W** = **2W0L**（n=2；**勿**併表 A）  
- **semi / npl dog +1／+1.25：** GCU **W** + Olympic **L** = **1W1L**（獨立子帳；Knights 表 A L 不洗白）  
- **league ARG C -0.75：** Estudiantes **W** = **1W0L**（n=1）  
- **08-02 pending A′：** Orenburg +1.25

---

---

## 常駐表 B · 分支速查（完整逐場表見 legacy）

> 完整表 B 逐場：post-match/batches/2026-07-archive-legacy.md

### 分支 WR 速記（累計粗算 · 更新用）

| 分支 | 粗結果 | WR-first 含義 |
|------|--------|----------------|
| **cup-fav-0.75** | 穿（伊、競賽會）+ 殺（基輔、慶南、連菲類）→ **mixed** | 外圍 **強/中/輕** 分檔；不硬拒、不預設 PLAY |
| **cup-fav-1.0** | 波希 L、比奧格特 L | 對 **-1** 更嚴；需更強外圍檔 |
| **league-shallow-0.25** | 保地 W、CRB W、蒙特利爾 W；**里昂 light 跨國 L** | clear 聯賽 lean 仍友好；**light+跨國勿套 soft band** |
| **league-fav-0.75** | 歐北 **3W1L**；MLS 1W1L；Botafogo L；**Juventude 巴乙 W**；**跨池和殺 n=3** | **P1 維持 + §1b-2 閘**；巴乙 n=1；W 帳分開、和殺 n 跨池 |
| **league-fav-deep** | **Hammarby -1.5/-2 W**；Motherwell -2 **P** / -2.5 **L** | Strong 仍可能剛好球數；拒深維持 |
| **level-ball-lean** | 牙山 W；Sheriff 客 lean cover | 外圍 lean + 平手可跟；真五五仍 PASS；cover 不回寫 |
| **2nd-leg** | 斯利納單場 W 仍出局 | 推斷必須加總比分 |
| **npl-aus / aus-semi** | 羅奇 L；Knights 正式 L；Manly cover；Heidelberg 和殺；Hume lean **L** | **watchlist 維持**；正式極稀；cover **不回寫** |
| **euro-qual-light-0.25** | AEK/Hammarby **L**；Beşiktaş/Hajduk **W** → mixed | 首回 light **維持 PASS**；cover 不回寫 |
| **euro-qual-fav-1.0 / deep** | Qarabağ 和殺；Twente **L**；Boca **P**；HJK cover；Benfica 逆 | 拒 -1/深 **Strategy Held** 居多 |
| **league-dog-plus1** | 比利時/半島 **P**；**Atlante W**；**Örgryte L**（Strong 主 2-0） | 防漏可出手；**Strong≤1.50 主場爆破** watch；輸≥2=L |
| **league-shallow-light** | 07-25 Viborg/Huracán/Tijuana cover；SJK lean **L** | light 仍 **不**正式；cover 不回寫 |

### 對未開賽的用法（WR-first）

- **盃賽熱門 -0.75**：外圍力度分檔 + 本分支 mixed + 輪換/回合 → `WR-inferred`；soft 降信心，非自動硬 PASS。  
- **聯賽 -0.25**：B 帳偏 W + 外圍 **clear** lean → 主寫 WR case；薄價只標 auxiliary；**light / 跨國 → PASS 或 Low**。  
- **NPL 半一**：Knights watch → **Low**，正式極稀，Track A 保守。  
- **穩定聯賽 -0.75 Medium**（~1.60–1.70 + 榜階梯）：可評估小注 PLAY（Lillestrøm 型）；勿與 NPL 混。  
- **出手優先序（執行主表）：** 見 **`analysis-checklist.md` §6b**（P1 歐北半一 → P2 clear -0.25 → P3 dog +1 非 Strong → … → 避 light/NPL/深/-1）。

### 對 SKILL.md

- Dual Track：**B primary / A auxiliary**；League Shallow `-0.25`；盃 gap tier；真五五 = **外圍無 lean**（非圖 AH 對稱 alone）。  
- 常駐表 A/B + 分支 watchlist / 速查清單：**本檔維護**（原 home 已合併）。  
- **08-01 formal 已 consummate：** 執行層 **§1b-2**（跨池和殺 n、價位推定、雙錨翻案）；**仍不改** skill 硬規則。再升級門檻見熱帳 §6（如 n=5）。

---

## 正式 PLAY 失誤短表（執行層）

## 正式 PLAY 失誤反省（執行層 · 2026-07-24 整理）

> **目的：** 每腳正式 L 對齊「錯在哪一類」；**不是**推倒 skill。  
> **門檻：** 1=Observation · 2=Watchlist · 3=Formal review · 5+=local tweak。  
> **原則：** 反省執行品質；同分支 n 未到不改硬規則；有 W 有 L 是小樣本常態。

### 1) 正式 L 總表（表 A · 有結算）

| 注 | 賽果 | B | 分支 | 錯誤類型 | 狀態 |
|----|------|---|------|----------|------|
| 羅奇代爾 **+0.25** | 1-2 | **L** | `aus-semi-dog-shallow` | 澳半職業 **薄 dog**；緩衝不夠 | Observation；prefer +1 |
| **里昂 -0.25** | 2-3 | **L** | `shallow-fav-0.25-light-lean` | **light + 跨國** 當 clear 聯賽 shallow | Observation；已入 light 紀律 |
| **Knights -0.75** | 1-2 | **L** | `npl-aus home-fav-0.75` | **NPL** 主半一正式偏進 | **Watchlist**；正式極稀 |
| **Houston -0.75** | **1-1** | **L** | `league-fav-0.75 MLS` | §1 升格後走 **和=L** 路徑 | **跨池和殺 #1**；入 formal 池 |
| **LAFC -0.75** | **3-1** | **W** | `league-fav-0.75 MLS` | 同型命中；**定 max W@1.85** | Observation W（調價位線用） |
| **Botafogo -0.75** | **0-0** | **L** | `league-fav-0.75 BRA` | 和=L；賽前已標交手**和磁鐵**仍出手 | **跨池和殺 #2** |
| **Örgryte +1** | **2-0** 主 | **L** | `league-dog-plus1` | Strong 主穿 2 球；§1d 防漏 | Observation |
| **Atlante +1** | **1-1** | **W** | `league-dog-plus1 MX` | 和局護 +1 命中 | Observation W |
| **Häcken -0.75**（08-01 主） | **1-1** | **L** | `league-fav-0.75 SWE W1` | @**1.90** 推定膠著應硬否；和=L | **跨池和殺 #3** → **formal consummate** |

**跨池和殺 n（聯賽 -0.75 正式 和→L only）：** **3** → formal **已** → `analysis-checklist.md` **§1b-2**（**不改 skill**）。  
**MLS -0.75 正式合計：** Houston L + LAFC W = **1W1L**（分支帳；和殺已入跨池）。  
**巴甲 -0.75 正式：** Botafogo **L**（和殺入跨池）。  
**dog +1 正式（W/L）：** Atlante **W** + Örgryte **L** = **1W1L**（比利時/半島多 **P**）。  
**對照（勿只記仇）：** 艾比安/隆德里納 W、牙山 W、Lillestrøm W、Hacken 客 W、Hammarby W、LAFC W、Atlante W、**Rosenborg W**、**Juventude W** → 框架非「一出手就錯」。  
粗計有 W/L：約 **10W / 7L**（**禁止**用混分支總 WR 改 skill；**和殺 n 可跨池**）。  
**歐北 -0.75 正式：** **3W1L**（含 Häcken 主 L）。  
**巴乙 -0.75 正式：** **Juventude W**（n=1；與 Botafogo 甲分開）。

### 2) 結構性失腳（已吸收 · 執行勿回潮）

| 教訓 | 執行要求 |
|------|----------|
| **里昂** | light lean（主勝 ~≥1.90）+ 跨國/盃 **不**套聯賽 -0.25 soft band；→ PASS |
| **Knights** | NPL / 澳盃主 -0.75 **正式極稀**；寧可 lean；勿用歐聯 W 洗白 |
| **羅奇** | 薄 dog +0.25 預設 PASS；prefer **+1** 緩衝 |
| **Örgryte +1** | Strong 主（~1.45）**2-0 穿 +1** → dog+1 對碾壓主仍可 L；對照 Atlante Med 客 **W** |

### 2b) dog +1 防漏（07-25 · 1W1L）

| 注 | 結果 | 含義 |
|----|------|------|
| Örgryte +1 | **L** 負 2 | Strong 主爆破 = +1 最大殺路徑 |
| Atlante +1 | **W** 和 | Med 客熱 + 和 = 保護成立 |
| 比利時 / 半島 +1 | **P** | 輸 1 走（早期） |

**執行：** §1d-C **保留**；主勝外圍 **~≤1.50 Strong** 的 dog +1 標更高爆破風險（可更小注或 lean only）；**不**因 1L 取消防漏整段。

### 3) Houston 專節（§1 升格後首腳和殺）

| 項目 | 內容 |
|------|------|
| 賽前案 | MLS + 主 ~1.58 Medium + **-0.75** → checklist §1 → **PLAY Low @1.86** |
| 賽果 | **1-1** |
| Track B | **-0.75 和 = L**（半一需取勝才 W） |
| 非 | 身份錯、錯場外圍、結算錯 |
| 是 | **路徑風險兌現**：案已寫「和=L」仍出手 |

**反省要點（執行 · 非必然改規則）：**

1. **-0.75 對「不敗熱門」不友好** — 若讀成主場偏膠著/1-1 劇本，應想 **-0.25** 或 **PASS**，不是半一。  
2. **§1 全勾 = 可小注 Low，不是高信心** — 倉位勿放大；勿與 LAFC 等同型連串。  
3. **MLS ≠ 挪超** — Lillestrøm/Hacken 相似度最多 Medium；**歐聯 vs MLS 分支宜分開計 n**。  
4. **n：** Houston L + **LAFC W** = MLS 正式 **1W1L** — 仍 Observation 級；**不**取消 §1；**不**禁所有 -0.75；**也不**因 LAFC 放寬。  
5. **Galaxy 同晚 lean L 未正式** — 貼線不升格與 Columbus 一致，**執行正確**。

**賽前速查（MLS / 聯賽 Medium -0.75 · 補強）：**

1. [ ] 和局對本盤是 **W 還是 L**？（-0.75 → **L**；-0.25 → **W**）  
2. [ ] 是否「膠著/不敗」劇本多於「淨勝 1+」？→ 偏向 PASS 或更淺盤（Houston 型）  
3. [ ] gap 是否清楚 Medium（~≤1.55–1.60 優於貼 1.58–1.70）？→ 貼線更嚴（Galaxy/Columbus）  
4. [ ] 是否誤用歐聯命中洗白 MLS/巴甲？  
5. [ ] 小注 + Confidence **僅 Low**？  
6. [ ] 同卡第二腳同型？→ **不串關**（若串 Houston+LAFC 會 1 腳 L 拖累）

### 4) 要做 / 不要做

| 要做 | 不要做 |
|------|--------|
| 每腳 L 標分支 + 錯誤類型 | 因 Houston 禁所有 -0.75 |
| Houston：MLS Medium -0.75 記 observation；強化「和殺」 | 因 1 場 L 取消 §1 整段 |
| 出手前強制寫清和/小勝對盤口 W/L | 用 cover-after-PASS 說「該打更多」 |
| NPL / light / 薄 dog **維持極稀** | 混分支總 WR 改 skill |
| 小注紀律 | 同型 Low 連串放大方差 |

### 5) 對後續出手（含 Botafogo 和殺後）

- §1 升格後仍是 **可評估小注**，不是穩賺標的。  
- Medium + **-0.75** 在 **和局率不低** 的聯賽：可維持 PLAY 但更小注，或膠著讀法 → **降 lean / PASS**。  
- **Botafogo 0-0：** 賽前已寫近交手和磁鐵 → 執行反省是 **§1-fund 和磁鐵應否決或降 lean，卻仍正式**；非身份/結算錯。  
- **不**因 Houston/Botafogo/Häcken 否定歐北 W 樣本；**W 帳分支仍分開**；**和殺 n 跨池**（Grill 08-01）。  
- **LAFC 3-1 W** 定 **max 正式 W 水位 1.85** → 推定線 **@≥1.86**（§1b-2 動態）。  

### 6) Formal review 2026-08-01（Grill 鎖定 · 已落檔 checklist §1b-2）

| 項目 | 內容 |
|------|------|
| 觸發 | 聯賽 **-0.75 正式和殺** 跨池 **n=3**（Houston、Botafogo、Häcken） |
| 產出 | **執行閘常態化**；**不改** skill 硬規則；W1 **保留** |
| 閘 | 和磁鐵／明確膠著 **硬否**；**@ > max 正式 W**（現 ≥1.86）推定膠著；翻案須 **外圍 clear + 結構錨** |
| Häcken 08-01 | @1.90 → 推定；應 **硬否** → L = **執行可避免** |
| 再升級 | n 續累加；例如 **n=5** 再議是否動 skill／凍結半一 |
| 不牽連 | A′ GCU W、cover 不回寫、Gangwon 拒 -1、友誼 PASS |

> …（全文見 legacy「正式 PLAY 失誤反省」）

---

## 維護清單（每批必做）

- [ ] 表 A / A′ 新行（若有）
- [ ] 粗 WR / 分支速查一行更新
- [ ] post-match/batches/<date>.md 賽後批 + §9
- [ ] post-match/INDEX.md 置頂最近批
- [ ] **不要**把 batch 全文貼回本熱檔

# 競合他社クローリングの業務要件定義

## 1. 目的・KPI（ビジネス要件）

### 1.1 背景・課題
- 企画G、PT部門等(*1)にて、競合他社Webサイトから情報収集作業を手動で実施している。これを自動化し、YMC内の作業工数年間 **約 1000 時間** を削減する  
- コピー＆ペースト起因の誤記・漏れをゼロに近づけ、品質を向上させる  
(*1)現状その他の部門でも訴求できるのか確認中である

### 1.2 目標指標

| 指標                | 現状     | 目標値          |
|---------------------|----------|-----------------|
| 取得作業時間／年     | 1000時間   | **基本0分**   |
| 取得作業頻度／年     | 1回   | **1回/月**   |
| 取得サイト数     | 45サイト   | **45サイト**   |
| 出力形式     | CSV or Excel   | **同等**   |
| 情報取得項目(カラム)数     | XXX  | **同等**   |
| Webからの情報取得率     | 推定 99%   | **同等** (☆)|
| 取得した情報のカラムへの転記ミス率<br>(名寄せミス)| 推定 5 % | **同等** (☆) |
| 取得の運用費／年     | 現状は運用無し  | **YMSLI 200万円前後** (☆☆)| 

(☆) 重要な指標  
(☆☆) ROI = (利益 ÷ 投資額) × 100

---

## 2. 現行業務（As-Is）

1. 参照 Web サイト 45 件の過去取得した Excel を準備する登録し担当者が巡回
2. 対象サイトを1つ決め、取得したいカラムの情報を探索  
3. 取得したいカラム（全長／エンジン仕様／カラーなど）をWebサイト内で確認＆手動転記
4. 1つのサイトに対し、全てのカラムの調査を実施
5. 1つのサイトが終わり次第、次のサイトに移行し、45サイト全てを実施
6. Excelを部門内のシェアポイントに配置＆共有
7. 状況によっては、共有依頼が来た際はそのファイルを共有

  (1~2を外部ベンダーに依頼している場合は、YMCで内容精査作業が発生)

> **問題**: 手間・漏れ・更新遅延・属人化

---
# 3以降機能要件や非機能要件はまだ叩き台



## 3. 将来像（To-Be）の機能要件

### 3.1 クローリング機能
- **対象 URL 管理**: ブラウザ UI で追加／編集  
- **スケジューラ**: 日次 06:00 実行（Cron 形式）  
- **動的ページ対応**: Selenium / Playwright で JS レンダリング後に HTML 抽出  
- **抽出ロジック**: CSS セレクタ指定または ML 自動判定  
- **重複検知**: URL＋ハッシュで既取得データ除外  
- **出力**: JSON → PostgreSQL → BI ツール連携  

### 3.2 管理・運用機能
- ジョブ結果ダッシュボード（成功／失敗／差分件数）  
- リトライ＆アラート (Slack / Teams)  
- クローラ実行ログを 90 日保持  

---

## 4. 非機能要件

| 区分       | 要件例                                               |
|------------|------------------------------------------------------|
| 性能       | 1 サイト 5 秒以内、全サイト 10 分以内で完了           |
| 可用性     | 平日 06:00 実行成功率 99 % 以上                       |
| セキュリティ| 通信は TLS1.2 以上、保存データは AES-256 で暗号化     |
| 拡張性     | 対象サイトを月 5 件増やしても性能劣化しない           |
| 運用保守   | 設定変更・ログ閲覧をノーコードで実施可能             |

---

## 5. 制約・リスク & コンプライアンス

1. **robots.txt 準拠**（Disallow パスはクロール禁止、User-Agent 明示）  
2. **著作権・利用規約**の確認と必要な許諾取得  
3. **社内ネットワーク制約**（プロキシ／FW のホワイトリスト登録）  
4. **個人情報・機密情報の取得禁止**  

---

## 6. 受け入れ基準（テスト観点）

| テスト種類   | 合格条件例 |
|-------------|-----------|
| 機能テスト   | 10 サイト全てで正確に抽出・DB 格納できる |
| 負荷テスト   | 同時 3 ジョブ実行で CPU 使用率 70 % 未満 |
| 障害テスト   | 1 サイトが 404 でも残り 9 サイトが正常終了 |
| セキュリティ | OWASP Top10 に該当する脆弱性が検出されない |

---

## 7. ドキュメント構成例（テンプレート）

1. 表紙・版数管理  
2. プロジェクト概要・背景  
3. ステークホルダー & RACI  
4. 用語集  
5. As-Is 業務フロー  
6. To-Be 機能要件一覧（CRUD マトリクス付き）  
7. 非機能要件マトリクス  
8. 制約条件・前提  
9. リスク登録簿 & 対応策  
10. 受け入れ基準 & テスト計画  

---

# E1. Purpose and KPI (Business Requirements)

### 1.1 Background and Issues
- In departments such as Planning G and PT (*1), information is manually collected from competitor websites. By automating this process, approximately **1,000 hours** of annual work within YMC can be reduced.
- Reduce errors and omissions caused by copy & paste to near zero, improving quality.
(*1) Currently confirming whether this can be promoted to other departments as well.

### 1.2 Target Indicators

| Indicator                | Current     | Target Value      |
|--------------------------|-------------|-------------------|
| Annual data collection time | 1,000 hours | **Basically 0 min** |
| Data collection frequency/year | 1 time | **1 time/month** |
| Number of target sites   | 45 sites    | **45 sites**      |
| Output format            | CSV or Excel| **Equivalent**    |
| Number of data columns   | XXX         | **Equivalent**    |
| Data acquisition rate from web | Estimated 99% | **Equivalent** (☆)|
| Data entry error rate (name matching errors) | Estimated 5% | **Equivalent** (☆) |
| Annual operation cost    | No current operation | **Around 2 million yen for YMSLI** (☆☆)|

(☆) Important indicator  
(☆☆) ROI = (Profit ÷ Investment) × 100

---

# E2. Current Operations (As-Is)

1. Prepare previously collected Excel files for 45 reference websites; assigned staff circulate and register them.
2. Select a target site and search for the information of the columns to be collected.
3. Check and manually transfer the desired columns (e.g., total length, engine specifications, color, etc.) from the website.
4. Investigate all columns for one site.
5. After finishing one site, move to the next, and repeat for all 45 sites.
6. Place and share the Excel file on the department's SharePoint.
7. If requested, share the file as needed.

  (If steps 1~2 are outsourced to an external vendor, YMC performs content verification.)

> **Issues**: Labor, omissions, update delays, dependence on individuals

---

# E3. Future State (To-Be) Functional Requirements

### 3.1 Crawling Functions
- **Target URL Management**: Add/edit via browser UI
- **Scheduler**: Execute daily at 06:00 (Cron format)
- **Dynamic Page Support**: Extract HTML after JS rendering using Selenium/Playwright
- **Extraction Logic**: Specify CSS selectors or use ML auto-detection
- **Duplicate Detection**: Exclude already acquired data using URL + hash
- **Output**: JSON → PostgreSQL → BI tool integration

### 3.2 Management and Operation Functions
- Job result dashboard (success/failure/difference count)
- Retry & alert (Slack/Teams)
- Retain crawler execution logs for 90 days

---

# E4. Non-Functional Requirements

| Category    | Example Requirement                                    |
|-------------|-------------------------------------------------------|
| Performance | Complete within 5 seconds per site, 10 minutes for all sites |
| Availability| Success rate of 99% or higher for 06:00 weekday runs   |
| Security    | Communication via TLS1.2 or higher, data stored with AES-256 encryption |
| Scalability | No performance degradation even if 5 sites are added per month |
| Operation & Maintenance | No-code configuration changes and log viewing possible |

---

# E5. Constraints, Risks & Compliance

1. **Compliance with robots.txt** (Do not crawl Disallow paths, specify User-Agent)
2. **Check and obtain necessary permissions for copyright and terms of use**
3. **Internal network constraints** (Proxy/FW whitelist registration)
4. **Prohibition of acquiring personal or confidential information**

---

# E6. Acceptance Criteria (Test Perspectives)

| Test Type    | Example Pass Criteria |
|--------------|----------------------|
| Functional Test | Accurate extraction and DB storage for all 10 sites |
| Load Test    | CPU usage below 70% when running 3 jobs simultaneously |
| Failure Test | Even if 1 site returns 404, the remaining 9 sites complete normally |
| Security     | No vulnerabilities corresponding to OWASP Top 10 detected |

---

# E7. Example Document Structure (Template)

1. Cover & version control
2. Project overview & background
3. Stakeholders & RACI
4. Glossary
5. As-Is business flow
6. To-Be functional requirements list (with CRUD matrix)
7. Non-functional requirements matrix
8. Constraints & assumptions
9. Risk register & countermeasures
10. Acceptance criteria & test plan

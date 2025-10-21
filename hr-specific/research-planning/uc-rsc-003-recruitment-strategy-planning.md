---
use_case_id: UC-RSC-003
title: 採用戦略立案支援システム
category: HR-specific
sub_category: Research & Planning
complexity_level: Lv4-5
implementation_types:
  - RAG
  - Prompt
tags:
  - 採用戦略
  - 戦略立案
  - データ分析
  - 人材計画
  - 予算策定
version: 1.0.0
last_updated: 2025-01-15
rag_config:
  knowledge_base_type: 過去採用実績・市場データ・成功事例
  embedding_strategy: 戦略要素別のチャンキング（ターゲット、チャネル、施策）
  chunk_size: 850
  top_k: 10
  similarity_threshold: 0.73
  sample_content: 採用KPI、チャネル別効果、過去の成功・失敗事例
---

# 採用戦略立案支援システム

## Overview

データに基づいた採用戦略の立案をAIが支援するシステム。過去の採用実績、市場データ、成功事例をRAGで分析し、ターゲット人材の定義、採用チャネル選定、予算配分、KPI設定などを包括的に提案します。従来は経験と勘に頼っていた戦略立案をデータドリブンに変革し、採用成功率を向上させます。年間採用計画の策定や中途採用強化施策の立案など、様々なシーンで活用できる戦略支援ツールです。

## Business Value

- 時間削減: 採用戦略立案時間を3週間→5日に短縮（75%削減）
- コスト削減: 採用コストの最適化により年間約1,000万円の削減
- 品質向上: データに基づく戦略立案、採用成功率20%向上、ミスマッチ率30%削減
- その他の効果: チャネル別ROIの可視化、予算配分の最適化、再現性のある採用活動

## Required Components

- LLM（GPT-4またはClaude）
- ベクトルデータベース（Pinecone, Qdrant, Weaviate等）
- Difyワークフロー機能
- 採用実績データベース（過去3-5年分）
- 市場データ（求人倍率、賃金動向等）
- 成功事例ライブラリ

## Implementation Steps

1. 過去の採用データを収集・整理（採用人数、チャネル、コスト、定着率等）
2. 成功事例・失敗事例を分析・構造化
3. ナレッジベースに登録（戦略要素別にカテゴリー化）
4. 戦略立案プロンプトテンプレートを作成（ペルソナ定義、チャネル選定、KPI設定等）
5. RAG検索で関連事例・データを抽出するワークフローを構築
6. 戦略案自動生成機能の実装
7. 経営層向けプレゼン資料の自動生成

## Sample Prompts

```
以下の条件で、年間採用戦略を立案してください：
採用目標: {hiring_target}（新卒{new_grad_count}名、中途{mid_career_count}名）
予算: {budget}
重点職種: {key_positions}
課題: {current_challenges}

以下を含む戦略案を提示してください：
1. ターゲット人材のペルソナ定義
2. 採用チャネル選定と予算配分
3. 月次採用スケジュール
4. KPI設定（応募数、内定率、定着率等）
5. リスクと対策

過去の成功事例も参考に、実行可能な計画を作成してください。
```

```
{job_category}の中途採用を強化するための戦略を立案してください。
現状:
- 応募数: {current_applications}/月
- 内定承諾率: {offer_acceptance_rate}
- 採用コスト: {cost_per_hire}

目標:
- 採用人数: {target_hires}/年
- コスト削減: {cost_reduction_target}

戦略には以下を含めてください：
- ターゲット層の再定義
- 効果的な採用チャネルの選定
- スカウト戦略
- 候補者体験の改善施策
- 予算配分案
```

```
過去の採用データから、最も効果的だったチャネルとその成功要因を分析してください。
分析対象: {analysis_period}
対象職種: {job_categories}

各チャネルについて以下を評価：
- 応募数・採用数
- 採用コスト（CPA, CPH）
- 定着率（入社1年後）
- 質（パフォーマンス評価）

上位3チャネルの成功要因を抽出し、今年度の戦略への示唆をまとめてください。
```

## Data Requirements

- データの種類: 採用実績データ、チャネル別効果データ、市場データ、成功・失敗事例
- データ量: 過去3-5年分の採用データ、各職種につき10件以上の事例
- データ形式: CSV、Excel形式の採用データ、テキスト形式の事例集
- データ準備:
  1. ATS（採用管理システム）からデータエクスポート
  2. チャネル別のコスト・効果を整理
  3. 定着率データと紐付け
  4. 成功・失敗事例のドキュメント化

## Technical Considerations

- システム要件: LLM API（月間2,000リクエスト程度）、ベクトルDB（20,000エントリー）
- セキュリティ: 採用データの機密保持、個人情報の匿名化、アクセス権限管理
- パフォーマンス: 1回の戦略立案を30分以内、データ分析を含めて1時間以内
- 制限事項:
  - 過去データが少ない場合は予測精度が低下
  - 市場環境の急変時は人間の判断が必要
  - 業界特有の慣習は別途考慮が必要
- スケーラビリティ: 複数部門・職種の戦略を同時立案可能

## Reference Links

- https://www.shrm.org/resourcesandtools/tools-and-samples/toolkits/pages/strategicrecruitmentplan.aspx
- https://www.linkedin.com/business/talent/blog/talent-strategy/data-driven-recruiting-strategy
- https://www.dify.ai/blog/ai-powered-recruitment-strategy

## Tags

- 採用戦略
- 戦略立案
- データ分析
- 人材計画
- 予算策定
- RAG
- KPI設定
- チャネル最適化

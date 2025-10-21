---
use_case_id: UC-BRD-003
title: 経営KPIレポート自動生成
category: General Business
sub_category: Shareholder & Board Meetings
complexity_level: Lv4-5
implementation_types:
  - RAG
  - Automation
  - AI Tools
tags:
  - KPI
  - 経営ダッシュボード
  - データ分析
  - 経営報告
  - 可視化
version: 1.0.0
last_updated: 2025-01-15
rag_config:
  knowledge_base_type: 過去のKPIレポート・経営計画・業界ベンチマーク・分析ガイドライン
  embedding_strategy: KPIカテゴリー別チャンキング（財務、顧客、業務、成長指標別）
  chunk_size: 800
  top_k: 6
  similarity_threshold: 0.78
  sample_content: KPI定義、目標値、実績推移、分析コメント、改善アクション
---

# 経営KPIレポート自動生成

## Overview

各部門から収集したデータを統合し、経営層向けのKPIレポートを自動生成するAIシステム。財務指標、顧客指標、業務効率指標、成長指標などを統合的に分析し、トレンド、異常値、目標達成状況を可視化します。従来は経営企画部が各部門からデータを集めて週5時間かけて作成していたレポートを、30分で自動生成し、分析コメントも含めて提供します。

## Business Value

- 時間削減: レポート作成時間を週5時間→30分に短縮（90%削減）
- コスト削減: 経営企画部の工数削減により年間約300万円のコスト削減
- 品質向上: データの即時性向上、分析の一貫性確保、異常値の早期検知
- その他の効果: 経営判断のスピード向上、データドリブン経営の促進、部門間の情報共有強化

## Required Components

- LLM（GPT-4、Claude、Gemini等）
- ベクトルデータベース（Pinecone, Qdrant, Weaviate等）
- Dify Workflow機能
- BIツール連携（Tableau、Power BI、Looker等）
- 各種業務システム（ERP、CRM、SFA等）
- データウェアハウス

## Implementation Steps

1. 経営KPIの定義と目標値を整理（財務、顧客、業務、成長等）
2. 各データソースからの自動取得ロジックを構築
3. 過去のレポートフォーマットと分析パターンをナレッジベース化
4. データの異常値検知ルールを設定
5. レポート生成→グラフ作成→分析コメント→配信のワークフローを構築
6. 経営層からのフィードバックを収集してレポート形式を改善
7. 定期自動配信を開始（週次・月次）

## Sample Prompts

```
あなたは経営企画部のデータアナリストです。以下のKPIデータを分析し、経営層向けのサマリーレポートを作成してください：

【財務指標】
売上高: {revenue}（前月比: {revenue_mom}、前年比: {revenue_yoy}）
営業利益率: {operating_margin}（目標: {target_margin}）
営業CF: {operating_cashflow}

【顧客指標】
新規顧客獲得数: {new_customers}（目標: {target_new_customers}）
顧客解約率: {churn_rate}（前月: {previous_churn}）
LTV: {ltv}、CAC: {cac}、LTV/CAC比: {ltv_cac_ratio}

【業務効率指標】
従業員生産性: {productivity}（売上/人）
プロジェクト完了率: {project_completion_rate}
顧客満足度: {csat_score}

【成長指標】
MRR成長率: {mrr_growth}
新製品売上比率: {new_product_ratio}
市場シェア: {market_share}

以下の構成でレポートを作成してください：
1. エグゼクティブサマリー（200字以内）
2. 重要な成果とハイライト
3. 注意が必要な指標と課題
4. 前月・前年との比較分析
5. 次月の重点アクション提案

経営層が5分で状況を把握できる、明確で実用的なレポートにしてください。
```

```
以下のKPIデータに異常値や注意すべきトレンドがないか分析してください：

KPIデータ: {kpi_data}
過去6ヶ月の推移: {historical_trend}
目標値: {targets}
業界ベンチマーク: {industry_benchmark}

以下の観点で分析してください：
1. 目標から大きく乖離している指標（±20%以上）
2. 前月から急激に変化した指標（±30%以上）
3. 持続的に悪化している指標（3ヶ月連続で低下）
4. 業界平均から大きく外れている指標
5. 指標間の矛盾（例：売上は増加だが利益率は低下）

各異常値について：
- 検知内容
- 考えられる原因
- 推奨される対応アクション
- 優先度（高・中・低）
をリスト形式で出力してください。
```

```
以下の部門別KPIを統合し、全社レベルの経営ダッシュボード用のストーリーを作成してください：

営業部門: {sales_kpis}
マーケティング部門: {marketing_kpis}
開発部門: {development_kpis}
カスタマーサクセス部門: {cs_kpis}
財務部門: {finance_kpis}

経営会議で発表するためのストーリーラインを以下の流れで構成してください：
1. 今月の全社業績サマリー
2. 各部門のパフォーマンスハイライト
3. 部門間の連携効果や相関関係
4. 課題と改善機会
5. 次月の重点施策

部門間の因果関係や連携効果を明確にし、全社最適の視点でストーリーを組み立ててください。
```

## Data Requirements

- データの種類: 財務データ、営業データ、顧客データ、業務データ、過去のKPIレポート
- データ量: 50-100種類のKPI、過去2年分の実績データ、業界ベンチマーク
- データ形式: JSON、CSV、Excel、API連携（各種業務システム）
- データ準備:
  1. 全社KPIツリーの定義と優先順位付け
  2. 各データソースからの自動取得APIを整備
  3. 過去のレポートと分析コメントをデータベース化
  4. 目標値と閾値の設定
  5. データクレンジングルールの定義

## Technical Considerations

- システム要件: LLM API（月間100-200リクエスト）、ベクトルDB（1,000エントリー）、データパイプライン
- セキュリティ: 経営情報の厳格な管理、役員レベルのアクセス制限、監査ログ
- パフォーマンス: 週次レポート生成に30分以内、月次レポートに1時間以内
- 制限事項:
  - データの正確性は元システムのデータ品質に依存
  - 複雑な因果関係の分析は人間の補完が必要
  - 新規KPIの追加は設定変更が必要
  - 業界ベンチマークデータの更新頻度に制約
- スケーラビリティ: 100種類のKPI、週次・月次・四半期レポート対応可能

## Reference Links

- https://www.dify.ai/blog/executive-kpi-automation
- https://docs.anthropic.com/claude/docs/business-intelligence
- https://www.tableau.com/learn/articles/business-intelligence
- https://www.mckinsey.com/capabilities/strategy-and-corporate-finance/our-insights/the-new-metrics-of-corporate-performance

## Tags

- KPI
- 経営ダッシュボード
- データ分析
- 経営報告
- 可視化
- RAG
- Automation
- AI Tools
- BI連携

---
use_case_id: UC-REC-004
title: 採用KPIダッシュボード・分析レポート自動生成システム
category: HR-specific
sub_category: Recruitment
complexity_level: Lv4-5
implementation_types:
  - AI Tools
  - Automation
tags:
  - 採用分析
  - KPI管理
  - レポート生成
  - データドリブン採用
  - 意思決定支援
version: 1.0.0
last_updated: 2025-01-15
rag_config:
  knowledge_base_type: null
  embedding_strategy: null
  chunk_size: null
  top_k: null
  similarity_threshold: null
  sample_content: null
---

# 採用KPIダッシュボード・分析レポート自動生成システム

## Overview

ATS（Applicant Tracking System）や求人サイトのデータを取り込み、採用KPI（応募数、書類選考通過率、面接設定率、内定率、採用コスト等）を自動計算し、経営層・採用責任者向けの分析レポートを生成するシステム。データの可視化、トレンド分析、ボトルネック特定、改善提案を自動実行します。従来は週次レポート作成に半日かかっていたところを、30分以内に完成させることができ、採用担当者はデータに基づいた戦略立案により多くの時間を割くことができます。

## Business Value

- 時間削減: レポート作成時間を85%削減（半日→30分）
- コスト削減: 採用担当者の作業時間削減により年間約200万円のコスト削減
- 品質向上: データドリブンな意思決定、採用プロセスの最適化、採用効率15-20%向上
- その他の効果: ボトルネックの早期発見、採用戦略の精度向上、経営層への可視性向上

## Required Components

- LLM（GPT-4、Claude、Gemini等）
- Difyワークフロー機能
- ATS（Applicant Tracking System）
- 求人サイト・採用媒体の管理画面
- 採用データ（応募数、選考通過数、内定数、採用コスト等）
- レポートテンプレート
- BIツール（Tableau、Power BI等）またはグラフ生成ライブラリ

## Implementation Steps

1. レポートテンプレート（週次、月次、四半期）を準備
2. ATSや求人サイトからデータを取得するAPI連携を構築
3. KPI計算ロジックを実装（応募数、通過率、採用コスト、採用期間等）
4. データ分析・トレンド検出のワークフローを構築
5. ボトルネック特定・改善提案プロンプトを設定
6. レポート生成ワークフローを構築（サマリー、詳細、グラフ）
7. サンプルデータでテストし、レポートの精度を検証
8. 実運用開始（採用責任者が最終確認・意思決定）

## Sample Prompts

```
以下の採用データから、週次KPIレポートを作成してください：

採用データ:
- 応募数: {application_count}
- 書類選考通過数: {screening_passed}
- 一次面接設定数: {first_interview_count}
- 最終面接数: {final_interview_count}
- 内定数: {offer_count}
- 内定承諾数: {offer_accepted}
- 採用コスト: {recruitment_cost}
- 平均採用期間: {average_hiring_time}
- 前週比・前月比データ: {comparison_data}

レポートには以下を含めてください：
1. エグゼクティブサマリー（重要な変化・トレンド）
2. 主要KPIの達成状況と推移
3. ファネル分析（各ステージの通過率）
4. ボトルネックの特定（通過率が低いステージ）
5. 採用ソース別の効果分析
6. 採用コスト分析（CPA、CPH等）
7. 改善提案とアクションプラン

信号機方式（緑・黄・赤）で各KPIのステータスを示してください。
```

```
採用ファネルで「{stage_name}」ステージの通過率が{percentage}%と低いです。
原因分析と改善策を提案してください。

分析データ：
- 通過率の推移: {conversion_trend}
- 職種別・部門別の通過率: {conversion_by_role}
- 採用ソース別の通過率: {conversion_by_source}
- 競合他社のベンチマーク: {benchmark_data}

具体的な改善施策と実施スケジュールを提案してください。
```

```
複数月の採用データから、長期的なトレンドと戦略的改善ポイントを分析してください：
{historical_recruitment_data}

分析項目：
- 季節性やトレンドの有無
- 職種別・部門別の採用難易度
- 採用ソースの効果変化
- 採用コストの推移
- 次四半期の採用計画への提言
```

## Data Requirements

- データの種類: 応募データ、選考データ、内定データ、採用コスト、レポートテンプレート
- データ量: 月間100-1000件の応募データ、求人50-200件
- データ形式: CSV、JSON（ATSデータ）、Excel（コストデータ）、Word/PowerPoint（テンプレート）
- データ準備:
  1. ATSのデータエクスポート設定
  2. 採用ステージの定義統一
  3. 採用コストの集計方法確立
  4. レポートテンプレートの標準化
  5. 過去6ヶ月分のデータを準備

## Technical Considerations

- システム要件: LLM API（月間100リクエスト程度）、API連携、BIツール
- セキュリティ: 採用データの機密管理、レポートのアクセス権限管理必須
- パフォーマンス: レポート生成は15分以内、データ集計は10分以内
- 制限事項:
  - ATSのデータ品質に依存
  - 定性的な評価（候補者体験等）は人間が追記
  - KPI定義変更時は設定更新が必要
  - 最終確認・意思決定は採用責任者が必須
  - 複雑な原因分析は専門家の判断が必要
- スケーラビリティ: 同時に50職種までレポート生成可能

## Reference Links

- https://www.dify.ai/blog/recruitment-analytics-automation
- https://www.greenhouse.io/blog/recruitment-metrics
- https://www.lever.co/blog/hiring-metrics

## Tags

- 採用分析
- KPI管理
- レポート生成
- データドリブン採用
- 意思決定支援
- ファネル分析
- 採用効率化
- 業務自動化

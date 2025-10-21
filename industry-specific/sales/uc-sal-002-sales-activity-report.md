---
use_case_id: UC-SAL-002
title: 営業活動レポート自動作成システム
category: Industry-specific
sub_category: Sales & Business Development
complexity_level: Lv2-3
implementation_types:
  - AI Tools
  - Prompt-only
tags:
  - 営業支援
  - レポート生成
  - 活動管理
  - 自動化
  - データ分析
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

# 営業活動レポート自動作成システム

## Overview

CRMやSFAに記録された営業活動データから、日報・週報・月報を自動生成するシステム。訪問記録、商談履歴、見込み案件の情報を整理し、定型フォーマットの活動レポートを作成します。従来は日報作成に30分、週報作成に1時間かかっていたところを、それぞれ5分、15分に短縮でき、営業担当者は顧客対応により多くの時間を割くことができます。

## Business Value

- 時間削減: 日報作成時間を80%削減（30分→5分）、週報作成時間を75%削減（1時間→15分）
- コスト削減: 営業担当者の事務作業時間削減により年間約200万円のコスト削減（営業10名の場合）
- 品質向上: 記載漏れ防止、活動の可視化、報告の標準化
- その他の効果: 営業活動への集中時間増加、マネージャーの状況把握改善、ナレッジ共有促進

## Required Components

- LLM（GPT-4、Claude、Gemini等）
- Difyプロンプトまたはチャットボット機能
- CRM/SFAシステム（Salesforce、HubSpot、Zoho等）
- 活動データ（訪問記録、商談履歴、タスク完了状況）
- レポートテンプレート

## Implementation Steps

1. CRM/SFAから活動データをエクスポートする方法を確立
2. レポートテンプレート（日報、週報、月報）を準備
3. 活動データをプロンプトに投入し、レポートを生成するワークフローを構築
4. レポート生成プロンプトを設定（活動サマリー、成果、課題、次のアクション）
5. サンプルデータでテストし、レポートの完成度を検証
6. 実運用開始（営業担当者が補足・調整）

## Sample Prompts

```
以下の営業活動データから、日報を作成してください：

本日の活動:
- 訪問件数: {visit_count}
- 訪問先: {visited_companies}
- 商談内容: {meeting_summaries}
- 新規案件: {new_opportunities}
- フォローアップタスク: {follow_up_tasks}
- その他の活動: {other_activities}

日報には以下を含めてください：
1. 本日の活動サマリー（2-3行）
2. 主要な商談・訪問の詳細
3. 成果（受注、見込み案件の進展等）
4. 課題・問題点
5. 明日以降のアクションプラン

簡潔で分かりやすい形式で作成してください。
```

```
今週の営業活動データから、週報を作成してください：

今週の実績:
- 総訪問件数: {total_visits}
- 商談件数: {meeting_count}
- 見積提出件数: {quotation_count}
- 受注件数: {order_count}
- 売上金額: {sales_amount}
- 新規顧客獲得数: {new_customers}

週報には以下を含めてください：
1. 今週のハイライト（主要な成果）
2. 活動実績サマリー（数値）
3. 注目すべき商談の進捗
4. 課題と対策
5. 来週の重点活動

KPI達成状況を明示してください。
```

```
営業活動レポートを、上司向けとチーム共有用の2バージョンで作成してください。
上司向けには数値と成果を中心に、チーム共有用には活動のノウハウや学びを含めてください。

活動データ: {activity_data}
```

## Data Requirements

- データの種類: 営業活動データ（訪問記録、商談履歴、タスク、案件情報）、レポートテンプレート
- データ量: 日次5-10件の活動記録、週次20-50件
- データ形式: CSV、JSON（CRM/SFAエクスポート）、Word（テンプレート）
- データ準備:
  1. CRM/SFAのデータエクスポート設定
  2. レポートテンプレートの標準化
  3. KPI定義の明確化

## Technical Considerations

- システム要件: LLM API（月間500リクエスト程度）
- セキュリティ: 顧客情報の機密管理、レポートのアクセス権限管理
- パフォーマンス: レポート生成は30秒以内
- 制限事項:
  - CRM/SFAのデータ入力品質に依存
  - 定性的な情報（顧客の反応、商談の雰囲気等）は手動追記
  - 機密性の高い商談内容は個別調整が必要
  - 最終確認は営業担当者が必須
- スケーラビリティ: 営業50名分まで対応可能

## Reference Links

- https://www.dify.ai/blog/sales-reporting-automation
- https://www.salesforce.com/resources/articles/sales-reports/
- https://www.hubspot.jp/sales-reports

## Tags

- 営業支援
- レポート生成
- 活動管理
- 自動化
- データ分析
- CRM連携
- 日報週報
- 業務効率化

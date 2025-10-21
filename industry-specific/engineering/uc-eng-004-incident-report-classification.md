---
use_case_id: UC-ENG-004
title: 障害報告書自動分類・分析システム
category: Industry-specific
sub_category: Engineering & Development
complexity_level: Lv4-5
implementation_types:
  - RAG
  - AI Tools
  - Automation
tags:
  - 障害管理
  - インシデント対応
  - 分類
  - 分析
  - ナレッジ管理
version: 1.0.0
last_updated: 2025-01-15
rag_config:
  knowledge_base_type: 過去の障害事例・対応手順・根本原因分析
  embedding_strategy: 障害カテゴリ別チャンキング（システム、原因、影響度別）
  chunk_size: 900
  top_k: 10
  similarity_threshold: 0.75
  sample_content: 障害事例、対応手順、根本原因分析レポート、予防策
---

# 障害報告書自動分類・分析システム

## Overview

システム障害やインシデントの報告書をアップロードすると、過去の類似障害事例を参照しながら、障害を自動分類し、優先度判定、原因分析、対応手順の提案を行うシステム。RAGで過去の障害事例、対応手順、根本原因分析を検索し、迅速な初動対応と効果的な恒久対策の立案を支援します。障害対応時間を短縮し、再発防止につなげることができます。

## Business Value

- 時間削減: 障害分類・分析時間を60%削減（1時間→25分）
- コスト削減: エンジニアの障害対応時間削減により年間約300万円のコスト削減（月20件対応の場合）
- 品質向上: 類似障害の早期発見、対応手順の標準化、再発防止
- その他の効果: ダウンタイム削減、顧客満足度向上、ナレッジの蓄積

## Required Components

- LLM（GPT-4、Claude、Gemini等）
- ベクトルデータベース（Pinecone, Qdrant, Weaviate等）
- Difyワークフロー機能
- 過去の障害報告書データベース
- 対応手順書・ランブック
- 根本原因分析（RCA）レポート
- システム構成図・依存関係図

## Implementation Steps

1. 過去の障害報告書をカテゴリ別（システム、原因、影響度別）に整理し、ナレッジベースに登録
2. 対応手順書、ランブック、エスカレーション基準を登録
3. 根本原因分析（RCA）レポート、再発防止策を登録
4. システム構成図、依存関係図を登録
5. 障害報告フォームを作成（症状、発生時刻、影響範囲等）
6. RAG検索で類似障害事例・対応手順を取得するワークフローを構築
7. 自動分類・分析プロンプトを設定（分類、優先度、原因推定、対応提案）
8. サンプル障害でテストし、分析精度を検証
9. 実運用開始（エンジニアが最終判断・対応実施）

## Sample Prompts

```
以下の障害報告を分析し、分類・優先度判定・対応提案を行ってください：

障害情報:
- 発生時刻: {occurrence_time}
- 影響システム: {affected_system}
- 症状: {symptoms}
- 影響範囲: {impact_scope}
- エラーメッセージ: {error_message}

分析項目:
1. 障害カテゴリ（ハードウェア、ソフトウェア、ネットワーク、設定ミス等）
2. 優先度（Critical、High、Medium、Low）
3. 推定される原因
4. 類似の過去障害事例
5. 推奨される初動対応手順
6. エスカレーション要否

過去の障害事例を参照し、具体的な対応手順を提案してください。
```

```
障害「{incident_description}」の根本原因分析（RCA）を支援してください。

分析フレームワーク：
- 直接原因の特定
- 根本原因（5 Whys分析）
- 再発防止策の検討

過去の類似障害のRCAレポートを参考に、分析を深めてください。
```

```
複数の障害報告から、トレンド分析と予防的改善策を提案してください：
{incident_reports}

分析項目：
- 頻発している障害パターン
- システム間の関連性
- 潜在的なリスク要因
- 優先的に取り組むべき改善策
```

## Data Requirements

- データの種類: 過去の障害報告書、対応手順、RCAレポート、システム構成図
- データ量: 過去2年分の障害報告200-500件、対応手順50-100件、RCAレポート50-100件
- データ形式: テキスト、Word、PDF、Excel
- データ準備:
  1. 障害報告をカテゴリ・原因別に分類
  2. 対応手順を最新化・標準化
  3. RCAレポートから再発防止策を抽出
  4. システム構成図を最新化

## Technical Considerations

- システム要件: LLM API（月間200リクエスト程度）、ベクトルDB（1500エントリー）
- セキュリティ: 障害情報の機密管理、アクセス権限管理必須
- パフォーマンス: 分類・分析は5分以内、検索レスポンスは3秒以内
- 制限事項:
  - 新種の障害は過去事例が少なく精度が低い
  - 最終的な原因特定・対応はエンジニアが必須
  - 複合的な障害は人間の判断が必要
  - システム構成変更時はナレッジ更新が必要
  - Critical障害は即座に人間がエスカレーション
- スケーラビリティ: 月間100件の障害分析まで対応可能

## Reference Links

- https://www.dify.ai/blog/incident-management-automation
- https://docs.anthropic.com/claude/docs/technical-troubleshooting
- https://www.atlassian.com/incident-management/kpis/common-metrics

## Tags

- 障害管理
- インシデント対応
- 分類
- 分析
- ナレッジ管理
- トラブルシューティング
- RCA
- 業務効率化

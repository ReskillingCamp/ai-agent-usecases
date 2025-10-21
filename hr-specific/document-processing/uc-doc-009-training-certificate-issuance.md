---
use_case_id: UC-DOC-009
title: 研修受講証明書発行システム
category: HR-specific
sub_category: Document Processing
complexity_level: Lv2
implementation_types:
  - Prompt-only
  - RAG
tags:
  - 研修
  - 証明書
  - 教育記録
  - 修了証
  - 自動発行
version: 1.0.0
last_updated: 2025-01-15
rag_config:
  knowledge_base_type: 証明書テンプレート・研修プログラム情報
  embedding_strategy: 研修種別・レベル別のテンプレート検索
  chunk_size: 500
  top_k: 2
  similarity_threshold: 0.85
  sample_content: 証明書フォーマット、研修プログラム詳細、認定基準
---

# 研修受講証明書発行システム

## Overview

研修プログラム修了者に対して、正式な受講証明書・修了証を自動発行するシステム。受講者情報と研修内容を入力するだけで、統一フォーマットの証明書をPDF形式で即座に生成できます。従来は人事担当者が1件あたり10-15分かけて手作業で作成していた証明書を、数秒で自動発行できます。研修名、受講日、修了基準、発行日等を正確に記載し、従業員のスキル証明やキャリア開発記録として活用できます。デジタル証明書として発行することで、紛失リスクを低減し、いつでも再発行が可能です。

## Business Value

- 時間削減: 証明書発行時間を95%削減（10-15分→数秒）
- コスト削減: 人事担当者の工数削減により年間約60万円のコスト削減
- 品質向上: 統一フォーマット、誤記防止、即時発行による従業員満足度向上
- その他の効果: スキル可視化、キャリア開発支援、研修効果の記録管理

## Required Components

- LLM（GPT-4またはClaude）
- ベクトルデータベース（Pinecone, Qdrant等）（オプション）
- Difyワークフロー機能
- PDF生成ツール
- 証明書テンプレート（研修種別）
- 電子署名機能（オプション）

## Implementation Steps

1. 証明書テンプレートを設計（研修種別、難易度別）
2. テンプレートをナレッジベースに登録
3. 入力フォームを設定（受講者情報、研修名、受講日、修了日等）
4. RAG検索で適切なテンプレートを取得し、個別情報を反映するワークフローを構築
5. PDF形式で証明書を自動生成する機能を実装
6. サンプルデータでテストし、証明書の品質を検証
7. 実運用開始（研修修了後即座に証明書発行）

## Sample Prompts

```
以下の情報をもとに、研修受講証明書を作成してください：

受講者名: {participant_name}
従業員番号: {employee_id}
研修名: {training_name}
研修期間: {training_period}
受講日: {training_dates}
修了日: {completion_date}
研修時間: {training_hours}時間
研修内容概要: {training_overview}
修了基準: {completion_criteria}（例：全セッション出席、テスト80点以上）
発行日: {issue_date}
発行者: {issuer}（人事部長等）

正式な証明書フォーマットで作成してください。
企業ロゴの配置場所、署名欄も含めてください。
```

```
{training_level}（初級/中級/上級）の{training_category}研修の修了証を作成してください：
受講者情報: {participant_info}
研修詳細: {training_details}

修了証には以下を含めてください：
- 研修名（正式名称）
- 受講者名（敬称付き）
- 修了認定文言
- 習得スキル・知識の概要
- 研修時間・期間
- 発行日・発行者・公印
```

```
以下の一括受講者リストから、証明書を一括生成してください：
{participant_list}

各受講者に対して、同一の研修プログラム{training_name}の証明書を発行します。
受講者名以外の情報は共通です。
```

## Data Requirements

- データの種類: 受講者情報、研修プログラム詳細、修了基準、発行者情報
- データ量: 受講者1人あたり基本情報（10項目程度）、研修プログラム情報
- データ形式: 構造化データ（JSON/CSV）、証明書テンプレート（Word/PDF）
- データ準備:
  1. 証明書テンプレートを作成（研修種別）
  2. 研修プログラムのマスターデータを整備
  3. 修了基準を明確化
  4. 発行者・承認者の署名データを準備

## Technical Considerations

- システム要件: LLM API（月間500リクエスト程度）、PDF生成ライブラリ
- セキュリティ: 証明書の偽造防止（電子署名、QRコード等）、個人情報保護
- パフォーマンス: 1件の証明書生成を10秒以内、バッチ処理で100件/分
- 制限事項:
  - 正式な資格認定証（国家資格等）は対象外
  - 企業内研修の修了証明のみ
  - 法的効力を持つ証明書は別途対応が必要
- スケーラビリティ: 月間1,000件の証明書発行まで対応可能

## Reference Links

- https://www.dify.ai/blog/ai-certificate-generation
- https://docs.anthropic.com/claude/docs/document-automation
- https://www.iso.org/standard/62085.html (ISO 29993 Learning services)

## Tags

- 研修
- 証明書
- 教育記録
- 修了証
- 自動発行
- スキル証明
- PDF生成
- デジタル証明書

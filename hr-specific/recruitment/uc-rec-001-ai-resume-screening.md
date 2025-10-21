---
use_case_id: UC-REC-001
title: AI履歴書スクリーニングシステム
category: HR-specific
sub_category: Recruitment
complexity_level: Lv4-5
implementation_types:
  - RAG
  - AI Tools
tags:
  - 採用
  - 履歴書
  - スクリーニング
  - 自動化
  - 人材評価
version: 1.0.0
last_updated: 2025-01-15
rag_config:
  knowledge_base_type: 求人要件・職務記述書・評価基準
  embedding_strategy: セクションベースのチャンキング（スキル、経験、学歴）
  chunk_size: 800
  top_k: 10
  similarity_threshold: 0.75
  sample_content: 職務記述書、求めるスキルセット、過去の成功事例
---

# AI履歴書スクリーニングシステム

## Overview

大量の応募書類から適切な候補者を効率的に選別するAI履歴書スクリーニングシステム。求人要件と候補者の履歴書をRAGで照合し、マッチ度をスコアリングして上位候補者を自動抽出します。従来の人手による一次スクリーニングを80%削減し、見落としリスクを低減します。職務記述書をナレッジベースに登録することで、要件とのマッチングを自動化し、採用担当者は質の高い候補者との面接に集中できます。

## Business Value

- 時間削減: 履歴書スクリーニング時間を80%削減（100件の履歴書を2時間→24分）
- コスト削減: 採用担当者の工数削減により年間約500万円のコスト削減
- 品質向上: 見落としによる優秀な候補者の取りこぼしを防止、マッチング精度向上
- その他の効果: 客観的な評価基準による公平性の向上、無意識バイアスの低減

## Required Components

- LLM（GPT-4またはClaude）with function calling機能
- ベクトルデータベース（Pinecone, Qdrant, Weaviate等）
- PDF解析ツール（PyPDF2, pdfplumber等）
- Difyワークフロー機能
- 履歴書データ（PDF、Word形式）
- 職務記述書・求人要件データベース

## Implementation Steps

1. 職務記述書と求人要件をナレッジベースに登録（カテゴリー別に整理）
2. 履歴書解析ワークフローを設定（PDF→テキスト抽出→構造化）
3. マッチングスコアリング基準をプロンプトで定義（スキル、経験年数、学歴等）
4. RAG検索でTop候補者を抽出するワークフローを構築
5. サンプル履歴書でテストし、スコアリング精度を検証
6. 実運用開始（毎日の新規応募を自動スクリーニング）

## Sample Prompts

```
この履歴書を分析し、{job_title}のポジションに対する適合度を0-100点でスコアリングしてください。
評価基準：
- 必須スキルの保有状況（50点）
- 関連業務経験年数（30点）
- 学歴・資格（20点）

具体的な評価理由も併せて提示してください。
```

```
以下の求人要件に最もマッチする上位10名の候補者を抽出してください：
ポジション: {job_title}
必須スキル: {required_skills}
経験年数: {years_of_experience}
優遇条件: {preferred_qualifications}

各候補者のマッチ度スコアと推薦理由を提示してください。
```

```
この候補者の履歴書から、強み・弱み・懸念点を3点ずつ抽出してください。
面接で確認すべきポイントもリストアップしてください。
```

## Data Requirements

- データの種類: 職務記述書（JD）、求人要件、過去の成功採用事例、評価基準マトリクス
- データ量: 各職種につき3-5件の詳細なJD、過去1年分の採用成功事例
- データ形式: PDF、Word、テキスト形式の履歴書および職務記述書
- データ準備:
  1. 既存の職務記述書を収集・整理
  2. 求めるスキルセットを明確化
  3. 評価基準を数値化・体系化
  4. 過去の成功事例をテンプレート化

## Technical Considerations

- システム要件: LLM API（月間10,000リクエスト程度）、ベクトルDB（10,000エントリー）
- セキュリティ: 個人情報保護法対応、履歴書データの暗号化保存、アクセス制御必須
- パフォーマンス: 1件の履歴書スクリーニングを15秒以内、バッチ処理で100件/1時間
- 制限事項:
  - 手書き履歴書やスキャン画像は事前にOCR処理が必要
  - 特殊な職種（クリエイティブ、研究職等）はポートフォリオ評価が別途必要
  - 最終判断は人間が行う（AIは補助ツール）
- スケーラビリティ: 月間1,000件の応募まで対応可能、それ以上はAPI利用料増加

## Reference Links

- https://www.dify.ai/blog/rag-for-recruitment
- https://docs.anthropic.com/claude/docs/resume-screening-best-practices
- https://openai.com/research/gpt-4-recruitment

## Tags

- 採用
- 履歴書
- スクリーニング
- 自動化
- 人材評価
- RAG
- マッチング
- 候補者選考

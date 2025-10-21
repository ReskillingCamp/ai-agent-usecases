---
use_case_id: UC-BRD-006
title: IR資料作成支援
category: General Business
sub_category: Shareholder & Board Meetings
complexity_level: Lv2-3
implementation_types:
  - RAG
  - AI Tools
tags:
  - IR資料
  - 投資家向け資料
  - プレゼンテーション
  - 決算説明会
  - スライド作成
version: 1.0.0
last_updated: 2025-01-15
rag_config:
  knowledge_base_type: 過去のIR資料・決算説明資料・事業計画・市場データ
  embedding_strategy: スライドテーマ別チャンキング（業績、戦略、財務、見通し別）
  chunk_size: 750
  top_k: 5
  similarity_threshold: 0.77
  sample_content: スライド構成、説明文、グラフ種類、過去のストーリーライン
---

# IR資料作成支援

## Overview

決算説明会や投資家向けプレゼンテーションで使用するIR資料のコンテンツ案を自動生成するAIツール。過去の決算説明資料、事業計画、財務データを活用し、スライドの構成案、説明文、グラフのアイデアを提供します。従来はIR担当者が1週間かけて作成していた資料の初稿を、1日で完成させ、デザインとストーリー構築に集中できる時間を確保します。

## Business Value

- 時間削減: IR資料作成時間を1週間→1日に短縮（85%削減）
- コスト削減: IR担当者の工数削減により年間約150万円のコスト削減（四半期説明会の場合）
- 品質向上: 過去の優良資料の活用、ストーリーラインの一貫性、投資家視点の強化
- その他の効果: デザインクオリティ向上のための時間確保、説明会準備の余裕確保

## Required Components

- LLM（GPT-4、Claude、Gemini等）
- ベクトルデータベース（Pinecone, Qdrant, Weaviate等）
- Difyチャットボット機能
- 過去のIR資料アーカイブ
- 財務データ・事業データ

## Implementation Steps

1. 過去3-5年分のIR資料（決算説明資料、事業説明資料等）を収集
2. 優良なスライド構成とストーリーラインをパターン化
3. 財務データと事業データの可視化パターンを整理
4. スライド生成プロンプトを作成（構成、説明文、グラフ案）
5. IR担当者からフィードバックを収集して改善
6. 本格運用開始（四半期説明会、投資家ミーティング準備）

## Sample Prompts

```
あなたはIR資料作成の専門家です。以下の情報を基に、決算説明会のスライド構成案を作成してください：

今期の業績データ: {quarterly_results}
主要な事業トピックス: {business_highlights}
今後の事業計画: {business_plan}
過去の説明会資料: {past_presentation}
想定される投資家の関心事: {investor_interests}

以下の構成でスライド案を作成してください：
1. オープニング（会社概要、ハイライト）
2. 業績サマリー
3. セグメント別業績
4. 事業トピックス・戦略の進捗
5. 財務状況
6. 今後の見通し
7. まとめ・Q&A

各スライドについて：
- スライドタイトル
- 主要メッセージ（1-2行）
- 含めるべき内容・データ
- グラフの種類の推奨
- 投資家への訴求ポイント
を提示してください。
```

```
以下の財務データを投資家向けに可視化するアイデアを提案してください：

データ: {financial_data}
データの種類: {data_type}（売上推移/利益率/セグメント構成/キャッシュフロー等）
伝えたいメッセージ: {key_message}
過去の可視化例: {past_examples}

以下を提案してください：
1. 最適なグラフの種類（棒グラフ/折れ線/円グラフ/ウォーターフォール等）
2. グラフのタイトルとサブタイトル
3. 強調すべきデータポイント
4. 補足説明文（2-3行）
5. 投資家の理解を助けるための工夫

複数案を提示し、それぞれの長所・短所も説明してください。
```

```
決算説明会のストーリーラインを構築してください：

業績サマリー: {performance_summary}
今期の成果: {achievements}
今期の課題: {challenges}
次期の重点施策: {next_initiatives}
投資家の懸念事項: {investor_concerns}

以下の観点でストーリーラインを組み立ててください：
1. オープニング（投資家の関心を引く）
2. 実績の報告（成果と課題のバランス）
3. 戦略の説明（投資家の懸念に応える）
4. 将来の展望（期待感を持たせる）
5. クロージング（明確なメッセージ）

各パートで伝えるべきメッセージと、使用するデータ・事例を具体的に示してください。
投資家が「この会社に投資したい」と思えるストーリーを構築してください。
```

## Data Requirements

- データの種類: 過去のIR資料、財務データ、事業計画、市場データ、投資家からの質問
- データ量: 過去5年分のIR資料50-100件、財務データ、市場・競合データ
- データ形式: PowerPoint、PDF、Excel、JSON（財務データ）
- データ準備:
  1. 過去のIR資料をスライド単位で分類・タグ付け
  2. 優良スライドの評価基準を設定
  3. 財務データの可視化パターンを整理
  4. ストーリーラインのパターンを抽出
  5. 投資家からの過去の質問を分析

## Technical Considerations

- システム要件: LLM API（四半期あたり100-150リクエスト）、ベクトルDB（800エントリー）
- セキュリティ: 未公表情報の管理、開示前のアクセス制限、バージョン管理
- パフォーマンス: 20-30ページのスライド構成案を2-3時間で生成
- 制限事項:
  - スライドデザインは人間のデザイナーが担当
  - グラフの詳細な設計は別途調整が必要
  - 業界固有の表現やトレンドは人間が補完
  - 最終的なストーリーはIR担当者が判断
- スケーラビリティ: 年4回の決算説明会＋随時の投資家向け資料作成に対応

## Reference Links

- https://www.jpx.co.jp/equities/listed-co/ir/index.html
- https://www.dify.ai/blog/investor-presentation-automation
- https://docs.anthropic.com/claude/docs/content-creation
- https://www.mckinsey.com/capabilities/strategy-and-corporate-finance/our-insights/the-art-of-investor-relations

## Tags

- IR資料
- 投資家向け資料
- プレゼンテーション
- 決算説明会
- スライド作成
- RAG
- AI Tools
- コンテンツ生成

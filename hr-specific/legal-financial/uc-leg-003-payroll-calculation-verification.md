---
use_case_id: UC-LEG-003
title: 給与計算の検証
category: HR-specific
sub_category: Legal & Financial
complexity_level: Lv4
implementation_types:
  - RAG
  - AI Tools
tags:
  - 給与計算
  - 検証
  - エラーチェック
  - 労働法
  - 賃金
version: 1.0.0
last_updated: 2025-01-15
rag_config:
  knowledge_base_type: 給与計算規定・最低賃金法・社会保険料率表
  embedding_strategy: 計算ルールベースのチャンキング（基本給、手当、控除項目）
  chunk_size: 700
  top_k: 10
  similarity_threshold: 0.75
  sample_content: 給与規定、最低賃金法、社会保険料率、税率表、計算例
---

# 給与計算の検証

## Overview

給与計算結果の正確性を自動検証するシステム。給与規定、労働法規、社会保険料率をナレッジベースに登録し、計算結果を分析して誤り、法令違反、異常値を検出します。給与計算ミスによる従業員トラブルや追加支払いリスクを低減し、給与担当者の確認作業を効率化します。最低賃金法違反や社会保険料の計算誤りを事前に発見することで、コンプライアンスを強化します。

## Business Value

- 時間削減: 給与計算チェック時間を50%削減（月次100名分の検証: 8時間→4時間）
- コスト削減: 計算ミスによる追加支払いや罰金リスクの低減（年間約100万円）
- 品質向上: 給与計算精度の向上、従業員満足度の向上、トラブル防止
- その他の効果: 法令遵守の徹底、監査対応の効率化、属人化の解消

## Required Components

- LLM（GPT-4またはClaude）with function calling機能
- ベクトルデータベース（Pinecone, Qdrant, Weaviate等）
- Excel/CSV解析ツール（pandas等）
- Difyワークフロー機能
- 給与規定・計算ルールデータベース
- 最低賃金・社会保険料率の最新データ

## Implementation Steps

1. 給与規定・法定計算ルールをナレッジベースに登録（項目別に整理）
2. 給与データ解析ワークフローを設定（Excel/CSV→データ抽出→検証）
3. 検証基準をプロンプトで定義（最低賃金、割増賃金、控除項目等）
4. RAG検索で規定・法規との照合を行うワークフローを構築
5. サンプル給与データでテストし、検出精度を検証
6. 実運用開始（月次給与計算の自動検証）

## Sample Prompts

```
以下の給与計算結果を検証し、誤りや法令違反がないかチェックしてください：
- 基本給: {base_salary}
- 時間外手当: {overtime_pay}
- 通勤手当: {commute_allowance}
- 社会保険料: {social_insurance}
- 所得税: {income_tax}
- 差引支給額: {net_salary}

労働時間: {total_hours}、時間外時間: {overtime_hours}
最低賃金や割増賃金の計算に誤りがないか、詳細に確認してください。
```

```
この従業員の給与計算について、以下の観点から異常値をチェックしてください：
1. 前月比での大きな変動（±20%以上）
2. 最低賃金法違反の可能性
3. 社会保険料・税金の計算誤り
4. 支給項目・控除項目の過不足

問題があれば、具体的な原因と修正案を提示してください。
```

```
{number_of_employees}名分の給与データから、以下の項目に該当する事例を抽出してください：
- 割増賃金の計算誤り
- 社会保険料の計算誤り
- 最低賃金を下回る可能性
- 異常な控除額

各事例について、詳細な検証結果を報告してください。
```

## Data Requirements

- データの種類: 給与規定、最低賃金法、社会保険料率表、所得税率表、計算例
- データ量: 全給与項目の計算ルール、過去1年分の給与データサンプル
- データ形式: Excel、CSV形式の給与データ、PDF形式の規定・法規
- データ準備:
  1. 社内給与規定を整理・体系化
  2. 最新の法定料率・税率を収集
  3. 計算式を明確化・文書化
  4. 異常値判定基準を設定

## Technical Considerations

- システム要件: LLM API（月間5,000リクエスト程度）、ベクトルDB（10,000エントリー）
- セキュリティ: 給与データの厳重な暗号化、アクセス制御、監査ログ必須
- パフォーマンス: 100名分の給与データ検証を30分以内で完了
- 制限事項:
  - 最終的な給与確定は給与担当者が行う
  - 複雑な変動給や特殊手当は個別確認が必要
  - 法改正時はナレッジベースの即時更新が必要
- スケーラビリティ: 1,000名規模の給与計算検証まで対応可能

## Reference Links

- https://www.mhlw.go.jp/stf/seisakunitsuite/bunya/koyou_roudou/roudoukijun/minimumichiran/
- https://www.nenkin.go.jp/service/kounen/hokenryo/hoshu/20150515-01.html
- https://www.dify.ai/blog/payroll-verification

## Tags

- 給与計算
- 検証
- エラーチェック
- 労働法
- 賃金
- RAG
- コンプライアンス
- 品質管理

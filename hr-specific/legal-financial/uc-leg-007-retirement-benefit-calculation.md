---
use_case_id: UC-LEG-007
title: 退職金計算支援
category: HR-specific
sub_category: Legal & Financial
complexity_level: Lv4
implementation_types:
  - RAG
  - AI Tools
tags:
  - 退職金
  - 計算支援
  - 退職給付
  - 規定
  - シミュレーション
version: 1.0.0
last_updated: 2025-01-15
rag_config:
  knowledge_base_type: 退職金規定・計算式・勤続年数別係数表
  embedding_strategy: 計算ルールベースのチャンキング（基本給、勤続年数、退職事由）
  chunk_size: 700
  top_k: 10
  similarity_threshold: 0.78
  sample_content: 退職金規定、計算式、係数表、税金計算ルール、計算例
---

# 退職金計算支援

## Overview

退職金の正確な計算を支援するシステム。退職金規定、計算式、税制ルールをナレッジベースに登録し、従業員の勤続年数、役職、退職事由に基づいて退職金額を自動計算します。複雑な計算ルールや税金控除を正確に処理し、計算ミスによるトラブルを防止します。シミュレーション機能により、従業員への事前説明や退職金制度の見直し検討にも活用できます。

## Business Value

- 時間削減: 退職金計算時間を70%削減（1件あたり2時間→36分）
- コスト削減: 計算ミスによる追加支払いリスクの低減（年間約200万円）
- 品質向上: 計算精度の向上、従業員への適切な説明、トラブル防止
- その他の効果: 退職金制度の透明性向上、シミュレーションによる制度見直し支援

## Required Components

- LLM（GPT-4またはClaude）with function calling機能
- ベクトルデータベース（Pinecone, Qdrant, Weaviate等）
- 計算ツール（数式処理ライブラリ）
- Difyワークフロー機能
- 退職金規定データベース
- 従業員マスタデータ（勤続年数、役職等）

## Implementation Steps

1. 退職金規定・計算式をナレッジベースに登録（退職事由別に整理）
2. 計算ワークフローを設定（従業員データ→計算→結果出力）
3. 計算ロジックをプロンプトで定義（基本給×係数×勤続年数等）
4. RAG検索で規定・係数表を参照するワークフローを構築
5. サンプルケースでテストし、計算精度を検証
6. 実運用開始（退職予定者の退職金計算、シミュレーション）

## Sample Prompts

```
以下の従業員の退職金を計算してください：
- 氏名: {employee_name}
- 勤続年数: {years_of_service}年{months_of_service}ヶ月
- 退職時基本給: {base_salary}円
- 退職時役職: {position}
- 退職事由: {retirement_reason}（自己都合/会社都合/定年）

退職金規定に基づき、支給額、控除税額、手取り額を計算してください。
計算過程も詳細に示してください。
```

```
{number_of_employees}名の退職予定者について、退職金の概算を一括計算してください。
各従業員のデータ: {employee_data_list}

合計支給額と、会計上の引当金との差異も報告してください。
```

```
以下の条件で退職金シミュレーションを実施してください：
- 現在の勤続年数: {current_years}年
- 現在の基本給: {current_salary}円
- 予想退職年齢: {retirement_age}歳

1年後、3年後、5年後、定年時の退職金額を試算し、
グラフ化可能なデータで提示してください。
```

## Data Requirements

- データの種類: 退職金規定、計算式、勤続年数別係数表、税制ルール
- データ量: 退職金規定全文、係数表、過去の計算事例50件以上
- データ形式: PDF形式の規定、Excel形式の係数表、CSV形式の従業員データ
- データ準備:
  1. 退職金規定を最新版に更新
  2. 係数表を整備・検証
  3. 税制ルールを整理
  4. 計算式を明確化・文書化

## Technical Considerations

- システム要件: LLM API（月間2,000リクエスト程度）、ベクトルDB（5,000エントリー）
- セキュリティ: 退職金データの厳重管理、アクセス制御、計算結果の監査ログ
- パフォーマンス: 1件の計算を3分以内、複数件の一括計算対応
- 制限事項:
  - 最終的な支給額決定は人事責任者が確認
  - 特殊な退職事由は個別判断が必要
  - 規定変更時はナレッジベースの即時更新が必須
- スケーラビリティ: 月間100件の退職金計算まで対応可能

## Reference Links

- https://www.nta.go.jp/taxes/shiraberu/taxanswer/gensen/2732.htm
- https://www.mhlw.go.jp/stf/seisakunitsuite/bunya/koyou_roudou/roudoukijun/zigyonushi/taishokukin/
- https://www.dify.ai/blog/payroll-automation

## Tags

- 退職金
- 計算支援
- 退職給付
- 規定
- シミュレーション
- RAG
- 自動化
- 精度向上

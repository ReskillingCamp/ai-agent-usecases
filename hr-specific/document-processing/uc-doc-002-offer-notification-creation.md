---
use_case_id: UC-DOC-002
title: 内定通知書作成システム
category: HR-specific
sub_category: Document Processing
complexity_level: Lv2-3
implementation_types:
  - Prompt-only
  - RAG
tags:
  - 採用
  - 内定
  - 文書作成
  - 通知書
  - 自動化
version: 1.0.0
last_updated: 2025-01-15
rag_config:
  knowledge_base_type: 内定通知書テンプレート・法的文言・企業情報
  embedding_strategy: テンプレートベースの検索
  chunk_size: 600
  top_k: 3
  similarity_threshold: 0.80
  sample_content: 内定通知書フォーマット、雇用条件標準文言、法的必須項目
---

# 内定通知書作成システム

## Overview

候補者情報と採用条件を入力するだけで、法的に適切で丁寧な内定通知書を自動生成するシステム。企業の内定通知書テンプレートをナレッジベースに登録し、RAGで適切な文言を取得しながら、個別の候補者情報を反映した通知書を作成します。従来は人事担当者が30-45分かけて作成していた内定通知書を、5分程度で完成させることができ、誤記や漏れを防止します。法的必須事項の記載漏れがないか自動チェックし、コンプライアンスを確保します。

## Business Value

- 時間削減: 内定通知書作成時間を85%削減（30-45分→5分）
- コスト削減: 人事担当者の工数削減により年間約100万円のコスト削減
- 品質向上: 法的必須事項の記載漏れ防止、統一フォーマットによる品質標準化
- その他の効果: 内定承諾率の向上（迅速な通知による）、候補者体験の向上

## Required Components

- LLM（GPT-4またはClaude）
- ベクトルデータベース（Pinecone, Qdrant等）
- Difyワークフロー機能
- 内定通知書テンプレート（職種別、雇用形態別）
- 法的チェックリスト

## Implementation Steps

1. 内定通知書テンプレートをナレッジベースに登録（職種別、雇用形態別）
2. 法的必須記載事項のチェックリストを作成
3. 入力フォームを設定（候補者情報、職種、給与、入社日等）
4. RAG検索で適切なテンプレートを取得し、個別情報を反映するワークフローを構築
5. サンプルデータでテストし、法的記載事項の漏れがないか検証
6. 実運用開始（内定決定後即座に通知書を生成）

## Sample Prompts

```
以下の候補者情報をもとに、正式な内定通知書を作成してください：

候補者名: {candidate_name}
職種: {job_title}
部署: {department}
入社予定日: {start_date}
雇用形態: {employment_type}
基本給: {base_salary}
勤務地: {work_location}
試用期間: {probation_period}

テンプレートから適切な文言を使用し、以下の法的必須事項が含まれているか確認してください：
- 雇用期間の定めの有無
- 就業場所・従事業務
- 始業・終業時刻、休憩時間
- 賃金の決定・計算・支払方法
- 退職に関する事項

丁寧で誠実な表現を使用してください。
```

```
以下の内定通知書案をレビューし、法的必須記載事項の漏れがないか確認してください：
{draft_offer_letter}

不足している項目があれば指摘し、適切な文言を提案してください。
```

```
{employment_type}（正社員/契約社員/パート）の内定通知書を作成してください。
候補者情報: {candidate_info}
労働条件: {employment_conditions}

企業の標準テンプレートを使用し、温かみのある表現で作成してください。
```

## Data Requirements

- データの種類: 内定通知書テンプレート、法的必須記載事項、企業情報、労働条件標準文言
- データ量: 雇用形態別テンプレート3-5種類、職種別カスタマイズ文言
- データ形式: Word/PDFテンプレート、構造化テキスト
- データ準備:
  1. 既存の内定通知書を収集・分析
  2. 労働基準法に基づく必須記載事項を整理
  3. 雇用形態別・職種別テンプレートを作成
  4. 法務部門による文言レビュー

## Technical Considerations

- システム要件: LLM API（月間200リクエスト程度）、ベクトルDB（100エントリー）
- セキュリティ: 個人情報保護法対応、内定情報の機密管理、アクセス制御必須
- パフォーマンス: 1件の内定通知書生成を30秒以内
- 制限事項:
  - 最終確認は人事担当者・法務部門が必須
  - 特殊な雇用条件は個別調整が必要
  - 労働条件の正確性は人事担当者が責任を持つ
- スケーラビリティ: 月間100件の内定通知書作成まで対応可能

## Reference Links

- https://www.dify.ai/blog/ai-offer-letter-generation
- https://docs.anthropic.com/claude/docs/hr-document-automation
- https://www.mhlw.go.jp/stf/seisakunitsuite/bunya/koyou_roudou/roudoukijun/

## Tags

- 採用
- 内定
- 文書作成
- 通知書
- 自動化
- 労働条件
- コンプライアンス
- テンプレート

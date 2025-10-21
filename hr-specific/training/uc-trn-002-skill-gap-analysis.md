---
use_case_id: UC-TRN-002
title: スキルギャップ分析システム
category: HR-specific
sub_category: Training & Development
complexity_level: Lv4-5
implementation_types:
  - RAG
  - AI Workflow
  - External Integration
tags:
  - スキル分析
  - 人材育成
  - ギャップ分析
  - Skill Assessment
  - Workforce Planning
version: 1.0.0
last_updated: 2025-01-15
---

# スキルギャップ分析システム

## Overview

組織全体のスキル保有状況と事業戦略上必要なスキルを比較し、スキルギャップを定量的に分析するシステム。従業員の現在のスキルレベル、業界トレンド、将来の事業計画を総合的に評価し、組織として不足しているスキル領域を特定します。さらに、個人レベルでのスキルギャップも可視化し、育成計画の立案を支援。従来は人事部門が四半期ごとに1週間かけて実施していた分析を、AIが数時間で完了し、リアルタイムでのスキル状況把握を可能にします。

## Business Value

- 時間削減: 分析時間を95%削減（1週間→4時間）
- コスト削減: 分析工数削減と効率的な育成計画により年間約500万円のコスト削減
- 品質向上: データドリブンな意思決定、スキル可視化精度の向上
- その他の効果: 戦略的な人材配置、事業リスクの早期発見、採用計画の最適化

## Required Components

- LLM（GPT-4 Turbo/Claude 3 Opus）
- Dify RAG機能（スキルデータベース検索）
- Dify Workflow機能（分析パイプライン）
- 人事データベース連携（API）
- スキル評価ツール連携（オプション）
- データ可視化ツール（Tableau/Power BI）

## Implementation Steps

1. スキル分類体系を定義（技術スキル、ビジネススキル、ソフトスキル）
2. 従業員スキルデータベースを構築（自己評価、上司評価、テスト結果）
3. 事業戦略から必要スキルを抽出（戦略文書の分析）
4. RAGによる業界トレンド情報の収集・分析
5. ギャップ分析ワークフローを構築（現状vs理想のマッピング）
6. レポート自動生成機能を実装
7. ダッシュボードでの可視化
8. 四半期ごとの自動分析スケジュールを設定

## Sample Prompts

```
以下のデータをもとに、組織全体のスキルギャップを分析してください：

現在の組織スキルプロファイル:
{current_skill_matrix}

事業戦略上必要なスキル:
{required_skills_from_strategy}

業界トレンド:
{industry_skill_trends}

分析項目:
1. 最も不足しているスキル領域（上位5つ）
2. スキルギャップの深刻度（高/中/低）
3. 不足が事業に与える影響
4. 推奨される対策（採用 vs 育成 vs 外部委託）
5. 優先順位付け

定量的データと定性的評価の両方を含めてください。
```

```
個人レベルのスキルギャップ分析を実施し、育成計画を提案してください：

従業員情報:
- 名前: {employee_name}
- 職種: {job_title}
- 現在のスキルセット: {current_skills}
- 自己評価スコア: {self_assessment}
- 上司評価スコア: {manager_assessment}

目標ポジション: {target_position}
目標ポジションに必要なスキル: {required_skills}

以下を出力してください:
1. スキルギャップマップ（現状と目標の差分）
2. 優先的に伸ばすべきスキル（上位3つ）
3. 具体的な育成アクション
4. 推奨研修プログラム
5. 達成目標タイムライン（6ヶ月/1年/2年）
```

```
以下の部署のスキルポートフォリオを分析し、
リスクと機会を特定してください：

部署名: {department_name}
部署目標: {department_goals}
チームメンバーのスキルデータ: {team_skill_data}
今後6ヶ月の重点プロジェクト: {upcoming_projects}

分析観点:
- スキルの偏り（リスク評価）
- 単一障害点（キーパーソン依存）
- 強化すべきスキル領域
- 外部採用の必要性
- スキル活用の最適化案
```

## Data Requirements

- データの種類: 従業員スキル評価データ、事業戦略文書、業界レポート、職務記述書
- データ量: 従業員数×スキル項目（50-100項目/人）、戦略文書（20-50ページ）、業界レポート（継続更新）
- データ形式: JSON/CSV（スキルマトリックス）、PDF（戦略文書）、テキスト（業界レポート）
- データ準備:
  1. スキル評価システムを導入（定期的な評価実施）
  2. 事業戦略文書をデジタル化
  3. 業界トレンドデータソースを確保
  4. ベクトルデータベースにインデックス化

## Technical Considerations

- システム要件: LLM API（月間5,000-10,000リクエスト）、ベクトルDB（Pinecone/Weaviate）、データウェアハウス
- セキュリティ: 従業員評価データの機密保持、ロールベースアクセス制御、データ暗号化
- パフォーマンス: 組織全体の分析を2時間以内、個人分析を30秒以内
- 制限事項:
  - スキル評価の主観性（自己評価と客観評価の乖離）
  - 暗黙知の可視化の難しさ
  - 評価データの鮮度（定期更新が必要）
- スケーラビリティ: 従業員5,000名、スキル項目500種類まで対応可能

## Reference Links

- https://www.dify.ai/blog/ai-skill-gap-analysis
- https://docs.anthropic.com/claude/docs/workforce-analytics
- https://openai.com/research/talent-intelligence
- https://www.mckinsey.com/capabilities/people-and-organizational-performance/our-insights/skill-shift-automation-and-the-future-of-the-workforce

## Tags

- スキル分析
- 人材育成
- ギャップ分析
- Skill Assessment
- Workforce Planning
- タレントマネジメント
- データ分析
- 戦略的人材開発

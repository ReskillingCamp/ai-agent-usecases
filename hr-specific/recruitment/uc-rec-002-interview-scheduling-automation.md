---
use_case_id: UC-REC-002
title: AI面接日程調整自動化システム
category: HR-specific
sub_category: Recruitment
complexity_level: Lv4-5
implementation_types:
  - Automation
  - AI Tools
tags:
  - 面接
  - スケジューリング
  - 自動化
  - カレンダー連携
  - 採用効率化
version: 1.0.0
last_updated: 2025-01-15
---

# AI面接日程調整自動化システム

## Overview

候補者と面接官の予定を自動調整し、最適な面接日時を提案・確定するシステム。カレンダーAPIと連携し、双方の空き時間を自動検索、候補日時を複数提示して確認メールを自動送信します。従来の手動調整では1件あたり平均30分かかっていた業務を3分に短縮。複数面接官の調整も自動化し、採用担当者の負担を大幅に削減します。タイムゾーン対応やリマインド機能も含め、面接の調整ミスやドタキャンを防止します。

## Business Value

- 時間削減: 1件あたりの調整時間を30分→3分に短縮（90%削減）
- コスト削減: 月間50件の面接調整で採用担当者の工数25時間削減、年間約150万円のコスト削減
- 品質向上: 調整ミス・ダブルブッキングの防止、候補者体験の向上
- その他の効果:
  - 候補者への返信スピード向上（即日返信率95%以上）
  - 面接官の予定を効率的に活用
  - リマインド機能による面接ドタキャン率20%削減

## Required Components

- カレンダーAPI（Google Calendar API、Outlook Calendar API）
- LLM（GPT-4 or Claude）with function calling
- メール送信システム（SendGrid, AWS SES等）
- Dify Automation ワークフロー
- スケジュール管理データベース
- 候補者情報データベース

## Implementation Steps

1. カレンダーAPIとの連携設定（OAuth認証、API権限設定）
2. 面接官の空き時間検索ロジックを構築（複数人の場合の調整ロジック含む）
3. 候補日時提案プロンプトを作成（優先順位ルール、制約条件の設定）
4. 確認メールテンプレートを作成（日本語/英語対応）
5. リマインドメール送信ワークフローを設定（面接24時間前に自動送信）
6. テスト実施（実際の面接官カレンダーで動作確認）
7. 本番運用開始とモニタリング

## Sample Prompts

```
以下の条件で面接日程の候補を3つ提案してください：
- 候補者の希望: {candidate_availability}
- 面接官: {interviewer_names}
- 面接形式: {interview_type} (対面 or オンライン)
- 所要時間: {duration}分
- 制約条件: 平日10:00-18:00、ランチタイム（12:00-13:00）を避ける

各候補日時について、選定理由も添えてください。
```

```
以下の面接日程確定メールを作成してください：
候補者名: {candidate_name}
面接日時: {confirmed_datetime}
面接場所: {location}
面接官: {interviewer_names}
持参物: {required_items}

丁寧で分かりやすい文面で、Googleカレンダーへの追加リンクも含めてください。
```

```
この候補者から「{rescheduling_request}」という日程変更依頼が来ています。
現在の予定: {current_schedule}
面接官の空き状況: {interviewer_availability}

代替日程を3つ提案し、丁寧な返信メールを作成してください。
```

## Data Requirements

- データの種類: 面接官カレンダー、候補者情報、面接タイプ別の所要時間設定
- データ量: 面接官20-30名分のカレンダー、月間50-100件の面接予定
- データ形式: iCal形式カレンダーデータ、JSON形式の候補者情報
- データ準備:
  1. 面接官のGoogleカレンダーまたはOutlookカレンダーへのアクセス権取得
  2. 面接タイプ別の標準所要時間を定義（一次面接：60分、最終面接：90分等）
  3. 禁止時間帯の設定（ランチタイム、定例会議時間等）
  4. メールテンプレートの準備

## Technical Considerations

- システム要件:
  - Calendar API利用（Google/Microsoft）
  - LLM API（月間3,000リクエスト程度）
  - メール送信API（月間200通程度）
- セキュリティ:
  - カレンダーへのアクセス権限は最小限に（read-only推奨）
  - 候補者の個人情報保護
  - OAuth認証の適切な管理
- パフォーマンス:
  - カレンダー検索：5秒以内
  - メール送信：10秒以内
  - 複数面接官の調整：30秒以内
- 制限事項:
  - タイムゾーンが異なる場合は手動確認推奨
  - 複雑な調整（3名以上の面接官）は候補日時が限定的
  - 緊急の日程変更は電話対応が必要な場合あり
- スケーラビリティ: 月間200件の面接まで対応可能

## Reference Links

- https://developers.google.com/calendar/api/guides/overview
- https://learn.microsoft.com/graph/outlook-calendar-concept-overview
- https://www.dify.ai/blog/calendar-automation-with-ai

## Tags

- 面接
- スケジューリング
- 自動化
- カレンダー連携
- 採用効率化
- ワークフロー
- メール自動化
- 候補者体験

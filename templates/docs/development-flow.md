# 開発フロー

1. **作業開始前に必ずIssueを立てる**
   - 機能追加・バグ修正・ドキュメント修正など、全てIssueを起点とする
   - Issueテンプレートを活用し、必要情報を記載する
     - Featureのテンプレート：.github/ISSUE_TEMPLATE/feature_request.yml
     - Bugのテンプレート：.github/ISSUE_TEMPLATE/bug_report.yml

2. **Issueの内容を明記**
   - 目的、完了条件、関連ファイル・仕様（SPEC.md）を明示
   - 参考資料やリンクを追記する

3. **IssueをもとにPull Requestを作成**
   - 1Issue=1PRを基本とし、レビュー・テストを通す
   - PRには必ず関連するIssueをリンクする

4. **コードレビューを依頼**
   - レビューアを指定し、レビューを依頼する

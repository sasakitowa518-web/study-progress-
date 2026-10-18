# Study Progress｜参考書進捗ノート

参考書や教材の学習進捗を管理する日本語のWebアプリです。スマートフォン・パソコンから使えます。

## 機能

- 教材ごとの進捗率、学習済み量、残量の表示
- 記録単位を自由に設定（ページ、問題、章、単語、時間など）
- 完了目標日と、目標に間に合うための1日あたりの学習ペース表示
- 教材の追加・編集・削除、科目別フィルター
- JSON形式のバックアップ／読み込み
- Firebase Authentication によるメールアドレス登録・ログイン
- Cloud Firestore を使った端末間クラウド同期

## 利用・公開方法

1. `index.html` を静的Webホスティングで配信します。
2. アカウント登録とクラウド同期を有効にするには、Firebaseプロジェクトの作成と設定が必要です。
3. 手順は [FIREBASE_SETUP.md](./FIREBASE_SETUP.md) を参照してください。
4. Firestoreには [firestore.rules](./firestore.rules) のユーザー別アクセス制限を設定してください。

Firebase Webアプリ設定は `study-progress-9245d` 用に反映済みです。アカウント登録・クラウド同期を使うには、Firebase Consoleでメール／パスワード認証とFirestoreを有効化し、Firestoreルールをデプロイしてください。詳細は [FIREBASE_SETUP.md](./FIREBASE_SETUP.md) を参照してください。

## Firebase Hosting で公開する場合

リポジトリのルートで Firebase CLI を利用し、プロジェクトを選択してからデプロイします。

```bash
npm install -g firebase-tools
firebase login
firebase use --add
firebase deploy
```

## 注意

- Firebase Web設定の値を設定する必要があります。プレースホルダーのままではアカウント登録・同期は動作しません。
- 同一アカウントで複数端末から同時編集すると、同時更新のタイミングによって最後の保存内容が優先される場合があります。
- パスワード再設定メールの送信機能はまだ実装していません。

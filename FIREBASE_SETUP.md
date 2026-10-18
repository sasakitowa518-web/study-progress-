# Firebase アカウント登録・クラウド同期の設定

このアプリは Firebase Authentication（メールアドレス／パスワード）と Cloud Firestore を使います。Firebase のプロジェクト設定を一度行えば、同じアカウントでログインしたスマートフォンとパソコンで学習データを共有できます。

## 1. Firebase プロジェクトを作成

1. [Firebase Console](https://console.firebase.google.com/project/study-progress-9245d/overview) で既存プロジェクト `study-progress-9245d` を開きます。
2. すでに登録済みのWebアプリ設定をこのリポジトリに反映済みです。再登録や設定値の貼り付けは不要です。
3. このアプリのWeb設定は公開用です。サービスアカウント鍵などの秘密情報を取得・共有・ブラウザへ追加しないでください。

## 2. メールアドレス認証を有効化

Firebase Console → **Authentication** → **始める** → **Sign-in method** → **メール / パスワード**を有効にします。

## 3. Firestore を作成してルールを設定

1. Firebase Console → **Firestore Database** → **データベースを作成**します。
2. リポジトリのルートで `firebase login` を実行し、`firebase use --add` で `study-progress-9245d` を選択します。
3. `firebase deploy --only firestore:rules` を実行し、`firestore.rules` を公開します。
4. ルールはログイン中のユーザーが自分の UID 配下のデータだけを読み書きできる設定です。テスト用の全公開ルールは使わないでください。

## 4. アプリのFirebase設定

`index.html` には、提供された `study-progress-9245d` のWebアプリ設定を反映済みです。GitHubの `main` にあるファイルを使えば、追加のコード編集は不要です。これはブラウザ用の公開設定であり、認証とFirestoreルールでデータへのアクセスを保護します。

## 5. 公開する

Firebase CLI をインストールしてログインし、このリポジトリのルートで実行します。

```bash
npm install -g firebase-tools
firebase login
firebase use --add
firebase deploy --only hosting
```

Hosting とルールをまとめて公開する場合は `firebase deploy` を実行します。初回はCLIの案内に従ってHostingを設定してください。

## 6. 同期を確認

1. アプリを開いてメールアドレスと6文字以上のパスワードで新規登録します。
2. 既存のブラウザ内データがある場合、クラウドデータがまだないアカウントでは初回データとしてアップロードされます。
3. 別の端末で同じアカウントにログインし、教材一覧と進捗が表示されることを確認します。
4. 進捗を変更して、もう一方の端末にも反映されることを確認します。

## データの扱いについて

- 未ログイン時はブラウザの `localStorage` に保存されます。
- ログイン時はクラウドにも保存され、Firestore のリアルタイム購読で変更を受け取ります。
- **初回ログイン時にそのアカウントのクラウドデータがすでに存在する場合、クラウド側のデータが優先されます。** 既存データが重要な場合は、ログイン前に「データを書き出す」でバックアップしてください。
- 同じアカウントを複数端末で同時に編集した場合、アプリは単一ドキュメントを同期するため、ほぼ同時の編集は最後に保存された内容が優先されることがあります。
- パスワードを忘れた場合の再設定メール送信機能は、現在の実装には含まれていません。

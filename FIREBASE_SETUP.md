# Firebase アカウント登録・クラウド同期の設定

このアプリは Firebase Authentication（メールアドレス／パスワード）と Cloud Firestore を使います。Firebase のプロジェクト設定を一度行えば、同じアカウントでログインしたスマートフォンとパソコンで学習データを共有できます。

## 1. Firebase プロジェクトを作成

1. [Firebase Console](https://console.firebase.google.com/) でプロジェクトを作成します。
2. プロジェクトの「アプリを追加」から Web アプリを登録します。
3. 表示された Firebase 設定オブジェクトの `apiKey`、`authDomain`、`projectId`、`appId` をコピーします。

## 2. メールアドレス認証を有効化

Firebase Console → **Authentication** → **始める** → **Sign-in method** → **メール / パスワード**を有効にします。

## 3. Firestore を作成してルールを設定

1. Firebase Console → **Firestore Database** → **データベースを作成**。
2. 「ルール」タブで、このリポジトリの `firestore.rules` の内容を公開します。
3. ルールはログイン中のユーザーが自分の UID 配下のデータだけを読み書きできるよう制限します。テスト用の全公開ルールは使用しないでください。

## 4. アプリに Firebase 設定を反映

リポジトリの `index.html` を開き、`firebaseConfig` のプレースホルダーを Firebase Console の実際の値に置き換えます。

```js
const firebaseConfig = {
  apiKey: "実際のapiKey",
  authDomain: "実際のauthDomain",
  projectId: "実際のprojectId",
  appId: "実際のappId"
};
```

`apiKey` だけでなく、`authDomain`、`projectId`、`appId` もすべて実際の値にします。Firebase Web 設定はブラウザに公開される前提ですが、データ保護は Authentication と Firestore セキュリティルールで行います。サービスアカウント鍵などの秘密情報をブラウザに埋め込まないでください。

## 5. 公開する

Firebase CLI をインストールしてログインし、このリポジトリのルートで実行します。

```bash
npm install -g firebase-tools
firebase login
firebase use --add
firebase deploy
```

Hosting だけを公開する場合は `firebase deploy --only hosting`、ルールだけを反映する場合は `firebase deploy --only firestore:rules` を使えます。初回は Firebase Console で Hosting を有効化するか、CLI の案内に従ってください。

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

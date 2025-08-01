
[English](README.md)  
[العربية](README-ar.md)  
[Português](README-pt.md)  
[Español](README-es.md)  

# 🎓 教育用 UniApp クライアント

これは **UniApp**（Vue.js に基づくクロスプラットフォームフレームワーク）で開発された教育プラットフォーム向けのモバイルクライアントです。学生や教師が教育コンテンツにアクセスし、課題を管理し、学習資料とやり取りできるシームレスな体験を提供します。

> 🔗 このクライアントは（このリポジトリには含まれていない）バックエンド API と連携し、学生向けモバイルポータルとして機能します。

## 📱 機能一覧

- 📚 **コース一覧と登録**
  - 利用可能なコースを閲覧
  - コースの登録・退会
  - 動画や教材付きの詳細ページ

- 📝 **課題と宿題**
  - 宿題の閲覧
  - 解答の提出
  - 教師からのフィードバックや得点の確認

- 🧪 **クイズと試験**
  - 選択式・記述式問題に対応
  - リアルタイム採点とフィードバック

- 🗓 **学習進捗**
  - 完了済み・未完了タスクのダッシュボード
  - コース進捗統計

- 💬 **メッセージと通知**
  - 締切のリマインダーを受信
  - 教師との連絡モジュール（有効化時）

- 👤 **ユーザーセンター**
  - プロフィール編集・パスワード変更
  - 成績と進捗レポートの表示

## 🛠️ 技術スタック

- **フレームワーク：** UniApp（Vue.js ベース）
- **UI ライブラリ：** uView またはカスタムコンポーネント
- **ルーティング：** UniApp ネイティブルーティング
- **ストレージ：** Vuex + localStorage
- **ビルド対象：** H5、WeChat ミニプログラム、Android / iOS（HBuilderX 経由）

## 🚀 はじめに

### 前提条件

- [HBuilderX](https://www.dcloud.io/hbuilderx.html)（UniApp 推奨 IDE）
- Node.js & npm（CLI ビルドを使用する場合）
- バックエンド API（本リポジトリには含まれません）

### インストール手順

1. プロジェクトをクローン：

```bash
git clone https://github.com/feng-lai/Educational_uniapp_client.git
cd Educational_uniapp_client
````

2. **HBuilderX** または Vue 対応の任意のエディタでプロジェクトを開きます。

3. API エンドポイントを設定：

   * `common/request.js` などの設定ファイル内のベース URL を更新します。

4. 対象プラットフォームで実行：

   * HBuilderX：「ブラウザ / ミニプログラム / エミュレーターで実行」をクリック
   * CLI：`npm run dev:%platform%`（例：`dev:h5`, `dev:mp-weixin`）

## 📁 プロジェクト構成

```
Educational_uniapp_client/
├── pages/              # ページコンポーネント
├── components/         # 再利用可能な UI コンポーネント
├── store/              # Vuex 状態管理
├── static/             # 静的リソース（画像など）
├── common/             # ユーティリティ・リクエスト設定
├── uni.scss            # グローバルスタイル
└── main.js             # エントリーポイント
```

## ⚙️ 設定のヒント

* ✅ H5 でテストする際は API サーバーが CORS に対応していることを確認
* 🔒 認証にはトークンベース（JWT 推奨）を使用
* 🌐 多言語対応には i18n プラグインを使用可能

## 📄 ライセンス

このプロジェクトは MIT ライセンスの下で公開されています。個人・商用を問わず、出典を明記すれば自由に改変・使用可能です。

## 🙋 作者

開発・保守： [feng-lai](https://github.com/feng-lai)

---

> 📢 *プルリクエストやフィードバックは大歓迎です。教育アプリを構築している方にとって、本クライアントは素晴らしい出発点となります！*



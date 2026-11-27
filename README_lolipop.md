# 試作をロリポップに移す手順

GitHub Pages（誰でも見られる）から、ロリポップのパスワード付きの場所に移すための手順です。
上から順に1つずつ進めます。

## 準備済みのもの（このリポジトリ）

- 8th Wall のエンジン（`lib/8thwall/`）と XRExtras（`lib/xrextras/`）をリポジトリ内に置き、外部の配信サービスから読み込まないようにした
- 検索に出ないように `noindex` を入れた
- `.github/workflows/deploy-lolipop.yml`：GitHub からロリポップへ FTPS でアップロードする設定（手動で動かす）
- 五台橋の3Dデータ（`godai/`）はロリポップには上げない設定

## 手順

1. **置き場所を決める**
   asamatwin.jp とは分ける。例：サブドメイン `ar-test.asamatwin.jp`、公開フォルダ `webar-test`。
   ロリポップのユーザー専用ページ →「サーバーの管理・設定」→「独自ドメイン設定」で追加する。
2. **HTTPS（無料SSL）を設定する**
   カメラは HTTPS でないと動かない。「セキュリティ」→「独自SSL証明書導入」で、1のサブドメインに無料SSLを設定する。
3. **パスワードをかける**
   「セキュリティ」→「アクセス制限」で、フォルダ `webar-test` にIDとパスワードを設定する。
4. **FTPの情報を確認する**
   「アカウント情報」で、FTPサーバー名とFTPアカウントを確認する。
5. **GitHub に登録する**
   リポジトリの Settings → Secrets and variables → Actions →「New repository secret」で3つ登録する。
   - `LOLIPOP_FTP_SERVER`：FTPサーバー名
   - `LOLIPOP_FTP_USER`：FTPアカウント
   - `LOLIPOP_FTP_PASSWORD`：FTPパスワード
6. **アップロードする**
   Actions →「Deploy to Lolipop」→「Run workflow」。緑のチェックが付けば完了。
   フォルダ名を `webar-test` 以外にした場合は、`deploy-lolipop.yml` の `server-dir` を合わせる。
7. **スマホで確認する**
   `https://ar-test.asamatwin.jp/` を開き、パスワードを入れて、カメラ・位置情報・撮影まで動くか見る。
8. **GitHub Pages を止める**
   7で問題がなければ、リポジトリを非公開にする（Settings → General → 一番下の「Change visibility」）。
   無料プランでは、非公開にすると GitHub Pages も見られなくなる。

## うまくいかないとき

- 6で失敗する：ロリポップのFTPが海外からの接続を制限していないか確認する（GitHub の実行環境は海外）。
- 7で何も出ない：パスワード付きの場所で、エンジンの追加ファイル（`lib/8thwall/xr-slam.js` など）が読めているかを確認する。
- 本番も同じ流れ（GitHub → Actions → ロリポップ）にそろえる。竹内さんの既存サイトの設定があれば、それに合わせる。

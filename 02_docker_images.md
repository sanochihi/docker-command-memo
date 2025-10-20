# Docker イメージ についてのメモ

## このパートの目的

- Docker イメージの概要と目的を理解しよう
- コンテナレジストリについて知ろう
 - コンテナレジストリの目的と、様々なオプションを知ろう
 - Docker Hub でのイメージの探し方を知ろう
- CLI で Docker イメージを管理しよう
- Dockerfile を作って image を build できるようになろう
- コンテナとイメージの違いを知ろう

## Docker イメージとは？

- コンテナの設計図
- 同じイメージを run したら、初期状態は必ず同じコンテナが出来上がる

### Docker イメージのレイヤー

- 設定
	- アプリケーションや環境の設定
- アプリケーションのコード
	- 稼働したいアプリケーション自体のコード or バイナリ
- ライブラリと依存関係
	- アプリケーションに必要なすべての外部コード
- ランタイムの環境
	- アプリケーションに必要なソフトウェア（Python, Node.js など）
- ベースレイヤー
	- OS(ubuntu, Alpine など)

コンテナのある時点の情報をフリーズしてイメージを作ることもできるが、諸々の理由で Dockerfile を使う方がおすすめ（理由は後述）

## Docker イメージの主な入手元

- Docker Hub 
 - Docker の公式なイメージレポジトリ
- プライベートレジストリ
 - 組織特有のイメージをストアする場合やエアギャップ環境などで、個別にレポジトリを作る
- 自分でイメージを作る

## コンテナレジストリ

なぜ必要なのか
- 共有のため
- バージョニング
- セキュリティ
- 自動化（CI/CDとの接続）

コンテナレジストリの種類
- パブリック（Docker Hub など）
- プライベート

## タグづけのコマンド

```
docker tag 手持ちのイメージ名:バージョン 別名のイメージ名:バージョン
```

## レポジトリに docker イメージを push する

`docker login` が完了した状態で以下を実施

```
docker tag 手持ちのイメージ名:バージョン dockerhubのユーザー名/イメージ名:バージョン
```

## Dockerfile について

### Dockerfile の構造

- ベースイメージを指定し、それに対する変更点を記述する

### Dockerfile の利点

- 再現性をもってイメージを構築できる
- イメージの構築を自動化できる
- 透明性があり、ドキュメントとしての役割も果たす
- 最適化：ニーズに合ったベースイメージを選択し、それをアレンジできる

### 留意点

- Dockerfile の各指示ごとに、中間的なイメージファイルが作られる（イメージIDがふられる）[ほんとか？]

## Dockerfile を作ってみよう

project02 配下に以下のファイルを作成

- Dockerfile

### Dockerfile の構文と作法

```
# どのイメージをベースにするか
FROM nginx:1.27.0

# そのイメージに対して必ずかける処理
RUN apt-get update
RUN apt-get -y install vim # 対話は許容しないので、Y/n のプロンプトが出る系コマンドは -y を指定

```

デフォルトのファイル名は `Dockerfile`。
Dockerfile を作成したら、以下のように build する。

`docker build -t [イメージ名] [Dockerfileがあるフォルダへのパス(.など)]`

できたら `docker images` で確認。

イメージができていたら、あとは以下のコマンドで run してみる。

`docker run -d [イメージ名]`

できたら `docker ps` で確認。

`docker exec -it [コンテナID] [シェルへのパス（sh, /bin/bash など）]` で中に入る

-> 上記の例の Dockerfile を作っていれば、最初から vim がたたける状態になっている。

### キャッシュ

同じ Dockerfile を使って2回目以降に build する場合、同じコマンドはキャッシュされていることがある

```
╭─chihiro@chihiro ~/Desktop/docker-command-memo/project02 ‹main●› 
╰─$ docker build -t web_server_image .
[+] Building 0.0s (7/7) FINISHED                                                                                                                                                                     
 => [internal] load build definition from Dockerfile                                                                                                                                            0.0s
 => => transferring dockerfile: 101B                                                                                                                                                            0.0s
 => [internal] load metadata for docker.io/library/nginx:1.27.0                                                                                                                                 0.0s
 => [internal] load .dockerignore                                                                                                                                                               0.0s
 => => transferring context: 2B                                                                                                                                                                 0.0s
 => [1/3] FROM docker.io/library/nginx:1.27.0                                                                                                                                                   0.0s
 => CACHED [2/3] RUN apt-get update                                                                                                                                                             0.0s ### <- これ!!
 => CACHED [3/3] RUN apt-get -y install vim                                                                                                                                                     0.0s ### <- これ!!
 => exporting to image                                                                                                                                                                          0.0s
 => => exporting layers                                                                                                                                                                         0.0s
 => => writing image sha256:09e2b24f0462fc316103243f02bc0a0a10313ade7b80885334d61292319c90c5                                                                                                    0.0s
 => => naming to docker.io/library/web_server_image                              
```



















# Docker の基本コマンド

## Docker のライフサイクルに関わるコマンド

### コンテナを起動させる

```
docker run <image>
```

⬆️ `docker create` と `docker start` を同時にやってくれる


```
docker run -d -p 8080:80 --name=web-server nginx
```

⬆️バックグラウンドで(`-d` <- detouched mode という)、コンテナの 80番ポートを 8080 にポートフォワードして（`-p 8080:80`）、web-server という名前で（`--name=web-server`）、nginx という image のコンテナを動かしてね

### コンテナの一時停止／一時停止の解除

```
docker pause <コンテナID>
```
-> メモリの内容を保持する


```
docker unpause <コンテナID>
```

### コンテナの停止

```
docker stop <コンテナID>
```
-> メモリの内容は保持されない

### コンテナの強制停止

```
docker kill <コンテナID>
```
-> メモリの内容は保持されない

### コンテナの削除

```
docker rm <コンテナID>
```
-> メモリの内容は保持されない

## docker ps のオプション

`-a` - 停止中のコンテナの情報も表示する（rmすると表示されなくなる）

`-q` - コンテナIDのみを表示する

-> このコマンドを利用して、`docker rm $(docker ps -aq)` で全コンテナ削除もできる

`--filter` - 表示対象のコンテナをフィルタリングする（例 `name=<コンテナ名>` など）

## コンテナ稼働中に実施できるコマンド

```
docker logs [-f] <コンテナID もしくは コンテナ名>
```

⬆️稼働中のコンテナのログを表示する
`-f` をつけると、ログを tail して watch するようなイメージになる

```
docker inspect
```

```
docker inspect
```
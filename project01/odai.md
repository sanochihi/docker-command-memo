# プロジェクトのお題

- NGINXのイメージをベースとして、1.27.0 というバージョンのタグをつけて稼働させる
- コンテナの中でインタラクティブシェルを稼働させる
- vim をインストールする
- index.html を編集してカスタムコンテンツを公開する

# 手順

## 1. nginx:1.27.0 を pull して起動させる

Docker Hub で Nginx の Docker イメージの 1.27.0 を探す
https://hub.docker.com/_/nginx/tags?name=1.27.0

何もおまけがついていない Docker イメージのコマンドを探し、pull する

```
docker pull nginx:1.27.0
```

nginx の 1.27.0 を detached モードで稼働させる
（ポート80は競合しやすいので、別のポートを使う）
```
docker run -d  -p 8088:80 --name web_server nginx:1.27.0
```

nginx の 1.27.0 を detached モードで稼働させる
```
curl http://localhost:8088
```

## 2. nginx のコンテナの中身を修正する 
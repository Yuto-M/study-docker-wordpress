# study-docker-wordpress

# 概要
dockerでwordpress環境構築（勉強用）

## Goal
- `docker-compose up -d`でwordpressの環境が作成できるようにする。
    - docker-compose.ymlを書く
- コンテナを削除してもデータが消えないように永続化する。
    - data volumeを使用して永続化
    - data volume conainerを使用して永続化
- 永続化したデータのbackup・restoreができるようにする。
    - ホストマシンのディレクトリとマウントしている、data volume containerをマウントしたコンテナを立ち上げて、そこでtarでbackupを固めてだす（backupの取得）。
    - そのあと、そのbackupを使用してdata volume containerを立ち上げてそのcontainerにマウントする。

# dockerコマンド
## docker-compose系
### コンテナ起動・停止・削除
```
docker-compose up -d
docker-compose down
```

## network系
### network確認
```
docker network ls
```

### ネットワークに接続しているコンテナを調べる
```
 docker network inspect ${network-name}
 ```

## data volume系
### data volume確認・削除
```
docker volume ls
docker volume rm ${data_volume_name}
```

### data volumeにマウントされていることを確認
```
docker inspect ${db_container_name}
```

### data volumeバックアップ??
http://docs.docker.jp/engine/userguide/dockervolumes.html
```
docker create -v /studydockerwordpress_db_data --name dbdata training/postgres /bin/true

docker run --volumes-from dbdata -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /dbdata
```

### ~~data volumeを使用してやると分かりづらそうなら、純粋にホストのディレクトリとコンテナのディレクトリを同期させる事もできる~~
### ホストのディレクトリと同期させるのは基本開発に使用するソースコードとかで、動作を確認したいものに使う
```
http://docs.docker.jp/compose/wordpress.html
```

## 参考にしたサイト
https://keruuweb.com/docker%E3%81%A7wordpress%E3%82%92%E7%B0%A1%E5%8D%98%E3%81%AB%E7%AB%8B%E3%81%A1%E4%B8%8A%E3%81%92%E3%82%8B%E6%96%B9%E6%B3%95/
https://keruuweb.com/docker-wordpress%E3%82%92%E6%B0%B8%E7%B6%9A%E5%8C%96%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95/
http://dqn.sakusakutto.jp/2015/10/docker_mysqld_tutorial.html
http://docs.docker.jp/engine/userguide/containers/dockervolumes.html

networkについて
http://docs.docker.jp/engine/userguide/networkingcontainers.html

## 参考にしたいサイト
https://docs.docker.com/compose/wordpress/#define-the-project
https://cntnr.io/setting-up-wordpress-with-docker-262571249d50

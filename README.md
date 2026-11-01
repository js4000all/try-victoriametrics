# demo-victoriametrics

VictoriaMetricsとGrafanaのデモ環境

## 構成

- vmagent: メトリクスの収集と転送
- vmstorage: データの保存
- vminsert: データの挿入処理
- vmselect: クエリ処理
- grafana: 可視化

## 起動方法

```bash
docker compose up -d
```

## アクセス方法

- Grafana: http://localhost:43000
  - ユーザー名: admin
  - パスワード: admin

## データソースの設定

Grafanaにログイン後、以下の手順でVictoriaMetricsをデータソースとして追加してください：

1. 左メニューの「Configuration」→「Data sources」を選択
2. 「Add data source」をクリック
3. 「Prometheus」を選択
4. URLに `http://vmselect:8481/select/0/prometheus/` を入力
5. 「Save & Test」をクリック


## バックアップ取得

```bash
mkdir -p ./backup
docker compose \
  -f docker-compose.yml \
  -f compose.backup.yml \
  run --rm --no-deps vmbackup
sudo chown -R 1000:1000 ./backup
```

固める。
```bash
tar -czf vmstorage-backup.tar.gz -C ./backup vmstorage
tar -tzf vmstorage-backup.tar.gz > /dev/null
sha256sum vmstorage-backup.tar.gz > vmstorage-backup.tar.gz.sha256
sha256sum -c vmstorage-backup.tar.gz.sha256
```

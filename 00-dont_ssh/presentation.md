footer: (C) Tsukuru Tanimichi, 2016
slidenumbers: true

# SSH 禁止

---

# 自己紹介

- フリーランスのプログラマー
- 現在は Aiming で働いています

ここにスキルの表を入れておく。ttanimichi.com からのコピペでいい

- github.com/ttanimichi
- blog.ttanimichi.com

---

# TL;DR

- 手動で SSH しなくていいはず
  - ちゃんと自動化されていれば
  - DevOps 徹底しよう
- 誰かが SSH したら Slack に通知する設定を仕込んでおく
  - 怒るよ！！
  - というのは嘘で、何が自動化されてないのか把握したい

---

# [fit] 構成変更に SSH 不要なはず

- Ansible, Chef
  - あるいは Docker
- 確認も手動で SSH する必要ないはず
  - Serverspec 使ったほうが楽だよ
  - Serverspec はシンプルで学習コストも低い

---

# [fit] リソース使用量を見るのに SSH 不要なはず

- リソース使用量を確認する Unix コマンド
  - top, df, du, ps
- Mackerel とか Sensu とかで監視していれば不要なはず
  - サーバーが 1000 台あったら 1000 回 top 叩くのか

---

# [fit] ログを見るのに SSH 不要なはず

- ログは fluentd とかで収集しよう
  - BigQuery, Redshift, TD, Treasure Data
- 収集していれば手動で SSH して grep する必要ないはず
- Heroku の The Twelve-Factor App にも書いてある
  - [http://12factor.net/ja/logs](http://12factor.net/ja/logs)

---

# はい、これは理想論です

---

# [fit] 実際には SSH しちゃうこともある

---

# [fit] でも頻繁に SSH していたら何かがおかしい

---

# 頻繁な SSH は不吉な匂い

- 何かが自動化できていない？
  - 構成変更を手動でやっている？
  - デプロイのフローに自動化できていない部分がある？
- メトリクスを監視していない？
- ログを収集していない？

---

# 誰かが SSH したら Slack に通知

ここに画像を入れる

```sh
# /etc/ssh/sshrc

client_ip=`echo ${SSH_CLIENT} | cut -d " " -f 1`
curl -X POST --data-urlencode "payload={\"channel\": \"#link-dev\",
```

- デプロイやプロビジョニング用のユーザーは分けておく
- 誰かが手動で SSH している場合にだけ通知を飛ばす

---

# 次のステップ

- サーバーで実行されたコマンドを Slack に通知したい
  - `.bash_history` の差分を Slack に通知
  - fluentd の tail input plugin を使えばできそう

---

# [fit] ご静聴ありがとうございました


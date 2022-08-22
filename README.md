# slack_export

Slack のチャンネルから全メッセージをダウンロードするスクリプトです。
対象範囲は、パブリックチャンネル、プライベートチャンネル、複数人のダイレクトメッセージ、ダイレクトメッセージです。
ただし実行したユーザーが閲覧権限を持っているものに限ります。

## 動作環境

- Python: 3.9.13
- requests: 2.28.1

## OAuth トークンの入手

[Slack Web API](https://api.slack.com/methods) を実行するために、Slack App を作成して OAuth トークンを入手します。

1. Slack App を作成する。[App の作成ページ](https://api.slack.com/apps)を開き、`Create New App` を押す。

![Create New App](./images/create.svg)

2. `From an app manifest` を選択し、`manifest.yaml` の内容を貼り付けて、あとは指示通り進む。

![From an app manifest](./images/from-manifest.svg)
![Paste manifest](./images/paste-manifest.svg)

3. 作成した App の Basic Information ページで `Install your app` を押し、インストール先のワークスペースを選択する。

![Install to Workspace](./images/install.svg)

4. OAuth & Permissions ページの `User OAuth Token` を控えておく。

![User OAuth Token](./images/token.svg)

## 使い方

`slack_export.py` を以下のように実行します。
`token` パラメータには、先ほど入手したトークンを指定してください。
以下の例では、実行するとカレントディレクトリに `output` ディレクトリが作成され、その中にチャンネルごとに JSON 形式で保存されます。

```console
$ python slack_export.py \
    --token xxxxx \
    --output-dir output
INFO:__main__:Fetching users
INFO:__main__:xxx users fetched
INFO:__main__:Fetching channels
INFO:__main__:xxx channels fetched
INFO:__main__:Fetching messages: channel_name='xxxxx'
INFO:__main__:xxx messages/replies fetched
...
```

NDJSON 形式で出力したい場合は `--output-format jsonl` オプションを追加してください。

```console
python slack_export.py \
    --token xxxxx \
    --output-dir output \
    --output-format jsonl
```

## 出力ファイルの例

```console
$ head -n 10 output/random.json
[
    {
        "subtype": "channel_join",
        "text": "<@UXXXXXXXXXX>さんがチャンネルに参加しました",
        "ts": "0000000000.000000",
        "type": "message",
        "user": "UXXXXXXXXXX"
    },
    ...
```

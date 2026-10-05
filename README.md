# aidd-playbook-marketplace

研修「Claude Code チーム開発実践〜AI-native な開発サイクルを体験する〜」の講師用 Claude Code プラグイン marketplace です。組織の API ポリシーを Skill にして配布し、Design 工程（ワーク2）で spec に反映させます。

## 収録しているプラグイン

| プラグイン | Skill | 内容 |
|---|---|---|
| `secure-api` | `secure-api` | 外部公開エンドポイントを作る・変えるときに、認証・認可、入力検証、個人情報の最小化、監査ログ、エラー応答の観点を spec と実装に反映させる |

## 構成

```text
.
├── .claude-plugin/
│   └── marketplace.json              # marketplace の定義（name: aidd-playbook-marketplace）
└── plugins/
    └── secure-api/
        ├── .claude-plugin/plugin.json
        └── skills/secure-api/SKILL.md
```

## 題材リポジトリへの登録（ワーク2）

題材リポジトリ（aidd-playbook-sample から作った自分のリポジトリ）の `.claude/settings.json` に次を追記してコミットします。`<講師の org>` は講師が当日提示します。リポジトリをクローンした人が Claude Code を起動すると、marketplace の追加とプラグインの有効化を確認されます。

```json
{
  "extraKnownMarketplaces": {
    "aidd-playbook-marketplace": {
      "source": { "source": "github", "repo": "<講師の org>/aidd-playbook-marketplace" }
    }
  },
  "enabledPlugins": {
    "secure-api@aidd-playbook-marketplace": true
  }
}
```

`enabledPlugins` のキーは `<プラグイン名>@<marketplace 名>` です。

Claude Code のセッションで `/plugin` を開き、`secure-api` が有効になっていることを確かめます。Skill は spec を書く場面で自動的に使われます。明示的に使う場合は `/secure-api:secure-api` と入力します。

## 手元での確認

```bash
claude plugin validate .
claude plugin marketplace add ./
claude plugin install secure-api@aidd-playbook-marketplace
```

GitHub に置く前に試すときは、`.claude/settings.json` の `source` をローカルのディレクトリにします。

```json
{
  "extraKnownMarketplaces": {
    "aidd-playbook-marketplace": {
      "source": { "source": "directory", "path": "/path/to/aidd-playbook-marketplace" }
    }
  }
}
```

## 更新

Skill を変えたら、`plugins/secure-api/.claude-plugin/plugin.json` と `marketplace.json` の `version` を上げてコミットします。参加者は `/plugin marketplace update aidd-playbook-marketplace` で更新を取り込めます。

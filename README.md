# CoSpec
CoSpecは、AIとの議論・確定仕様・実行を分離し、人間が仕様決定権を保持したまま開発を進めるためのCodex Skillです。
## Installation

CoSpecを対象プロジェクトへ配置します。

```text
<project>/.agents/skills/CoSpec/
```

`AGENTS.md` から参照します。

```markdown
このリポジトリで作業する際は、`.agents/skills/CoSpec/SKILL.md`を参照し、指示に従うこと。
```

通常のGit操作からCoSpec自身を除外するため、`.git/info/exclude` に追加します。

```gitignore
.agents/skills/CoSpec/
```

追加の依存関係やシェルスクリプトはありません。
## Usage

通常の入力はDiscussionとして扱われます。

```text
認証方式について検討したい
```

議論を確認：

```text
check
```

未解決点を暗黙Checkし、原則3パターンの仮実装仕様として一括検討：

```text
br
```

Brute Reasoning（BR）の出力はAIによる仮決定です。実装する場合は、対象パターンと修正内容を人間が明示してから`exe`を使用します。

確定した内容をSpecificationへ反映：

```text
この方針を仕様へ反映して
update
```

直前のDiscussionを実行：

```text
exe
```

同じ入力内で承認して実行：

```text
ログ出力を追加してください
exe
```

`exe` による一時的な変更はSpecificationへ自動保存されません。恒久化する場合は `update` を使用します。
### Commands
それぞれのアクションを手動で起動するには、これらのコマンドを実行してください。
なお、Codexそのもののコマンドと混合するのを防ぐために/は排除しています。
`exe`、`br`、`shelf`は明示tokenでのみ起動します。その他のアクションは文脈判断からも起動する場合があります。
| Token | Role |
|---|---|
| `check` | 矛盾・曖昧さ・不足の確認 |
| `assumption` | 暗黙の前提の整理 |
| `summary` | 議論状態の要約 |
| `br` | 暗黙Checkと複数の仮実装仕様の一括構成 |
| `search` | 外部調査 |
| `shelf` | 登録資料の参照 |
| `mem` / `memo` | Logへの保存 |
| `update` / `upd` | Specificationへの反映 |
| `exe` | 実装・外部操作の承認 |

## Structure
```text
CoSpec/
├── SKILL.md
├── discussion.md
├── brute_reasoning.md
├── search.md
├── shelf.md
├── memo.md
├── update.md
├── exe.md
├── Git.md
├── memory_schema.md
├── memory/
│   ├── log/
│   └── specification/
│       └── archive/
└── shelf/
    └── index.md
```

## Execution Safety

CoSpecはExeにおける主要なexecution faultを4つに分類します。

- Secret Leakage
- Overengineering
- Reward Hacking
- Specification Neglect

OverengineeringとReward Hackingには、**一般→最小選抜**を適用します。

```text
承認された目的
↓
一般的に成立する実装候補
↓
最小の候補を選抜
↓
実行
```

サンプルやテストだけを攻略する実装を避けながら、目的達成に不要な抽象化・refactor・依存関係・future-proofingも抑制します。

Discussionで案を提案する場合も、目的を一般的に満たす案を先に判定し、その中から不要な論点や抽象化を含まない最小の案を優先します。

Exeで既存ファイルを消去する場合は、消去直前の内容がGitから復元可能であることを確認します。未保存内容はsecret検査後に対象限定のローカルbackup commitへ保存し、安全なバックアップを保証できなければ削除前に停止します。backup commitを自動的にpushすることはありません。

## Notes

- AIの提案は自動的に確定仕様になりません。
- Log・archive・Shelfは現行Specificationではありません。
- デフォルトの状態では、Memoryはプロジェクト間で共有されません。
- secretsはMemoryへ保存しません。
- 通常のGit操作ではCoSpec自身を除外します。

このREADMEは利用方法の概要です。  
正確な動作規則は `SKILL.md`、`exe.md`、`Git.md` など各制御文書を正本とします。

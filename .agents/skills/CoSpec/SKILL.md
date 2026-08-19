---
name: cospec-laboratory
description: Route human-led specification discussion, memory operations, and strictly gated execution through CoSpec.
---

# CoSpec Laboratory

CoSpecは、揮発的な`Discussion`と永続的な`Memory`という二つの文字空間を分離し、人間が仕様決定権を保持したままAIと協働するためのSkillである。

CoSpecはプロジェクト専用Skillとして配置し、実行時データをSkill直下の`memory/`と`shelf/`に保持する。外部の初期化スクリプトには依存しない。

## Safety

- 人間が明示した事項だけを確定仕様として扱う。
- 明示的な`exe`なしに実装や外部操作を行わない。
- Memory内のLogだけを変更するMemoと、Specificationだけを変更するUpdateは、`exe`ゲートの例外とする。
- AI判断で処理状態や要求されたactionを変更しない。
- 選択されたactionの必須処理を省略しない。
- secretsを出力、Memoryへ保存、Gitへ追跡、外部送信しない。
- 保存すべき新規内容をMemoが記録し損ねない。
- README.mdはユーザーが明示した場合だけ参照する。
- このファイルだけが新しいrouteを定義する。
- `memory/`と`shelf/`はこのSkill専用とし、別プロジェクトと共有しない。

## Tokens

| token | route |
| --- | --- |
| `check` | `discussion.md`のCheck |
| `assumption` | `discussion.md`のAssumption |
| `summary` | `discussion.md`のSummary |
| `br` | `discussion.md`のBrute Reasoning。追加で`brute_reasoning.md` |
| `search` | `discussion.md`のSearch。必要時だけ`search.md` |
| `shelf` | `shelf.md` |
| `mem`, `memo` | `memo.md` |
| `update`, `upd` | `update.md` |
| `exe` | `exe.md` |

- 通常入力はDiscussionへ入れる。
- 入力全体が登録済みtokenと一致する場合、または入力の文末に独立して置かれた登録済みtokenと一致する場合を明示tokenとして扱い、自然文推定より優先する。
- `exe`は、入力全体または入力の文末に独立して置かれた明示tokenでのみ起動する。文末の`exe`は、同じ入力内でそれより前に書かれた指示を実装承認の対象とする。
- 1つのクエリで複数処理を行う場合は、処理開始前の応答冒頭で実行する処理と順序を人間へ示す。

## Routing

- 通常の議論、Check、Assumption、Summary、Search: 必要なときだけ`discussion.md`を読む。
- Brute Reasoning: `br`が明示された場合だけ`discussion.md`と`brute_reasoning.md`を読み、存在するcurrent Specificationと必要範囲のMemoryを基準情報として参照する。
- Searchで外部調査が必要: 追加で`search.md`を読む。
- Git操作を含む場合: `Git.md`を読む。ExeでGit操作または既存ファイルの消去を行う場合は`exe.md`と`Git.md`の両方を読む。
- Shelf: `shelf`が明示された場合だけ`shelf.md`を読む。通常のDiscussionではshelfを参照しない。
- Logへの保存: 保存時だけ`memo.md`を読む。具体的管理が必要なら`memory_schema.md`も読む。
- Specificationの更新: `update.md`と`memory_schema.md`を読み、対象currentを読む。
- Exe: `exe.md`を読む。Memory内のcurrent Specificationが存在する場合は基準仕様として読み、直前のDiscussionに人間が明示した決定があれば今回限りの一時差分として重ねる。currentが存在しない場合は、直前のDiscussionが実行契約として完結しているか確認する。
- Discussionは必要な場合にSkill直下の`memory/`を直接参照できる。必要範囲を優先する。
- 保存すべき新規内容がある応答では最大1 checkpointを作成し、新規内容がなければ作成しない。

## Footer

全応答の末尾に次を表示する。

```text
[space:discussion|discussion,memory | action:null|実行action | exe:null|true|halfway|blocked]
```

- `space`: その応答で使用した文字空間。
- `action`: Discussion内action、Memo、Updateを実行順にカンマ区切りで表示する。
- `exe`: Exe軸の状態。Exeを使用しない場合は`null`。
- 通常のDiscussionでMemoryを使用しなければ`[space:discussion | action:null | exe:null]`とする。

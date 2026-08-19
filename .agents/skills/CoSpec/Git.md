---
type: cospec_git_contract
axis: exe
---

# Git contract

Git操作には、この文書の追加契約を適用する。

## CoSpec exclusion

- 通常のプロジェクトGit操作では、`.agents/skills/CoSpec/**`をstage、commit、pushの対象に含めない。
- ローカルな予防策として、`.git/info/exclude`に`.agents/skills/CoSpec/`を登録する。ただし、この設定だけに依存しない。
- CoSpec自身の開発または配布を人間が明示した場合だけ、対象を示したうえで除外規則を解除できる。
- stage前に追加対象を確認し、CoSpecを含む広範なstageを避ける。
- stage後かつcommit前にstaged fileとstaged diffを確認し、CoSpecが含まれていれば停止する。
- push前にremoteへ送信予定のcommitと差分を確認し、CoSpecが含まれていれば停止する。
- `.git/info/exclude`は追跡済みファイルへ作用しない。CoSpecが既に追跡、stage、commitされている場合は状態を報告して停止し、AI判断でunstage、履歴改変、commitの作り直しを行わない。
- pushはcommit単位の操作であり、push時に個別ファイルを除外できるとは扱わない。

## Secret inspection

- stage前、commit前、push前の各段階で、Gitへ入る対象にsecretsまたは疑わしい値がないか検査する。
- stage前は、追加予定のファイル名と内容を検査する。秘密情報を示唆する設定、環境変数、秘密鍵、認証情報、backup、dumpその他のファイルを重点確認する。
- commit前は、staged fileとstaged diffを検査する。API key、token、password、秘密鍵、接続文字列、cookie、認証headerその他の既知パターンに加え、高entropyまたは不自然に長い認証文字列を疑わしい値として扱う。
- push前は、remoteとの差分として送信予定の全commit、変更ファイル、内容を検査する。直近commitだけを検査対象にしない。
- binary、archive、生成物その他の十分な内容検査ができない対象は、検査不能または限定的検査として扱う。
- 利用可能な専用secret scannerが既に存在する場合は使用できる。未承認のscannerまたは依存関係をインストールしない。
- secretsまたは疑わしい値を検出した場合は、commitまたはpushを停止する。
- 検査結果にはファイルパス、必要最小限の位置、疑いの分類だけを示し、secretの実値を出力、Memoryへ保存、Logへ保存、Gitへ追跡、外部送信しない。
- 既にcommitまたは公開されたsecretを検出した場合は状態とcredentialの無効化または再発行の必要性を報告し、AI判断で削除、履歴改変、credential操作を行わない。
- 検査不能な対象がある場合、または検査を通過した場合も、secretが存在しないと断定しない。確認できた範囲と限界を示す。

## Deletion backup

- Exeで既存ファイルを消去する場合は、消去直前の内容がGit上の到達可能なcommitから復元できることを消去前に確認する。
- 消去直前の内容と同一の内容が既に到達可能なcommitへ保存されている場合、新しいバックアップcommitは作成しない。
- 未commitの変更またはuntrackedファイルを消去する場合は、対象のファイル名と内容をsecret検査し、消去対象だけを含むバックアップcommitを作成する。無関係なstaged file、作業中の変更、CoSpec除外対象を混入させない。
- Specificationまたは承認された実行契約で、再生成可能かつ廃棄可能と明示された一時物だけはバックアップ対象から除外できる。AI判断で一時物または廃棄可能と推定しない。
- 削除を承認するExeは、この契約に必要なローカルのバックアップcommit作成を付随操作として承認する。pushその他のremote操作の承認には使用しない。
- Gitリポジトリでない、対象内容を参照できない、secretもしくは疑わしい値を含む、安全な対象限定commitを作成できない、または復元可能性を確認できない場合は、消去前に停止する。
- secretまたは疑わしい値をバックアップ目的でGitへ追跡しない。CoSpec自身の消去では、CoSpec exclusionの明示的な例外承認も必要とする。

## Stop conditions

- CoSpecの混入、secretsもしくは疑わしい値、検査不能な送信対象、送信範囲の不確定、またはremote差分の参照不能があれば、該当するGit操作を停止する。
- 停止時は機密値を伏せたまま、原因と人間に必要な判断だけをDiscussionへ戻す。

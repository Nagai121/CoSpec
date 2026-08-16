---
type: cospec_update
bridge: discussion_to_specification
---

# Update

Updateは、人間が明示的に確定した事項だけをDiscussionからSkill直下の`memory/specification/current.md`へ反映する。

Updateは`exe`ゲートの例外であり、明示的な更新要求だけで単独実行できる。

- `update`、`upd`、または「仕様に反映して」「specを更新して」等の明示的な更新要求で実行する。
- AIの提案、未採用アイデア、推論を確定仕様へ入れない。
- currentが存在しない場合は、書込み時点で`memory_schema.md`の最小書式に従って作成する。
- current更新前に既存版をarchiveへ保存する。
- versionと更新日を更新する。
- 更新内容をMemoによりLogへ記録する。
- Memory以外の成果物、コード、設定を変更しない。

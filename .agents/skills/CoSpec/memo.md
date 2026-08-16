---
type: cospec_memo
bridge: discussion_to_log
---

# Memo

MemoはDiscussionからSkill直下の`memory/log/`への情報移送だけを担当する。Specificationとarchiveを変更しない。

Memoは`exe`ゲートの例外であり、明示的なMemoまたは保存対象が生じた応答のpost-actionとして単独実行できる。

## Save contract

- 通常の発見、アイデア、決定、却下、保留、blocker、次作業、Exe結果を保存対象にできる。
- 保存すべき新規内容がある応答につき最大1 checkpointへまとめる。
- 保存すべき新規内容がなければ書き込まない。
- decision、finding、idea等の固定分類を必須としない。
- 既存Memoryを参照して実質的な重複を判断する。必要ならMemory全体を参照してよい。
- secretsおよびsecrets混入が疑われる実値を保存しない。
- rolling、最新Log特定、書式は`memory_schema.md`に従う。
- Memoは明示的な`mem`・`memo`のほか、保存対象が生じた応答のpost-actionとして実行できる。

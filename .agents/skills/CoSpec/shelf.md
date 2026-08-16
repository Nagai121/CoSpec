---
type: cospec_shelf_action
space: discussion
action: shelf
---

# Shelf

Shelfは、人間がDiscussionでAIへ参照させるプロジェクト固有資料の領域であり、Skill直下の`shelf/`に置く。Skillの制御文書、Memory、Log、Specification、archiveはshelfに含めない。

## Invocation

- `shelf`が明示された場合だけ実行する。
- 通常のDiscussion、Check、Assumption、Summary、Search、Memo、Update、Exeでは自動実行しない。

## Behaviour

1. Skill直下の`shelf/index.md`を読む。
2. indexに列挙された利用可能な資料をDiscussionへ示す。
3. `shelf`だけでは資料本文を一括読込みしない。
4. ユーザーが資料を指定した場合、または後続の問いに必要な資料を特定できる場合だけ、indexに登録された関連資料を読む。
5. shelf全体の探索、indexにない資料の推測参照、毎回の存在確認を行わない。
6. shelf資料を人間の決定または確定仕様として扱わない。

## Path

Shelfの固定パスは、この文書を基準とする`shelf/index.md`である。参照できない場合はSkill配付物の欠損として報告し、AI判断で別の場所を探索、作成、修復しない。

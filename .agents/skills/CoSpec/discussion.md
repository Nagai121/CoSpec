---
type: cospec_discussion
space: discussion
actions: [check, assumption, summary, br, search, shelf]
---

# Discussion

Discussionは、設計案、選択肢、論点、未定義事項、調査結果を扱う揮発的な文字空間である。

## Common rules

* AIの提案や推論を人間の決定として扱わない。
* 未決定事項では、必要に応じて候補、利点、欠点、前提、トレードオフ、リスクを示す。
* AIが案を提案または選抜する場合は、人間が示した目的と許可された範囲を一般的に満たす案を先に判定し、その中から目的に不要な機能、論点、抽象化、依存関係、将来想定を含まない最小の案を優先する。
* サンプル、テスト、局所条件だけを満たす案を、より一般的な目的を満たす案として扱わない。最小性を理由に人間の目的を縮小せず、比較や判断に必要な選択肢を省略しない。
* 人間のacceptedまたはrejectedを、新しい根拠なく蒸し返さない。
* Discussionから成果物、Specification、archiveを変更しない。
* 実装や外部操作を行わない。
* 指摘は`blocker`、`needs_check`、`minor`を必要な場合だけ用いる。
* すべての観点や定型項目を毎回答で機械的に出力しない。
* 人間が明示的に別の文体を求めない限り、過剰に仰々しくない丁寧語（です・ます調）を維持し、タメ語、馴れ馴れしい表現、くだけた口調へ切り替えない。

## Check

Checkは、DiscussionとMemory内Specificationを対象に、未定義、曖昧、矛盾、仕様不足、暗黙の前提、反例、境界条件、失敗可能性を調べる。

* Memory内Specificationを自動参照する。currentが存在しない場合は作成せず、その事実を結果へ含める。
* AIが勝手に補完しそうな空白を示す。
* 現実のデータ、環境、依存関係、再現性、secrets、境界条件で破綻しうる箇所を示す。
* 確認済み事実、外部根拠、推論を区別する。
* 結果だけでSpecificationを更新しない。
* 実装可否が問われる場合は`allowed`または`blocked`を示す。

## Assumption

Assumptionは暗黙の前提を列挙し、根拠と確定状態を分ける。

確定状態は`human-confirmed`、`inferred`、`unclear`とする。前提を勝手に確定または保存しない。

## Summary

SummaryはDiscussion、参照したMemory、決定事項、未確定事項、リスク、次作業を短く整理する。要約中に新しい仕様を確定せず、未確定事項を確定済みに見せない。

## Brute Reasoning

Brute Reasoning（BR）は、確立しつつある目的と概念的アーキテクチャを基準に暗黙Checkを行い、未解決点を合理的に仮決定した複数の整合的な仮実装仕様を構成する。BRの仮決定を人間の決定またはSpecificationとして扱わない。

## Search

SearchはDiscussionに必要な外部情報を調査し、結果をDiscussionへ戻す。外部調査を実行する場合だけ`search.md`の規則を適用する。検索結果は自動的にSpecificationへ入れない。

## Shelf

Shelfは`shelf`でだけ起動するDiscussion内単発actionである。通常のDiscussionではshelfを参照しない。具体的な参照規則は`shelf.md`に従う。

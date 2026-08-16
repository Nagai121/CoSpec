---
type: cospec_memory_schema
space: memory
---

# Memory schema

Memoryはプロジェクト専用Skillの内部へ置く。この文書を基準とする`memory/`を固定ルートとし、別の場所へ初期化または複製しない。

## Layout

```text
memory/
├── log/
└── specification/
    ├── current.md
    └── archive/
```

- `memory/`、`log/`、`specification/`、`archive/`はSkill配付物に含める。
- `current.md`は初回Updateまでは存在しなくてよい。
- 必須ディレクトリが欠けている場合は配付物の欠損として報告し、通常処理の副作用として初期化しない。

## Log

Logはセッション再開に必要な議論状態をcheckpointとして保持する。1応答につき最大1 checkpointとする。

checkpointの最小内容:

```markdown
## checkpoint

timestamp:
summary:
items:
next:
```

- `items`は固定分類を要求しない。
- 同一内容を重複保存しない。
- Logは`memory/log/`に置き、新旧はcheckpoint内の`timestamp`だけで判定する。
- Logの基本ファイル名は`YYYY-MM-DD.md`とする。同日に複数のLogファイルを作成する場合、2件目以降は`YYYY-MM-DD-2.md`、`YYYY-MM-DD-3.md`のように作成順の連番を付ける。

## Specification

currentの最小内容:

```markdown
---
type: current_specification
spec_version:
last_updated:
---

# Specification

## Purpose
## Scope
## Confirmed decisions
## Unresolved items
## Execution boundary
## Blocking conditions
```

- currentにない事項は確定仕様として扱わない。
- 初回Updateより前にcurrentを自動作成しない。
- Update時は変更前currentをarchiveへ保存する。
- archiveは過去版であり、現行仕様として扱わない。

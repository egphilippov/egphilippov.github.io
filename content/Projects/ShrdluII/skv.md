# надо спец git/vcs репозиторий для таких знаний, видимо

Да. И я бы не пытался натянуть это на обычный Git. Нужен **knowledge VCS**, а Git можно оставить backend'ом для исходников.

Условно:

```text id="b4v8r2"
              SHRDLU-KVCS
                   │
        ┌──────────┼──────────┐
        ↓          ↓          ↓
      Facts      Models     Programs
        ↓          ↓          ↓
      Rules     Axioms     Procedures
        └──────────┼──────────┘
                   ↓
             Verification
                   ↓
              Commit K
                   │
        ┌──────────┴──────────┐
        ↓                     ↓
   knowledge DAG          provenance
        │
        ↓
   Git integration
```

### Главная разница с Git

Git мыслит:

```text
file → blob → tree → commit → branch
```

Knowledge VCS должен мыслить:

```text
claim
model
rule
axiom
procedure
proof
experiment
observation
source
```

и связывать их зависимостями:

```text
Claim C
 ├── supported-by → Proof P
 ├── derived-from → Rule R
 ├── depends-on → Fact F
 ├── tested-by → Experiment E
 └── introduced-by → Agent A
```

Тогда commit становится примерно:

```text id="7m1k2p"
KCommit 8f31...

parents:
  7aa2...

changes:
  + Fact(...)
  + Rule(...)
  + Procedure(...)

verification:
  tests: PASS
  proofs: PASS
  experiments: PASS

provenance:
  generated_by: llm-X
  reviewed_by: human-Y
```

### И нужен не только DAG, но и branching

Например:

```text
main
 │
 ├── hypothesis/new-physics-model
 │
 ├── hypothesis/parser-v2
 │
 └── hypothesis/alternative-world-model
```

Каждая ветка — **альтернативное состояние знания**.

А merge должен быть не просто:

> одинаковые строки → объединяем.

А:

> совместимы ли эти утверждения, зависимости, типы и доказательства?

То есть появляется **semantic merge**.

---

И я бы сделал очень маленький MVP.

Не database на 100 компонентов. Просто CLI:

```bash
skv init
skv add fact.json
skv add rule.json
skv prove claim-42
skv test
skv commit -m "Add collision rule"
skv branch hypothesis-x
skv diff
skv merge hypothesis-x
skv log
skv checkout <commit>
```

Хранилище первоначально можно вообще сделать поверх:

```text
.git/
knowledge/
  facts/
  rules/
  models/
  procedures/
  proofs/
  experiments/
manifest.json
```

То есть **не писать собственный storage engine до появления необходимости**.

Собственная ценность начинается выше Git: **семантические объекты + зависимости + verification + provenance + semantic diff/merge**.

И название я бы пока держал максимально утилитарным: **SHRDLU Knowledge VCS / SKVCS**.

Это уже отдельный потенциально интересный кусок технологии: если он получится достаточно универсальным, его можно будет использовать не только SHRDLU II, но и самим AI-programmer'ом как **version control для машинно производимого знания**.


# отставим пока версионирование в сторону

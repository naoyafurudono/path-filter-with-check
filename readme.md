# path-filter-with-check

以下を達成するためのGitHub Actionsの工夫をまとめたリポジトリです。

- モノレポでCIが通ったときにのみマージ可能にしたい
- ディレクトリごとにCIを用意するので、該当のCIだけの実行でOKとしたい
- ファイル変更の検査はCIの実行ごとに一回だけ行いたい
- ブランチプロテクションルールはシンプルにしたい。特に、ワークフローを追加したときに設定画面にアクセスしないで済ませたい

## ディレクトリ構造

```sh
🐧 tree -I .git -a
.
├── .github
│     └── workflows
│         ├── backend-ci.yaml
│         ├── ci.yaml
│         └── frontend-ci.yaml
├── backend
│     └── readme.md
├── frontend
│     └── readme.md
└── readme.md

5 directories, 6 files
```

## ブランチプロテクションルール

```sh
🐧 gh ruleset view
? Which ruleset would you like to view? 2848573: check CI for merge | active | contains 3 rules | configured in naoyafurudono/path-filter-with-check (repo)

check CI for merge
ID: 2848573
Source: naoyafurudono/path-filter-with-check (Repository)
Enforcement: Active
You can bypass: never

Bypass List
This ruleset cannot be bypassed

Conditions
- ref_name: [exclude: []] [include: [~DEFAULT_BRANCH]]

Rules
- deletion
- non_fast_forward
- required_status_checks: [do_not_enforce_on_create: false] [required_status_checks: [map[context:check-result integration_id:15368]]] [strict_required_status_checks_policy: false]
```
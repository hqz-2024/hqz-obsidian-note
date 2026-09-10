---
title: cli-anything-erpnext（ERPNext）
tags:
  - ai-agent
  - erp
  - cli
category: AI Agent / 开发工具
---

# cli-anything-erpnext（ERPNext）

ERPNext Docker 栈 + AI CLI，通过 Frappe API 控制 ERPNext（本地 localhost:8090）
## GitHub

[仓库链接](https://github.com/hqz-2024/cli-anything-erpnext)

## 相关

[[项目总览]]

## 项目 README

Chinese deployment guide: [README.zh-CN.md](README.zh-CN.md)

This repository contains a local ERPNext Docker stack plus the
`cli-anything-erpnext` Python CLI harness. The CLI lets AI agents control
ERPNext through real Frappe HTTP APIs with JSON output, dry-run support, generic
DocType operations, and role-scoped `flow` commands.

### Current Local Deployment

On this machine the CLI is already deployed as an editable Python package:

```powershell
python -m pip show cli-anything-erpnext
```

Expected key fields:

```text
Name: cli-anything-erpnext
Editable project location: C:\Users\bestarc\Desktop\erpnext-cli\erpnext\agent-harness
```

The local ERPNext site is expected at:

```text
URL: http://localhost:8090
User: Administrator
Password: admin
```

The global agent skill has also been synced to:

```text
C:\Users\bestarc\.agents\skills\cli-anything-erpnext\SKILL.md
```

### Prerequisites

Install these before a fresh deployment:

- Docker Desktop with Compose support
- Python 3.10 or newer
- Git
- PowerShell on Windows

Confirm the tools:

```powershell
docker compose version
python --version
git --version
```

### Deploy ERPNext Locally

From the repository root:

```powershell
cd C:\Users\bestarc\Desktop\erpnext-cli
docker compose up -d
```

The compose file starts ERPNext v16, MariaDB, Redis, workers, scheduler,
websocket, and the frontend exposed on host port `8090`.

Check containers:

```powershell
docker compose ps
```

Wait until `frontend` is reachable, then open:

```text
http://localhost:8090
```

If this is a first-time boot, the `create-site` service creates the `frontend`
site with Administrator password `admin`.

### Install The CLI

Install the CLI from the local harness in editable mode:

```powershell
cd C:\Users\bestarc\Desktop\erpnext-cli\erpnext\agent-harness
python -m pip install -e .
```

Verify the console entry:

```powershell
cli-anything-erpnext --help
cli-anything-erpnext --help | Select-String -Pattern "flow"
```

The output should include:

```text
flow      Run role-scoped ERPNext workflow helpers.
```

Editable mode is recommended for local development because the installed
`cli-anything-erpnext` command points directly at this checkout. After `git pull`
or local edits, reinstalling is usually not needed unless dependencies or entry
points in `setup.py` change.

### Configure CLI Authentication

Create or refresh the local profile:

```powershell
cli-anything-erpnext --json auth login --url http://localhost:8090 --user Administrator --password admin --profile local
```

Verify the profile and site:

```powershell
cli-anything-erpnext --json auth whoami
cli-anything-erpnext --json site status
```

Profile data is stored under:

```text
%USERPROFILE%\.erpnext-cli\config.json
```

For isolated testing, set `ERPNEXT_CLI_HOME`:

```powershell
$env:ERPNEXT_CLI_HOME = "C:\Temp\erpnext-cli-home"
cli-anything-erpnext --json auth login --url http://localhost:8090 --user Administrator --password admin --profile local
Remove-Item Env:\ERPNEXT_CLI_HOME
```

### Deploy The Agent Skill

Agents discover ERPNext CLI usage from `SKILL.md`. Keep these copies identical:

```text
skills\cli-anything-erpnext\SKILL.md
erpnext\agent-harness\cli_anything\erpnext\skills\SKILL.md
C:\Users\bestarc\.agents\skills\cli-anything-erpnext\SKILL.md
```

To sync the global skill after editing the repository copy:

```powershell
$src = "C:\Users\bestarc\Desktop\erpnext-cli\skills\cli-anything-erpnext\SKILL.md"
$dstDir = "C:\Users\bestarc\.agents\skills\cli-anything-erpnext"
New-Item -ItemType Directory -Force -Path $dstDir | Out-Null
Copy-Item -LiteralPath $src -Destination (Join-Path $dstDir "SKILL.md") -Force
```

Verify all three hashes match:

```powershell
Get-FileHash C:\Users\bestarc\Desktop\erpnext-cli\skills\cli-anything-erpnext\SKILL.md
Get-FileHash C:\Users\bestarc\Desktop\erpnext-cli\erpnext\agent-harness\cli_anything\erpnext\skills\SKILL.md
Get-FileHash C:\Users\bestarc\.agents\skills\cli-anything-erpnext\SKILL.md
```

### Smoke Test The Deployment

Run these after deployment:

```powershell
cli-anything-erpnext --json site status
cli-anything-erpnext --json doctype schema Customer
cli-anything-erpnext --json doc list Customer --fields "[`"name`",`"customer_name`"]" --limit 5
```

For `flow` commands, prefer JSON files in PowerShell:

```powershell
$rates = "C:\Temp\rates.json"
[System.IO.File]::WriteAllText($rates, '{"CELL-001":12.5}', [System.Text.UTF8Encoding]::new($false))

cli-anything-erpnext --json --dry-run flow purchase-order create-from-bom BOM-SMOKE `
  --supplier "Battery Supplier" `
  --company "Demo Company" `
  --schedule-date 2026-06-30 `
  --qty 1 `
  --warehouse "Stores - DC" `
  --item-rates $rates
```

Expected result:

```text
ok: true
command: flow.purchase_order.create_from_bom
data.dry_run: true
data.next.command_template: cli-anything-erpnext --json flow purchase-receipt receive-from-po ...
```

### Run Verification Tests

From the harness directory:

```powershell
cd C:\Users\bestarc\Desktop\erpnext-cli\erpnext\agent-harness
python -B -m pytest cli_anything\erpnext\tests -v
```

Verify the installed console command path:

```powershell
$env:CLI_ANYTHING_FORCE_INSTALLED = "1"
python -B -m pytest cli_anything\erpnext\tests\test_full_e2e.py::TestCLISubprocess::test_help_displays_command_groups -v -s
Remove-Item Env:\CLI_ANYTHING_FORCE_INSTALLED
```

The output should show:

```text
[_resolve_cli] Using installed command: ...\cli-anything-erpnext.EXE
```

### Remote ERPNext Deployment

For a remote ERPNext site, the CLI deployment is the same. Only the profile
changes.

Use API key/secret authentication:

```powershell
cli-anything-erpnext --json auth token `
  --url https://erp.example.com `
  --api-key KEY `
  --api-secret SECRET `
  --profile remote

cli-anything-erpnext --json auth use remote
cli-anything-erpnext --json site status
```

Or pass a URL override per command:

```powershell
cli-anything-erpnext --json --url https://erp.example.com site status
```

### Upgrade

For local code updates:

```powershell
cd C:\Users\bestarc\Desktop\erpnext-cli
git pull
cd .\erpnext\agent-harness
python -m pip install -e .
python -B -m pytest cli_anything\erpnext\tests -v
```

If `SKILL.md` changed, sync the global skill again using the command in
`Deploy The Agent Skill`.

### Rollback

Find the commit history:

```powershell
git log --oneline -5
```

Preferred rollback is a normal revert commit:

```powershell
git revert <commit-to-undo>
cd .\erpnext\agent-harness
python -m pip install -e .
```

To test an older version without changing the branch, use a temporary detached
checkout:

```powershell
git switch --detach <commit>
cd .\erpnext\agent-harness
python -m pip install -e .
```

Return to the normal branch after testing:

```powershell
git switch master
cd .\erpnext\agent-harness
python -m pip install -e .
```

Then rerun the smoke tests and skill hash checks.

### Operational Notes For Agents

- Put root options before command groups: `cli-anything-erpnext --json flow ...`
- Use `--dry-run` before mutations when shaping data.
- Use `--submit` only when the user explicitly asks to submit.
- Do not invent business data. Prices, quantities, warehouses, customers,
  suppliers, and dates must come from the user or an upstream system.
- Treat successful non-dry-run ERPNext mutations as committed server-side
  operations.
- Use the `flow` group for one role-scoped business step, not a full automatic
  production-to-sales chain.

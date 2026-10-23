# Install slop-audit with an AI agent

Paste this file into an agent that can run shell commands and edit files. It installs the skill into the directory the agent reads, verifies it, and reports how to run a first scan.

## Step 0 - Detect the OS

- Windows: `$PSVersionTable.PSVersion`, `$env:OS`
- macOS or Linux: `uname -s`

Use the matching path and command style for the rest of this file.

## Step 1 - Choose the target

Default target, where OpenCode discovers skills:

- Windows: `%USERPROFILE%\.agents\skills\slop-audit`
- macOS / Linux: `~/.agents/skills/slop-audit`

If the user's agent reads skills from a different directory, ask and use that path instead. Do not install into a project directory unless the user asks for a project-scoped skill.

If the target already exists, stop and ask whether to update it in place. Do not delete an existing skill.

## Step 2 - Fetch

Clone to a temporary directory, not into the target:

```bash
git clone https://github.com/Aniketsomkuwar/slop-audit.git /tmp/slop-audit
```

If git is unavailable, download the repository zip and extract it.

## Step 3 - Copy

```bash
mkdir -p ~/.agents/skills/slop-audit
cp -R /tmp/slop-audit/SKILL.md /tmp/slop-audit/references /tmp/slop-audit/README.md ~/.agents/skills/slop-audit/
```

Windows PowerShell:

```powershell
$target = Join-Path $env:USERPROFILE '.agents\skills\slop-audit'
New-Item -ItemType Directory -Force -Path $target | Out-Null
Copy-Item "$env:TEMP\slop-audit\SKILL.md" $target -Force
Copy-Item "$env:TEMP\slop-audit\references" $target -Recurse -Force
Copy-Item "$env:TEMP\slop-audit\README.md" $target -Force
```

Do not copy `.git`, `assets/` or `prompt.md` into the skill directory.

## Step 4 - Verify

- `SKILL.md` exists and starts with a `---` frontmatter block.
- The frontmatter has `name: slop-audit` and a non-empty `description`.
- `references/` contains ten Markdown files.
- No file was truncated: `SKILL.md` is about 46 KB.

If a check fails, report it and stop. Do not patch the skill to make it pass.

## Step 5 - Optional pair

`slop-audit` detects slop; `slop-design` prevents it. If the user wants both:

```bash
git clone https://github.com/Aniketsomkuwar/slop-design.git
```

Target: `~/.agents/skills/slop-design`. `slop-design` has eight reference files.

## Step 6 - First scan

Do not create `.slop-tracker` inside the skill directory. Ask for the project to audit, then run:

```text
/skill slop-audit --scan <project path>
```

The skill creates `.slop-tracker/` inside that project and writes `slop-log.json` there.

## Step 7 - Report

Tell the user:

- The target directory and the files installed.
- That a session restart is needed for the skill to appear.
- The scan command and which project it will write to.
- That `references/report-template.md` and `references/fix-order.md` should be read before presenting or applying findings.

## Hard rules

- Never modify `SKILL.md` or the references. They are the product.
- Never install into a project repository unless the user asks.
- Never write `.slop-tracker/` outside the project being audited.
- Do not claim success before Step 4 passes.

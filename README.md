# Engineering Interview Depth Coach

Engineering Interview Depth Coach is an adaptive Codex skill for software-engineering interview preparation. It assesses demonstrated technical depth, identifies gaps, teaches production concepts, and creates focused practice plans. It supports system design, HLD, LLD, backend engineering, distributed systems, databases, APIs, networking, caching, messaging, concurrency, reliability, storage, observability, infrastructure, capacity planning, and Java runtime behavior. Coding and behavioral preparation are available when explicitly requested.

The skill teaches through mechanisms, examples, failure modes, scale calculations, and trade-offs. It can run diagnostics, targeted drills, realistic mock interviews, incident simulations, system-design exercises, and revision sessions. It tracks expected, current, target, and stretch depth for each domain. It favors simple designs first and introduces additional infrastructure only when requirements justify it.

## Repository contents

- `SKILL.md` contains the main behavior contract.
- `AGENTS.md` provides the Codex and ChatGPT adapter instructions.
- `CLAUDE.md` provides the Claude adapter instructions.
- `agents/` contains the Interview Director and specialist guides.
- `knowledge/` contains the depth model, foundations, patterns, and practice catalog.
- `workflows/` contains diagnostic, learning, interview, incident, and design workflows.
- `evidence/` contains the optional interview-evidence schema.
- `examples/` contains usage examples.

## Installation for Codex

The repository root is the skill folder. You can install it by cloning the repository and copying its contents into the Codex skills directory.

On Windows, run these commands in PowerShell:

```powershell
$clonePath = "$env:TEMP\engineering-interview-depth-coach"
git clone https://github.com/shahnawazahmadkk/interview-depth-guide.git $clonePath
$destination = "$env:USERPROFILE\.codex\skills\engineering-interview-depth-coach"
New-Item -ItemType Directory -Path $destination -Force | Out-Null
Copy-Item -Path "$clonePath\*" -Destination $destination -Recurse -Force
```

On macOS or Linux, run:

```bash
clone_path="${TMPDIR:-/tmp}/engineering-interview-depth-coach"
git clone https://github.com/shahnawazahmadkk/interview-depth-guide.git "$clone_path"
mkdir -p "$HOME/.codex/skills/engineering-interview-depth-coach"
cp -R "$clone_path"/* "$HOME/.codex/skills/engineering-interview-depth-coach/"
```

If you already downloaded the repository, replace the clone path with the extracted folder path. If you are inside a local checkout, copy the repository contents with `Copy-Item -Path .\* -Destination $destination -Recurse -Force` on Windows or `cp -R ./* "$HOME/.codex/skills/engineering-interview-depth-coach/"` on macOS or Linux.

Start a new Codex turn after installation. The skill should then appear as `engineering-interview-depth-coach`.

## Installation for Claude

Clone the repository first:

```bash
git clone https://github.com/shahnawazahmadkk/interview-depth-guide.git
cd interview-depth-guide
```

For Claude Code, keep the cloned folder available in the workspace and ask Claude to read `CLAUDE.md` and `SKILL.md`. If your Claude setup supports a skills directory, copy the repository contents there and start a new session. The exact skills-directory path depends on the Claude product and operating system.

You can ask Claude:

```text
Read CLAUDE.md and SKILL.md from this repository. Use Engineering Interview Depth Coach for this session. Assess my current depth before teaching, then create the timeline and begin.
```

## Installation for GitHub Copilot

Copilot does not provide one universal native skill-directory format across all editors. The most portable approach is to use the repository as project instructions.

Clone the repository and copy the main contract into the project where Copilot will work:

```bash
git clone https://github.com/shahnawazahmadkk/interview-depth-guide.git
mkdir -p .github
cp interview-depth-guide/SKILL.md .github/copilot-instructions.md
cp -R interview-depth-guide/agents interview-depth-guide/knowledge interview-depth-guide/workflows .github/
```

On Windows PowerShell, use:

```powershell
git clone https://github.com/shahnawazahmadkk/interview-depth-guide.git
New-Item -ItemType Directory -Path .github -Force | Out-Null
Copy-Item -LiteralPath .\interview-depth-guide\SKILL.md -Destination .\.github\copilot-instructions.md -Force
Copy-Item -Path .\interview-depth-guide\agents, .\interview-depth-guide\knowledge, .\interview-depth-guide\workflows -Destination .\.github -Recurse -Force
```

Then ask Copilot:

```text
Read .github/copilot-instructions.md and the related files under .github. Use this interview coach workflow. Complete the depth assessment before teaching and provide the timeline before beginning.
```

## Fallback installation

If a tool does not support native skills or repository instructions, clone or download this repository, open `SKILL.md`, and provide it to the tool as project context. Keep the `agents/`, `knowledge/`, and `workflows/` folders beside it so the tool can load the relevant references. You can also paste the README usage prompt into a new session and explicitly ask the tool to read `SKILL.md` first.

## Usage

Ask Codex to read the installed skill and state your goal. For example:

```text
Use engineering-interview-depth-coach. I have a senior backend interview in six weeks. Assess my current depth, then create a focused preparation plan.
```

You can also request a one-question drill, a system-design mock interview, a production incident simulation, or a focused lesson on any supported topic.

## Validation

Validate the skill after changes with:

```bash
python C:/Users/DELL/.codex/skills/.system/skill-creator/scripts/quick_validate.py .
```

The learning references are original and generic. They exclude personal identities, account details, testimonials, and external source branding. Technology names remain where required for accurate instruction.

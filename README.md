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

Copy the same folder into the skills directory supported by your Claude setup, then load `CLAUDE.md` and `SKILL.md` when invoking the skill.

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

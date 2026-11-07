# Setup Version Policy

[![Codex skill](https://img.shields.io/badge/Codex-skill-blue?style=flat)](SKILL.md)
[![Versioning: SemVer-style](https://img.shields.io/badge/versioning-SemVer--style-blue?style=flat)](#default-increment-policy)
[![Instructions only](https://img.shields.io/badge/instructions-only-blue?style=flat)](SKILL.md)
[![Installation guide](https://img.shields.io/badge/install-guide-blue?style=flat)](#installation)

An agent skill for establishing a project's canonical version source and deciding when completed changes require a version increment.

The skill guides an agent through reviewing existing version information, confirming a starting version with the user, and documenting a policy suited to the project's release stage.

## Installation

Use a Codex client that supports local skills. This skill contains instructions and metadata, so it has no build step or package dependencies. The manual clone commands below require Git.

### Install with Codex

Send this prompt to Codex, rather than running it in a terminal:

```text
Use $skill-installer to install the skill from
https://github.com/phoem/setup-version-policy on branch main.
The skill is at the repository root (path ".").
Install it with the name setup-version-policy.
```

The explicit root path matters because `SKILL.md` is at the top level of this repository.

### Install manually for all your projects

Clone this repository into your personal skills directory using the command for your shell. Choose this method or the installer above; one installation is enough.

**macOS or Linux (Bash/Zsh):**

```sh
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/phoem/setup-version-policy.git "$HOME/.agents/skills/setup-version-policy"
```

**Windows (PowerShell):**

```powershell
$skillDirectory = Join-Path $HOME '.agents/skills'
New-Item -ItemType Directory -Force -Path $skillDirectory | Out-Null
git clone https://github.com/phoem/setup-version-policy.git (Join-Path $skillDirectory 'setup-version-policy')
```

The installed directory should contain `setup-version-policy/SKILL.md` and `setup-version-policy/agents/openai.yaml` directly, without an extra nested repository folder.

### Install for one project

To share the skill with a project's contributors, copy `SKILL.md` and the `agents` directory from this repository into `<project-root>/.agents/skills/setup-version-policy/`:

```text
<project-root>/
└── .agents/
    └── skills/
        └── setup-version-policy/
            ├── SKILL.md
            └── agents/
                └── openai.yaml
```

Include those files in the target project's version control if the team should receive the skill with the repository.

### Verify the installation

Open the project you want to configure in Codex. In the CLI or IDE extension, use `/skills` or type `$` and select `setup-version-policy`. Codex detects installed skills automatically; restart it if the skill does not appear. If it is still missing, check the directory layout above and whether the skill is disabled in your Codex configuration.

See the [official OpenAI skills documentation](https://learn.chatgpt.com/docs/build-skills) for supported locations, discovery, and configuration.

## Usage

After installation, open a chat in the project whose version policy you want to establish or review and request the skill by name:

```text
Use $setup-version-policy to establish this project's canonical VERSION
source and document when completed changes require a version increment.
```

For an existing policy:

```text
Use $setup-version-policy to review this project's version sources and
bump rules. Identify conflicting version authorities and unclear rules.
```

The skill will inspect that project and ask you to choose a starting version before creating or changing its canonical `VERSION`. For another agent that accepts skill instructions, provide [SKILL.md](SKILL.md) and follow that agent's installation conventions.

### Follow up: synchronize package and manifest versions

After the skill establishes `VERSION` and the version policy, ask your agent to update package and manifest fields that represent the same product version to match it. Keep `VERSION` as the authority and have those fields follow its value whenever it changes. Dependency versions and independently versioned components should retain their own values.

This is a recommended follow-up task; synchronization is outside the skill's default scope. For example:

```text
Now that VERSION and the version policy are established, update all package
and manifest fields that mirror this project's version to match VERSION.
Preserve dependency versions and independently versioned component versions.
Update any corresponding lockfile metadata using the project's normal tools.
Set up a synchronization step and a validation check suited to this project
so those fields stay aligned whenever VERSION changes. Document the process
in the contributor or agent guidance, keeping VERSION as the single source
of truth.
```

## Workflow

1. **Inspect the project.** Read applicable project guidance and look for version candidates in the root `VERSION`, package manifests, runtime constants, release notes, and Git tags. Distinguish the current product version from dependency and component versions.
2. **Confirm the starting version.** Ask the user to adopt a discovered candidate, request a proposal based on shipped milestones in commit history, or supply a version. Confirmation is required even when only one plausible candidate exists.
3. **Establish the canonical source.** Once the user chooses, use the repository-root `VERSION` as the single source of truth. If an existing policy names another authority, resolve that conflict with the user before changing it.
4. **Document increment rules.** Follow a coherent existing policy or propose a SemVer-style policy appropriate to the project's release stage. Record the canonical source and agreed rules in existing contributor or agent guidance.
5. **Summarize the result.** Report the chosen version and its evidence, the increment policy, documentation changes, and unresolved compatibility questions.

The agent must wait for the user's choice before creating or changing the canonical `VERSION`. A history-based proposal must explain shipped milestones; commit count alone is not sufficient evidence.

## Default increment policy

These defaults apply when the target project does not already have a coherent documented policy.

| Change | Increment |
| --- | --- |
| Compatible bug fixes, security fixes, or corrections to supported user documentation | Patch |
| New user-visible features, compatible API or configuration additions, or newly supported integrations | Minor |
| Breaking changes at version 1.0 or later | Major |
| Breaking changes before version 1.0 | Define an explicit project rule |
| Unfinished work, tests alone, planning, internal refactors, or changes without a user-visible product or supported-documentation effect | None |

For changes spanning several categories, apply only the largest required increment. Increment when the change is complete and ready for handoff. Do not bump the same change twice or reuse a version for distinct release-worthy product states. Follow the project's existing changelog convention when recording user-visible changes.

## Scope

The skill covers starting-version selection, canonical version authority, and increment policy. It preserves unrelated project instructions and existing version conventions.

Manifest synchronization, lockfile updates, packaging, and release automation require a separate request. Creating tags, commits, packages, publishing artifacts, or releasing the project also requires separate authorization.

## Repository contents

| File | Purpose |
| --- | --- |
| [SKILL.md](SKILL.md) | Skill instructions and workflow |
| [agents/openai.yaml](agents/openai.yaml) | Agent interface metadata, including the display name and short description |
| [README.md](README.md) | Overview, usage examples, and policy summary |

## Maintaining the skill

Keep this README aligned with `SKILL.md`, which defines the skill's behavior. When changing the workflow, preserve the starting-version confirmation step, the distinction between product and dependency versions, and explicit handling of breaking changes before version 1.0. Update the agent metadata if the skill's name or purpose changes.

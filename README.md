# Oracle AI Agent Studio CLI Setup Helper for Google Antigravity

Prepare a local Oracle AI Agent Studio development environment from Google Antigravity Desktop, then work in Desktop or Visual Studio Code with Antigravity.

This repository contains exactly two files: this README and the self-contained [SKILL.md](SKILL.md). The consuming agent executes the applicable Windows, macOS, or Linux procedure. The layout helper is embedded in SKILL.md and materialized during setup; no separately distributed scripts, Codex installation, or custom plugin package are required.

## What it prepares

- Node.js and npm, preserving compatible existing developer installations.
- On Linux, `secret-tool` and a usable user-session Secret Service for the CLI's encrypted credential storage, checked with a temporary non-sensitive item.
- Visual Studio Code and the official `Google.google-antigravity` extension.
- A verified Antigravity CLI (`agy`) installation and terminal command.
- A recorded snapshot of Oracle's `fusion-ai-studio` repository.
- Oracle's AI Studio skills, referenced resources, and sample apps in a local workspace.
- The Oracle Fusion AI Studio VS Code extension.
- A persistent user-level `aistudio` launcher that works outside the workspace: an executable on macOS/Linux and a `.cmd` launcher for Windows PowerShell and CMD.
- A blank AI Studio scaffold inside `fusion-ai-workspace`, with complete skills and samples.
- An executable layout check that blocks a success report if paths or copies do not match the required structure.

The skill detects the native execution OS and architecture, checks what is already installed, installs only missing or incompatible components, and verifies the result. Existing files and command-name conflicts are inspected before changes. Downloads use official sources through the terminal.

## Step-by-step execution

Setup runs in ten sequential phases: discovery and paths, Node/npm, Linux credential storage, VS Code and the Google extension, Antigravity CLI, Oracle source, workspace creation and copies, Oracle extension, global launcher and project opening, and final checks. The agent announces each phase, reports its result, and continues within the authorization already given. It waits for each installation to finish and does not run parallel installers or repeated background copies.

The agent saves progress in `oracle-ai-agent-studio/downloads/setup-progress.json` before changes and after verification. Compact results, reused evidence, and bounded retries reduce unnecessary model/tool requests. Progress messages provide visibility; they do not remove model rate or token limits. If a quota or repeated rate limit interrupts setup, resume from the checkpoint rather than starting over:

> Continue using SKILL.md and the existing downloads/setup-progress.json under my oracle-ai-agent-studio setup root. Confirm the recorded state and resume the first unfinished phase. Preserve completed work and do not reinitialize the project.

Both VS Code extensions are required. The agent installs and verifies `Google.google-antigravity` and Oracle's VSIX (currently `oracle.fusion-aistudio-vscode`) for the actual user, profile, and extension host. A downloaded VSIX, working CLI, or installed editor alone does not satisfy this requirement. Installation and UI activation are reported separately, and a missing extension prevents a complete-success report.

## Before you start

You need Google Antigravity Desktop already installed, a local writable project folder, internet access to the official software sources, and permission to install missing software. Some Node.js installers require administrator authorization. Managed-device restrictions may require help from your IT administrator.

Supported execution targets are Windows, macOS, and Linux. Windows PowerShell 5.1 and 7 are supported. On Linux, the skill identifies the distribution and its package manager (APT, DNF/YUM, Zypper, pacman, APK, or a documented alternative), checks architecture/libc compatibility, and configures the actual user shell. WSL is treated as a Linux target; it does not configure the Windows host. Headless or remote Linux can prepare CLI and project files, while Desktop and GUI integration remain pending until verified on the relevant host. Vendor support for each component still applies; detecting a distribution does not guarantee every desktop component supports it.

The agent verifies current product and architecture requirements. An existing Desktop installation does not guarantee that the standalone CLI or VS Code extension is installed.

## Use from Google Antigravity Desktop

1. Open or create a local project with a writable folder for the Oracle setup.
2. Make this repository's `SKILL.md` available to the agent using the installed Desktop version's file attachment or file reference feature.
3. Start a chat and use this prompt:

   > Use the attached SKILL.md to set up Oracle AI Agent Studio CLI for Google Antigravity. Work through its ten phases sequentially, show brief progress, and save a checkpoint after each phase. Preserve existing files, create the exact required folder structure, install and verify both the Google Antigravity and Oracle AI Studio extensions in my VS Code profile, and register aistudio on PATH. Verify the result before reporting success; if interrupted, resume from the checkpoint.

4. Review the initial findings and approve any required installation or persistent configuration changes when the host asks.
5. Follow any instructions to reopen terminals or restart applications. Complete Google sign-in yourself if requested by its official interface.

Attaching or referencing the file is enough for a one-time setup. To install it as a reusable project skill, create `.agents/skills/setup-oracle-ai-agent-studio-antigravity/` in the project and place this repository's `SKILL.md` inside that folder. Verify it appears in the agent's skill interface before relying on automatic discovery.

According to [Google's skill documentation](https://www.antigravity.google/docs/skills), project skills use `.agents/skills/`. Desktop's documented global location is `~/.gemini/config/skills/`; the standalone CLI has its own global location, `~/.gemini/antigravity-cli/skills/`. On Windows, `~` means the current user's home directory. Recheck those locations for your installed version. A global helper installation is optional; it is not required to run setup.

## Use from VS Code or Antigravity CLI

If VS Code with the Google Antigravity extension is already available, open the local setup folder, reference this `SKILL.md` in the Antigravity panel, and use the same prompt. If these components are missing, start from Desktop so the skill can install them.

If the standalone CLI is already available, start `agy` in the local setup folder and make the skill available as a file reference or project skill. Use `/skills` to confirm discovery when supported by your installed version. Do not use an editor-opening `agy` launcher as evidence that the standalone agent CLI is installed.

## Resulting workspace

The required layout is:

```text
oracle-ai-agent-studio/
├── downloads/
├── fusion-ai-repo/
├── extension-staging/
└── fusion-ai-workspace/
    ├── .agents/skills/
    ├── aiapps/
    └── ...generated project files
```

The four directory names are fixed. The setup root is never the development project. The agent reuses an existing matching root, or creates `oracle-ai-agent-studio` under the selected parent. Conflicts require a migration decision or a different parent; they do not justify suffixes, nested workspaces, or silently changing the layout. The workflow initializes the empty project before copying skills and samples. It records the Oracle branch/ref and commit in `downloads/oracle-source.json`. When your Fusion release is known, the selected branch should match it; otherwise the repository default release branch is provisional, and environment compatibility remains unverified. [Oracle's release guidance](https://github.com/oracle/fusion-ai-studio).

The embedded Node helper checks the actual directories, source-to-destination file hashes, required scaffold, source record, and extracted VSIX. A successful check writes `downloads/layout-verification.json`. An incomplete or conflicting layout remains blocked, and existing user content is preserved. This check does not replace verification of the installed tools or PATH.

Open `fusion-ai-workspace` in Desktop or VS Code for development. The Oracle VSIX is installed in VS Code; standalone Desktop uses the Oracle CLI and skills.

The `aistudio` launcher refers to a stable, absolute CLI path. Keep that directory in place. If you move it or remove a Node version referenced by the launcher, rerun the helper to repair registration.

From a fresh terminal outside the workspace, these should succeed:

```text
aistudio version
aistudio --help
aistudio init --help
```

For another new project, use `aistudio init --dir <new-project-folder>` after checking the installed CLI's help, copy the complete matching Oracle `.agents/skills` tree into that project, and open it in the desired surface. Samples are optional for later projects. A globally accessible command does not automatically install skills in every project.

## Authentication and readiness

The final report separates local tooling, agent integration, credential-storage readiness, and authentication. It lists passed checks, pending UI verification, declined changes, and blockers rather than treating partial installation as complete.

On Linux, `Failed to store encryption passphrase in Secret Service: spawn secret-tool ENOENT` means the CLI cannot launch `secret-tool`. On Debian/Ubuntu, install `libsecret-tools` in the environment where Node runs. Authentication also needs a Secret Service provider, such as GNOME Keyring, accessible and unlocked in the same user session. Remote desktops and containers may need session configuration even after package installation. The skill checks the executable and a temporary store/read/delete cycle; a successful `aistudio --help` alone does not establish authentication readiness. Repairing this prerequisite does not require reinitializing the project.

If the session bus works but reports `The name org.freedesktop.secrets was not provided by any .service files`, the skill checks provider installation and activation. When no provider is installed, it installs GNOME Keyring (`sudo apt-get install -y gnome-keyring` on Debian/Ubuntu, or a verified distribution equivalent). An installed provider with broken activation is diagnosed separately. The temporary check uses a unique attribute and deletes only its own test item.

For a headless/container host without a usable user bus, the skill provides a persistent `dbus-run-session -- bash` shell and starts GNOME Keyring inside it when activation is unavailable and no competing provider exists. The keyring must also be initialized/unlocked, and the CLI must run in that same session. The bus ends when that shell exits; an editor already running outside it needs its own verified session. Credential storage remains pending until the temporary store/read/delete cycle succeeds in the intended execution environment.

If the next error is `createDLGroup failed: maybe libqca-ossl is missing`, inspect the active KWallet provider and its QCA OpenSSL plugin. Debian/Ubuntu use `libqca-qt5-2-plugins` for Qt 5 and `libqca-qt6-plugins` for Qt 6. The skill selects the matching package, then requires a fresh wallet session and a successful storage check. It preserves the existing wallet and does not install a competing provider to hide the failure.

Google sign-in is user-controlled. Fusion authentication is deferred: later run **Fusion AI Studio: Configure Authentication** in VS Code using details supplied by your administrator. Do not share credentials or `env.properties` in the agent chat. Setup does not fetch, save, or publish remote Fusion artifacts.


## Official sources

- [Google Antigravity getting started](https://www.antigravity.google/docs/getting-started/)
- [Antigravity CLI installation](https://www.antigravity.google/docs/cli/install/)
- [Antigravity agent skills](https://www.antigravity.google/docs/skills)
- [Antigravity for VS Code](https://www.antigravity.google/docs/ide/extensions/vscode/) and [official extension](https://marketplace.visualstudio.com/items?itemName=Google.google-antigravity)
- [Oracle Fusion AI Studio repository](https://github.com/oracle/fusion-ai-studio)
- [Oracle Fusion AI Studio CLI overview](https://blogs.oracle.com/fusioncoe/fusion-aistudio-cli)
- [Node.js downloads](https://nodejs.org/en/download)
- [VS Code documentation](https://code.visualstudio.com/docs) and [Linux installation](https://code.visualstudio.com/docs/setup/linux)

This community setup helper is not an official Google or Oracle installer. The software and Oracle repository retain their respective licenses.

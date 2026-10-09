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

## Before you start

You need Google Antigravity Desktop already installed, a local writable project folder, internet access to the official software sources, and permission to install missing software. Some Node.js installers require administrator authorization. Managed-device restrictions may require help from your IT administrator.

Supported execution targets are Windows, macOS, and Linux. Windows PowerShell 5.1 and 7 are supported. On Linux, the skill identifies the distribution and its package manager (APT, DNF/YUM, Zypper, pacman, APK, or a documented alternative), checks architecture/libc compatibility, and configures the actual user shell. WSL is treated as a Linux target; it does not configure the Windows host. Headless or remote Linux can prepare CLI and project files, while Desktop and GUI integration remain pending until verified on the relevant host. Vendor support for each component still applies; detecting a distribution does not guarantee every desktop component supports it.

The agent verifies current product and architecture requirements. An existing Desktop installation does not guarantee that the standalone CLI or VS Code extension is installed.

## Use from Google Antigravity Desktop

1. Open or create a local project with a writable folder for the Oracle setup.
2. Make this repository's `SKILL.md` available to the agent using the installed Desktop version's file attachment or file reference feature.
3. Start a chat and use this prompt:

   > Use the attached SKILL.md to prepare my local Oracle AI Agent Studio CLI environment for Google Antigravity Desktop and Visual Studio Code. Check existing tools, install missing prerequisites, register aistudio so it works from any directory, and create the required oracle-ai-agent-studio folder with downloads, fusion-ai-repo, extension-staging, and fusion-ai-workspace. Initialize the project only inside fusion-ai-workspace, copy the complete Oracle skills and aiapps there, and run the embedded layout verification before reporting success. Keep existing files and credentials safe.

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

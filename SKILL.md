---
name: setup-oracle-ai-agent-studio-antigravity
description: Prepare or repair a local Windows or macOS Oracle AI Agent Studio CLI environment for users with Google Antigravity Desktop installed, including Node.js, VS Code, Google's Antigravity extension, Antigravity CLI, Oracle skills and samples, and a persistent aistudio command.
---

# Oracle AI Agent Studio CLI Setup for Google Antigravity

Prepare a local development environment usable from Google Antigravity Desktop and from Visual Studio Code with Antigravity. Complete the applicable operating-system branch and verify observable results. This skill contains the full setup procedure; no companion scripts are required.

## Scope and operating rules

- The user already has Google Antigravity Desktop. Verify its presence; do not replace it with Antigravity IDE, Gemini CLI, or Codex. No Codex installation or Codex project is required.
- Use the user's local account and local workspace. Detect the OS where commands actually execute, not the OS inferred from a screenshot. Support native Windows and macOS. If the agent runs in WSL, a container, SSH, or a remote host, explain the mismatch and move setup to a local native session before changing the desktop environment. Linux setup is outside scope.
- Follow the user's language in conversation. This skill, generated launchers, and documentation use English.
- Begin with read-only discovery. Summarize the missing components and exact proposed installation paths. Proceed within the user's existing authorization; do not repeatedly request permission for the same approved action. Obtain any required host approval for installers, privileges, persistent PATH/profile changes, or writes outside the workspace. Explain the concrete change before a new approval. Do not weaken enterprise policies or disable certificate checks.
- Download through the terminal from official sources. Download installer scripts to files and inspect them before execution rather than piping unseen network content into a shell. A native installer UI is acceptable. Browser use is appropriate for user-controlled sign-in, not required for downloading software.
- Preserve existing compatible tools and files. An existing Node installation is not automatically incompatible. Inspect documented runtime requirements and actually run the Oracle help command before recommending changes. A runtime without usable npm is incomplete for this developer setup; first look for `npm.cmd` on Windows and existing developer installations before proposing another Node installation.
- Inspect existing destinations and command names. Reuse matching files; never overwrite conflicting user content or blindly merge directories. Offer a new destination or obtain a specific replacement decision. Do not delete failed downloads or user files as automatic cleanup.
- Never request, display, log, commit, or inspect credentials or `env.properties` contents. Do not run authentication, server fetch, save, publish, or login commands during local setup. Google and Fusion sign-in are performed by the user through their official interfaces.
- Keep software acquisition, the Oracle source snapshot, and the development workspace distinct. Keep the registered CLI at a stable path; moving or deleting that directory breaks the launcher.
- Use current official documentation to resolve changed layouts or installers. Never invent package names, release numbers, command flags, extension commands, or an npm package for `aistudio`. If necessary information cannot be verified, report the specific blocker and continue independent checks.

## 1. Establish context and inspect the environment

Use the active local project directory as the setup root if writable. If no directory is open, ask for one writable local parent folder and guide the user to open it in Antigravity Desktop. Do not perform setup inside this helper skill's installation folder. Default to a new `oracle-ai-agent-studio` child when the open folder contains unrelated material.

Record OS, architecture, shell, root, detected executable paths and versions. Run independent guarded probes; one missing tool must not prevent the remaining checks. Classify results as installed and usable, installed but not on PATH, missing, incompatible, or not verified. Report a concise component table, not raw command transcripts.

### macOS discovery

Run `sw_vers`, `uname -m`, and inspect the current shell from the host context. Use `command -v` before running `node --version`, `npm --version`, `git --version`, `code --version`, and `agy --version`. Inspect `type -a aistudio` and `type -a agy` for conflicting aliases, functions, and executables. An `agy` that only opens an editor is not proof of Antigravity CLI; inspect its resolved path and `--help` output.

Find Antigravity Desktop using installed application metadata in `/Applications`, `$HOME/Applications`, or the current application's actual bundle path; confirm its identity and version rather than assuming a filename. Do not infer Desktop availability solely from `agy`.

If `code` is absent, inspect the installed VS Code application and its `Contents/Resources/app/bin/code` launcher. Use that verified absolute path for extension commands until PATH is repaired. Never reinstall VS Code merely because its shell command is absent.

### Windows discovery

Use native PowerShell 5.1 or 7; do not require PowerShell 7 installation. Inspect:

```powershell
$PSVersionTable.PSVersion
[System.Runtime.InteropServices.RuntimeInformation]::OSArchitecture
[Environment]::OSVersion.Version
Get-ExecutionPolicy -List
```

Use `Get-Command -ErrorAction SilentlyContinue` separately for `node.exe`, `npm.cmd`, `git.exe`, `code.cmd`, `agy`, and `aistudio`; inspect all matches for conflicts. Run versions using discovered absolute paths and the call operator `&`. Prefer `npm.cmd` and `code.cmd` over `.ps1` shims, avoiding unnecessary execution-policy changes.

If VS Code is absent from PATH, inspect its actual installation. The common user launcher is `$env:LOCALAPPDATA\Programs\Microsoft VS Code\bin\code.cmd`; also check an existing system or portable installation when applicable. Windows does not use the macOS **Install 'code' command in PATH** command.

Find Antigravity Desktop in the current app context or Windows installed-app metadata, including user installations and packaged applications. Confirm its executable/package and version. Do not equate the VS Code extension with Desktop. If Desktop is missing, report that this skill's starting prerequisite is unmet and provide its official download link; do not silently install a different Google product.

### Shared checks

- Node and npm versions, architecture, and actual paths; Git only when usable.
- VS Code version and active profile. Query extensions with its verified launcher: `--list-extensions --show-versions`; use `--profile <name>` consistently if a named profile is active.
- Google extension ID `Google.google-antigravity` (compare IDs case-insensitively).
- Antigravity CLI `agy` path, version, and help behavior; distinguish missing PATH from missing binary. Probe Google's documented user installation directory before reinstalling.
- Writable root, available space, existing Oracle repository/workspace, and conflicting `aistudio` commands. A reported sandbox restriction is not an OS permission failure.

## 2. Install only missing or incompatible prerequisites

Choose architecture-compatible official packages. Use an existing package manager only when appropriate for the user's environment; do not install a package manager solely for this setup. Wait for each installer to finish and inspect its exit/result before continuing. Restart or refresh the relevant process when PATH changes.

### Node.js and npm

If needed, resolve the newest supported LTS from `https://nodejs.org/dist/index.json`, considering Oracle's documented constraints. Do not hard-code an LTS number. Download the matching file and its `SHASUMS256.txt` from `https://nodejs.org/dist/<version>/`; compare the file's SHA-256 against the exact filename entry before running the installer.

- **Windows:** use an available `winget` with exact package `OpenJS.NodeJS.LTS`, or the official `node-<version>-x64.msi` / `node-<version>-arm64.msi` listed in distribution metadata. Native Windows ARM64 must not be inferred from an x64-emulated shell. Run the MSI through its normal installer and obtain elevation if requested; never claim MSI installation is always per-user.
- **macOS:** use the official `node-<version>.pkg` when it supports the host architecture and OS, or an already installed developer package manager if preferred. Verify package signature with `pkgutil --check-signature` before installation. Explain system installation/elevation when applicable.

Recheck both Node and npm through the intended user terminal. Preserve version-manager installations; do not replace their configuration or Antigravity's internal runtime. Do not run `npm install` for the bundled Oracle CLI unless the selected repository explicitly documents additional dependencies.

### Visual Studio Code

- **Windows:** default to Microsoft's User Installer from `https://update.code.visualstudio.com/latest/win32-x64-user/stable` or `win32-arm64-user/stable`, according to native OS architecture. Save it in the setup downloads folder, verify `Get-AuthenticodeSignature` returns a valid Microsoft publisher signature, then launch it and wait. Select the installer's PATH option when authorized. An existing `winget` may use exact package `Microsoft.VisualStudioCode`; verify scope rather than promising a user installation. Locate `code.cmd` directly after installation.
- **macOS:** use Microsoft's stable archive endpoint `https://update.code.visualstudio.com/latest/darwin-arm64/stable` or `darwin/stable` for Intel, verifying current availability. Inspect the ZIP and extract into a new staging directory using `ditto -x -k`. Place the app in `$HOME/Applications` when using a user installation, or `/Applications` if authorized and writable. Verify its code signature with `codesign --verify --deep --strict` and normal macOS assessment; never remove quarantine as a workaround. Use its absolute CLI launcher or the documented Command Palette shell-command installation.

Recheck the VS Code version against Google's current extension requirements. Preserve existing profiles and extension settings.

### Google Antigravity extension

Install into the selected VS Code profile, using its verified launcher:

```text
code --install-extension Google.google-antigravity
code --list-extensions --show-versions
```

Replace `code` with its actual path (`& $codeLauncher ...` in PowerShell); add the same `--profile` option to both commands when needed. Reload the editor when requested. Have the user open the Google Antigravity panel and complete any required sign-in themselves. Its automatic backend installation does not prove a terminal-accessible CLI exists. Installation verification and interactive activation/sign-in verification are separate results.

### Antigravity CLI

Use [Google's current installation guide](https://www.antigravity.google/docs/cli/install/). Current official installer script URLs are:

| Platform | Installer | Documented binary location |
| --- | --- | --- |
| macOS | `https://antigravity.google/cli/install.sh` | `$HOME/.local/bin/agy` |
| Windows | `https://antigravity.google/cli/install.ps1` | `$env:LOCALAPPDATA\agy\bin` |

Download the script to a new file under the setup downloads directory with `curl -fL` on macOS or `Invoke-WebRequest` on Windows. Inspect it for the official download endpoints, architecture handling, PATH changes, and alias removal before executing it. Use the documented `--skip-aliases` option when supported to preserve legacy aliases; diagnose any alias collision separately. Use `--skip-path` only if you will manage and verify PATH explicitly. Do not assume Unix and PowerShell argument syntax are identical: read the downloaded script's parameter declarations.

On Windows, inspect effective policy in the exact shell running the installer. If blocked, do not use `-ExecutionPolicy Bypass`. If enterprise policy enforces the block, report it for IT resolution. A user-scope policy change requires explicit authorization and must be narrowly justified for the installer; the `aistudio.cmd` launcher itself needs no PowerShell policy change. If the script's documented installation method is disallowed, pause this component rather than inventing an unsigned binary download URL.

Verify the installed executable directly, then `agy --version` and `agy --help` through a fresh terminal PATH. Preserve a conflicting editor launcher or alias until the user chooses how to resolve it. Let the user complete first-launch trust and sign-in; do not read the Google keyring or generate an API key. Report CLI installation separately from account readiness.

## 3. Obtain and record the Oracle source snapshot

Use only `https://github.com/oracle/fusion-ai-studio`. Inspect remote metadata before assuming a branch or file layout. Prefer the branch corresponding to a Fusion release already stated by the user. If unknown, use the repository's default release branch as a provisional local setup snapshot and clearly state that compatibility with the user's Fusion environment remains unverified. Ask for a product release only when needed to resolve a known mismatch. Do not select the highest-looking branch name without Oracle documentation.

Use this layout under the selected writable setup root; adapt names safely when paths exist and verify that the final structure matches this outline, creating the `fusion-ai-workspace` folder and its content.
Do not treat the root folder as the fusion AI workspace.

```text
oracle-ai-agent-studio/
├── downloads/             # Installers and original archives
├── fusion-ai-repo/        # Recorded Oracle source snapshot
├── extension-staging/     # Extracted Oracle VSIX
└── fusion-ai-workspace/   # User development project
    ├── .agents/skills/
    └── aiapps/
```

With Git available, discover refs with `git ls-remote --symref https://github.com/oracle/fusion-ai-studio.git HEAD`, then clone the selected existing branch/tag into a nonexistent destination using `git clone --branch <ref> --single-branch <url> <destination>`. Record `git rev-parse HEAD` and the commit date. For an existing clone, inspect origin, branch, revision, and worktree status; do not reset, switch, or pull automatically over local modifications.

Without usable Git, use GitHub API repository metadata to resolve the default branch, then resolve the selected ref to a commit SHA. Download `https://github.com/oracle/fusion-ai-studio/archive/<sha>.zip`; record repository, ref, SHA, URL, and acquisition date in the final report. If rate limiting prevents this, retry through an official accessible metadata endpoint once or report the blocker; never request a personal access token for this public download.

Before extracting either a repository ZIP or extension ZIP, list entries and reject absolute paths, `..` traversal, and entries resolving outside the chosen destination. On macOS list with `unzip -Z1` and extract with `ditto`; on Windows inspect `System.IO.Compression.ZipFile.OpenRead()` entries, dispose the archive, then use `Expand-Archive -LiteralPath ... -DestinationPath ...` into a new directory. Verify the single expected repository root before placing it at the chosen stable location. Preserve dot-directories and Oracle license files.

Inspect the selected repository's README and `how-to/` installation documentation. The current expected assets are `.agents/skills/aistudio/SKILL.md`, `.agents/skills/aistudio/scripts/aistudio.js`, `aiapps/`, and `extensions/aistudio-extension.zip`. Discover documented alternatives rather than inventing a legacy layout. Check that the base skill's referenced scripts/resources are present. Do not adopt Codex-specific host requirements from an Oracle guide when configuring Antigravity.

## 4. Prepare the workspace and Oracle extension

Create a new development workspace inside the root folder using the folder name `fusion-ai-workspace`. Copy the complete Oracle `.agents/skills` tree and `aiapps` tree, including hidden items and all referenced resources. Reuse identical files; stop only conflicting copies and continue independent checks. Do not invoke or rewrite Oracle authoring skills as part of setup.
Ensure that the folder `fusion-ai-workspace` is created inside the root folder `oracle-ai-agent-studio`.

- **macOS:** for an absent/empty destination, `ditto "$source" "$destination"` copies contents, including hidden files. Do not run it against an unchecked nonempty destination.
- **Windows:** enumerate contents with `Get-ChildItem -LiteralPath $source -Force` and copy each entry with `Copy-Item -LiteralPath $entry.FullName -Destination $destination -Recurse`. This includes dot-items and avoids wildcard treatment of unusual names. Do not use `-Force` to bypass a conflict decision.

Verify recursive relative file lists/counts and the base skill/script paths after copying. Resolve the copied CLI to an absolute path. Run `node <absolute-aistudio.js> version`, `--help`, and `init --help` from the workspace using the verified Node executable. Treat syntax/runtime errors as a concrete compatibility diagnostic, not proof that any existing Node must be replaced.

Inspect and extract the extension ZIP into a new staging directory; locate its VSIX and inspect the VSIX manifest/package identity. Install that file into the selected VS Code profile with `--install-extension <absolute-vsix-path>`. Compare the resulting extension list with the inspected identity; the expected current ID is `oracle.fusion-aistudio-vscode`. Confirm its manifest declares **Fusion AI Studio: Configure Authentication**, or confirm the command appears in VS Code. Report UI activation separately if it could not be inspected. Do not attempt to install VSIX packages into standalone Antigravity Desktop; its Oracle integration uses CLI and skills.

## 5. Register a persistent user-level `aistudio` command

This step is required on both platforms. Use the stable copied CLI path in `fusion-ai-workspace/.agents/skills/aistudio/scripts/aistudio.js`, or another user-approved stable Oracle skill installation. Do not point to a temporary extraction. Inspect existing command resolution before creating a launcher. A user-selected compatible existing launcher can be reused after verification.

Create launchers with normal file-writing tools, not interpolated shell `echo` commands. Substitute actual absolute paths; the examples below are templates, not literal commands to run. Do not place a wrapper beside Node.js, install a fictitious npm package, or change machine-level PATH.

### macOS launcher and PATH

Default launcher: `$HOME/.local/bin/aistudio`. Obtain the actual Node executable path; for a version manager, explain that removing the referenced Node version will require refreshing the launcher.

Generate this POSIX shell launcher with each absolute path encoded as a shell single-quoted string (encode embedded apostrophes as `'"'"'`):

```sh
#!/bin/sh
# Managed by setup-oracle-ai-agent-studio-antigravity
exec '/actual/absolute/path/to/node' '/actual/absolute/path/to/aistudio.js' "$@"
```

Set only this launcher's executable permission, e.g. `chmod u+x <launcher>`. It must not change directory; `exec` preserves argument boundaries and the CLI exit status.

If its directory is already on the effective persistent PATH, do not edit a profile. Otherwise, inspect the user's interactive shell and startup files, including an existing `ZDOTDIR` for zsh. After authorization, append one uniquely marked block to the actual zsh `.zshrc` or bash interactive startup file; preserve encoding, content, and a backup. Do not duplicate the block on repeat runs. Example for zsh/bash:

```sh
# BEGIN setup-oracle-ai-agent-studio-antigravity PATH
case ":$PATH:" in
  *":$HOME/.local/bin:"*) ;;
  *) export PATH="$HOME/.local/bin:$PATH" ;;
esac
# END setup-oracle-ai-agent-studio-antigravity PATH
```

For another shell, use its documented user-level PATH mechanism instead of adding POSIX syntax to its configuration. Verify in a fresh instance of the actual interactive shell (e.g. `/bin/zsh -lic 'command -v aistudio; aistudio --help'` with cwd outside the workspace). Also verify the executable directly from a noninteractive process. An alias/function shadowing it remains a blocker until resolved.

### Windows launcher and PATH

Default directory: `Join-Path $env:LOCALAPPDATA 'OracleAIStudio\bin'`. Launcher: `aistudio.cmd`. Use resolved absolute paths to `node.exe` and `aistudio.js`. Generate CRLF lines, encoded without a BOM, with this content:

```bat
@echo off
rem Managed by setup-oracle-ai-agent-studio-antigravity
setlocal DisableDelayedExpansion
"C:\actual\path\node.exe" "C:\actual\path\aistudio.js" %*
exit /b %errorlevel%
```

Encode literal `%` characters in the two embedded absolute paths as `%%` before writing; do not alter `%*` or `%errorlevel%`. Use an encoding that preserves the actual account/path characters, and verify paths containing non-ASCII characters on that Windows host. If CMD cannot represent the paths reliably, choose a user-approved stable compatible location and report the limitation. Reject newline or quote characters in generated path literals. `DisableDelayedExpansion` protects literal `!` when executing the launcher; callers still need their own shell's quoting rules for metacharacters. Preserve cwd and forward arguments through `%*`.

Persist only the user PATH. Read its existing raw user value and append the launcher directory only when not already present (compare expanded, normalized directory entries case-insensitively). Preserve every existing entry and its literal environment-variable references. Do not use `setx PATH`, which can expand/truncate values. Example after setting `$launcherDirectory` to its actual absolute path:

```powershell
$userPath = [Environment]::GetEnvironmentVariable('Path', 'User')
$entries = @($userPath -split ';' | Where-Object { $_ -ne '' })
$alreadyPresent = $false
foreach ($entry in $entries) {
    $expanded = [Environment]::ExpandEnvironmentVariables($entry).TrimEnd('\')
    if ($expanded -ieq $launcherDirectory.TrimEnd('\')) { $alreadyPresent = $true }
}
if (-not $alreadyPresent) {
    $separator = if ([string]::IsNullOrEmpty($userPath) -or $userPath.EndsWith(';')) { '' } else { ';' }
    $newUserPath = $userPath + $separator + $launcherDirectory
    [Environment]::SetEnvironmentVariable('Path', $newUserPath, 'User')
}
```

Record the previous user PATH for rollback without printing unrelated environment variables. Refresh only the current process PATH by appending this directory if absent; do not write the combined process PATH back to the user variable. Restart Antigravity Desktop, VS Code, and their terminals when needed, because child terminals can inherit stale environment values. If a freshly opened process still has stale PATH, use a new sign-in session or the host's documented environment refresh; do not reinstall tools.

Verify `Get-Command aistudio -All` and `aistudio --help` from native PowerShell and `where.exe aistudio` / `aistudio --help` from `cmd.exe /d`, with cwd outside the workspace. Check exit codes immediately after each native command. Test with `-NoProfile` in PowerShell as well: the launcher must not depend on a function or policy change. A subprocess with manually appended PATH tests execution, not persistent environment propagation; report persistence only after a refreshed user process resolves it. If an old alias/function wins resolution, obtain a specific decision before altering it.

## 6. Initialize the new project and open it

A user request to set up a new blank AI Studio project authorizes local scaffolding. Before running `init`, confirm the chosen directory has no existing project artifacts. Installing this helper into a project does not authorize reinitializing that project.

Run the verified CLI's `init --help` first. For the currently documented CLI, use `aistudio init --dir <absolute-new-project-path>`. Prefer initializing the new workspace before copying Oracle skills/samples when practical, or verify existing contents consist only of the setup copies. Never run `init` over existing user source, package configuration, or `env.properties`. If the command changes in a later release, follow its actual help. Do not use force options to bypass existing files.

Check generated artifact/test directories and configuration filenames without displaying sensitive file contents. Confirm `.gitignore` excludes `env.properties` and any credential material before suggesting version control; add the exact missing ignore entries without overwriting existing rules. Do not initialize Git, commit, or publish unless requested.

Open/select the development workspace as a local project in Antigravity Desktop using the installed version's UI. Do not invent an `agy` Desktop-opening command. Also open that exact folder in VS Code using its verified launcher. Ask the user to decide workspace trust. Confirm Antigravity discovers the Oracle `aistudio` skill and domain skills through its Customizations/skills interface; use `/skills` in the CLI when supported. If interactive inspection is unavailable, state that file placement passed and discovery still requires user verification.

For future projects, explain the repeatable sequence: create a new folder, run `aistudio init --dir <folder>`, copy the complete matching Oracle `.agents/skills` tree there, optionally copy samples, and open the folder in the chosen surface. The global launcher alone does not make project-local skills discoverable in unrelated directories.

## 7. Verify readiness and report

Perform applicable checks once after the final changes; repeat only failed checks or checks affected by a repair. Use harmless `version`, `--help`, and `init --help` commands; never authenticate or contact Fusion to prove local installation.

| Check | Evidence required |
| --- | --- |
| Native host and Desktop | OS/architecture and actual Desktop installation |
| Node/npm | Versions and resolved user executable paths |
| VS Code and Google extension | Version, profile, installed extension ID/version |
| Antigravity CLI | CLI identity, executable path, successful version/help |
| Oracle snapshot | Source, branch/ref, commit SHA, compatibility status |
| Workspace | Complete copied skills/resources, samples, new scaffold if requested |
| Oracle VS Code extension | Installed manifest identity and declared/discoverable commands |
| Global `aistudio` | Correct launcher resolved outside workspace in refreshed terminals, successful help and exit status |
| Agent skill discovery | Observed active skills, or explicitly pending UI verification |

Report each as passed, blocked, declined, or pending verification. Never summarize a partial setup as fully ready. Distinguish **local tooling ready**, **agent integration verified**, and **authentication pending**. Give the actual root, repository, workspace, CLI and launcher paths; versions and snapshot; changes made; and one next action for each remaining blocker.

The user must later complete Google sign-in if needed and run **Fusion AI Studio: Configure Authentication** in VS Code using administrator-provided details. Do not request those details in chat. Local readiness is not proof of Fusion access or remote functionality.

## Official sources

Checked when this skill was authored on October 1, 2026; verify current details at execution time when necessary.

- [Google Antigravity getting started and Desktop](https://www.antigravity.google/docs/getting-started/)
- [Antigravity CLI installation and authentication](https://www.antigravity.google/docs/cli/install/)
- [Antigravity agent skills](https://www.antigravity.google/docs/skills)
- [Antigravity for VS Code](https://www.antigravity.google/docs/ide/extensions/vscode/)
- [Official Google VS Code extension](https://marketplace.visualstudio.com/items?itemName=Google.google-antigravity)
- [Oracle Fusion AI Studio repository](https://github.com/oracle/fusion-ai-studio) — read the selected snapshot's README, installation guide, and CLI help.
- [Oracle CLI overview](https://blogs.oracle.com/fusioncoe/fusion-aistudio-cli)
- [Node.js official downloads](https://nodejs.org/en/download) and [distribution metadata](https://nodejs.org/dist/index.json)
- [VS Code on macOS](https://code.visualstudio.com/docs/setup/mac), [Windows](https://code.visualstudio.com/docs/setup/windows), and [CLI](https://code.visualstudio.com/docs/configure/command-line)
- [PowerShell execution policies](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies)

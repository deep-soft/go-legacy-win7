# The Go Programming Language

**go-legacy-win7** is a fork of the Go programming language that maintains support for Windows 7 and Windows Server 2008 R2, and allows for deprecated `go get` behaviour. This project aims to provide a stable Go environment for users who need to support legacy Windows systems or prefer the traditional Go workflow.

![Gopher image](https://golang.org/doc/gopher/fiveyears.jpg)
_Gopher image by [Renee French][rf], licensed under [Creative Commons 4.0 Attribution licence][cc4-by]._

## Differences from Upstream Go

1. **Windows 7 and Windows Server 2008 R2 Support**  
   While the official Go project has dropped support for Windows 7 and Windows Server 2008 R2, this fork maintains compatibility with these legacy Windows systems.

   Tested on Windows 7 RTM (build 7600) — no updates required — through Windows 11 25H2

2. **Classic `go get` Behaviour**  
   This fork allows for the deprecated `go get` behaviour when `GO111MODULE` is set to "off" or "auto". This means:

   - In `GOPATH/src`, `go get` and `go install` can operate in `GOPATH` mode.
   - Outside of `GOPATH/src`, these commands can use module-aware mode when appropriate.

3. **Compatibility Notes**  
   Please be aware that some newer Go features may not be fully compatible with Windows 7 or Windows Server 2008 R2. We try to maintain as much functionality as possible, but some limitations may exist.

## Changes in Each Release

Current release includes the following modifications:

- Switched back to RtlGenRandom from ProcessPrng, which breaks Win7/2008R2 (reverted [693def1](https://github.com/golang/go/commit/693def151adff1af707d82d28f55dba81ceb08e1))
- Added back LoadLibraryA fallback to load system libraries (reverted [a17d959](https://github.com/golang/go/commit/a17d959debdb04cd550016a3501dd09d50cd62e7))
- Added back sysSocket fallback for socket syscalls (reverted [7c1157f](https://github.com/golang/go/commit/7c1157f9544922e96945196b47b95664b1e39108))
- Added back Windows 7 console handle workaround (reverted [48042aa](https://github.com/golang/go/commit/48042aa09c2f878c4faa576948b07fe625c4707a))
- Added back 5ms sleep on Windows 7/8 in (\*Process).Wait (reverted [f0894a0](https://github.com/golang/go/commit/f0894a00f4b756d4b9b4078af2e686b359493583))
- Restored deprecated `go get` behavior for use outside modules (reverted [de4d503](https://github.com/golang/go/commit/de4d50316fb5c6d1529aa5377dc93b26021ee843))
- Reverted to the previous `removeall_noat` variant for Windows (fixed [issue #2](https://github.com/thongtech/go-legacy-win7/issues/2))
- Rolled back `race_windows.syso` to the previous compatible version (fixed [issue #3](https://github.com/thongtech/go-legacy-win7/issues/3))
- Includes all improvements and bug fixes from the corresponding upstream Go release

We now provide two build options for Windows amd64:

- **Standard build**  
  Maximum Windows 7/2008R2 compatibility with the reverted race detector. Use this if you need Windows 7 and Windows Server 2008 R2 support.

- **Race detector build** (version suffix `-race`)  
  Uses Go's latest stable race detector without modifications. Recommended for Windows 10+ when running race tests to avoid potential edge cases with the legacy race detector. See [issue #6](https://github.com/thongtech/go-legacy-win7/issues/6)

## Download and Install

### Binary Distributions

| OS | Architecture | Filename | SHA‑256 Hash |
|----|--------------|----------|--------------|
| **macOS** | Intel (amd64) | [go-legacy-win7-1.25.7-1.darwin_amd64.tar.gz](https://github.com/thongtech/go-legacy-win7/releases/download/v1.25.7-1/go-legacy-win7-1.25.7-1.darwin_amd64.tar.gz) | `65ccc4ef5e1bd286415f6b23018f56ff81f831ebf7099f31313ca5ea3375887e` |
| macOS | Apple (ARM64) | [go-legacy-win7-1.25.7-1.darwin_arm64.tar.gz](https://github.com/thongtech/go-legacy-win7/releases/download/v1.25.7-1/go-legacy-win7-1.25.7-1.darwin_arm64.tar.gz) | `fb35f5a564395388ab8e40d058e1c923969c57c11b416be4b447bb8abc7cfb1c` |
| **Linux** | x86 (386) | [go-legacy-win7-1.25.7-1.linux_386.tar.gz](https://github.com/thongtech/go-legacy-win7/releases/download/v1.25.7-1/go-legacy-win7-1.25.7-1.linux_386.tar.gz) | `22b9190535a9557dd1c5489ed221f890c6ac4dfdb6e7ceec3a5e888471d63ba5` |
| Linux | x64 (amd64) | [go-legacy-win7-1.25.7-1.linux_amd64.tar.gz](https://github.com/thongtech/go-legacy-win7/releases/download/v1.25.7-1/go-legacy-win7-1.25.7-1.linux_amd64.tar.gz) | `fba423c06063ae4afb1e4275953e24d1abdaf4a575b19b267d5672cb2acbdba3` |
| Linux | ARM (32‑bit) | [go-legacy-win7-1.25.7-1.linux_arm.tar.gz](https://github.com/thongtech/go-legacy-win7/releases/download/v1.25.7-1/go-legacy-win7-1.25.7-1.linux_arm.tar.gz) | `749c3ec67cc1f7d8736bba38d7e10ed1e496a8e299245eda6f8ff436dba92ed1` |
| Linux | ARM64 | [go-legacy-win7-1.25.7-1.linux_arm64.tar.gz](https://github.com/thongtech/go-legacy-win7/releases/download/v1.25.7-1/go-legacy-win7-1.25.7-1.linux_arm64.tar.gz) | `fe762dd27aef6106d56ab7145a833ddab10af4ef86281cb41474c4c7b7aafc6b` |
| **Windows** | x86 (386) | [go-legacy-win7-1.25.7-1.windows_386.zip](https://github.com/thongtech/go-legacy-win7/releases/download/v1.25.7-1/go-legacy-win7-1.25.7-1.windows_386.zip) | `4aa0c310d52755f0d9542daf898bff0b1612339c695136d707494db56b9da552` |
| Windows | x64 (amd64) | [go-legacy-win7-1.25.7-1.windows_amd64.zip](https://github.com/thongtech/go-legacy-win7/releases/download/v1.25.7-1/go-legacy-win7-1.25.7-1.windows_amd64.zip) | `de0e038efbe9b4db680cb40de27907e7f0d455512f423aeb545889b3bb319003` |
| Windows | x64 (amd64) - Race | [go-legacy-win7-1.25.7-1-race.windows_amd64.zip](https://github.com/thongtech/go-legacy-win7/releases/download/v1.25.7-1/go-legacy-win7-1.25.7-1-race.windows_amd64.zip) | `f2eba08a1a19bc747423b1e769d9f4ba10ae53f6ad2198a8395cf253b6b8a300` |
| Windows | ARM64 | [go-legacy-win7-1.25.7-1.windows_arm64.zip](https://github.com/thongtech/go-legacy-win7/releases/download/v1.25.7-1/go-legacy-win7-1.25.7-1.windows_arm64.zip) | `058c34bcca3e780e9b8afd4ec2ca3f5d7b5ab468c9330135f6c3a08e72c991a5` |

### Before you begin
To avoid PATH/GOROOT conflicts and mixed toolchains, uninstall any existing Go installation first.

#### Windows Installation

1. Download the `go-legacy-win7-<version>.windows_<arch>.zip` file.
2. Extract the ZIP to `C:\` (or any preferred location). This will create a `go-legacy-win7` folder.
3. Add the following to your system environment variables:
   - Add `C:\go-legacy-win7\bin` (or your chosen path) to the system `PATH`.
   - Set `GOROOT` to `C:\go-legacy-win7` (or your chosen path).
4. Add the following to your user environment variables:
   - Add `%USERPROFILE%\go\bin` to the user `PATH`.
   - Set `GOPATH` to `%USERPROFILE%\go`.

#### macOS and Linux Installation

1. Download the appropriate `go-legacy-win7-<version>.<os>_<arch>.tar.gz` file.

   - For macOS: `go-legacy-win7-<version>.darwin_<arch>.tar.gz`
   - For Linux: `go-legacy-win7-<version>.linux_<arch>.tar.gz`

2. Extract the archive to `/usr/local`:

   ```
   sudo tar -C /usr/local -xzf go-legacy-win7-<version>.<os>_<arch>.tar.gz
   ```

3. Add the following to your shell configuration file:

   - For bash, add to `~/.bash_profile` or `~/.bashrc`
   - For zsh, add to `~/.zshrc`

   ```bash
   export GOROOT=/usr/local/go-legacy-win7
   export GOPATH=$HOME/go
   export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
   ```

4. Apply the changes:

   - For bash: `source ~/.bash_profile` or `source ~/.bashrc`
   - For zsh: `source ~/.zshrc`

   Note:

   - On macOS Catalina and later, zsh is the default shell.
   - On most Linux distributions, bash is the default shell.

After installation, verify the installation by opening a **new terminal** and running:

```
go version
```

### Install From Source

To install from source, please follow the steps on the [official website](https://go.dev/doc/install/source).

## Contributing

Feedback and issue reports are welcome, and we encourage you to open pull requests to contribute to the project. We appreciate your help!

Note that the Go project uses the issue tracker for bug reports and
proposals only. See https://go.dev/wiki/Questions for a list of
places to ask questions about the Go language.

[rf]: https://reneefrench.blogspot.com/
[cc4-by]: https://creativecommons.org/licenses/by/4.0/

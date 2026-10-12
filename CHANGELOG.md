# Changelog

Every notable change to preflight is recorded here.

The format is [Keep a Changelog 1.1.0](https://keepachangelog.com/en/1.1.0/)
and the version numbers follow
[Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html).

## [0.1.3] - 2026-10-12

Nothing about scanning changed! preflight.exe finds exactly what 0.1.2 found;
this release is all about how it gets built, tested and released.

### Changed

- Synced with QCDX v0.1.0. The ProjectKit scripts (`pk`, verify, release) are
  Python now and run the same from PowerShell, cmd, MSYS2, macOS and Linux.
- Releases go through ProjectKit's release pipeline (`scripts/release.py`)
  instead of my own workflow. `release.conf` keeps them looking like before:
  `preflight.exe` and `README.md`, Windows x64 only.
- Release notes list every file with its size and SHA-256, and each SHA-256
  links to its VirusTotal report.
- A release fails if `preflight.exe` needs *any* MinGW runtime DLL (it has to
  run on PCs without MSYS2). This replaces the old `ldd` check.
- `CMakeLists.txt` and `conanfile.py` describe preflight now, not the
  template.

### Added

- `SHA256SUMS` attached to every release.
- This changelog.

### Fixed

- `pk test` on Windows failed with `0xc0000139` when the shell's `PATH` had an
  older `libstdc++-6.dll` first (e.g. a MINGW64 shell after a GCC update). The
  tests now put the compiler's own folder first on `PATH`.
- A stray comma in the clang-tidy command (`StaticAnalysis.cmake`) reached
  clang-tidy as its own argument and caused a CMake dev warning.
- The hash check in the releases README showed an old SHA-256 as its prompt;
  someone could easily mistake it for the hash to compare against.
- The repository README was meant for another repo.

## [0.1.2] - 2026-10-08

### Added

- preflight.exe has its own icon (16 to 256 px) instead of the generic program
  icon.
- Release workflow: pushing a `v*` tag tests and builds `preflight.exe`,
  checks the tag matches the version it reports and that it needs no MSYS2
  DLLs, then creates a draft release in `preflight-releases`.
- `release/README.md` is copied to the front page of `preflight-releases`
  whenever it changes; one file serves both.

### Changed

- Resolved the clang-tidy warnings in `main.cpp`, no more `reinterpret_cast`
  when reading files or converting paths.

## [0.1.1] - 2026-10-07

### Added

- Windows version info: Properties > Details shows the name, version and
  publisher.
- Scanning preflight.exe with itself (the exact same file) reports it as
  preflight instead of scanning it. Its rules contain the very text it
  searches for; a real scan would only find itself!

### Fixed

- Release builds are stripped. MinGW left 1.8 MiB of debug info after the last
  section, and some malware scanners treat that as hidden data.
- SHA-256 padding is written into the block buffer directly. GCC reported a
  `-Warray-bounds` false positive at `-O2`, breaking `--werror` release
  builds.

## [0.1.0] - 2026-10-06

The first release! Drag a file onto preflight.exe and it tells you if anything
about it looks *suspicious* before you run it.

### Added

- Static scanning of Windows programs: the file is only read, never run or
  loaded.
- Red flags for stealer behaviour (Discord tokens, browser passwords and
  cookies, crypto wallets, game launcher logins), Discord webhooks and
  Telegram bots (where stolen data gets sent), turning off Microsoft Defender,
  starting with Windows, hidden PowerShell, checking for analysis tools and
  process injection imports.
- Checks on the file itself: disguised extensions (a right-to-left character
  or a double extension), scripts, shortcuts, `.jar` files, archives and
  password-protected archives.
- Digital signature check through Windows (`WinVerifyTrust`).
- Notes files downloaded from Discord.
- The SHA-256 of every file, with VirusTotal and Hybrid Analysis links to look
  it up.
- A verdict for each file (DANGEROUS, SUSPICIOUS or NO RED FLAGS FOUND) and
  exit codes 0 to 3.
- Folders are scanned recursively; drop a whole extracted archive onto
  preflight.exe.

[0.1.3]: https://github.com/cooldood155/preflight-releases/releases/tag/v0.1.3
[0.1.2]: https://github.com/cooldood155/preflight-releases/releases/tag/v0.1.2
[0.1.1]: https://github.com/cooldood155/preflight-releases/releases/tag/v0.1.1

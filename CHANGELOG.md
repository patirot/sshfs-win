# Changelog

## 2026.1 Beta (2026-11-09)

Unofficial build from the `patirot/sshfs-win` fork. The MSI is not code signed.

### Updated
- SSHFS 3.7.1 → 3.7.6 (upstream libfuse/sshfs). Includes the connection
  cleanup deadlock fix, readdir/cache fixes, malformed SFTP reply handling,
  and fixes for CVE-2026-47187 (symlink escape) and CVE-2026-48711
  (hostname argument injection).
- Bundled Cygwin runtime and OpenSSH updated (Cygwin 3.6.12, OpenSSH 10.6p1).

### Changed defaults (`sshfs-win svc`, i.e. Explorer / `net use` mounts)
- `ServerAliveInterval=15`, `ServerAliveCountMax=3`: detect dead connections
  (e.g. after sleep or network change) instead of hanging the drive.
- `ConnectTimeout=10` and `reconnect`: re-establish dropped connections.
- `no_contain_symlinks`: keep pre-3.7.6 symlink behavior (absolute and `..`
  symlink targets are allowed).
- `max_read=65536,max_write=65536` (the sshfs maximum).
- `max_conns=4` for key authentication (`sshfs.k`, `sshfs.kr`); not available
  with password authentication.

GUI front ends that run `sshfs.exe` directly (e.g. SSHFS-Win-Manager) do not
get these defaults; set them in the front end or in `~/.ssh/config`.

### Fixed
- No visible console window when `sshfs.exe` is started by a GUI front end
  (bundle `cygwin-console-helper.exe`).
- Password authentication no longer writes to a closed stdout on reconnect.

### Installer and build
- Installing a newer MSI upgrades an existing installation in place
  (including the official 2021 release); uninstalling first is no longer required.
- MSI version, file name and product name include the CI build number.
- GitHub Actions workflow builds the MSI.
- Dropped `patches/30-stdiomode.patch` (merged upstream).

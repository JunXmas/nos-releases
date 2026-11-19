# What the apps do on your computer / App làm gì trên máy bạn

## Download only here / Chỉ tải ở đây

Official downloads: this repository's Releases and <https://nostalgiamc.org/>. Anything re-uploaded
elsewhere (Facebook groups, short links, other sites) is not ours.

Chỉ tải ở Releases của kho này và nostalgiamc.org. Bản up lại ở chỗ khác không phải của chúng tôi.

Every release has `SHA256SUMS` and a signature `SHA256SUMS.sig`. The launcher checks the signature
with the release key built into it before it installs an update or a Nos Turbo build. Release
public key: [`nos-release.pub.pem`](nos-release.pub.pem).

## Servers the apps talk to / Máy chủ app kết nối tới

**Ours / Của chúng tôi**

- `api.nostalgiamc.org`, `nostalgia-payment-admin.junbob.workers.dev`: Nos account, Nos+,
  cosmetics, the session ticket that lets Nos Turbo show cosmetics. They never receive your
  Minecraft token or password.
- `github.com`, `api.github.com`: launcher updates and Nos Turbo downloads from this repository.
- `raw.githubusercontent.com`: the home page news card.

**Sign-in / Đăng nhập**

- `login.microsoftonline.com`, `microsoft.com/link`: Microsoft sign-in. You type your password on
  Microsoft's page, never in the launcher.
- `user.auth.xboxlive.com`, `xsts.auth.xboxlive.com`, `api.minecraftservices.com`: turn the
  Microsoft sign-in into your Minecraft profile and check you own the game.
- `sessionserver.mojang.com`: confirms you when you join a server.
- `authserver.ely.by`, `account.ely.by`, `skinsystem.ely.by`: Ely.by accounts and skins. An Ely.by
  password goes straight to Ely.by.

**Game files and mods / File game và mod**

- `piston-meta.mojang.com`, `libraries.minecraft.net`, `resources.download.minecraft.net`:
  Minecraft's own files (needed once to prepare Nos Turbo, and for modpacks).
- `api.modrinth.com`, `cdn.modrinth.com`, CurseForge (`api.curseforge.com`,
  `mediafilez.forgecdn.net`, through our proxy `nostalgia-backend.junbob.workers.dev/cf`),
  `meta.fabricmc.net`, `meta.quiltmc.org`, `meta.legacyfabric.net`, `maven.minecraftforge.net`,
  `maven.neoforged.net`: mods, modpacks and loaders you choose.
- `authlib-injector.yushi.moe`: the small library that lets Ely.by skins show in game.

**Inside Nos Turbo / Trong Nos Turbo**

- `lrclib.net`: lyrics for the song playing on your computer, only when sky lyrics are on. It
  receives the song title and artist.
- The game server you choose to join.

**Usage statistics / Thống kê sử dụng**: off unless you agree. When on, once a day it sends the
date, launcher version, operating system and a random ID that changes every day. Nothing else.

## Passwords and tokens / Mật khẩu và token

- There is no Microsoft password box in the launcher.
- Minecraft sign-in tokens are kept in `accounts.json` in the launcher's config folder, readable
  only by your user (mode 600 on Linux).
- Your Nos account session is kept in the system keychain (Windows Credential Manager, GNOME
  Keyring or KWallet), never in a plain file.
- Minecraft tokens are never sent to our servers.

## On your computer / Trên máy bạn

- No admin rights needed after install, no driver, no background service.
- Uninstalling removes the app. Your worlds and screenshots stay unless you delete them.
- Nostalgia 2.0 reads the old Nostalgia 1.2 folder once to copy your instances, accounts and skins.
  It never changes or deletes anything there.

## Check a file yourself / Tự kiểm file

```
# Windows (PowerShell)
Get-FileHash <file> -Algorithm SHA256
# Linux / macOS
sha256sum <file>
```

Compare the result with the line for that file in `SHA256SUMS`.

To check the signature (OpenSSL 3), for a release tag such as `turbo-v1.0.0`:

```
{ printf 'nos-turbo release turbo-v1.0.0\n'; cat SHA256SUMS; } > message
base64 -d SHA256SUMS.sig > sig
openssl pkeyutl -verify -pubin -inkey nos-release.pub.pem -rawin -in message -sigfile sig
```

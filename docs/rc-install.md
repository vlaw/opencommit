# Local release candidate install

This guide covers the local release candidate (RC) channel on a developer machine. It does not apply to the published stable release.

## Why the RC stays local

An RC is not published to a public registry. It ships as a local tarball built from the release branch. The stable release comes from the public registry under the `latest` dist-tag.

Both packages own the same binary names (`oco` and `opencommit`). They therefore need separate npm prefixes, because one prefix would overwrite the other. This machine uses the two node installations as those prefixes:

| Channel | npm prefix                          | Source          |
| ------- | ----------------------------------- | --------------- |
| RC      | `~/.asdf/installs/nodejs/<version>` | local tarball   |
| Stable  | `/opt/homebrew`                     | public registry |

The asdf node version changes when the toolchain changes, so the RC channel disappears on the next asdf upgrade. The homebrew prefix stays put across `brew upgrade node`, so the stable channel survives. That split is intentional: rebuild the RC after an asdf upgrade, and the stable channel needs no attention.

## Install the RC channel

```sh
npm run build
mkdir -p /tmp/oco-pack
npm pack --pack-destination /tmp/oco-pack
npm install -g /tmp/oco-pack/opencommit-<version>.tgz --registry=https://registry.npmjs.org/ --min-release-age=0
```

The asdf npm wrapper runs `asdf reshim` after a global install, so the shim appears in `~/.asdf/shims`.

Use the npmjs registry for the install. The user registry is a mirror, and the mirror refuses the tarball install. Disable `min-release-age` for the same reason. Keep the tarball in the system temp directory and delete it after the install.

## Install the stable channel

```sh
/opt/homebrew/bin/npm install -g opencommit@latest --registry=https://registry.npmjs.org/ --min-release-age=0
```

## Path resolution

No `PATH` setup is needed. The zim asdf module puts `~/.asdf/shims` in `PATH`, and `.zprofile` puts `/opt/homebrew/bin` there. The profile file loads first, so the asdf shim directory comes first and the RC channel wins.

Do not add launcher scripts and do not add opencommit entries to `PATH`.

## Verify the install

```sh
which oco
which -a oco
oco --version
```

`which oco` must print the asdf shim path. `which -a oco` must list the asdf shim, then the homebrew path. `oco --version` must print the RC version from `package.json`.

## Release the RC

Tag the release commit, push the tag, and keep the RC version in `package.json` so the local install reports the RC version.

```sh
git tag -a v<version> -m "<version>"
git push origin v<version>
```

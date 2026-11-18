<!--
Copyright © 2026 <https://github.com/technosf>
SPDX-FileCopyrightText: © 2026 <https://github.com/technosf>

SPDX-License-Identifier: GPL-3.0-or-later
-->
# io.github.tuner_labs.tuner

Flathub Manifest and build information for *io.github.tuner_labs.tuner*

## Builds

[Build Bot](https://builds.flathub.org/status/io.github.tuner_labs.tuner)

### Assets

#### Platform

- org.gnome.Platform 51 (built on org.freedesktop.Platform 26.08; GTK4 is in the GNOME runtime)

#### Modules

- libgee 0.20.8
- libqrencode 4.1.1
- Vala, from the `org.freedesktop.Sdk.Extension.vala` SDK extension

#### Sources

The manifest builds two repositories, each pinned to an exact commit:

- *[tuner-gtk4](https://github.com/technosf/tuner-gtk4)*: the app
- *[tuner-core](https://github.com/technosf/tuner-core)*: the toolkit-free core, built as a meson subproject in `subprojects/tuner-core` (Flathub builds offline, so it is a source, not a meson wrap)

The app is built with `-Dpackaging=flatpak -Dapp_id=io.github.tuner_labs.tuner`.

## Process

### Check Metadata

From the *[tuner-gtk4](https://github.com/technosf/tuner-gtk4)* and *[tuner-core](https://github.com/technosf/tuner-core)* source code for the given release, verify:

- [ ] *tuner-gtk4 meson.build*  =>  project.version (the app's version)

- [ ] *tuner-core meson.build*  =>  project.version (the core's version; the app reports it in its About box and needs at least `core_minimum`)

- [ ] *tuner-gtk4 data/tuner-gtk4.metainfo.xml.in* => the newest `<release>` has the right date and notes (its version is filled in from the project version when built)

### Pin the Sources

Flathub can only fetch commits that are pushed. In *io.github.tuner_labs.tuner.yml* set the `commit:` of both git sources (tuner-gtk4 and tuner-core), and a `tag:` as well for a release.

### Check Build

Before checking in, test the app build and lint:

``` bash
flatpak run org.flatpak.Builder --force-clean --sandbox --user --install --install-deps-from=flathub --ccache --repo=repo builddir io.github.tuner_labs.tuner.yml

flatpak run --command=flatpak-builder-lint org.flatpak.Builder manifest io.github.tuner_labs.tuner.yml

flatpak run --command=flatpak-builder-lint org.flatpak.Builder repo repo

appstreamcli validate ~/.local/share/flatpak/app/io.github.tuner_labs.tuner/current/active/files/share/metainfo/io.github.tuner_labs.tuner.metainfo.xml
```

Linting results are documented  at <https://docs.flathub.org/linter>

## History

### 3.0.0

- UI moved from GTK3 to GTK4
- Platform changed from org.freedesktop.Platform 26.08 to org.gnome.Platform 51 (GTK4 is in the GNOME runtime, which is built on freedesktop 26.08)
- Built from two repositories, tuner-gtk4 and tuner-core, each pinned to a commit
- Added the libqrencode module
- Added `--device=dri` for GPU rendering
- The app is built with `-Dapp_id=io.github.tuner_labs.tuner`, so its desktop file, metainfo and icons keep the Flathub id

### 2.2.0

- granite 6.2.0 dep removed
- org.freedesktop.Platform updated to 26.08

### 2.1.1

- Added Adwaita-dark extension

### 2.0.3

- Removed Granite module
- Separate Locales

### 2.0.2

- Renaming from *com.github.louis77.tuner* to *io.github.tuner_labs.tuner* per flathub #7468

### 2.0.1

- Much thanks to @yakushabb for streamlining the build
- ~~Renamed app from *com.github.louis77.tuner* to *io.github.tuner_labs.tuner* to reflect the new app name. See [#7468](https://github.com/flathub/flathub/issues/7468)~~
- Added *separate-locales: false* to glob all translations into the same build image
- Rationalized Icons
- [@yakushabb](https://github.com/yakushabb) removed *vala* module as it is now built into *FreeDesktop* 25.08. Module will remain in the source manifest for local builds. See [#40](https://github.com/flathub/com.github.louis77.tuner/pull/40)
- [@yakushabb](https://github.com/yakushabb) removed *libsoup* module as it is now built into *FreeDesktop* 25.08. Module will remain in the source manifest for local builds. See [#43](https://github.com/flathub/com.github.louis77.tuner/pull/43)

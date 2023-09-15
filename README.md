# F-OH PWA

<img src="./src/assets/images/icon.svg" width=192 height=192 alt="Project logo" title="Project logo"/>

F-OH is an application center for FOSS (Free and Open Source Software) on the OpenHarmony platform with download and installation support.

F-OH PWA + BrowserCE, best practices for distributing open source hongmeng hap apps via web pages, might be a good choice for teams with internal distribution of test hap apps.

F-OH PWA is a browser-based version of F-OH based on the [Sparkling Store Demo](https://gitee.com/sparkling-store/webv3demo).

[![License](https://img.shields.io/github/license/Jesse205/F-OH-PWA)](./LICENSE)

[![QQ group (开鸿派): 752399947](https://img.shields.io/badge/QQ_group:_开鸿派-752399947-0099FF?logo=tencentqq)](https://qm.qq.com/q/jWeBdnvPz2)

[![Gitee repository](https://img.shields.io/badge/Gitee-repository-C71D23?logo=gitee)](https://gitee.com/ohos-dev/F-OH-PWA)
[![Github repository](https://img.shields.io/badge/Github-repository-0969DA?logo=github)](https://github.com/Jesse205/F-OH-PWA)

[中文](./README.zh.md) |
**English**

## Screenshots

<div>
<img src="./public/screenshots/Snipaste_2023-09-06_21-32-26.webp" width=30% />
<img src="./public/screenshots/Snipaste_2023-09-06_21-32-39.webp" width=30% />
<img src="./public/screenshots/Snipaste_2023-09-06_21-32-50.webp" width=30% />
</div>
<div>
<img src="./public/screenshots/Snipaste_2023-09-06_21-33-22.webp" width=30% />
<img src="./public/screenshots/Snipaste_2023-09-06_21-33-28.webp" width=30% />
<img src="./public/screenshots/Snipaste_2023-09-06_21-33-35.webp" width=30% />
</div>

## Download or launch

- F-OH Tauri: [Gitee Releases](https://gitee.com/ohos-dev/F-OH-PWA/releases/latest)
- F-OH Lite、F-OH PWA (Web): <http://170.178.208.105:5000/>

> **NOTE**\
> F-OH PWA is temporarily unavailable because the server does not have SSL and has cross-domain issues ([No security context to meet the minimum requirements to be a PWA][PWASecureContextRequirement]), please use F-OH Tauri or F-OH Lite (Web).

## Series of projects

- [F-OH](https://gitee.com/ohos-dev/f-oh): F-OH OpenHarmony Mobile
- [F-OH Data][F-OH-Data]: metadata for all F-OH apps, where developers PR submit their apps
- [F-OH Server](https://gitee.com/ohos-dev/f-oh-server): F-OH server, providing interface services, platform management, etc. (to be developed)
- [F-OH Website](https://gitee.com/ohos-dev/f-oh-website): F-OH website, including documents, blogs, selected applications, etc. (to be developed)

## Project setup

1. Install NodeJS v19
2. Install Yarn
3. Set up the Tauri environment according to [Tauri prep](https://tauri.app/v1/guides/getting-started/prerequisites/).
   - Windows: Microsoft Visual Studio C++ Builder, WebView2, Rust.
   - macOS: CLang and macOS development dependencies, Rust.
   - Linux: system dependencies, Rust.
4. Run `yarn install`

### Compiling and hotloading for development

1. Clone [F-OH Data][F-OH-Data] and start a server on port `5500`.
2. Open a terminal in the project and run commands according to the following rules.
   - Web and PWA applications: run `yarn dev`.
   - Windows Tauri software: run `yarn tauri dev`.

### Compiling and streamlining for production

1. Set up the `.env.production` file.
2. Open a terminal in the project and run commands according to the following rules.
   - Web pages and PWA applications:
      1. Run `yarn build`.
      2. Pull [F-OH Data][F-OH-Data] into `dist/data`.
      3. Deploy `dist/*` to the server.
   - Windows Tauri software:
      1. Run `yarn tauri build`.
      2. Release `src-tauri\target\release\F-OH Tauri.exe` and `src-tauri\target\release\bundle\nsis\F-OH Tauri_<version>_x64-setup.exe`.

## Lint and fixing files

```bash
yarn lint
```

## Support Program

Sponsorship can be contacted by private message or scanning the QR code below (WeChat, Alipay)

> **NOTE**\
> Please note "F-OH" or private message to [@westinyang (Gitee)][@westinyang] for sponsorship fee, so that it can be counted in [Sponsor List][SponsorList].

![QRCode](https://gitee.com/ohos-dev/f-oh/raw/master/screenshot/wx+zfb.png)

## License

```txt
Copyright (C) 2023 Jesse205

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.
```

[F-OH-Data]: http://170.178.208.105:3000/ohos-dev/F-OH-Data
[PWASecureContextRequirement]: https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable#secure_context
[SponsorList]: https://gitee.com/ohos-dev/f-oh#%E8%B5%9E%E5%8A%A9%E5%88%97%E8%A1%A8
[@westinyang]: https://gitee.com/westinyang

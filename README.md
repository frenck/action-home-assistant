# 🚀 Frenck's Github Action: Home Assistant Core Configuration Check

[![GitHub Release][releases-shield]][releases]
![Project Stage][project-stage-shield]
![Project Maintenance][maintenance-shield]
[![License][license-shield]](LICENSE.md)

[![Sponsor Frenck via GitHub Sponsors][github-sponsors-shield]][github-sponsors]

🚀 Frenck's GitHub Action for running a Home Assistant Core configuration check.

## About

This GitHub action runs a Home Assistant Core configuration check against your
repository.

Please note; that this Action is useable for all Home Assistant installation
types, and thus **NOT** limited to Home Assistant Core users. It also works
if you are running Home Assistant Container, Supervised or OS.

## Usage

```yaml
name: Check
on: [push, pull_request]
jobs:
  home-assistant:
    name: Home Assistant Core Configuration Check
    runs-on: ubuntu-latest
    steps:
      - name: ⤵️ Check out configuration from GitHub
        uses: actions/checkout@v2
      - name: 🚀 Run Home Assistant Configuration Check
        uses: frenck/action-home-assistant@v1
        with:
          path: "./config"
          secrets: fakesecrets.yaml
          version: stable
```

## Arguments

|   Input   |                             Description                              |   Usage    |
| :-------: | :------------------------------------------------------------------: | :--------: |
|  `path`   | Path to the folder containing the Home Assistant Core configuration. | _Optional_ |
| `secrets` |      Alternative secrets file to use, e.g., "fakesecrets.yaml".      | _Optional_ |
| `version` |    Version to use; dev/beta/stable or a specific version number.     | _Optional_ |

### Specific configuration folder

By default, this GitHub Action will use the root folder as the Home Assistant
Core configuration folder. If you store your Home Assistant configuration in a
subfolder, the `path` argument can be used to inform the Action about that.

For example, if you configuration is in the `config` folder:

```yaml
- name: 🚀 Run Home Assistant Core Configuration Check
  uses: frenck/action-home-assistant@v1
  with:
    path: "./config"
```

### Using a fake secrets file

Of course, you don't want to commit your secrets file. However, without a
secrets file, your configuration check will most likely not pass.

This GitHub Action offers a way around that, but using a fake secrets file.

To use this, add a fake secrets file to your repository (e.g.,
`fakesecrets.yaml`) and make sure the content is the same as your real
`secrets.yaml` (with, of course, fake credentials/data). The GitHub Action
will use this file during checking your configuration.

For example, if you fake secrets file is `fakesecrets.yaml`:

```yaml
- name: 🚀 Run Home Assistant Core Configuration Check
  uses: frenck/action-home-assistant@v1
  with:
    secrets: "fakesecrets.yaml"
```

### Running against a specific version

This GitHub Action allows you to specify a specific version to run
your Home Assistant Core configuration against. However, by default, the
integration will try to find the `.HA_VERSION` file in your configuration
folder.

If the `.HA_VERSION` file is found, the version in that file is used. If
the `.HA_VERSION` file is not found; the Action will use the latest stable
version of Home Assistant to test your configuration with.

However, you can specify/override any version you like to check against,
for example, check with Home Assistant Core `2021.1.0`:

```yaml
- name: 🚀 Run Home Assistant Core Configuration Check
  uses: frenck/action-home-assistant@v1
  with:
    version: "2021.1.0"
```

Alternatively, you can also use `stable`, `beta` or `dev` to run against
latest versions of those stability channels.

```yaml
- name: 🚀 Run Home Assistant Core Configuration Check
  uses: frenck/action-home-assistant@v1
  with:
    version: beta
```

## More extensive example

A more extensive example, that runs your configuration against the latest
development, beta and stable versions:

```yaml
name: Check
on: [push, pull_request]
jobs:
  home-assistant:
    name: "Home Assistant Core ${{ matrix.version }} Configuration Check"
    runs-on: ubuntu-latest
    strategy:
      matrix:
        version: ["stable", "beta", "dev"]
    steps:
      - name: ⤵️ Check out configuration from GitHub
        uses: actions/checkout@v2
      - name: 🚀 Run Home Assistant Configuration Check
        uses: frenck/action-home-assistant@v1
        with:
          path: "./config"
          secrets: fakesecrets.yaml
          version: "${{ matrix.version }}"
```

## Real-world examples

The following repositories are using this GitHub Action, and thus provide
you with some real-world uses of this GitHub Action.

- [Frenck's Home Assistant Configuration](https://github.com/frenck/home-assistant-config)

Are you using this GitHub Action? Feel free to open up a PR to add your
configuration to this list 😍

## Changelog & Releases

This repository keeps a change log using [GitHub's releases][releases]
functionality.

Releases are based on [Semantic Versioning][semver], and use the format
of `MAJOR.MINOR.PATCH`. In a nutshell, the version will be incremented
based on the following:

- `MAJOR`: Incompatible or major changes.
- `MINOR`: Backwards-compatible new features and enhancements.
- `PATCH`: Backwards-compatible bugfixes and package updates.

## Contributing

This is an active open-source project. We are always open to people who want to
use the code or contribute to it.

We've set up a separate document for our
[contribution guidelines](CONTRIBUTING.md).

Thank you for being involved! :heart_eyes:

## Authors & contributors

The original setup of this repository is by [Franck Nijhof][frenck].

For a full list of all authors and contributors,
check [the contributor's page][contributors].

## License

MIT License

Copyright (c) 2021 Franck Nijhof

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

[contributors]: https://github.com/frenck/action-home-assistant/graphs/contributors
[frenck]: https://github.com/frenck
[github-sponsors-shield]: https://frenck.dev/wp-content/uploads/2019/12/github_sponsor.png
[github-sponsors]: https://github.com/sponsors/frenck
[license-shield]: https://img.shields.io/github/license/frenck/action-home-assistant.svg
[maintenance-shield]: https://img.shields.io/maintenance/yes/2021.svg
[project-stage-shield]: https://img.shields.io/badge/project%20stage-production%20ready-brightgreen.svg
[releases-shield]: https://img.shields.io/github/release/frenck/action-home-assistant.svg
[releases]: https://github.com/frenck/action-home-assistant/releases
[semver]: http://semver.org/spec/v2.0.0.html

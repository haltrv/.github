# Special Interest Group for Home Assistant on RISC-V

## General steps for building

Not all of the following dependency order must be followed in sequence. It is possible to skip up to whichever repository fork you are interested in and then any previous dependencies for it will refer to the published haltrv build.

Assumed that you will have:
1. GitHub personal or organization account referred to as _myusername_
2. [RISE RISC-V runners (personal)](https://github.com/apps/rise-risc-v-runners-personal) or [RISE RISC-V runners (organization)](https://github.com/apps/rise-risc-v-runners) installed as appropriate to _myusername_ GitHub Apps settings
3. SSH rsync access `wheelsuser`@`wheelshost`:`/home/wheelsuser/public_html` to a TLS-enabled (e.g. Lets Encrypt) IPv4 (GitHub does not have any IPv6 support) webhost referred to as _https://${wheelshost}/~${wheelsuser}_ with ~1GB storage

### Dependency order:
* #### cosign-installer
  - [Fork the upstream repository sigstore/cosign-installer](https://github.com/sigstore/cosign-installer/fork) to your GitHub account
  - Apply patches to a clone of your fork of upstream repository and push new branch:
    ```
    # clone your GitHub fork of upstream repository with your ssh URL:
    git clone git@github.com:myusername/cosign-installer.git cosign-installer.git
    git -C cosign-installer.git remote add haltrv https://github.com/haltrv/cosign-installer.git
    git -C cosign-installer.git fetch --all
    git -C cosign-installer.git checkout -b mybranch --track origin/HEAD
    git -C cosign-installer.git cherry-pick haltrv/HEAD..haltrv/next
    git -C cosign-installer.git rebase -i origin/HEAD  # (optional) review patches to pick, edit, or drop
    git -C cosign-installer.git push origin mybranch
    ```
  - Disable workflows in actions of your GitHub repository fork
    <br/>(_cosign-installer repository : Settings : General : Code and automation : Actions : General : Actions permissions : **Disable actions**_)
  - Publish a new release having selected option to create from the push branch name target a new unique tag name on publish
* #### tempio
  - [Fork the upstream repository home-assistant/tempio](https://github.com/home-assistant/tempio/fork) to your GitHub account
  - Apply patches to a clone of your fork of upstream repository and push new branch:
    ```
    # clone your GitHub fork of upstream repository with your ssh URL:
    git clone git@github.com:myusername/tempio.git tempio.git
    git -C tempio.git remote add haltrv https://github.com/haltrv/tempio.git
    git -C tempio.git fetch --all
    git -C tempio.git checkout -b mybranch --track origin/HEAD
    git -C tempio.git cherry-pick haltrv/HEAD..haltrv/next
    git -C tempio.git rebase -i origin/HEAD  # (optional) review patches to pick, edit, or drop
    git -C tempio.git push origin mybranch
    ```
  - Enable workflows in actions of your GitHub repository fork with default runners
    <br/>(_tempio repository : Settings : General : Code and automation : Actions : General : Actions permissions : **Allow all actions and reusable workflows**_)
  - Trigger the build in GitHub actions by publishing a new release having selected option to create from the push branch name target a new unique tag name on publish
* #### builder
  - [Fork the upstream repository home-assistant/builder](https://github.com/home-assistant/builder/fork) to your GitHub account
  - Apply patches to a clone of your fork of upstream repository and push new branch:
    ```
    # clone your GitHub fork of upstream repository with your ssh URL:
    git clone git@github.com:myusername/builder.git builder.git
    git -C builder.git remote add haltrv https://github.com/haltrv/builder.git
    git -C builder.git fetch --all
    git -C builder.git checkout -b mybranch --track origin/HEAD
    git -C builder.git cherry-pick haltrv/HEAD..haltrv/next
    git -C builder.git rebase -i origin/HEAD  # (optional) review patches to pick, edit, or drop
    git -C builder.git push origin mybranch
    ```
  - Disable workflows in actions of your GitHub repository fork
    <br/>(_builder repository : Settings : General : Code and automation : Actions : General : Actions permissions : **Disable actions**_)
  - Publish a new release having selected option to create from the push branch name target a new unique tag name on publish
* #### docker-base
  - [Fork the upstream repository home-assistant/docker-base](https://github.com/home-assistant/docker-base/fork) to your GitHub account
  - Apply patches to a clone of your fork of upstream repository and push new branch:
    ```
    # clone your GitHub fork of upstream repository with your ssh URL:
    git clone git@github.com:myusername/docker-base.git docker-base.git
    git -C docker-base.git remote add haltrv https://github.com/haltrv/docker-base.git
    git -C docker-base.git fetch --all
    git -C docker-base.git checkout -b mybranch --track origin/HEAD
    git -C docker-base.git cherry-pick haltrv/HEAD..haltrv/next
    git -C docker-base.git rebase -i origin/HEAD  # (optional) review patches to pick, edit, or drop
    git -C docker-base.git push origin mybranch
    ```

  - Enable workflows in actions of your GitHub repository fork with additional RISE RISC-V runners
    <br/>(_docker-base repository : Settings : General : Code and automation : Actions : General : Actions permissions : **Allow all actions and reusable workflows**_)
  - Trigger the build in GitHub actions by publishing a new release having selected option to create from the push branch name target a new unique tag name on publish

* #### wheels
* #### docker
* #### go2rtc
* #### core

# mySilverblue &nbsp; [![bluebuild build badge](https://github.com/arturasb/mysilverblue/actions/workflows/build.yml/badge.svg)](https://github.com/arturasb/mysilverblue/actions/workflows/build.yml)

The purpose of this custom OS image is to provide relevant settings and apps for personalized OS setup:

- Install needed Flatpak apps to `--user` space.
- Fix `libvirt` and `virt-manager` so it works in the Fedora Atomic Destop-based setup.
> Easier way to get VMs is the GNOME Boxes, but the problem is that flatpak version of Boxes does not suport USB forwarding, e.g., for headset forward to VM.
- Enable `systemd-homed` managed users for better privacy and data security. This requires specific SELinux policies which are not yet available to Fedora Atomic Desktops. There are more tweaks necessary to get `homed` working. There is a good [workaround](https://discussion.fedoraproject.org/t/building-a-new-home-with-systemd-homed-on-fedora/72690) to this situation. This OS build tries to provide `systemd-homed` OOTB.

See the [BlueBuild docs](https://blue-build.org/how-to/setup/) for quick setup instructions for setting up your own repository based on this template.

After setup, it is recommended you update this README to describe your custom image.

## Installation

> **Warning**  
> [This is an experimental feature](https://www.fedoraproject.org/wiki/Changes/OstreeNativeContainerStable), try at your own discretion.

To rebase an existing atomic Fedora installation to the latest build:

- First rebase to the unsigned image, to get the proper signing keys and policies installed:
  ```
  rpm-ostree rebase ostree-unverified-registry:ghcr.io/arturasb/my-silverblue:latest
  ```
- Reboot to complete the rebase:
  ```
  systemctl reboot
  ```
- Then rebase to the signed image, like so:
  ```
  rpm-ostree rebase ostree-image-signed:docker://ghcr.io/arturasb/my-silverblue:latest
  ```
- Reboot again to complete the installation
  ```
  systemctl reboot
  ```

The `latest` tag will automatically point to the latest build. That build will still always use the Fedora version specified in `recipe.yml`, so you won't get accidentally updated to the next major version.

## ISO

If build on Fedora Atomic, you can generate an offline ISO with the instructions available [here](https://blue-build.org/learn/universal-blue/#fresh-install-from-an-iso). These ISOs cannot unfortunately be distributed on GitHub for free due to large sizes, so for public projects something else has to be used for hosting.

## Verification

These images are signed with [Sigstore](https://www.sigstore.dev/)'s [cosign](https://github.com/sigstore/cosign). You can verify the signature by downloading the `cosign.pub` file from this repo and running the following command:

```bash
cosign verify --key cosign.pub ghcr.io/arturasb/mysilverblue
```

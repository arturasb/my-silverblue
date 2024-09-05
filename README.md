# my-silverblue &nbsp; [![bluebuild build badge](https://github.com/arturasb/mysilverblue/actions/workflows/build.yml/badge.svg)](https://github.com/arturasb/mysilverblue/actions/workflows/build.yml)

The purpose of this custom OS image is to provide relevant settings and apps for personalized OS setup. Customizations are necessary to make `libvirt`, `virt-manager` and `systemd-homed` functional OOTB.
Current goals are:

1. Add `systemd-homed` managed user home for better portability and security.
	1. Install needed Flatpak apps to `--user` space in oder to contain apps with settings in user's `home`.
	2. Setting proper SELinux contexts to `systemd-homed` is required as a workaround. Also setting `authselect` with `systemd-homed`. There is a good [instruction](https://discussion.fedoraproject.org/t/building-a-new-home-with-systemd-homed-on-fedora/72690) to this situation.
2. Fix membership of the `libvirt` group, `swtpm`(for Windows 11 VMs), `libvirt`, so `virt-manager` works in the Fedora Atomic Destop-based setup.
	1. Setting proper SELinux contexts to `swtpm`, creating `swtpm-localca` in `/var/lib` and setting proper ownership of the `/var/lib/swtpm-localca` are required as a workaround.
 	2. Adding members of `wheel` to `libvirt` group.
	> Easier way to get VMs is the GNOME Boxes, but the problem is that flatpak version of Boxes does not suport USB forwarding, e.g., for headset forward to VM.

Some of fixes&workarounds were inspired by/copied from the [bluefin](https://github.com/ublue-os/bluefin/pkgs/container/bluefin) project.

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

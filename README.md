This script should be created per-product and provide basic product prerequisities.

There is two main scripts called at the sanitize stage: `install` and `configure`.

## Runtime environment

The product scripts are executed in ArchLinux environment. These are the constraints,
which they should take into account:

* Used to provision both development and integration environments
* Minimal dependencies and self-contained

## Install

The script adds custom dependencies needed for the product. The `tools` repository
is already configured at this point. The script is executed in its own union mount
point `base+install`. Modifications are applied to the `install` layer.

Note that sanitation does not clean this layer, so the `install` script should
only apply needed changes. Hence doing irreversible changes is not allowed.

## Configure

This script may irreversibly update the environment by e.g. patching `/etc/pacman.conf`
and `/etc/makepkg.conf`. The script receives own mount point to run on: `base+install+config`.
Changes are applied to the `config` layer and sanitize script always resets it, so the
`configure` script may assume a known state.

The configure stage hence should be effective and avoid doing massive modifications to the
environment.

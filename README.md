# nginx dynamic module tooling

This set of tools is used to create installable packages of dynamic modules
for nginx and can be used following the instructions at
<https://www.nginx.com/blog/creating-installable-packages-dynamic-modules/>.

This repository is manually mirrored from <https://hg.nginx.org/pkg-oss> using
<https://github.com/schacon/hg-git> and has been extended in a variety of
ways. The mirror will be updated periodically, but is done manually so YMMV
at any given point in time.

Note that toolchain changes only affect the RPM build; Debian is not supported
here.

## Changes from upstream

* Added this README.
* `build_module.sh` clones this `pkg-oss` repository using `git` rather than
  `hg` from the original nginx repository.
* Switch to using branches in `build_module.sh` so we can introduce new fixes;
  previously, tagged releases were used which means no further could be
  introduced to that version's scripts.

### Bugs fixed

* Tag version `1.12.1-1` as this is missing from the original repo.
  (reported at <https://trac.nginx.org/nginx/ticket/1334>)
* Change URLs to be `https://`. Ensures packages and more get downloaded over
  a secure connection.  Note only the script-used URLs are changed;
  documentation and package descriptions are left alone for ease of merging.
  (reported at <https://trac.nginx.org/nginx/ticket/1335>)
* Improve scriptability by auto-accepting build dependency installation.
  (reported at <https://trac.nginx.org/nginx/ticket/1336>)
* Handle `dist` being specified as `.el7.centos` in EL7, but nginx releases
  packages as with dist as `.el7` (see
  <https://bugs.centos.org/view.php?id=7416>).  This breaks the `Requires:`
  dependency for dynamic modules as the core `nginx` package doesn't have this
  same `dist` specified.

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
* Change URLs to be `https://`. Ensures packages and more get downloaded over
  a secure connection.  Note only the script-used URLs are changed;
  documentation and package descriptions are left alone for ease of merging.
  (reported at <https://trac.nginx.org/nginx/ticket/1335>)

## Management

### Creating the mirror

1. Install hg-git via https://github.com/schacon/hg-git#installing

2. Clone the repo and create the Git counterpart:

   ```bash
   hg clone http://hg.nginx.org/pkg-oss
   mkdir nginx-pkg-oss && cd nginx-pkg-oss
   git init && cd ..
   ```

3. Push from hg to Git and checkout:

   ```bash
   cd pkg-oss
   hg bookmarks hg
   hg push ../nginx-pkg-oss
   cd ../nginx-pkg-oss
   git checkout -b master hg
   ```

4. Push to GitHub:

   ```bash
   git remote add origin git@github.com:jcu-eresearch/nginx-pkg-oss.git
   git push -u origin master
   ```

## Keeping this up to date

1. Pull from hg and push to git; this process will update the `hg` bookmark we
   set up earlier (which becomes the `hg` branch in Git):

   ```bash
   cd pkg-oss
   hg pull && hg update
   hg push ../nginx-pkg-oss
   ```

2. Rebase in git:

   ```bash
   cd ../nginx-pkg-oss
   git rebase -i hg
   # Fix anything that needs adjustment
   git push
   ```

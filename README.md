# Blackbox Buildpack

This is a buildpack for decrypting arbitrary files with [blackbox](https://github.com/StackExchange/blackbox) before running other heroku buildpacks. It allows you to place sensitive credentials under version control in encrypted format, then allow heroku to decrypt them at deploy time.

# Authorship

* Copyright @aguestuser, 2018
* Made for use by https://affinity.works
* LICENSE: AGPLv3

# Usage

- configure your `app.json` file such that this runs before your app is built, then all config files encrypted with blackbox will be decrypted before compile time
- for instructions on configuring the order of buildpacks, see: https://devcenter.heroku.com/articles/app-json-schema#buildpacks
- note that we read the private key from an env var named `$AFFINITY_HEROKU_PRIVATE_KEY`. you will likely want to rename that env var to something that makes sense for your org
- if you want to tinker, almost all of the action happens in `bin/compile`
- for docs on Heroku's buildpack API, see: https://devcenter.heroku.com/articles/buildpack-api

# Dependencies

this buildpack makes a lot of assumptions about dependencies, but tries to provide useful errors if assumptions don't hold. for examaple, it:

- assumes you have a pgp private key stored in a config variable (w/ `\n` chars at end of lines)
- assumes that private key has a subkey with no password (as outlined here: https://github.com/StackExchange/blackboxset-up-automated-users-or-role-accounts)
- assumes you have placed `blackbox` bash scripts in the `/bin` dir of your build directory (provides workaround if not)
- assumes you have the commands `gpg2`, `gpg-agent`, and `killall` available on your image
  - this will require the `gnupg2`, `gnupg-agent`, and `psmisc` packages, respectively
   - this will (likely) require `heroku-buildpack-apt` to support installing deb packages
   - for docs on the buildpack see: https://elements.heroku.com/buildpacks/heroku/heroku-buildpack-apt)
   - for example Aptfile with `gnupg2`, `gnupg-agent`, and `psmisc` packages for ubuntu xenial see: https://raw.githubusercontent.com/affinityworks/main/d067a33d4adffa0f87b994e53237d45217ab2f6a/Aptfile


#  Notes

* We are able to decrypt files without a password for the private key, because we created a subkey without a password (as per blackbox instructions)

* The naming reflect that
* If there is general interest in the buildpack, we can rename things

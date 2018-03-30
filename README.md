# Blackbox Buildpack

This is a buildpack for decrypting arbitrary files with [blackbox](https://github.com/StackExchange/blackbox) before running other heroku buildpacks. It allows you to place sensitive credentials under version control in encrypted format, then allow heroku to decrypt them at deploy time.

# Authorship

* Copyright @aguestuser, 2018
* LICENSE: AGPLv3
* Made for use by https://affinity.works

# Usage

- configure your `app.json` file such that this runs before your app is built
  - you will likely also need to configure `app.json` such that the `heroku-buildpack-apt` runs first
  - for instructions on configuring the order of buildpacks, see: https://devcenter.heroku.com/articles/app-json-schema#buildpacks
- THEN: all config files encrypted locally with blackbox will be decrypted on heroku before your app is compiled on each deploy (yay!)

# Dependencies

This buildpack makes a lot of assumptions about dependencies, but tries to provide useful errors if assumptions don't hold.

For examaple, it:

- assumes you have placed `blackbox` bash scripts in the `/bin` dir of your build directory (provides workaround if not)
- assumes you have a pgp private key stored in a config variable on heroku that:
  - has a subkey with no password (as outlined here: https://github.com/StackExchange/blackboxset-up-automated-users-or-role-accounts)
  - is named `$PGP_PRIVATE_KEY_HEROKU`
  - uses `\n` chars instead of linebreaks
  - (optionally) uses `\s` characters instead of spaces (b/c this format is easier to upload via the `heroku config` cli command)
- assumes you have the commands `gpg2`, `gpg-agent`, and `killall` available on your image
  - this will require the `gnupg2`, `gnupg-agent`, and `psmisc` packages, respectively
   - this will (likely) require `heroku-buildpack-apt` to support installing deb packages
   - for docs on the buildpack see: https://elements.heroku.com/buildpacks/heroku/heroku-buildpack-apt)
   - for example Aptfile with `gnupg2`, `gnupg-agent`, and `psmisc` packages for ubuntu xenial see: https://raw.githubusercontent.com/affinityworks/main/d067a33d4adffa0f87b994e53237d45217ab2f6a/Aptfile

# Hacking

If you want to tinker, almost all of the action happens in `bin/compile`. Poke around!

For docs on Heroku's buildpack API, see: https://devcenter.heroku.com/articles/buildpack-api

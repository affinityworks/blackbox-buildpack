# Blackbox Buildpack

This is a buildpack for decrypting arbitrary files with [blackbox](https://github.com/StackExchange/blackbox) before running other heroku buildpacks.

It:

1. Imports a private pgp key from the env var `$AFFINITY_HEROKU_PRIVATE_KEY`
2. Installs blackbox
3. Uses blackbox to decrypt all encrypted files
4. Allows the build to proceed

# Notes

* We are able to decrypt files without a password for the private key, because we created a subkey without a password (as per blackbox instructions)
* This was made for use by https://affinity.works
* The naming reflect that
* If there is general interest in the buildpack, we can rename things

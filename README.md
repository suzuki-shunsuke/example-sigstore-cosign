# example-sigstore-cosign

Example to sign and verify artifacts with [Sigstore](https://sigstore.dev/)'s [Cosign](https://docs.sigstore.dev/cosign/overview)

**:warning: This repository is still under development.**

## What's this?

In this repository I show how to sign artifacts and verify them by Cosign.
The purpose of this repository is to spread [Sigstore](https://sigstore.dev/) and [Cosign](https://docs.sigstore.dev/cosign/overview).
Sigstore is an awesome project, but unfortunately there are few OSS adopting it yet.
To make software supply chain secure, I'd like to spread Sigstore.

## :warning: Note

- Note that I'm not members of Sigstore. I'm just a user
- This repository focuses on not Container Signing but Signing standard files

## Install Cosign

https://docs.sigstore.dev/cosign/installation

You can also install Cosign by [aqua](https://aquaproj.github.io/).

```sh
git checkout https://github.com/suzuki-shunsuke/example-sigstore-cosign
cd example-sigstore-cosign
aqua i -l
cosign version
```

Or

```sh
aqua init
aqua g -i sigstore/cosign
aqua i
```

## Sign checksum files by Cosign in GitHub Actions

Cosign supports [Keyless Signing](https://docs.sigstore.dev/cosign/sign/#keyless-signing),
so you don't have to manage private keys and passphrases.

You can sign checksum files in GitHub Actions by the following command.

```sh
export COSIGN_EXPERIMENTAL=1
cosign sign-blob \
  --output-certificate=checksums.txt.pem \
  --output-signature checksums.txt.sig \
  --oidc-provider github \
  checksums.txt
```

## Verify checksum files by Cosign

You can verify a checksum file by Cosign.

```sh
COSIGN_EXPERIMENTAL=1 cosign verify-blob \
  --signature https://github.com/suzuki-shunsuke/example-sigstore-cosign/releases/download/v0.1.0-5/checksums.txt.sig \
  --certificate https://github.com/suzuki-shunsuke/example-sigstore-cosign/releases/download/v0.1.0-5/checksums.txt.pem \
  --certificate-github-workflow-repository suzuki-shunsuke/example-sigstore-cosign \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  checksums.txt
```

```sh
sha256sum --ignore-missing -c checksums.txt
```

## Advanced: Separate GitHub Actions jobs for least privilege

## Sign checksum files by GoReleaser


## LICENSE

[MIT](LICENSE)

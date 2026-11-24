# INACTIVE: This repository is no longer maintained

---

# CSR with Google Cloud KMS

## Overview

### What it is

- A Go tool that creates a certificate signing request (CSR) signed with a private key held in Google Cloud KMS or an HSM.
- The private key never leaves Google.

### How it relates to other repos

- This is a fork of [mattes/google-cloud-kms-csr](https://github.com/mattes/google-cloud-kms-csr).
- The org changed one thing (FED-901): the CSR is now signed with RSA SHA-256 instead of ECDSA SHA-256, to match our KMS key.
- The `synapse` repo mentions the upstream tool in `apps/node/dataset/README.md`. It is a step for making the dataset service certificate on GCP (KMS key, CA, CSR, issue the certificate). That doc links to the upstream repo, not this fork.

### How to run or deploy

- Run `go mod vendor`, then `go build -o csr`, then `./csr -key <key-resource-id> -out my.csr --common-name <name>`.
- The key must be asymmetric. It is not maintained. Use this only as a reference.

### Current status

- Inactive and no longer used.

---

Quick utility tool that creates a CSR cert and signs it with a private key coming from Google Cloud KMS or HSM.
The private key never leaves Google, everyone is happy. The CSR can then be used to get cert from CA.

I would've done it with `openssl`, but there is no Google Cloud KMS engine available. (Sept. 2018)

## Usage

```
go mod vendor
go build -o csr
./csr -key <key-resource-id> -out my.csr --common-name MyOrg
```

Get the key-resource-id by running the following command:

```
gcloud kms keys versions list  --key <keyname> --keyring <keyring-name> --location=<region>
```

Key Resource Id Version has the following format:

```
projects/xxx/locations/xxx/keyRings/xxx/cryptoKeys/xxx/cryptoKeyVersions/xxx
```

Make sure to use an asymmetric key.

You can verify `my.csr` with:

```
openssl req -text -noout -verify -in my.csr
```

Google's application credentials are used for authenticating with the Google API.
If you haven't done so already, you can set the application default credentials locally with:

```
gcloud auth application-default login
```

## Docs

- https://cloud.google.com/kms/docs/how-tos
- https://en.wikipedia.org/wiki/Certificate_signing_request

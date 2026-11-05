# Security policy

## Public data only

This repository may contain only public metadata, signed channel pointers, and installation-bound encrypted lease envelopes.

Do not commit:

- product or instance signing private keys;
- client signing or lease-decryption private keys;
- GitHub tokens, credentials, cookies, or SSH keys;
- passive-authorization request files;
- raw hardware serial numbers, MAC addresses, disk identifiers, or inventory data;
- plaintext lease payloads;
- active-service secrets, database dumps, or deployment configuration.

The schemas in this repository describe formats but do not grant trust. Clients must use their embedded product trust root and local installation identity for verification.

## Reporting a vulnerability

Do not post secrets or device data in a public issue. Use GitHub private vulnerability reporting or a private security advisory when available. Otherwise contact the repository owner privately before publishing details.

If sensitive material is committed, consider it compromised. Revoke or rotate it immediately, remove it from reachable history, and issue replacement trust material where applicable.

# S3 Desktop passive authorizations

This public repository is the transport channel for independently issued, encrypted S3 Desktop VIN V3 leases.

It is deliberately independent from the active authorization service, its database, and any deployment host. A client reads published artifacts anonymously through GitHub Raw. Lease confidentiality comes from per-installation HPKE encryption; authenticity and authorization come from the product signing root, not from repository trust.

## Raw endpoint

Base URL:

https://raw.githubusercontent.com/foobarrer/owner-s3desktop-authorizations/main/leases/v3/

The client derives a private-looking, non-secret channel ID from its permanent installation identity, then requests:

leases/v3/{channel-id-first-2}/{channel-id-first-4}/{channel-id}/head.json

The signed head points to an immutable encrypted lease object:

leases/v3/objects/{sha256-first-2}/{sha256}.lease

The channel ID is not the installation ID and cannot be used as a lease on another installation. The encrypted lease is bound to the destination installation identity and its X25519 lease-encryption public key. Another client cannot decrypt or validate it as its own lease.

## Publication flow

1. The client exports a signed passive-authorization request through an out-of-band channel.
2. A standalone offline Rust publisher validates the request and issues an installation-bound lease with the shared product signing root.
3. The publisher writes the immutable encrypted object and a signed channel head.
4. The administrator commits the immutable object first and the corresponding head last.
5. The client downloads both files anonymously, verifies hashes and signatures, decrypts the envelope, and applies anti-rollback rules before accepting the lease.

GitHub access does not require a client token. A missing head, including HTTP 404, means only “no publication is currently available”. It does not revoke an already cached valid lease. Suspension, revocation, or retirement must be represented by a newer, validly signed terminal lease with a strictly higher revision.

## Repository layout

- repository-policy.json: public transport policy metadata. It is descriptive and is not a trust root.
- schemas/: JSON Schemas for publisher input and public artifacts.
- leases/v3/: generated publication tree.
- leases/v3/objects/: immutable encrypted lease envelopes.

## Security boundaries

Never commit any signing private key, client private key, GitHub credential, raw hardware identifier, passive request, plaintext lease, active-service secret, or database export.

This repository is public by design. Repository write access is an operational control, but it is not sufficient to forge an accepted lease: clients accept only correctly signed, correctly bound, non-rollback artifacts.

Generated heads and objects should not be edited manually. Use the standalone publisher once it is available.

## Current status

The public layout, policy, validation schemas, and standalone offline publisher have been tested end to end through GitHub Raw. The repository currently contains one synthetic test lease under the manual-github-e2e instance. It is signed with a non-production test key, is bound to a synthetic client identity, and is not trusted by release clients. Production publication still requires a real client-signed request and an explicit administrator decision.

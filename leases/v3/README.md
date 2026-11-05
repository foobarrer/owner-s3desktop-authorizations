# Generated VIN V3 lease publications

This directory is a generated publication tree. Do not place client requests, keys, or plaintext leases here.

## Channel heads

Path:

{channel-id-first-2}/{channel-id-first-4}/{channel-id}/head.json

A head is signed by the product signing root. It identifies one immutable lease object, its exact byte length and SHA-256 digest, the issuing instance, and a monotonically increasing per-channel revision.

Only a valid higher revision may replace an accepted head. A lower or conflicting revision must be rejected as rollback or equivocation.

## Immutable objects

Path:

objects/{sha256-first-2}/{sha256}.lease

Objects are HPKE-encrypted envelopes bound to one installation. Once published, an object must never be modified or reused for different bytes. Repository history may retain expired encrypted objects; their presence does not make them valid after lease expiry.

## Publication order

Publish and verify the immutable object first. Publish the signed head last. This prevents a client from observing a pointer to an object that is not yet available.

HTTP 404 for a head means no publication is available for that channel. It is not a revocation signal. Revocation and suspension require a newer signed terminal lease.

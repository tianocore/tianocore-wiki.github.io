# RFC: Remove Firmware Signing Services from CryptoPkg

## Metadata

- **RFC Number**: TBD
- **Title**: Remove Firmware Signing Services from CryptoPkg
- **Status**: Draft

## Change Log

- 2026-10-03: Initial RFC created.
- 2026-10-04: Require Windows and Linux support for the offline vector helper.

## Motivation

CryptoPkg exposes asymmetric signing APIs that require access to private keys. Keeping general-purpose
signers in firmware libraries and protocols increases the code and key-handling surface that platforms
must review, configure, and maintain.

Removing the firmware signing implementations would leave less code to build, test, audit, and maintain.
It could also reduce firmware image size when the removed code and its dependencies are no longer linked;
the savings would depend on the library instances and platform configuration. Removing general-purpose
private-key signing paths from firmware could narrow the security-sensitive code and key-handling surface,
potentially reducing opportunities for defects and the security work required to support them. The
Protocol/PPI callback fields would remain as fail-closed ABI placeholders, so their layout would not shrink.

The audited edk2 trees show limited or conditional need for these services. The direct BaseCryptLib
calls outside CryptoPkg are LibSPDM callbacks; local SPDM signing is conditional in the inspected
platform flow. TLS can be configured with a local host key, but the inspected HTTP/TLS setup provisions
CA certificates only. These findings do not rule out downstream users.

This proposal would remove signing functions from library-class interfaces and implementations while
preserving Crypto Protocol/PPI layout. Retained callback slots would be deprecated and fail closed.
Verifier tests would call verification APIs only and consume fixed signatures checked into test sources.
Maintainers would generate new or changed vectors offline from test-only fixtures; normal tests would
not sign. BaseTools host-side signing would remain unchanged.

## Technology Background

CryptoPkg exposes cryptographic services through library classes such as `BaseCryptLib` and `TlsLib`,
and through the EDKII Crypto Protocol/PPI. The PPI aliases the protocol structure, whose driver forwards
callbacks to the selected library implementation. Signature verification remains necessary for Secure
Boot and other trust decisions; this proposal removes generation services, not verification.

The signing surface in scope is:

- `Pkcs7Sign`
- `RsaPkcs1Sign`
- `RsaPssSign` and `RsaPssSignDigest`
- `EcDsaSign`
- `EdDsaSign`
- `MlDsaSign`
- `SlhDsaSign` in the Crypto Protocol/PPI

HMAC and other MAC operations, signature verification, certificate parsing, key agreement, key
generation, TLS key configuration and handshake behavior, and build-host signing are out of scope.
BaseTools `Pkcs7Sign` signs host artifacts and capsule payloads; it is separate from CryptoPkg's
firmware API.

## Goals

1. Remove general-purpose asymmetric signature generation from CryptoPkg library-class interfaces.
2. Prevent Crypto Protocol/PPI signing callbacks from generating signatures without changing their ABI.
3. Preserve signature verification and host-side build signing.
4. Keep verifier tests independent of firmware signing APIs and runtime signature generation.
5. Explain compatibility and migration requirements to platform consumers.

## Requirements

1. Remove declarations, implementations, null implementations, and forwarding wrappers for the affected
   library-class signing functions.
2. Preserve Crypto Protocol/PPI member order and layout. Retained signing callbacks must be deprecated
   and fail closed (`FALSE` for Boolean callbacks or `EFI_UNSUPPORTED` for status callbacks).
3. Leave TLS key setter/getter APIs, credentials, and handshake behavior unchanged.
4. Preserve signature-verification APIs and behavior.
5. Remove tests that generate signatures through the affected firmware services; retain positive and
   negative verifier coverage using fixed signed inputs.
6. Preserve BaseTools signing utilities and existing build workflows.
7. Provide a maintainer-only offline vector generator under `CryptoPkg/Test/Tools`.
8. Require an explicit algorithm profile and support ECDSA, EdDSA, ML-DSA, attached PKCS#7, and SLH-DSA.
9. Update only the selected profile's arrays in an existing vector header; never emit private keys.
10. Provide standard-library Python unit tests and profile-specific maintainer instructions.
11. Require the offline helper to support Windows and Linux hosts, including empty-message vector
    generation through the host's OpenSSL `libcrypto` library.

## UEFI/PI Specification Impact

The proposal would not modify UEFI or PI specification behavior and would not require a specification
change. It would change edk2 APIs and behavior. Crypto Protocol/PPI callback slots would remain for
layout compatibility, but signing through those slots would become unsupported.

## Backward Compatibility

This proposal is non-backwards-compatible:

- **Source and link compatibility**: Removing declarations and symbols would break consumers that call
  or link against the affected functions.
- **Behavioral compatibility**: Retained Protocol/PPI callbacks would remain callable but return
  failure. TLS key configuration and handshake behavior would be unchanged.
- **Protocol/PPI layout**: Keeping callback fields at their current offsets would preserve layout, not
  signing behavior.
- **Migration**: Sign artifacts before firmware execution or use a separately reviewed, platform-owned
  signer where runtime signing is a platform requirement. Continue using BaseTools for host signing.

Firmware that depends on local SPDM signing or runtime signature generation for attestation,
provisioning, challenge-response, or platform-specific protocols would need a migration. Secure Boot,
capsule authentication, authenticated-variable verification, TLS peer verification, and host-side
artifact signing are not proposed to change.

## Platform/Package Impact

- **CryptoPkg**: Remove library signing APIs and implementations; retain fail-closed Protocol/PPI slots.
  Verifier tests would use checked-in vectors.
- **SecurityPkg**: LibSPDM callbacks that use CryptoPkg signing would need to report unsupported or use a
  platform-owned implementation.
- **NetworkPkg**: No change to TLS host-key configuration or handshake behavior is proposed.
- **edk2-platforms**: No direct CryptoPkg signing calls were found in the audited tree; downstream users
  may still exist.
- **BaseTools**: Host-side PKCS#7 signing and platform build options would remain unchanged.

## Unresolved Questions

- Should Protocol/PPI signing callbacks remain indefinitely as unsupported ABI placeholders?
- What deprecation and stable-tag window should apply before library declarations and symbols are removed?
- Should platform-owned firmware signing remain an explicitly supported extension point?

## Prior Art/Related Work

The EDK II breaking-change policy defines communication and migration expectations for API removals.
This RFC should be coordinated with that process and the applicable `BREAKING-CHANGES.md` entry. No
prior TianoCore proposal to remove CryptoPkg signing APIs was identified in the audit.

## Alternatives

### Alternative 1: Keep all signing APIs

- **Pros**: No compatibility impact for current consumers.
- **Cons**: Retains general-purpose private-key signing paths and their maintenance burden.
- **Why not chosen**: The proposal aims to reduce firmware signing surface while preserving verification
  and host-side signing.

### Alternative 2: Deprecate library APIs but keep them functional

- **Pros**: Provides a transition period without immediate behavior changes.
- **Cons**: Leaves signing capability and key-handling code in firmware.
- **Why not chosen**: The proposal removes library signing functions rather than retaining working
  implementations.

### Alternative 3: Remove Protocol/PPI callback fields

- **Pros**: Removes the obsolete callback surface completely.
- **Cons**: Changes structure layout and can break consumers.
- **Why not chosen**: Retain the fields as deprecated, unsupported callbacks for layout compatibility.

## Implementation Design

### Architecture Overview

The proposal would remove library-class signing methods and make retained Protocol/PPI signing callbacks
fail closed without changing their offsets. Verification would continue through existing APIs. Test
signatures would be produced offline from test-only fixtures, checked into vector headers, and consumed
by verification-only unit tests.

### Detailed Design

#### Usage Audit

The audited edk2 trees show limited or conditional firmware use:

- **PKCS#7**: No firmware caller was found. `RsaPkcs7Tests.c` and related tests verify fixed inputs;
  BaseTools signs host artifacts and capsules.
- **RSA PKCS#1 v1.5**: LibSPDM has conditional local/requester signing callbacks. `RsaTests.c` retains
  verifier coverage; the audited Intel platform does not establish an active signing flow.
- **RSA-PSS**: No caller outside CryptoPkg was found. `RsaPssTests.c` retains verifier coverage.
- **ECDSA**: LibSPDM has conditional signing callbacks. `EcTests.c` and certificate tests cover
  verification.
- **EdDSA and ML-DSA**: `EdDsaTests.c` and `MlDsaTests.c` cover fixed message and context signatures.
- **SLH-DSA**: The Protocol/PPI signing callback fails closed. Tests would use precomputed vectors.
- **TLS**: Key configuration and handshake behavior are outside scope.

#### Library Classes and Implementations

The proposal would remove these BaseCryptLib signing functions and their declarations,
implementations, null implementations, and forwarding wrappers:

- `Pkcs7Sign`
- `RsaPkcs1Sign`
- `RsaPssSign` and `RsaPssSignDigest`
- `EcDsaSign`
- `EdDsaSign`
- `MlDsaSign`

The LibSPDM adapter would no longer call the removed methods. It would report signing as unavailable;
platforms requiring local signing would need a separately reviewed platform implementation or another
flow. TLS key APIs, credentials, and handshake behavior would remain unchanged.

#### Proposed Offline Vector Generator

The proposal would provide `CryptoPkg/Test/Tools/GenerateBaseCryptLibTestSignatures.py` with one
profile per vector-producing algorithm:

- `ecdsa`: P-256 SHA-256 vector for `EcTests.c` in `VerifyTestSignatures.h`.
- `eddsa`: Ed448 message and context vectors for `EdDsaTests.c` in `VerifyTestSignatures.h`.
- `mldsa`: ML-DSA-87 message, context, empty-message, maximum-context, and multiple-message vectors for
  `MlDsaTests.c` in `VerifyTestSignatures.h`.
- `pkcs7`: Attached content and partial-chain vectors for `RsaPkcs7Tests.c` in
  `VerifyTestSignatures.h`.
- `slh-dsa`: Message, context, empty-message, maximum-context, and second-message vectors for
  `SlhDsaTests.c` in `SlhDsaTestVectors.h`.

The proposed tool would require Python 3.10 or newer and an OpenSSL command-line executable on both
Windows and Linux. OpenSSL 3.5 or newer would be required for the ML-DSA and SLH-DSA profiles; the
EdDSA context vector would require support for the `hexcontext-string` option. `libcrypto` would
additionally be required for ML-DSA and SLH-DSA empty-message vectors. On Windows, the matching
versioned `libcrypto` DLL would need to be beside the selected executable. On Linux, the helper would
prefer the matching `libcrypto.so.<major>` in the executable prefix's `lib` or `lib64` directory, then
use the system shared-library lookup only when its major version matches. All keys would be test-only
fixtures.

The proposed command-line options are:

- `--algorithm`: Required; `ecdsa`, `eddsa`, `mldsa`, `pkcs7`, or `slh-dsa`.
- `--openssl`: OpenSSL executable path; defaults to `openssl` from `PATH`.
- `--output`: Optional existing header; defaults to the selected profile's vector header.
- `--key`: Optional SLH-DSA PEM key; rejected for other profiles. Without it, use the fixture in
  `SlhDsaTestVectors.h`.

Each profile would read its fixtures and define its vector cases. Shared utilities would run OpenSSL,
manage temporary files, convert ECDSA DER signatures, format C arrays, and replace only selected
arrays and declaration lengths. Empty-message ML-DSA and SLH-DSA vectors would use OpenSSL EVP through
`libcrypto`, because `pkeyutl` cannot sign a zero-byte input. Generation would be manual, offline, and
separate from firmware and ordinary unit-test execution.

The proposal would include full usage documentation at `CryptoPkg/Test/Tools/README.md` and
per-test regeneration instructions at
`CryptoPkg/Test/UnitTest/Library/BaseCryptLib/README.md`. Standard-library tests would be placed in
`CryptoPkg/Test/Tools/test_generate_base_crypt_lib_test_signatures.py`. They would cover help execution,
argument validation, profile registration, fixture parsing, DER conversion, header updates, and binary
context handling with a mocked OpenSSL runner.

#### Planned Proof of Concept

A proof of concept (POC) is planned for completion at TBD. It should demonstrate all five profiles,
selected-header updates, the Python unit tests, and the offline-only boundary. POC profile runs should
write to temporary header copies; checked-in vectors should not be rewritten.

### Code Examples

A maintainer would select one profile explicitly, for example:

```bash
python CryptoPkg/Test/Tools/GenerateBaseCryptLibTestSignatures.py --algorithm mldsa \
  --openssl /path/to/openssl
```

## Testing Strategy

1. Build affected BaseCryptLib variants and Protocol/PPI implementations; confirm removed symbols are
   absent and callback fields remain initialized.
2. Test retained signing callbacks for fail-closed behavior and unchanged caller buffers.
3. Run the applicable OpenSSL and MbedTLS verifier suites, including RSA, RSA-PSS, ECDSA, EdDSA,
   ML-DSA, SLH-DSA, PKCS#7, Secure Boot, and capsule verification.
4. Run the proposed generator unit tests from the `edk2` root on both Windows and Linux:

   ```text
   python -m unittest discover -s CryptoPkg/Test/Tools -p "test_*.py" -v
   ```

   The suite must exercise the helper's `--help` command successfully.
5. For the POC, generate each profile into a temporary copy of its header on both Windows and Linux,
  using OpenSSL 3.5 or newer, and verify that only the selected arrays change.
6. Search representative platforms for removed symbols and run BaseTools capsule-signing tests.

## Migration/Adoption Plan

1. **Review and POC**: Complete the planned POC at TBD, consult edk2 and edk2-platforms consumers, and
   inventory downstream uses of affected APIs.
2. **Deprecation and migration**: Deprecate Protocol/PPI signing callbacks and guide consumers toward
   host-side signing or a platform-owned signer where required.
3. **Removal**: Remove library declarations, implementations, wrappers, and signer tests on the agreed
   breaking-change timeline. Retain unsupported Protocol/PPI fields and verifier tests with checked-in
   vectors.
4. **Validation**: Confirm verification and host-side signing remain functional; update integration
   documentation and breaking-change records.

The stable-tag timeline should follow the EDK II breaking-change process. Mitigations include a
repository-wide symbol audit, companion platform updates, and tests of unaffected verification and
build-tool flows.

## Guide-Level Explanation

### For Package Developers

CryptoPkg would continue to provide signature verification, but its library classes would no longer
expose signature-generation functions. Remove calls to those APIs and do not add dependencies on
Protocol/PPI signing callbacks. Verifier tests would use checked-in signatures generated offline, never
firmware signing APIs or runtime signing.

### For Platform Developers

Sign firmware, capsules, and other artifacts on the build host, then verify them in firmware. A platform
that requires runtime signing would need a reviewed platform-owned alternative before adopting this
proposal.

### For End Users (if applicable)

Secure Boot, capsule authentication, and other verification behavior are not intended to change. A
platform that depends on firmware-generated signatures would require an update.

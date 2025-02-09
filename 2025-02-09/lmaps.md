# Comprehensive Summary of ML-KEM Private Key Format Discussion

---

## Proposed Formats & Technical Debates
1. **Seed-Only**:
   - **Pros**: Compact (64 bytes), aligns with ML-KEM’s design, ideal for cloud/IoT.
   - **Cons**: Requires regeneration; incompatible with HSMs lacking seed APIs.
   - **Argument**: "Bit-flip concerns are better addressed with ECC, not format changes."

2. **ExpandedKey-Only**:
   - **Pros**: Directly usable, avoids regeneration errors, FIPS-compliant.
   - **Cons**: Larger (1568 bytes), no recovery from corruption.
   - **Warning**: "ExpandedKey-only risks regression to least-common-denominator support."

3. **CHOICE (Seed | ExpandedKey)**:
   - **Pros**: Flexibility for implementations. Tagged `[0]` for seed to avoid ambiguity.
   - **Cons**: Risk of split ecosystems (e.g., HSM1 uses seed, HSM2 uses expandedKey).
   - **Compromise**: "Successful standards make everyone equally unhappy."

4. **Both (Seed + ExpandedKey)**:
   - **Pros**: Enables cross-validation (e.g., regenerate expandedKey from seed).
   - **Cons**: Doubles storage/bandwidth. DJB: "Critical for fault detection in unreliable hardware."

---

## Critical Arguments
- **Interoperability**:
  - ExpandedKey ensures compatibility with legacy HSMs and FIPS devices (e.g., DoD smartcards).
  - Seed-only risks fragmentation if hardware lacks regeneration APIs.
- **Security**:
  - **Seed**: Allows regeneration to detect storage corruption (e.g., bit-flips).
  - **ExpandedKey**: Avoids runtime regeneration risks (side-channel leaks).
  - **Warning**: "Assume hardware fails—seed+key enables error detection."
- **FIPS Compliance**:
  - Some FIPS modules cannot process seeds, requiring expandedKey for compliance.
- **Bandwidth vs. Storage**:
  - Cloud providers ) favored seed for large-scale deployments but conceded expandedKey for HSMs.

---

## OID Strategy
- **New OIDs**: Required for seed/expandedKey to avoid conflicts with NIST’s ML-KEM OIDs.
- **Concern**: "Naive implementations might misparse keys if OIDs aren’t distinct."
- **IANA Actions**: Three OID registrations requested for S/MIME attributes (TBD1-TBD3).

---

## Validation & Implementation Guidance
1. **Mandatory Checks**:
   - Cross-validate seed and expandedKey if both present.
   - Public key hash check to ensure consistency.
2. **Statistical Tests**:
   - Viktor proposed χ² tests on `s` coefficients to detect malformed keys.
   - Sophie: "Unit tests only—statistical checks are unsafe for external keys."
3. **Error Handling**:
   - PCT (Pairwise Consistency Test) required in FIPS mode.
   -  "Assume hardware enforces policy (e.g., CAC/PIV cards block key exports)."

---

## Remaining Concerns
1. **Hardware Limitations**:
   - IoT devices often lack reliable RNGs, favoring server-generated keys.
   - HSMs may discard seeds during export, forcing expandedKey use.
2. **Undefined Behavior**:
   - If implementations ignore validation, mismatched seed/expandedKey could cause silent failures.
3. **Competing Standards**:
   - "Bandwidth demands might spawn parallel specs (e.g., compressed seeds)."

---

## Consensus & Next Steps
- **Adopted Format**:
  ```asn.1
  ML-KEM-PrivateKey ::= CHOICE {
      seed        [0] OCTET STRING (SIZE(64)),
      expandedKey     OCTET STRING (SIZE(1568))
  }
  ```
- Actions:

  - Finalize OID assignments (TBD1-TBD3) with IANA.

  - Add security considerations for bit-flips, validation checks, and error correction.

  - Clarify ASN.1 tagging to avoid parsing ambiguities.

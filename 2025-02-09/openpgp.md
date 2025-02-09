# Summary of OpenPGP & IETF Draft Discussions

---

## Key Proposals & Discussions

### 1. **DNS-Based Secure Authentication System (Phill Hallambaker)**
- **Objective**: Leverage DNS + DNSSEC for end-to-end encrypted social media and web interactions.
- **Mechanism**:
  - Store signed contact assertions in DNS TXT records (e.g., `_mesh.phill.hallambaker.com`).
  - Use **Mesh** for root-of-trust/service address binding.
  - Public contact assertions include OpenPGP/SSH keys, email hashes, and protocol preferences.
- **Use Cases**:
  - Secure email (OpenPGP over SMTP), git commit signing, code verification.
  - Avoid direct OpenPGP key storage in DNS due to usability/spam concerns.

### 2. **OpenPGP Key Replacement Draft (`draft-ietf-openpgp-replacementkey-03`)**
- **Purpose**: Define methods to replace expired/revoked OpenPGP primary keys.
- **Updates**:
  - Clarifications added (no wire format changes).
  - Open merge request for format changes (to be discussed at interim meeting).
- **Next Steps**: Present at IETF 122; address pending feedback.

### 3. **GREASE Codepoints for OpenPGP (`draft-gallagher-openpgp-grease-00`)**
- **Proposal**: Reserve codepoints in OpenPGP registries for interoperability testing.
- **Inspiration**: Analogous to TLS GREASE (Generate Random Extensions And Sustain Extensibility).
- **Goal**: Improve client/server robustness against non-standard implementations.

### 4. **NIST Backward Compatibility Draft (`draft-ehlen-openpgp-nist-bp-comp`)**
- **Status**: Dependent on `draft-ietf-openpgp-pqc` stability. Proposed for discussion at IETF 122.

---

## Key Technical Debates

- **DNS vs. Direct Key Storage**:
  - **Pros of DNS**: Scalable trust via DNSSEC, avoids key distribution complexity.
  - **Cons**: Maintenance burden for users, spam risks.
- **Key Replacement**:
  - Need for backward-compatible wire format changes vs. simplicity.
- **GREASE Applicability**:
  - Debate on whether OpenPGP needs GREASE-like testing frameworks.

---

## Next Steps & Action Items
1. **IETF 122 Agenda**:
   - Present `draft-ietf-openpgp-replacementkey` and `draft-ehlen-openpgp-nist-bp-comp`.
   - Discuss GREASE codepoints and persistent symmetric keys.
2. **Draft Finalization**:
   - Address open MRs for wire format changes in `replacementkey`.
   - Publish updated `draft-gallagher-openpgp-grease` with test vectors.
3. **Community Feedback**:
   - Solicit input on DNS-based trust models and OpenPGP key usability.


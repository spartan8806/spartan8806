# Security Researcher — `spartan8806`

---

## Published Security Advisories

| Advisory | CVE | Severity | Class | Project |
|---|---|---|---|---|
| [GHSA-79wm-x847-7cvg](https://github.com/davila7/claude-code-templates/security/advisories/GHSA-79wm-x847-7cvg) | **CVE-2026-73222** | **High · 8.8** | Unauthenticated OS command injection → RCE | claude-code-templates |
| [GHSA-6c66-jp8x-q8w8](https://github.com/marcusquinn/aidevops/security/advisories/GHSA-6c66-jp8x-q8w8) | — | **High · 7.6** | Unauthenticated `0.0.0.0` bind + fail-open auth → SSRF proxy / process spawn | aidevops |
| [GHSA-qq8c-fch4-cxq7](https://github.com/us/crw/security/advisories/GHSA-qq8c-fch4-cxq7) | — | **High · 7.3** | Broken access control — admin/metrics routes bypass API-key auth + permissive CORS | crw (fastCRW) |
| [GHSA-4cfr-w3v5-w5j5](https://github.com/Dicklesworthstone/destructive_command_guard/security/advisories/GHSA-4cfr-w3v5-w5j5) | — | **High · 7.1** | Algorithmic-complexity fail-open guard bypass | destructive_command_guard |
| [GHSA-cff8-4h3c-9r4q](https://github.com/avo-hq/avo/security/advisories/GHSA-cff8-4h3c-9r4q) | — | **High · 8.5** | Cross-resource IDOR — `MediaLibraryController` exposes every `ActiveStorage::Blob` to any authenticated user | avo |
| [GHSA-25rm-9wvm-m38v](https://github.com/aszepieniec/falcon-rust/security/advisories/GHSA-25rm-9wvm-m38v) | **CVE-2026-77382** | **Medium · 5.9** | Post-quantum cryptography — discrete-Gaussian sampler precision below Falcon's security threshold | falcon-rust |
| [GHSA-2697-fm9m-mqvw](https://github.com/NationalSecurityAgency/ghidra/security/advisories/GHSA-2697-fm9m-mqvw) | — | **Medium · 5.5** | Infinite loop (uncontrolled resource consumption) in the PEF loader — a crafted PEF header hangs Ghidra on import | **Ghidra** (NSA) |

<sub>CVE IDs are issued by GitHub after a compliance review of the published advisory, on their own schedule, and the lag is measured in weeks rather than days: CVE-2026-73222 was assigned four weeks after publication, and CVE-2026-77382 five. The remaining advisories are in that queue; the Ghidra advisory is not, as the Ghidra Team's policy states they are not authorized to generate CVEs. Further reports are in coordinated disclosure and will be listed here once published.</sub>

## Research Tooling

[**falcon-sampler-kat**](https://github.com/spartan8806/falcon-sampler-kat) — known-answer test vectors that detect a Falcon implementation whose `exp()` approximation is under-provisioned, the defect class behind the falcon-rust advisory above.

**The official Falcon KATs do not catch this class.** A sampler running at ~2⁻³³ precision instead of the reference's ~2⁻⁵⁰ still reproduces every published test vector, because the defect only flips a rejection decision about 2⁻³³ of the time, so catching it end-to-end would take ~2³³ samples. These vectors test the arithmetic directly, at the point where the error is deterministic rather than probabilistic, and are built by construction rather than by search.

Validated in both directions, which is the part that matters for a test suite:

| implementation | result |
|---|---|
| falcon-rust v0.3.0 (post-fix) | 20/20 agree |
| falcon-rust v0.1.3 (the GHSA-25rm defect) | **10/20 caught the defect** |
| `pornin/rust-fn-dsa` v0.4.0, `flr_native` and `flr_emu` | 20/20 agree, `z` bit-exact |

The vectors were [merged into falcon-rust](https://github.com/aszepieniec/falcon-rust/pull/15) at the maintainer's invitation.

## Upstream Security Fixes

Defects found, reported, and fixed upstream, credited by maintainers via commit, PR, or issue. The reporting channel differs from the advisories above; the work does not.

| Project | Finding | Outcome |
|---|---|---|
| **[falcon-rust](https://github.com/aszepieniec/falcon-rust)**<br><sub>Falcon / FN-DSA, NIST PQC signatures</sub> | Beyond the advisory above: Gaussian acceptance-probability fix ([`930d766`](https://github.com/aszepieniec/falcon-rust/commit/930d7662)), then a follow-up after re-checking my own fix against the spec and finding the sampler centre still short of Falcon's precision bound ([`fc60813`](https://github.com/aszepieniec/falcon-rust/commit/fc608133)). Later, an out-of-bounds index in `ber_exp`: the loop ran eight shifts against a seven-byte buffer, reachable whenever the first seven bytes tie with `z`, about 2⁻⁵⁶ from the sampler's own RNG, which is why it had never been observed | Four commits merged with me as **git author**, including the [OOB fix](https://github.com/aszepieniec/falcon-rust/commit/9f79bfb) and the [precision KAT vectors](https://github.com/aszepieniec/falcon-rust/pull/15) contributed at the maintainer's invitation. Listed among the repo's [contributors](https://github.com/aszepieniec/falcon-rust/graphs/contributors) |
| **[PyJWT](https://github.com/jpadilla/pyjwt)**<br><sub>JWT library</sub> | `HMACAlgorithm.prepare_key` accepted empty HMAC keys with only a warning | Fixed in **2.13.0**, which [credits the report in the release commit](https://github.com/jpadilla/pyjwt/commit/95791b1759b8aa4f2203575d344d5c78564cdc81). That release has since propagated into the dependency history of **90+ downstream repositories**, including Microsoft, Sentry, Apache, PyTorch, Canonical, Bazel and Red Hat projects |
| **[redis-py](https://github.com/redis/redis-py)**<br><sub>official Redis client</sub> | Plaintext password disclosure via `ConnectionPool.__repr__` (CWE-532) | Fixed ([#3993](https://github.com/redis/redis-py/issues/3993)) |
| **[leancrypto](https://github.com/smuellerDD/leancrypto)**<br><sub>PQC for bare-metal and Linux kernel</sub> | ASN.1 memory leaks on error paths across six key-parsing modules (ML-DSA, ML-DSA+Ed25519/Ed448, Ed25519, Ed448, SLH-DSA) ([`6581568`](https://github.com/smuellerDD/leancrypto/commit/6581568a992b7e5e1799323ed4714fe99e1b19a6)); `pathLenConstraint` not enforced per RFC 5280 §4.2.1.9, so a chain could exceed an intermediate CA's authorised depth ([`12cc2aea`](https://github.com/smuellerDD/leancrypto/commit/12cc2aeafdb257972ee3421e6761df8ac4cf947c)) | Both fixed upstream, both crediting me in the commit trailer. Path-length fix shipped with a new test-certificate matrix |
| **[wigolo](https://github.com/KnockOutEZ/wigolo)**<br><sub>local-first web-fetch MCP for AI agents</sub> | SSRF fetch path vulnerable to DNS-rebinding / TOCTOU | Hardened via fetch-time address resolution; [fix merged](https://github.com/KnockOutEZ/wigolo/pull/210) |
| **[libE57Format](https://github.com/asmaloney/libE57Format)**<br><sub>ASTM E57 point clouds — surveying, BIM, robotics</sub> | Out-of-bounds read in `BufferView::read`: `CheckedFile` requests a full physical page regardless of bytes remaining, so a short in-memory buffer reads past its allocation. Reproduced under AddressSanitizer, patched build verified to throw instead | [Fix merged as PR #352](https://github.com/asmaloney/libE57Format/pull/352) ([`0e1d480`](https://github.com/asmaloney/libE57Format/commit/0e1d48064e)); the maintainer added a [regression test](https://github.com/asmaloney/libE57Format/pull/353) adapted from my reproducer |
| **[tomlkit](https://github.com/python-poetry/tomlkit)**<br><sub>TOML parser behind Poetry</sub> | Uncontrolled-recursion DoS in `parser.py` (CWE-674) | Fixed ([#459](https://github.com/python-poetry/tomlkit/issues/459)) |
| **[whois](https://github.com/richardpenman/whois)**<br><sub>Python WHOIS client</sub> | SSRF via referral following, plus a follow-up hardening pass | Both merged ([#319](https://github.com/richardpenman/whois/pull/319), [#321](https://github.com/richardpenman/whois/pull/321)) |
| **[enc_rust](https://github.com/supinie/enc_rust)**<br><sub>ML-KEM</sub> | Decapsulation missing Fujisaki–Okamoto implicit rejection | Fix authored and merged ([`572a37f`](https://github.com/supinie/enc_rust/commit/572a37f395e3c3bcf60d89bea89885b4dc13c176)) |
| **[libpqc-dyber](https://github.com/dyber-pqc/libpqc-dyber)**<br><sub>post-quantum crypto</sub> | FN-DSA / Falcon `BerExp` sampler precision below the ~2⁻⁴⁰ Rényi-divergence floor the security proof requires ([`b012e00`](https://github.com/dyber-pqc/libpqc-dyber/commit/b012e00cb3d9f236a1ba22c6f3288c5484eb4eef)); then heap overflows in signing, XMSS one-time-key reuse, LMS signatures not verifying, and KEM oracles ([`ab8614b`](https://github.com/dyber-pqc/libpqc-dyber/commit/ab8614b945530fbc1ced9539242d72c939166764)); an intermittent XMSS verification failure and FN-DSA conversion UB ([`4eb96fb`](https://github.com/dyber-pqc/libpqc-dyber/commit/4eb96fb2)); and signature-buffer bounds, which was swept across **every registered algorithm** rather than patched in one place ([`5b5a6fd`](https://github.com/dyber-pqc/libpqc-dyber/commit/5b5a6fd6)) | Produced the project's **[0.2.0 security release](https://github.com/dyber-pqc/libpqc-dyber/commit/172312ec689cffa349663c4489e071ecf5418f1c)**. **15 commits** carry `Reported-by: Conner (Spartan8806)` |
| **[hermes-agent](https://github.com/NousResearch/hermes-agent)**<br><sub>Nous Research agent framework</sub> | Auto-approve edit gate bypassable by a multi-file patch touching a sensitive path | Fix + regression tests [submitted](https://github.com/NousResearch/hermes-agent/pull/63438), in review |

<sub>On calibration: I withdrew one libpqc-dyber finding of my own after re-testing it at runtime rather than from the packing arithmetic, and on libE57Format the maintainer assessed the issue as a plain bug rather than a security one, which I agreed with.</sub>

## Reverse Engineering & Platform Research

- **Android / Pixel** — vulnerability research submitted to Google's Android & Devices VRP (Buganizer): firmware reverse engineering, kernel/driver, and device trust-boundary work.
- **Windows** — local privilege escalation and kernel/driver research (MSRC; ZDI pipeline).
- **Focus areas** — memory corruption, TOCTOU / logic flaws, cryptanalysis, firmware RE.

---

<sub>Disclosure done cleanly: reported privately where a channel exists, fixes contributed upstream, no public 0-day drops. Additional reports are in coordinated disclosure and are not listed until a maintainer publishes.</sub>

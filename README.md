# Security Researcher — `spartan8806`

Vulnerability research and reverse engineering across open-source developer tooling, Windows, and Android. I turn real defects into **fixed, credited disclosures** — reported through proper channels, patches pushed upstream.

📫 **conner.webber000@gmail.com**

---

## Published Security Advisories

| Advisory | Severity | Class | Project |
|---|---|---|---|
| [GHSA-79wm-x847-7cvg](https://github.com/davila7/claude-code-templates/security/advisories/GHSA-79wm-x847-7cvg) | **High · 8.8** | Unauthenticated OS command injection → RCE | claude-code-templates |
| [GHSA-6c66-jp8x-q8w8](https://github.com/marcusquinn/aidevops/security/advisories/GHSA-6c66-jp8x-q8w8) | **High · 7.6** | Unauthenticated `0.0.0.0` bind + fail-open auth → SSRF proxy / process spawn | aidevops |
| [GHSA-qq8c-fch4-cxq7](https://github.com/us/crw/security/advisories/GHSA-qq8c-fch4-cxq7) | **High · 7.3** | Broken access control — admin/metrics routes bypass API-key auth + permissive CORS | crw (fastCRW) |
| [GHSA-4cfr-w3v5-w5j5](https://github.com/Dicklesworthstone/destructive_command_guard/security/advisories/GHSA-4cfr-w3v5-w5j5) | **High · 7.1** | Algorithmic-complexity fail-open guard bypass | destructive_command_guard |
| [GHSA-25rm-9wvm-m38v](https://github.com/aszepieniec/falcon-rust/security/advisories/GHSA-25rm-9wvm-m38v) | **Medium · 5.9** | Post-quantum cryptography — discrete-Gaussian sampler precision below Falcon's security threshold | falcon-rust |

<sub>No CVE IDs assigned — GitHub issues these when a maintainer requests one for a repository advisory. Further advisories are in coordinated disclosure and will be listed here once published.</sub>

## Upstream Security Fixes

Defects found, reported, and fixed upstream — credited by maintainers via commit, PR, or issue. The reporting channel differs from the advisories above; the work does not.

- **libpqc-dyber** (Dyber, post-quantum cryptography) — an ongoing engagement that produced the project's **[0.2.0 security release](https://github.com/dyber-pqc/libpqc-dyber/commit/172312ec689cffa349663c4489e071ecf5418f1c)**. Reported an FN-DSA / Falcon `BerExp` sampler precision violation (the `exp(-x)` approximation fell below the ~2⁻⁴⁰ Rényi-divergence floor the security proof requires) ([`b012e00`](https://github.com/dyber-pqc/libpqc-dyber/commit/b012e00cb3d9f236a1ba22c6f3288c5484eb4eef)), then a second round across the remaining algorithm families — **heap overflows in signing, XMSS one-time-key reuse, LMS signatures not actually verifying, and KEM oracles** ([`ab8614b`](https://github.com/dyber-pqc/libpqc-dyber/commit/ab8614b945530fbc1ced9539242d72c939166764)). **15 commits** in the project's history carry `Reported-by: Conner (Spartan8806)`. I also withdrew one finding of my own after re-testing it at runtime rather than from the packing arithmetic.
- **falcon-rust** — beyond the advisory above, I authored both sampler fixes and am listed among the repo's [contributors](https://github.com/aszepieniec/falcon-rust/graphs/contributors): the Gaussian acceptance-probability fix ([`930d766`](https://github.com/aszepieniec/falcon-rust/commit/930d7662)), and a follow-up after re-checking the first fix against the specification and finding the sampler center still short of Falcon's precision bound — moving `ffSampling` to `FixedPoint128` ([`fc60813`](https://github.com/aszepieniec/falcon-rust/commit/fc608133)).
- **PyJWT** — flagged that `HMACAlgorithm.prepare_key` accepted empty HMAC keys with only a warning. Fixed in **2.13.0**, which [credits the report in the release commit](https://github.com/jpadilla/pyjwt/commit/95791b1759b8aa4f2203575d344d5c78564cdc81): *"Hardening prompted by reports from @SnailSploit and @spartan8806."* That release has since propagated into the dependency history of **90+ downstream repositories**, including Microsoft, Sentry, Apache, PyTorch, Canonical, Bazel and Red Hat projects.
- **wigolo** (KnockOutEZ — local-first web-fetch MCP for AI agents) — hardened an SSRF fetch path against DNS-rebinding / TOCTOU via fetch-time address resolution; fix merged ([PR #210](https://github.com/KnockOutEZ/wigolo/pull/210)).
- **enc_rust** — ML-KEM decapsulation was missing Fujisaki–Okamoto implicit rejection; fix authored and merged ([`572a37f`](https://github.com/supinie/enc_rust/commit/572a37f395e3c3bcf60d89bea89885b4dc13c176)).
- **whois** (richardpenman) — SSRF referral filtering and a follow-up hardening fix, both merged ([#319](https://github.com/richardpenman/whois/pull/319), [#321](https://github.com/richardpenman/whois/pull/321)).
- **hermes-agent** (Nous Research) — hardened the auto-approve edit gate against a multi-file patch bypass; fix + regression tests submitted ([PR #63438](https://github.com/NousResearch/hermes-agent/pull/63438)).
- **redis-py** — plaintext password disclosure via `ConnectionPool.__repr__` (CWE-532); fixed ([#3993](https://github.com/redis/redis-py/issues/3993)).
- **tomlkit** — uncontrolled-recursion DoS in `parser.py` (CWE-674); fixed ([#459](https://github.com/python-poetry/tomlkit/issues/459)).

## Reverse Engineering & Platform Research

- **Android / Pixel** — vulnerability research submitted to Google's Android & Devices VRP (Buganizer): firmware reverse engineering, kernel/driver, and device trust-boundary work.
- **Windows** — local privilege escalation and kernel/driver research (MSRC; ZDI pipeline).
- **Focus areas** — memory corruption, TOCTOU / logic flaws, cryptanalysis, firmware RE.

---

<sub>Disclosure done cleanly: reported privately where a channel exists, fixes contributed upstream, no public 0-day drops. Additional reports are in coordinated disclosure and are not listed until a maintainer publishes.</sub>

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

<sub>CVE IDs pending assignment. Additional advisories are in coordinated disclosure and will be listed here once published.</sub>

## Upstream Security Fixes

Defects found, reported, and fixed upstream — credited by maintainers via commit, PR, or issue. The reporting channel differs from the advisories above; the work does not.

- **libpqc-dyber** (Dyber, post-quantum crypto) — reported an FN-DSA / Falcon `BerExp` sampler precision violation (the `exp(-x)` approximation fell below the ~2⁻⁴⁰ Rényi-divergence floor the security proof requires), plus constant-time hardening; fixed and credited as reporter ([commit `b012e00`](https://github.com/dyber-pqc/libpqc-dyber/commit/b012e00)).
- **wigolo** (KnockOutEZ — local-first web-fetch MCP for AI agents) — hardened an SSRF fetch path against DNS-rebinding / TOCTOU via fetch-time address resolution; fix merged ([PR #210](https://github.com/KnockOutEZ/wigolo/pull/210)).
- **hermes-agent** (Nous Research) — hardened the auto-approve edit gate against a multi-file patch bypass; fix + regression tests submitted ([PR #63438](https://github.com/NousResearch/hermes-agent/pull/63438)).
- **redis-py** — plaintext password disclosure via `ConnectionPool.__repr__` (CWE-532); fixed ([#3993](https://github.com/redis/redis-py/issues/3993)).
- **tomlkit** — uncontrolled-recursion DoS in `parser.py` (CWE-674); fixed ([#459](https://github.com/python-poetry/tomlkit/issues/459)).

## Reverse Engineering & Platform Research

- **Android / Pixel** — vulnerability research submitted to Google's Android & Devices VRP (Buganizer): firmware reverse engineering, kernel/driver, and device trust-boundary work.
- **Windows** — local privilege escalation and kernel/driver research (MSRC; ZDI pipeline).
- **Focus areas** — memory corruption, TOCTOU / logic flaws, cryptanalysis, firmware RE.

---

<sub>Disclosure done cleanly: reported privately where a channel exists, fixes contributed upstream, no public 0-day drops.</sub>

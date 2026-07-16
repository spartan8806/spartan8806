# Security Researcher — `spartan8806`

Vulnerability research and reverse engineering across open-source developer tooling, Windows, and Android. I turn real defects into **fixed, credited disclosures** — reported through proper channels, patches pushed upstream.

📫 **conner.webber000@gmail.com**

---

## Published Security Advisories

| Advisory | Severity | Class | Project |
|---|---|---|---|
| [GHSA-79wm-x847-7cvg](https://github.com/davila7/claude-code-templates/security/advisories/GHSA-79wm-x847-7cvg) | **High · 8.8** | Unauthenticated OS command injection → RCE | claude-code-templates |
| [GHSA-6c66-jp8x-q8w8](https://github.com/marcusquinn/aidevops/security/advisories/GHSA-6c66-jp8x-q8w8) | **High · 7.6** | Unauthenticated `0.0.0.0` bind + fail-open auth → SSRF proxy / process spawn | aidevops |
| [GHSA-4cfr-w3v5-w5j5](https://github.com/Dicklesworthstone/destructive_command_guard/security/advisories/GHSA-4cfr-w3v5-w5j5) | **High · 7.1** | Algorithmic-complexity fail-open guard bypass | destructive_command_guard |
| [GHSA-25rm-9wvm-m38v](https://github.com/aszepieniec/falcon-rust/security/advisories/GHSA-25rm-9wvm-m38v) | **Medium · 5.9** | Post-quantum cryptography — discrete-Gaussian sampler precision below Falcon's security threshold | falcon-rust |

<sub>CVE IDs pending assignment.</sub>

## Accepted — Pending Publication

Reported and **credit accepted**; advisories in the maintainers' publication pipeline.

| Project | Severity | Class |
|---|---|---|
| eigent | **Critical · 9.6** | Unauthenticated drive-by RCE — agent workforce runs with no auth under wildcard CORS |
| dimOS (dimensionalOS) | High · 8.8 | Unauthenticated agent-skill execution via wildcard CORS (drive-by / RCE) |
| avo | High · 8.8 | Media Library broken access control (IDOR) |
| Label Studio | High · 8.3 | Cross-organization IDOR — read/modify/delete across orgs |
| MLflow | High · 7.6 | Broken access control — Job-API RBAC bypass (CWE-862) |
| opencode | High · 7.0 | Command-approval bypass — `git -c` gadget executes arbitrary commands |
| Ghidra | Medium · 5.5 | PEF loader infinite-loop DoS on crafted binary import |

## Upstream Security Contributions

- **hermes-agent** (Nous Research) — hardened the auto-approve edit gate against a multi-file patch bypass; fix + regression tests submitted ([PR #63438](https://github.com/NousResearch/hermes-agent/pull/63438)).

## Reverse Engineering & Platform Research

- **Android / Pixel** — vulnerability research submitted to Google's Android & Devices VRP (Buganizer): firmware reverse engineering, kernel/driver, and device trust-boundary work.
- **Windows** — local privilege escalation and kernel/driver research (MSRC; ZDI pipeline).
- **Focus areas** — memory corruption, TOCTOU / logic flaws, cryptanalysis, firmware RE.

---

<sub>Disclosure done cleanly: reported privately where a channel exists, fixes contributed upstream, no public 0-day drops.</sub>

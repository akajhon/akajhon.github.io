---
title: "Mikedor: Cross-Platform Go RAT Targeting Brazilian Financial Infrastructure"
classes: wide
header:
  # teaser: /assets/images/Mikedor_01/mikedor_banner.jpg
ribbon: crimson
description: "Intelligence report and malware analysis of Mikedor — a cross-platform Go RAT targeting Brazil's Pix payment system, with EV code-signing abuse, HMAC-SHA256 C2 authentication, and TLS fingerprint evasion."
categories:
  - Threat Intelligence
  - Malware Analysis
tags:
  - RAT
  - Go
  - Brazil
  - Pix
  - TrojanProxy
  - Financial
  - Linux
  - Windows
toc: true
---

**Publication Date:** May 2026  
**TLP:** TLP:CLEAR  
**Threat Level:** 🔴 Critical  
**Malware Family:** Mikedor / mrmike / TrojanProxy  
**Platforms:** Windows x64, Windows ARM64, Linux ELF x64

Samples:
```
959dca4b7989546a18a3f5e016c4bd78cfd825a1e679cefe0a355e739605937f  # w-x86.exe (Windows x64)
6210caacd4c7a3219ad6327b714c53d286443104ba06e3c4270f7e9a5d25ecee  # w-arm64.exe (Windows ARM64)
b8de00249df3b152e5b28c21735a6acaa649b7098d0e4195b86fd29577367da5  # Linux ELF
```

---

# BLUF

dentified and disrupted an active campaign — tracked as **Mikedor** — targeting the Brazilian financial sector before any offensive action was recorded. The implant is a cross-platform Go RAT compiled for Windows x64, Windows ARM64, and Linux ELF x64, configured to tunnel into Pix-adjacent infrastructure via a reverse SOCKS5 proxy. The operator demonstrated advanced operational security: a valid Sectigo EV code-signing certificate issued to a Chinese front company, HMAC-SHA256 C2 authentication with replay protection, and TLS fingerprint spoofing via `uTLS` — earning a **0/100** score on ANY.RUN sandbox with no behavioral detections. All identified IOCs were blocked at the network perimeter during the ~15-day staging window. The technical and behavioral profile of this campaign presents strong overlaps with a prior Pix-targeted operation; it represents a direct and credible threat to financial institutions operating in Brazil and across LATAM.

---

# Executive Summary

In May 2026, an active infrastructure associated with a cross-platform remote access trojan (RAT) cluster tracked as **Mikedor** — also referenced in the community as *mrmike* and *TrojanProxy*. The campaign exhibits strong technical and behavioral overlaps with a prior Pix-targeted attack against the Brazilian financial system, suggesting a recurring ~15-day preparation window before offensive operations begin.

The implant is written in **Go** and compiled for three architectures: Windows x64, Windows ARM64, and Linux ELF x64 — all sharing the same C2 infrastructure, authentication scheme, and Go module path. The operator invested heavily in operational security: EV code-signing certificates from a Chinese front company, TLS fingerprint spoofing via `uTLS`, HMAC-SHA256 C2 authentication with replay protection, and autonomous self-destruct logic.

All initial IOCs were blocked at the network perimeter before any offensive activity was recorded.

**Key highlights at a glance:**

- Cross-platform Go RAT — Windows x64, ARM64, and Linux ELF from a single codebase
- EV code-signing via Chinese front company bypasses Windows SmartScreen
- HMAC-SHA256 C2 authentication with anti-replay timestamp protection
- uTLS spoofs Chrome 115 PQ / Safari fingerprints — JA3/JA4 detection is ineffective
- ANY.RUN score: **0/100** — all IOCs recovered via static analysis only
- Detected during ~15-day staging window, before first offensive action

---

# Campaign Intelligence

## The Brazilian Financial Threat Landscape

Latin America hosts some of the world's most active and targeted digital payment ecosystems. Brazil alone accounts for the majority of banking trojan activity tracked globally — families such as Grandoreiro, Mekotio, Casbaneiro, and Guildma have historically dominated the region. The threat actor ecosystem has matured considerably: what once consisted largely of commodity trojans distributed through phishing campaigns now includes sophisticated bespoke implants, ransomware operators with dedicated LATAM arms, and financially motivated groups with structured intelligence-gathering phases preceding large-scale fraud.

**Pix**, launched by Banco Central do Brasil in November 2020, is the single most transformative shift in the Brazilian financial attack surface. With more than 150 million registered users, 24/7 availability, and near-instant settlement, Pix has become the dominant payment channel in Brazil — and one of the most valuable targets for financially motivated actors. A successful fraudulent transaction settles in seconds, reversal is operationally complex, and at scale, the financial impact can reach multimillion-dollar territory within a single operational window.

<!-- <
<div style="text-align: center;">
   <img src="/assets/images/Mikedor_01/pix_attack_surface.png" alt="Pix attack surface" />
    <br>
    <sub>Figure 1: Pix's adoption scale makes it a high-value fraud target — instant settlement, 24/7 availability, 150M+ users</sub>
</div>
<br>
> -->
<!-- <Adicione uma imagem de diagrama mostrando o ecossistema Pix como superfície de ataque: usuários → Pix → banco central → liquidação instantânea, com seta de ator malicioso mirando o fluxo de pagamento> -->

## The ~15-Day Staging Window

 **Plump Spider** — a financially motivated cluster that executed a multimillion-dollar fraud operation against the Brazilian financial sector, using Pix as the primary exfiltration channel. What made that campaign analytically significant was not its peak execution, but its **preparation discipline**: the threat actor consistently maintained a ~15-day infrastructure staging phase before transitioning to offensive operations.

During this staging window, the actor would:

1. Register or activate C2 infrastructure using domain names mimicking legitimate security or technology vendors
2. Deploy and validate implants across target environments under low-detection conditions
3. Establish reverse SOCKS5 tunnels to confirm network reachability and pivot capability
4. Remain operationally dormant — avoiding noisy behaviors that would trigger detection — until the attack window opened

This pattern — extended staging, disciplined dormancy, rapid execution — is characteristic of actors who treat fraud operations as structured projects with defined timelines, not opportunistic spray-and-pray attacks.

## Detection: Identifying Mikedor During Staging

In May 2026, a new C2 infrastructure whose profile immediately matched the prior staging signature: domain names impersonating security services (`webhook902.securitysolut.com`, `webhook5000.securitysolut.com`), active DNS resolution with no prior detection history, and network behavior consistent with implant validation rather than active exploitation.

The detection occurred within the ~15-day preparation window — before any offensive action was observed.
<!-- <
<div style="text-align: center;">
    <img src="/assets/images/Mikedor_01/campaign_timeline.png" alt="Campaign timeline" />
    <br>
    <sub>Figure 2: Campaign timeline — EV certificate acquired in March 2026, infrastructure activated progressively, detected in mid-May during the staging window</sub>
</div>
<br>
> -->
<!-- <Adicione uma imagem de timeline horizontal mostrando: março/2026 = aquisição do certificado EV → abril/2026 = ativação progressiva da infraestrutura → maio/2026 = detecção na janela de staging → ação ofensiva (bloqueada)> -->

**Operational evolution compared to the prior campaign:**

| Dimension | Prior Campaign | Mikedor |
|---|---|---|
| Platforms | Windows only | Windows x64, Windows ARM64, Linux ELF x64 |
| Code signing | Not documented | Sectigo EV cert via Chinese front company |
| C2 authentication | Basic | HMAC-SHA256 with anti-replay timestamp |
| Network evasion | Minimal | uTLS — Chrome 115 PQ / Safari fingerprint |
| Sandbox detection | Detectable | 0/100 on ANY.RUN |
| Resilience | Not documented | Autonomous self-destruct after C2 failures |

Each dimension represents a specific lesson learned and a concrete engineering response. The operator observed — either from prior failure or from monitoring other actors' takedowns — and built defenses against each detection vector.

## Why This Matters Beyond Brazil

The campaign's cross-platform architecture signals broader ambitions. **Windows ARM64** support targets Qualcomm Snapdragon-based laptops and ARM64 VMs — an increasingly common form factor across LATAM enterprise. **Linux ELF** variants extend reach to server infrastructure, payment processing backends, and cloud workloads: categories with far larger transaction exposure than individual endpoints.

Organizations operating Pix-adjacent infrastructure across the LATAM financial sector should treat the IOCs in this report as active and the threat as near-term.

---

# Malware Analysis

## Overview

The Mikedor agent is a bespoke, full-featured RAT. Despite its Go module path (`github.com/custom-socks5/cmd/agent`) suggesting a simple SOCKS5 proxy, the binary implements a comprehensive C2 implant with command execution, bidirectional file transfer, reverse SOCKS5 tunneling, persistence, and autonomous self-destruct capabilities.

The implant is configured entirely at **compile time** via Go `-ldflags -X` flags, embedding the C2 URL, HMAC secret, verbosity settings, and sleep parameters directly in the binary. This design eliminates the need for a configuration file on disk and complicates dynamic analysis.
<!-- <
<div style="text-align: center;">
    <img src="/assets/images/Mikedor_01/malwarebazaar_entry.png" alt="MalwareBazaar entry" />
    <br>
    <sub>Figure 3: MalwareBazaar entry for the primary Windows samples — tagged mrmike and securitysolut-com</sub>
</div>
<br>
<!-- <Adicione um screenshot da entrada do MalwareBazaar para os samples do Mikedor, mostrando os hashes, tags mrmike e securitysolut-com, e a data de upload> -->

## Sample Analysis

### Sample A — w-arm64.exe (Windows ARM64)

| Field | Value |
|---|---|
| SHA256 | `6210caacd4c7a3219ad6327b714c53d286443104ba06e3c4270f7e9a5d25ecee` |
| File size | 7,635,968 bytes (7.6 MB) |
| Architecture | PE32+ ARM64 (AARCH64) |
| Compiler | Go 1.26.3 — cross-compiled on macOS |
| Build target | `GOARM64=v8.0`, `windows/arm64` |
| PE timestamp | Zeroed (anti-forensics) |
| Subsystem | GUI (headless — no visible window) |
| ANY.RUN result | Task error — ARM64 not supported |

The ARM64 variant specifically targets Qualcomm Snapdragon-based Windows devices (Surface Pro X, Snapdragon X Elite laptops) and Windows ARM64 virtual environments — an uncommon target for commodity malware.

### Sample B — w-x86.exe (Windows x64)

| Field | Value |
|---|---|
| SHA256 | `959dca4b7989546a18a3f5e016c4bd78cfd825a1e679cefe0a355e739605937f` |
| File size | 8,850,952 bytes (8.44 MB) |
| Architecture | PE32+ x64 (AMD64) — despite the "x86" filename |
| Compiler | Go 1.26.3 |
| PE timestamp | Zeroed |
| Subsystem | GUI (`-H windowsgui` ldflags) |
| YARA matches | Golang, TorUsage, PostHttpForm, EnumerateProcesses |
| ANY.RUN score | **0 / 100** — no threats detected |

The filename `w-x86.exe` is a deliberate mislabel. In the operator's build pipeline, "x86" likely denotes the Intel/AMD family relative to the ARM64 variant.
<!-- <
<div style="text-align: center;">
    <img src="/assets/images/Mikedor_01/anyrun_score.png" alt="ANY.RUN score" />
    <br>
    <sub>Figure 4: ANY.RUN sandbox result — 0/100, no threats detected. All IOCs were recovered exclusively via static analysis.</sub>
</div>
<br>
<!-- <Adicione um screenshot do resultado do ANY.RUN mostrando score 0/100 para o sample w-x86.exe, destacando a ausência de detecções comportamentais> -->

## Embedded Build Flags

The Go linker flags are recoverable from the binary's string table, revealing the full agent configuration:

```
-ldflags="-s -w
  -X 'github.com/custom-socks5/internal/agent.c2url=https://webhook902.securitysolut.com'
  -X 'github.com/custom-socks5/internal/agent.secret=b9b264513a4f9074dc8fa9c022fce263'
  -X 'github.com/custom-socks5/internal/agent.verbose=0'
  -H windowsgui"
```

The `-s -w` flags strip debug symbols and DWARF information, reducing binary size and complicating reverse engineering. Despite this, the module path, C2 URL, and HMAC secret remain recoverable from string offsets.
<!-- <
<div style="text-align: center;">
    <img src="/assets/images/Mikedor_01/static_analysis_strings.png" alt="Static analysis strings" />
    <br>
    <sub>Figure 5: Binary string extraction — C2 URL, HMAC secret, and build path recovered from the stripped Go binary</sub>
</div>
<br>
<!-- <Adicione um screenshot de uma ferramenta de análise estática (strings, FLOSS, ou Ghidra) mostrando as strings extraídas do binário: o módulo Go, a URL do C2, o segredo HMAC e o build path /Users/panda/> -->

## Code Signing Certificate

Both Windows samples are signed with the same Sectigo EV certificate:

| Field | Value |
|---|---|
| Subject | Pingxiang De'a Zhiyun Technology Co., Ltd. |
| Issuer | Sectigo Public Code Signing CA EV R36 |
| Serial | `6c5efe09cd24511fddd320dd409c2d03` |
| Country | CN (Jiangxi Sheng) |
| Validity | 2026-03-12 → 2027-03-12 |
<!-- <
<div style="text-align: center;">
    <img src="/assets/images/Mikedor_01/ev_certificate.png" alt="EV certificate details" />
    <br>
    <sub>Figure 6: Sectigo EV certificate — issued to a Chinese front company in Jiangxi Province two months before the campaign</sub>
</div>
<br>
<!-- <Adicione um screenshot dos detalhes do certificado EV no Windows (janela de propriedades do certificado digital) ou via ferramenta de análise, mostrando o subject, issuer, serial number e período de validade> -->

A valid EV certificate from a Chinese entity in Jiangxi Province provides implicit trust with Microsoft's SmartScreen, significantly reducing the likelihood of user-facing security warnings during delivery. EV certificates of this type are actively sold in underground markets at prices ranging from **$2,000 to $6,500**, with Sectigo as one of the most frequently listed CAs due to its accessible validation process. The certificate was acquired two months before the campaign began — a clear indicator of deliberate, long-horizon planning.

---

# C2 Communication

## Check-in Protocol

The agent polls its C2 over HTTPS using a cookie-based HMAC-SHA256 authentication scheme:

```
GET https://webhook902.securitysolut.com/api/check

Cookie: PHPSESSID={agentID}.{unixTimestamp}.{HMAC-SHA256(secret, timestamp)}
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 
            (KHTML, like Gecko) Chrome/144.0.0.0 Safari/537.36
```

The timestamp is normalized against a reference constant (`0xe7791f700`), providing **replay protection** — a captured authentication token is only valid within a narrow time window. The HMAC key `b9b264513a4f9074dc8fa9c022fce263` is embedded in the binary.
<!-- <
<div style="text-align: center;">
    <img src="/assets/images/Mikedor_01/c2_auth_flow.png" alt="C2 authentication flow" />
    <br>
    <sub>Figure 7: C2 check-in flow — HMAC-SHA256 cookie authentication with timestamp replay protection</sub>
</div>
<br>
<!-- <Adicione um diagrama de sequência mostrando o fluxo de check-in do agente: agente → gera agentID + timestamp + HMAC → monta cookie PHPSESSID → GET /api/check → C2 responde com comando> -->

## TLS Fingerprint Evasion

The implant uses the `refraction-networking/utls` library (v1.8.2) to spoof TLS client hello fingerprints. By impersonating **Chrome 115 with Post-Quantum KEM** (`Chrome115_PQ`) or Safari, the agent blends into legitimate browser traffic and evades TLS inspection systems that rely on fingerprint-based detection (JA3/JA4).

Post-quantum identifiers observed in the binary: `GC256A/B/C/D`, `GC512A/B/C`, `X25519`.

## Self-Destruct Logic

After a configurable number of consecutive C2 check-in failures, the agent autonomously limits its forensic footprint:

1. Calls `Unpersist()` — removes its startup LNK or systemd service
2. Generates 4 random bytes → encodes as an 8-char hex filename
3. Renames itself to the random name via `os.Rename` — this defeats detection rules that trigger on a process deleting its own binary
4. Calls `os.Exit(0)`

---

# Command Set

| Command | Action |
|---|---|
| `exec` | Arbitrary OS command execution; records last-exec timestamp |
| `push` | Receives file payload from C2 → writes to victim disk |
| `download` | Exfiltrates file or directory from victim to C2 (chunked upload) |
| `sleep` | Operator-adjustable beacon interval |
| `socks5` | Opens reverse SOCKS5 proxy tunnel (1h timeout, auto-reconnect with 0–2s random jitter) |
| `persist` | Installs Startup folder LNK shortcut via `IShellLinkW` COM |
| `ide` | Self-destruct: `Unpersist()` + rename binary + `os.Exit(0)` |
| `nop` | No-op — skip cycle |

The presence of `handleStreamShellCmd` (interactive streaming shell) alongside the standard `exec` command indicates **hands-on-keyboard activity** by the operator, not just automated tasking. The SOCKS5 tunnel enables arbitrary TCP pivoting through compromised hosts — a strong indicator of lateral movement intent.

---

# Persistence

## Windows

Persistence is established via a COM shortcut placed in the Windows Startup folder:

```
%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup\<name>.lnk
```

Created using `IShellLinkW::SetPath()` and written with `IPersistFile::Save()`. Observed lure names:

- `SearchAppStartup.lnk`
- `OfficeClickToRun.lnk`
- `OneDriveSyncHelper.lnk`
- `RuntimeBrokerHelper.lnk`

The binary can also register itself as a **Windows Service** via `golang.org/x/sys/windows/svc`, providing an elevated persistence vector.

## Linux

On Linux systems, the malware targets two persistence mechanisms:

```
/etc/systemd/system/system-helper.service   # systemd unit
/etc/cron.d/cron-update                     # cron job
```

---

# Infrastructure Analysis

The C2 infrastructure follows a deliberate naming convention designed to impersonate security vendors, with subdomains using numeric indices (`webhook902`, `webhook5000`) suggesting programmatic or indexed deployment of C2 endpoints.
<!-- <
<div style="text-align: center;">
    <img src="/assets/images/Mikedor_01/infrastructure_graph.png" alt="Infrastructure graph" />
    <br>
    <sub>Figure 8: C2 infrastructure network — confirmed C2 domains and pivoting-identified related infrastructure sharing the same Go codebase</sub>
</div>
<br>
<!-- <Adicione um grafo de infraestrutura (estilo Maltego ou diagrama de rede) mostrando os domínios C2 confirmados (securitysolut.com, ohlabartproject.com) conectados aos samples via compartilhamento de módulo Go, e os domínios identificados via pivotamento (sec-ailos.com, t3-ch.com, etc.)> -->

## Confirmed C2 Domains

| Domain | Notes |
|---|---|
| `webhook902.securitysolut.com` | Primary check-in — DNS confirmed in sandbox |
| `webhook5000.securitysolut.com` | Secondary endpoint |
| `securitysolut.com` | Parent domain |
| `ohlabartproject.com` | Identified via VirusTotal pivoting |

## Additional Infrastructure (Pivoting)

The following domains were identified through build-path and certificate pivoting. They share the same Go module as the primary samples — compiled with different C2 configurations but from the same codebase:

| Domain | Associated Hashes |
|---|---|
| `webhook.sec-ailos.com` / `sec-ailos.com` | `a24e6bd8...`, `273ffccc...` |
| `t3-ch.com` / `orcaanalyse.com` | `c577e555...` |
| `security-checkers.com` / `subdomain.security-checkers.com` | `54ac4ba4...` |

## Operator Build Environment

Build artifacts embedded in the samples reveal consistent details about the threat actor:

```
Build host:  /Users/panda/go/
             /Users/panda/Documents/__SERVER/
Toolchain:   /opt/homebrew/Cellar/go/1.26.3/libexec
Go version:  go1.26.3
```

The operator compiled all samples on a **macOS machine with Apple Silicon** — the Homebrew path under `/opt/homebrew/` is ARM64-native on Apple Silicon. The username `panda` is consistent across all analyzed samples. The `__SERVER` directory likely contains both the C2 server-side components and the agent source.

---

# Cross-Platform Similarity Analysis

Comparison between Windows PE and Linux ELF samples confirms they belong to the same cluster:

| Attribute | Windows (PE) | Linux (ELF) |
|---|---|---|
| Classification | Mikedor / TrojanProxy | MikeDor / Backdoor |
| Language | Go 1.26.3 | Go 1.26.3 |
| C2 infrastructure | `securitysolut.com`, `ohlabartproject.com` | `securitysolut.com`, `ohlabartproject.com` |
| Persistence | Startup LNK + Windows Service | systemd unit + cron job |
| Anti-analysis | `detect-debug-environment` | `detect-debug-environment` |
| Discovery | `EnumerateProcesses` via WMI | File/Directory Discovery (T1083) |
| C2 evasion | uTLS fingerprint spoofing | Encrypted channels |

The deliberate ARM64 support is notable beyond cross-platform coverage: it anticipates growing Windows ARM64 adoption in enterprise environments — including financial institutions increasingly adopting Qualcomm Snapdragon-based workstations and cloud ARM64 VMs.

---

# Attribution Context

Definitive attribution is beyond the scope of this report. Mikedor does not yet map to a confirmed, named threat actor in public reporting. The following indicators provide a partial picture of the operator's likely origin and capability tier.

## Operator Profile

- **Build path**: `/Users/panda/Documents/__SERVER/` on macOS with Apple Silicon (M-series)
- **Username**: `panda` — consistent across all analyzed samples; the alias appears in Chinese cybercrime forums in contexts related to financially motivated tooling
- **Toolchain**: Go 1.26.3 via Homebrew — current, well-maintained development environment
- **Solo or small team**: Unified build environment, single Go module path across three platforms, no CI/CD artifacts observed

The operator demonstrates a level of capability inconsistent with commodity malware development: EV certificate procurement via underground channels, protocol-level TLS fingerprint spoofing, replay-protected C2 authentication, and cross-architecture Go compilation from a single clean codebase.

## Summary Attribution Table

| Hypothesis | Confidence | Key Evidence |
|---|---|---|
| China-nexus operator | **MODERATE** | EV cert (Jiangxi, CN), macOS build path with East Asian alias, Sectigo procurement pattern |
| Financially motivated, not state-sponsored | **MODERATE-HIGH** | Proxy/tunnel capability, Pix-specific targeting, operator pseudonym in financial fraud forums |
| APT-Q-27 / GoldenEyeDog alignment | **LOW** | Possible IP overlap, concurrent 2026 activity, Sectigo EV abuse, Go tooling |
| Single operator / very small team | **MODERATE** | Unified build environment, consistent macOS dev path, single Go module across all samples |

---

# Detection Rules

## YARA

```yara
rule Mikedor_GoRAT_CustomSocks5
{
    meta:
        author      = "alakjhon"
        description = "Detects Mikedor RAT based on embedded Go module path and C2 strings"
        tlp         = "TLP:CLEAR"
        date        = "2026-05-15"
        reference   = "https://bazaar.abuse.ch/browse/tag/mrmike/"

    strings:
        $go_module   = "github.com/custom-socks5/internal/agent" ascii
        $c2_string   = "github.com/custom-socks5/internal/agent.c2" ascii
        $hmac_secret = "b9b264513a4f9074dc8fa9c022fce263" ascii
        $c2_domain_1 = "securitysolut.com" ascii nocase
        $c2_domain_2 = "ohlabartproject.com" ascii nocase
        $api_path    = "/api/check" ascii
        $utls_lib    = "refraction-networking/utls" ascii
        $build_path  = "Users/panda/" ascii

    condition:
        uint16(0) == 0x5A4D and
        filesize < 20MB and
        (
            $go_module or $c2_string or $hmac_secret or
            ($c2_domain_1 and $api_path) or ($c2_domain_2 and $utls_lib) or
            $build_path
        )
}
```
```yara
rule Mikedor_Linux_ELF
{
    meta:
        author      = "akajhon"
        description = "Detects Mikedor Linux ELF variant"
        tlp         = "TLP:CLEAR"
        date        = "2026-05-15"

    strings:
        $go_module   = "github.com/custom-socks5" ascii
        $systemd_ioc = "system-helper.service" ascii
        $cron_ioc    = "cron-update" ascii
        $c2_domain   = "securitysolut.com" ascii nocase
        $build_path  = "Users/panda/" ascii

    condition:
        uint32(0) == 0x464C457F and
        ( $go_module or ($systemd_ioc and $cron_ioc) or ($c2_domain and $build_path) )
}
```

## Sigma (Proxy — Network Detection)

```yara
title: Mikedor RAT C2 Check-in
id: a1b2c3d4-e5f6-7890-abcd-ef1234567890
status: experimental
description: Detects Mikedor RAT PHPSESSID-based C2 authentication pattern
references:
  - https://bazaar.abuse.ch/browse/tag/mrmike/
author: Akajhon
date: 2026/05/15
tags:
  - attack.command_and_control
  - attack.t1071.001
logsource:
  category: proxy
detection:
  selection:
    cs-uri-stem|contains: '/api/check'
    cs-Cookie|re: 'PHPSESSID=[a-f0-9]+\.[0-9]+\.[a-f0-9]{64}'
  condition: selection
falsepositives:
  - Unlikely — PHPSESSID format with 64-char hex suffix is non-standard
level: high
```

## Pivoting Queries (VirusTotal)

```yara
# Operator's build path — most precise pivot
content:"Users/panda/Documents/__SERVER/"

# Go module internal agent string — identifies all variants
content:"github.com/custom-socks5/internal/agent.c2"

# HMAC authentication secret
content:"b9b264513a4f9074dc8fa9c022fce263"

# Code signing certificate subject
signature:"Pingxiang De'a Zhiyun Technology Co., Ltd."

# Certificate serial
certificate_serial:"6c5efe09cd24511fddd320dd409c2d03"
```

---

# MITRE ATT&CK Mapping
<!-- <
<div style="text-align: center;">
    <img src="/assets/images/Mikedor_01/mitre_heatmap.png" alt="MITRE ATT&CK heatmap" />
    <br>
    <sub>Figure 9: MITRE ATT&CK Navigator heatmap for the Mikedor campaign</sub>
</div>
<br>
<!-- <Adicione um screenshot do MITRE ATT&CK Navigator com as técnicas do Mikedor destacadas em vermelho/laranja: T1553.002, T1036.001, T1071.001, T1573.001, T1090.001, T1547.001, T1543.003, T1059, T1105, T1041, T1057, T1082, T1083, T1070.004, T1497, T1219, T1027.002> -->

| ID | Technique | Implementation |
|---|---|---|
| T1553.002 | Subvert Trust Controls: Code Signing | Sectigo EV cert from CN entity bypasses Windows SmartScreen |
| T1036.001 | Masquerading: Invalid Code Signature | `w-x86.exe` filename for x64 binary; Go module named `custom-socks5` |
| T1071.001 | Application Layer Protocol: HTTPS | C2 check-in over HTTPS mimicking Chrome browser |
| T1573.001 | Encrypted Channel: Symmetric Cryptography | uTLS spoofing Chrome 115 PQ / Safari — evades TLS inspection |
| T1090.001 | Proxy: Internal Proxy | Reverse SOCKS5 tunnel — operator pivots through victim |
| T1547.001 | Boot/Logon Autostart: Startup Folder | LNK via `IShellLinkW` + `IPersistFile` COM interfaces |
| T1543.003 | Create/Modify System Process: Windows Service | `golang.org/x/sys/windows/svc.Run` — elevated persistence |
| T1059 | Command and Scripting Interpreter | `handleExecCmd` + `handleStreamShellCmd` (interactive streaming shell) |
| T1105 | Ingress Tool Transfer | `push` command — deploys additional payloads from C2 |
| T1041 | Exfiltration Over C2 Channel | `download` command — chunked file upload to C2 |
| T1057 | Process Discovery | `EnumerateProcesses` via WMI — sandbox/AV detection |
| T1082 | System Information Discovery | Timezone fingerprinting via Windows registry |
| T1083 | File and Directory Discovery | Automated filesystem enumeration (Linux samples) |
| T1070.004 | Indicator Removal: File Deletion | `SelfDestruct()` — renames binary to 8-char hex, defeats write-delete detection |
| T1497 | Virtualization/Sandbox Evasion | uTLS + process enumeration + C2 offline for sandbox IPs |
| T1219 | Remote Access Software | Full-featured RAT with interactive shell and SOCKS5 pivot |
| T1027.002 | Obfuscated Files: Software Packing | Go binary with stripped symbols (`-s -w` ldflags) |

---

# IOCs

## Domains

```yara
# Confirmed C2
webhook902.securitysolut.com
webhook5000.securitysolut.com
securitysolut.com
ohlabartproject.com

# Same family — pivoting
webhook.sec-ailos.com
sec-ailos.com
t3-ch.com
orcaanalyse.com
security-checkers.com
subdomain.security-checkers.com
```

## File Hashes (SHA256)

```yara
# Windows PE — confirmed
959dca4b7989546a18a3f5e016c4bd78cfd825a1e679cefe0a355e739605937f  # w-x86.exe (x64)
6210caacd4c7a3219ad6327b714c53d286443104ba06e3c4270f7e9a5d25ecee  # w-arm64.exe
67b1a9a245d4bee2793ddd875a3240dc89c934e1eadb08b9453128e499b3676b  # PE — via YARA

# Linux ELF — high confidence, same family
b8de00249df3b152e5b28c21735a6acaa649b7098d0e4195b86fd29577367da5
a24e6bd8ccda593a96d7c040b6ddb1647f0b2d4c4f2d257b764c774353de3191
273ffcccedc11027b5dd29ed295ec894e10cc062a2893c213a626908b0abb454
c577e555067b875117ef87f01de5b2305dfb794cfbd8cbc1107a95ca143a9aa1
54ac4ba45ac5a7bda0a311b97aa4263b94657f612ce73cd4e1407376a6f05d98
47f3e0c71cff9dca09e4d55246435b4a47c3a7ceff216b356934005355c39285
2904a8d1ef1e683a96a5cae4b8cff8a97bb08606054b486170b1a5f693eb8860
5a0b6ad41ef410c4182d0a0bdafd108f17bb397b4ab126c59b209e86664a85a7
```

## Network Indicators

| Type | Value | Notes |
|---|---|---|
| URL | `hxxps://webhook902[.]securitysolut[.]com/api/check` | Primary check-in endpoint |
| Cookie | `PHPSESSID={id}.{ts}.{hex64}` | 64-char hex suffix — non-standard |
| User-Agent | `Mozilla/5.0 ... Chrome/144.0.0.0 Safari/537.36` | Chrome impersonation |
| TLS Fingerprint | Chrome 115 PQ / Safari (via uTLS) | JA3/JA4 will not match known malware patterns |

## Host-Based Indicators

| Type | Value |
|---|---|
| Windows Startup LNK | `%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup\*.lnk` |
| Lure LNK names | `SearchAppStartup.lnk`, `OfficeClickToRun.lnk`, `OneDriveSyncHelper.lnk`, `RuntimeBrokerHelper.lnk` |
| Linux systemd | `/etc/systemd/system/system-helper.service` |
| Linux cron | `/etc/cron.d/cron-update` |
| Self-destruct artifact | `<binary_dir>/<8-char-hex>` (renamed binary post-exit) |

## Code Artifacts

| Type | Value |
|---|---|
| Go module | `github.com/custom-socks5/cmd/agent` |
| Build path | `/Users/panda/Documents/__SERVER/` |
| HMAC secret | `b9b264513a4f9074dc8fa9c022fce263` |
| Cert serial | `6c5efe09cd24511fddd320dd409c2d03` |
| Cert entity | Pingxiang De'a Zhiyun Technology Co., Ltd. (Jiangxi, CN) |
| Go version | `go1.26.3` |

---

# References

| Resource | Link |
|---|---|
| VT Collection — Investigation IOCs | [bb7bc014...](https://www.virustotal.com/gui/collection/bb7bc0148a338f73c44cef47b28ec0e476fbd6f19d04746b40704101733b5d1c/iocs) |
| VT Collection — ohlabartproject pivoting | [76907ba4...](https://www.virustotal.com/gui/collection/76907ba4f80e9207a65063c6a70222d3b29e7d13b443907aee8ff78d30032535) |
| MalwareBazaar — mrmike | [bazaar.abuse.ch/browse/tag/mrmike/](https://bazaar.abuse.ch/browse/tag/mrmike/) |
| MalwareBazaar — securitysolut-com | [bazaar.abuse.ch](https://bazaar.abuse.ch/browse/tag/securitysolut-com/) |
| MalwareBazaar — Pingxiang cert | [bazaar.abuse.ch](https://bazaar.abuse.ch/browse/tag/Pingxiang-De-a-Zhiyun-Technology-Co-Ltd/) |
| VirusView — Trojan/Linux/MikeDor/Backdoor | [virusview.net](https://www.virusview.net/malware/Trojan/Linux/MikeDor/Backdoor) |
| REMnux Report — w-arm64.exe | [GitHub](https://github.com/Squiblydoo/Remnux_Reports/blob/main/Reports%20by%20hash/6210caacd4c7a3219ad6327b714c53d286443104ba06e3c4270f7e9a5d25ecee_w-arm64_analysis_report.md) |
| REMnux Report — w-x86.exe | [GitHub](https://github.com/Squiblydoo/Remnux_Reports/blob/main/Reports%20by%20hash/959dca4b_w-x86/analysis_report.md) |
| @SquiblydooBlog — Analysis thread | [x.com](https://x.com/SquiblydooBlog/status/2055045916340658329) |
| Joe Sandbox analysis | [joesandbox.com](https://www.joesandbox.com/analysis/1910998/0/html) |
| ANY.RUN — w-x86.exe | [app.any.run](https://app.any.run/tasks/3f9294b8-3551-4dd6-a44b-500fc309c1ba) |

---

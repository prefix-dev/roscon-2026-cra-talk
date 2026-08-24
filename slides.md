---
# Tufte-inspired theme, local in ./slidev-theme-tufte
theme: ./slidev-theme-tufte
title: Surviving the Cyber Resilience Act
info: |
  ## Surviving the Cyber Resilience Act: Reproducible, Auditable ROS Stacks
  What the CRA asks of a robotics manufacturer, what SBOMs and VEX actually are,
  and how today's ROS packaging options hold up.
  ROSCon 2026, Wolf Vollprecht and Ruben Arts, prefix.dev
layout: center
class: text-center
drawings:
  persist: false
mdc: true
duration: 10min
transition: null
---

<!-- Replace public/slides-qr-code.png once the repo is public.
     Kept bottom-right so it never crowds the title. -->
<img src="/slides-qr-code.png" class="absolute bottom-10 right-10 w-24 opacity-90"
     alt="Slides QR code" onerror="this.style.display='none'" />

# Surviving the Cyber&nbsp;Resilience&nbsp;Act

## Reproducible, auditable ROS stacks

<div class="mt-6">

Wolf Vollprecht and Ruben Arts at
<img src="/prefix-logo.svg" class="inline align-middle h-5 mx-1" alt="prefix.dev" />

</div>

<div class="subtitle mt-6">ROSCon 2026 · Thursday 24 September</div>

---

# What is the CRA?

<DocLink href="https://eur-lex.europa.eu/eli/reg/2024/2847/oj" label="Reg. (EU) 2024/2847" />

<div class="mt-8 max-w-3xl">

The **Cyber Resilience Act** is an EU regulation, in force since December 2024.
It applies to any **product with digital elements** placed on the EU market.

If your robot has software and a data connection, it is in scope.

</div>

---
layout: center
class: text-center
---

# Secure by design

<div class="subtitle mt-4">No known vulnerabilities, secure defaults, encryption, signed updates.<br>Met before you ship — now part of CE marking.</div>

---
layout: center
class: text-center
---

# Software security for the product full lifecycle

<div class="subtitle mt-4">Security updates for a support period you declare. At least five years.</div>

---
layout: center
class: text-center
---

# Manufacturer is liable

<div class="subtitle mt-4">Open source, proprietary, you are responsible for all aspects of your product's security.</div>

---

# Timeline

<DocLink href="https://www.enisa.europa.eu/topics/product-security/single-reporting-platform-srp" label="ENISA SRP" />

<div class="grid grid-cols-[auto_1fr] gap-x-6 gap-y-5 mt-10 items-baseline">

<div class="text-3xl font-mono text-[var(--tufte-accent)]">11 Sep 2026</div>
<div>

**Reporting obligations.** Thirteen days ago.

</div>

<div class="text-3xl font-mono">11 Dec 2027</div>
<div v-click>

**Everything else.** SBOM, the security requirements, technical documentation,
a declared support period. **No CRA conformity, no CE mark, no EU market.**

</div>

</div>

<div v-click class="mt-10 pt-5 border-t border-[var(--tufte-rule)] border-opacity-20">

This covers products **already on the market**, not just new ones. <span class="text-sm text-[var(--tufte-muted)]">(Art. 69(3))</span>

**What:** an actively exploited vulnerability or a severe incident.
Early warning in 24 h, full notification in 72 h. <span class="text-sm text-[var(--tufte-muted)]">(Art. 14)</span>

**Where:** ENISA's <a href="https://www.enisa.europa.eu/topics/product-security/single-reporting-platform-srp">Single Reporting Platform</a>,
which forwards to your national CSIRT. One report, once.

</div>

---

# What a manufacturer owes

<DocLink href="https://www.european-cyber-resilience-act.com/Cyber_Resilience_Act_Article_13.html" label="Art. 13 · Annex I" />

<div class="mt-10 text-2xl max-w-2xl">

<v-clicks>

- **A risk assessment**, and it decides everything else
- **An SBOM**, machine-readable
- **Vulnerability handling**, free of charge
- **A support period**, declared and public

</v-clicks>

</div>

---
layout: center
class: text-center
---

# If you ship the robot,<br>you are the manufacturer.

<div class="subtitle mt-4">Including the code you did not write.</div>

<div v-click class="callout mt-10 max-w-xl mx-auto text-left">

Top tier of fines: **15M EUR or 2.5%** of worldwide annual turnover,
whichever is higher. <span class="text-[var(--tufte-muted)]">(Art. 64)</span>

</div>

---

# What is an SBOM, actually?

<DocLink href="https://github.com/package-url/purl-spec" label="PURL spec" />

<div class="grid grid-cols-2 gap-8 mt-4">

<div>

An ingredients list for your software, written so a machine can read it.
It lets a scanner answer one question: am I affected by this CVE?

<v-clicks>

- Two formats dominate: **SPDX** and **CycloneDX**
- The CRA mandates neither, only "commonly used, machine-readable"
- Writing one is not the hard part. **Naming things consistently** is.

</v-clicks>

</div>

<CodeWindow title="sbom.cdx.json">

```json {lines: true}
{
  "bomFormat": "CycloneDX",
  "specVersion": "1.7",
  "components": [
    {
      "type": "library",
      "name": "libcurl4",
      "version": "8.5.0-2",
      "purl": "pkg:deb/ubuntu/libcurl4@8.5.0-2",
      "licenses": [
        { "license": { "id": "curl" } }
      ]
    }
  ]
}
```

</CodeWindow>

</div>

<div v-click class="mt-4 text-sm">

That `purl` line is what makes the entry useful. A PURL is one identifier for a
package across every ecosystem. Without it, two SBOMs describing the same
library cannot be matched up.

</div>

---

# And VEX? It stops the drowning.

<div class="text-sm text-[var(--tufte-muted)] mt-1">VEX, the <strong>V</strong>ulnerability <strong>E</strong>xploitability e<strong>X</strong>change.</div>

<DocLink href="https://github.com/openvex/spec" label="OpenVEX" />

<div class="mt-4">

Scan a ROS environment and you get hundreds of CVEs.
Almost none of them are exploitable in *your* product.

</div>

<div class="grid grid-cols-2 gap-8 mt-6">

<CodeWindow title="vex.openvex.json">

```json {lines: true}
{
  "statements": [
    {
      "vulnerability": { "name": "CVE-2024-XXXX" },
      "products": [
        { "@id": "pkg:deb/ubuntu/my-robot@2.1.0" }
      ],
      "status": "not_affected",
      "justification":
        "vulnerable_code_not_in_execute_path"
    }
  ]
}
```

</CodeWindow>

<div>

<v-clicks>

- A signed, machine-readable statement saying **"we looked at this, and here is why it doesn't apply"**
- It turns an unreadable scanner dump into a short list of things that matter
- The CRA never uses the word *VEX*, but "handle vulnerabilities effectively" is not achievable without something like it

</v-clicks>

<div v-click class="mt-6 text-sm text-[var(--tufte-muted)]">

Still immature. The formats compete, and there is **no agreed way to publish or
discover** VEX documents. This is an open industry problem, not a solved one.

</div>

</div>

</div>

---
layout: center
class: text-center
---

# Your robot outlives your ROS distro

<div class="grid grid-cols-2 gap-16 mt-12 text-left max-w-3xl mx-auto">

<div>

<div class="text-5xl font-mono text-[var(--tufte-accent)]">5 years</div>

<div class="mt-3">

ROS 2 LTS support.
Jazzy runs from May 2024 to **May 2029**.

</div>

</div>

<div v-click>

<div class="text-5xl font-mono">10-15 years</div>

<div class="mt-3">

An industrial robot on a factory floor.

</div>

</div>

</div>

<div v-click class="mt-14 max-w-3xl mx-auto text-left">

The CRA lets you *consider* your upstream's support window when you set yours.
It does not let you **cap** your obligation at it.

**That decade-wide gap is yours.** Every security update has to stay
available for 10 years after you issue it. <span class="text-sm text-[var(--tufte-muted)]">(Art. 13(8), 13(9))</span>

</div>

---
layout: center
class: text-center
---

# One robot, many bills of materials

<div class="subtitle mt-4">Linux, ROS, Python, C++, fleet backend, microcontrollers.</div>

---
layout: center
class: text-center
---

# No lockfile in the classic path

<div class="subtitle mt-4">No single file describes the environment you actually run.<br>Rebuild that image in 2029, from what?</div>

---
layout: center
class: text-center
---

# Multiple identifiers

<div class="subtitle mt-4"><code>nav2</code> - <code>ros-jazzy-nav2</code> - <code>navigation2</code> - the CVE name</div>

---
layout: center
class: text-center
---

# Vendor patches are invisible

<div class="subtitle mt-4">Not upstream. Not a new version. Same name in every SBOM.</div>

---

# Somebody already tried this

<DocLink href="https://fosdem.org/2026/schedule/event/7YG9H7-embedded-product-with-three-sboms/" label="FOSDEM 2026" />

<div class="text-sm mt-2 text-[var(--tufte-muted)]">

Marta Rybczynska built a deliberately <em>simple</em> three-processor device, running Yocto Linux,
Zephyr sensors and a Python service, then tried to do CRA-style vulnerability management on it.

</div>

<div class="mt-4 table-grow">

| Step | Result |
|---|---|
| CycloneDX from the Python service | ✅ imported fine |
| SPDX 3.0 from Yocto | ❌ scanner won't take it |
| SPDX 2.3 from Zephyr <span class="text-[var(--tufte-muted)]">(tag:value)</span> | ❌ format unsupported |
| Convert via `pyspdxtools` | ❌ no luck |
| Convert via `syft` | ⚠️ experimental, worked |
| Yocto SPDX 3.0 to CycloneDX | ❌ **no working tool exists** |
| Downgrade to SPDX 2.2, loop-convert, merge | ✅ finally |

</div>

---

# Where the packaging options stand

<div class="table-grow mt-5">

| | Rebuild it in 2029 | Build-time SBOM | Vuln tracking |
|---|---|---|---|
| **Debian / `rosdep`** | ✗ no lockfile | ~ deb metadata | ✓ mature tracker |
| **Docker** | ~ digest pinning | ~ inferred from layers | ~ image scanning |
| **Nix** | ✓✓ strongest | ✓ tooling available | ~ improving but complex |
| **Yocto** | ✓ recipes + patches | ✓✓ SPDX by default | ✓ `cve-check` |
| **conda / Pixi** | ✓✓ `pixi.lock` | ~ feasible not implemented | ~ solutions rolling out |

</div>

<div v-click class="mt-5 text-sm">

**Nobody is finished.** Yocto has the best SBOM story and admits vendor patches break it.
Nix has the best reproducibility but thin tooling. Conda has a real
cross-language lockfile and an incomplete PURL story.

</div>

<div v-click class="mt-3 text-sm text-[var(--tufte-muted)]">

The consistent finding at FOSDEM: **a good SBOM starts from a lockfile.**

</div>

---

# Ideas for the ecosystem

<div class="mt-4 max-w-3xl">

Yocto, Zephyr and Erlang/OTP are showing what ecosystem-level answers
look like. ROS can do the same, and some pieces are close.

</div>

<div class="mt-8">

<v-clicks>

- **Build-time SBOMs** from `colcon`, so every workspace gets one for free
- **Stable identifiers**: one PURL per ROS package, across apt, conda and source
- **A steward** who can issue advisories, like Erlang's foundation becoming a CVE Numbering Authority
- **Richer metadata**: licenses, origins, `rosdep` mappings

</v-clicks>

</div>

---

# What you have to do

<div class="grid grid-cols-2 gap-10 mt-8">

<div>

### Today

<v-clicks>

- **Know your reporting path**: EU Login, ENISA's Single Reporting Platform, your national CSIRT
- **Write down your support period**, at least five years
- **Pin what you ship**, so you can rebuild it in five years

</v-clicks>

</div>

<div>

### By 11 Dec 2027

<v-clicks>

- **A risk assessment**, it decides which requirements apply
- **An SBOM**, machine-readable, from what you actually ship
- **Vulnerability handling**: monitor, patch, disclose, free of charge
- **CE conformity** with the technical documentation to back it

</v-clicks>

</div>

</div>

<div v-click class="mt-10 pt-5 border-t border-[var(--tufte-rule)] border-opacity-20 text-sm text-[var(--tufte-muted)] text-center">

Slides, sources and the full research notes:
<a href="https://github.com/prefix-dev/roscon-2026-cra-talk">github.com/prefix-dev/roscon-2026-cra-talk</a>

</div>


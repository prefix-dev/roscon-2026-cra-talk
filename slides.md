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

<div class="subtitle mt-4">No known exploitable vulnerabilities, secure defaults, encryption, signed updates.<br>Meet the requirements before shipping and document them for CE marking.</div>

---
layout: center
class: text-center
---

# Security throughout the product lifecycle

<div class="subtitle mt-4">You declare a support period and provide security updates for it. At least five years.</div>

---
layout: center
class: text-center
---

# Manufacturer is liable

<div class="subtitle mt-4">Whether the software is open source or proprietary, you are responsible for your product's security.</div>

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

**Remaining obligations.** SBOM, security requirements, technical documentation
and a declared support period. Products need CRA conformity to carry a CE mark and enter the EU market.

</div>

</div>

<div v-click class="mt-10 pt-5 border-t border-[var(--tufte-rule)] border-opacity-20">

This also covers products **already on the market**. <span class="text-sm text-[var(--tufte-muted)]">(Art. 69(3))</span>

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

- **A risk assessment** that determines which other requirements apply
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

# What is an SBOM?

<DocLink href="https://github.com/package-url/purl-spec" label="PURL spec" />

<div class="grid grid-cols-2 gap-8 mt-4">

<div>

A machine-readable inventory of your software components.
It helps a scanner answer: does this CVE affect my product?

<v-clicks>

- Two formats dominate: **SPDX** and **CycloneDX**
- The CRA mandates neither, only "commonly used, machine-readable"
- Generating the file is straightforward. **Consistent package names** are harder.

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

The `purl` identifies the package, version and packaging ecosystem. Consistent
PURLs let tools match components between SBOMs and vulnerability databases.

</div>

---

# VEX filters vulnerability results

<div class="text-sm text-[var(--tufte-muted)] mt-1">VEX, the <strong>V</strong>ulnerability <strong>E</strong>xploitability e<strong>X</strong>change.</div>

<DocLink href="https://github.com/openvex/spec" label="OpenVEX" />

<div class="mt-4">

A scan of a ROS environment can return hundreds of CVEs.
Almost none are exploitable in *your* product.

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

- A signed, machine-readable record of whether a vulnerability affects a product, with the reason
- It reduces scanner output to the vulnerabilities that need action
- The CRA does not mention *VEX*. In practice, effective vulnerability handling needs an equivalent record.

</v-clicks>

<div v-click class="mt-6 text-sm text-[var(--tufte-muted)]">

VEX is still immature. Formats compete, and there is **no agreed way to publish or
discover** VEX documents.

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

**You must cover the remaining years.** Every security update has to stay
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

<div class="subtitle mt-4"><code>nav2</code>, <code>ros-jazzy-nav2</code>, <code>navigation2</code>, the name in a CVE</div>

---
layout: center
class: text-center
---

# Vendor patches can be invisible

<div class="subtitle mt-4">A downstream patch may keep the upstream name and version, so the SBOM cannot distinguish it.</div>

---

# A three-SBOM test

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

Each option still has gaps. Yocto has the best SBOM support, but vendor patches can break it.
Nix has the best reproducibility but limited tooling. Conda has a
cross-language lockfile and incomplete PURL support.

</div>

<div v-click class="mt-3 text-sm text-[var(--tufte-muted)]">

The FOSDEM example reached a practical conclusion: **a good SBOM starts from a lockfile.**

</div>

---

# Ideas for the ecosystem

<div class="mt-4 max-w-3xl">

Yocto and Zephyr already generate build metadata. The Erlang Ecosystem
Foundation is becoming a CVE Numbering Authority. ROS could adopt similar infrastructure.

</div>

<div class="mt-8">

<v-clicks>

- **Build-time SBOMs** from `colcon`, generated for every workspace
- **Stable identifiers**: a ROS-level PURL linked to the apt, conda or source artifact
- **A steward** who can issue advisories, like Erlang's foundation becoming a CVE Numbering Authority
- **Richer metadata**: licenses, origins, `rosdep` mappings

</v-clicks>

</div>

---

# A PURL type for ROS?

<DocLink href="https://github.com/package-url/purl-spec/blob/main/docs/types/maintain-purl-types.md" label="Proposing a PURL type" />

<div class="grid grid-cols-2 gap-10 mt-8">

<div>

### ROS package

`pkg:ros/nav2_controller@1.2.3?distro=jazzy`

A proposed identity based on `package.xml` and the ROS distribution.

</div>

<div v-click>

### Installed artifact

`pkg:deb/ubuntu/ros-jazzy-nav2-controller@1.2.3-1`

The package and version installed on the product.

</div>

</div>

<div v-click class="callout mt-8">

These identify different things. A ROS PURL would name the ROS package. The
Debian or conda PURL would name the artifact that was shipped. Vendor patches
also need a distinct version, hash or SBOM pedigree.

</div>

<div v-click class="mt-6 text-sm text-[var(--tufte-muted)]">

The proposal only works if ROS defines canonical rules for names, versions,
distribution qualifiers and repository lookup.

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

- **A risk assessment** that determines which requirements apply
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


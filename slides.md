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

<div class="grid grid-cols-[1fr_1.3fr] gap-10 items-center mt-8">

<div>

The **Cyber Resilience Act** is an EU regulation, adopted in 2024 and applying
in stages from September 2026.
It applies to any **product with digital elements** placed on the EU market.

If your robot has software and a data connection, it is in scope.

</div>

<img src="/cra-robot-eu.png" class="w-full rounded" alt="A robot heading into the EU market" />

</div>

---

# Security becomes part of the CE mark

<div class="grid grid-cols-[1fr_1.8fr] gap-14 items-center mt-10">

<svg viewBox="0 0 150 100" class="w-4/5 mx-auto text-[var(--tufte-text)]" fill="currentColor" role="img" aria-label="CE mark">
  <path d="M50,0 A50,50 0 0 0 50,100 L50,88 A38,38 0 0 1 50,12 Z" />
  <path d="M150,0 A50,50 0 0 0 150,100 L150,88 A38,38 0 0 1 150,12 Z" />
  <rect x="105" y="44" width="40" height="12" />
</svg>

<div class="text-3xl">

<v-clicks>

- **Secure by design**
- **For the entire product lifecycle**
- **The manufacturer is liable**
- **Open source or proprietary**

</v-clicks>

<div v-click class="mt-8 text-base text-[var(--tufte-muted)]">

Top tier of fines: **15M EUR or 2.5%** of worldwide annual turnover,
whichever is higher. (Art. 64)

</div>

</div>

</div>

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

<!--
Reporting also covers products already on the market. Other requirements apply
to them only after a substantial modification (Art. 69(2), 69(3)).

What: an actively exploited vulnerability or a severe incident.
Early warning in 24 h, follow-up in 72 h, final report later (Art. 14).

Where: ENISA's Single Reporting Platform, which routes it to the coordinating
CSIRT. One path, with staged updates.
-->

---

# What the CRA asks of you

<DocLink href="https://www.european-cyber-resilience-act.com/Cyber_Resilience_Act_Article_13.html" label="Art. 13 · Annex I" />

<div class="mt-10 text-2xl max-w-2xl">

<v-clicks>

- **Assess your product's risks.** That decides which requirements apply to you.
- **Keep a machine-readable SBOM** of everything you ship.
- **Fix vulnerabilities** and ship security updates at no cost to your customers.
- **Declare a support period** and tell buyers when it ends.

</v-clicks>

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
- The CRA mandates neither; it requires at least top-level dependencies
- The SBOM must be documented, but need not be published
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

A scan of a ROS environment can return hundreds of CVE matches.
A component and version match does not establish exploitability.

</div>

<div class="grid grid-cols-2 gap-8 mt-6">

<CodeWindow title="OpenVEX excerpt">

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

- A machine-readable record of whether a vulnerability affects a product, with the reason
- It reduces scanner output to the vulnerabilities that need action
- The CRA does not mention *VEX*. VEX is one way to document these decisions.

</v-clicks>

<div v-click class="mt-6 text-sm text-[var(--tufte-muted)]">

VEX is still immature. Formats compete, and there is **no agreed way to publish or
discover** VEX documents.

</div>

</div>

</div>

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
| Yocto SPDX 3.0 to CycloneDX | ❌ no converter found in this test |
| Downgrade to SPDX 2.2, loop-convert, merge | ✅ finally |

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

<div class="text-5xl font-mono">10 years</div>

<div class="mt-3">

If that is how long you expect your robot to remain in use.

</div>

</div>

</div>

<div v-click class="mt-14 max-w-3xl mx-auto text-left">

The CRA lets you *consider* upstream support, but your support period must reflect
how long the product is expected to remain in use.

A five-year ROS distribution does not shorten a ten-year product obligation.
Each security update must remain available for at least 10 years, or for the
rest of the support period if that is longer. <span class="text-sm text-[var(--tufte-muted)]">(Art. 13(8), 13(9))</span>

</div>

---

# Why this is hard for ROS

<div class="mt-10 text-2xl max-w-3xl">

<v-clicks>

- **Many bills of materials**: Linux, ROS, Python, C++, fleet backend, microcontrollers
- **No lockfile in the classic path.** Rebuild that image in 2029, from what?
- **Multiple identifiers**: `nav2`, `ros-jazzy-nav2`, `navigation2`, the name in a CVE
- **Invisible vendor patches** that keep the upstream name and version

</v-clicks>

</div>

---

# Where the packaging options stand

<div class="table-grow mt-5">

| | Environment identity | SBOM path | Vulnerability data |
|---|---|---|---|
| **Debian / `rosdep`** | no standard environment lock | `syft` | Debian / Ubuntu trackers |
| **Docker / BuildKit** | image digest | BuildKit attestation | Docker Scout |
| **Nix** | lockfile + derivation closure | `sbomnix` | `vulnix` |
| **Yocto** | pinned layers + recipes | `create-spdx` | `cve-check` |
| **conda / Pixi** | `pixi.lock` | `pixi-sbom` | `pixi-audit` |

</div>

<!--
These tools solve different layers of the problem. The CRA does not require a
lockfile or reproducible build; both help produce and maintain an accurate SBOM.

In the FOSDEM test, conversion lost package names and CPEs. Dependency-Track then
reported no Linux kernel CVEs. Format conversion can silently damage matching.
-->

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

The proposal only works if ROS defines canonical rules for names, versions and
how ROS distributions are represented. Alongside it, ROS needs build-time SBOMs
from `colcon` and richer metadata: licenses, origins, `rosdep` mappings.

</div>

---

# ROS needs an open-source steward

<DocLink href="https://www.european-cyber-resilience-act.com/Cyber_Resilience_Act_Article_24.html" label="Art. 3(14) · Art. 24" />

<div class="callout mt-8 max-w-3xl text-lg">

**Open-source software steward:** an organization, not a manufacturer, that
supports open-source software intended for commercial use on a sustained basis.

</div>

<div class="mt-8 text-xl max-w-3xl">

<v-clicks>

- **Light obligations**: a cybersecurity policy, cooperation with authorities, reporting actively exploited vulnerabilities
- **No administrative fines** for stewards <span class="text-sm text-[var(--tufte-muted)]">(Art. 64(10))</span>

</v-clicks>

</div>

<div v-click class="mt-8 text-xl max-w-3xl">

**OSRF is the natural steward.** What ROS is missing is **one place for vulnerabilities**:
a place to report them, advisories to track them, and a feed that scanners can read.

</div>

---

# What you have to do today

<div class="mt-10 text-2xl max-w-3xl">

<v-clicks>

- **Know your reporting path**: EU Login, ENISA's Single Reporting Platform, your national CSIRT
- **Set your support period** from expected use
- **Start documenting and keep it for 10 years**: SBOMs, technical documentation, declaration of conformity <span class="text-sm text-[var(--tufte-muted)]">(Art. 13(13))</span>
- **Pin and archive what you ship**, so rebuilding does not depend on live repositories

</v-clicks>

</div>

<div v-click class="mt-10 pt-5 border-t border-[var(--tufte-rule)] border-opacity-20 text-sm text-[var(--tufte-muted)] text-center">

Slides, sources and the full research notes:
<a href="https://github.com/prefix-dev/roscon-2026-cra-talk">github.com/prefix-dev/roscon-2026-cra-talk</a>

Free 90-minute course: <a href="https://training.linuxfoundation.org/express-learning/understanding-the-eu-cyber-resilience-act-cra-lfel1001/">Understanding the EU Cyber Resilience Act (LFEL1001)</a>, Linux Foundation

</div>


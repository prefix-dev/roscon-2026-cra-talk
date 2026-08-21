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

<!--
10 minutes, two speakers. Goal is understanding, not fear.
Everyone should leave knowing: what the CRA asks, what an SBOM is,
what VEX is for, and where ROS packaging stands today.
-->

---

# What is the CRA?

<DocLink href="https://eur-lex.europa.eu/eli/reg/2024/2847/oj" label="Reg. (EU) 2024/2847" />

<div class="mt-8 max-w-3xl">

The **Cyber Resilience Act** is an EU regulation, in force since December 2024.
It applies to any **product with digital elements** placed on the EU market.

If your robot has software and a data connection, it is in scope.

</div>

<!--
One breath. It is a product regulation like machinery or RoHS: CE mark,
conformity assessment, market surveillance. The next three slides are the
three ideas that matter. One sentence each, keep moving.
-->

---
layout: center
class: text-center
---

# Secure by design

<div class="subtitle mt-4">Essential requirements, met before you ship. Now part of CE marking.</div>

<!--
Part of CE: Art. 28 EU declaration of conformity, Arts. 29-30 the CE
marking attests CRA conformity. Same New Legislative Framework as the
Machinery Regulation. If they have done CE before, this is familiar.
-->

---
layout: center
class: text-center
---

# Software security for the product full lifecycle

<div class="subtitle mt-4">Security updates for a support period you declare. At least five years.</div>

<!--
Art. 13(8): the support period reflects the expected time in use, and is at
least five years unless the product is expected to be used for less than that.
A robot is never expected to be used for less than five years.
This is the one that bites robotics. We come back to it with the 10-year slide.
-->

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

**Everything else.** SBOM, essential requirements, technical documentation,
CE marking, a declared support period.

</div>

</div>

<div v-click class="mt-10 pt-5 border-t border-[var(--tufte-rule)] border-opacity-20">

This covers products **already on the market**, not just new ones. <span class="text-sm text-[var(--tufte-muted)]">(Art. 69(3))</span>

**What:** an actively exploited vulnerability or a severe incident.
Early warning in 24 h, full notification in 72 h. <span class="text-sm text-[var(--tufte-muted)]">(Art. 14)</span>

**Where:** ENISA's <a href="https://www.enisa.europa.eu/topics/product-security/single-reporting-platform-srp">Single Reporting Platform</a>,
which forwards to your national CSIRT. One report, once.

</div>

<!--
This is not a 2027 problem, phase one is live.
If asked: not reportable = ordinary bugs, routine patches, unexploited CVEs.
The final report deadline for a vulnerability is 14 days from when a fix is
available, not from awareness. Registration runs through EU Login.
CHECK THE WEEK OF THE TALK: as of late June 2026 the SRP was not yet live.
If it still isn't on 24 Sep, say so: the deadline is fixed, the tool is not.
Helpdesk: cra-srp-helpdesk@enisa.europa.eu
-->

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

<!--
Risk assessment: the load-bearing document. It decides which essential
requirements apply to your product. No assessment, no conformity. Most people
assume the SBOM is the centre of the CRA. It isn't.
SBOM: covering "at the very least top-level dependencies".
Vulnerability handling: monitor, remediate, disclose and distribute updates.
Support period: declared, public, and honoured. Minimum five years.
-->

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

<!--
Upstream projects may count as open-source stewards, with lighter duties.
That does not transfer any of your duties back to them. nav2 is not going
to file your ENISA report.
-->

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
      "name": "libcurl",
      "version": "8.5.0",
      "purl": "pkg:conda/libcurl@8.5.0",
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

<!--
Keep this concrete. The JSON is there so nobody has to imagine what an SBOM is.
Point at the purl line specifically.
-->

---

# And VEX? It stops the drowning.

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
        { "@id": "pkg:conda/my-robot@2.1.0" }
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

<!--
Source: VEX Industry Collaboration WG, FOSDEM 2026.
Their headline finding was that discovery and distribution are unsolved.
-->

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

<!--
This is the slide people will remember. Let the numbers sit for a beat
before clicking. It is just arithmetic, and it is unarguable.
-->

---
layout: center
class: text-center
---

# Why ROS makes this hard

<div class="subtitle mt-4">Four reasons.</div>

<!--
HANDOVER candidate. Pause, then four quick slides.
-->

---
layout: center
class: text-center
---

# One robot, many bills of materials

<div class="subtitle mt-4">Linux, ROS, Python, C++, fleet backend, microcontrollers.</div>

<!--
Different tools, different formats, different identifiers. None of them
agree on what a component is called.
-->

---
layout: center
class: text-center
---

# No lockfile in the classic path

<div class="subtitle mt-4">Rebuild that image in 2029. From what?</div>

<!--
rosdep maps keys to whatever the distro has today. Nothing records what you
actually got. apt on a Tuesday is not a reproducible input.
-->

---
layout: center
class: text-center
---

# Multiple identifiers

<div class="subtitle mt-4"><code>nav2</code> · <code>ros-jazzy-nav2</code> · <code>navigation2</code> · the CVE name</div>

<!--
The ROS package name, the apt package name, the GitHub repo name, and the
name in the vulnerability database. Same code, no shared key.
Correlation between your SBOM and the CVE database fails silently: the
scanner says zero, and zero is wrong.
-->

---
layout: center
class: text-center
---

# Vendor patches are invisible

<div class="subtitle mt-4">Not upstream. Not a new version. Same name in every SBOM.</div>

<!--
Patch Fast DDS or nav2 and you ship something no scanner can tell apart from
upstream. Straight from the Yocto Project's own CRA analysis:
"What is the product name and version of the patched package?
 The current answer: it is both A, same version."
-->

---

# Somebody already tried this

<DocLink href="https://fosdem.org/2026/schedule/event/7YG9H7-embedded-product-with-three-sboms/" label="FOSDEM 2026" />

<div class="text-sm mt-2 text-[var(--tufte-muted)]">

Marta Rybczynska built a deliberately <em>simple</em> three-processor device, running Yocto Linux,
Zephyr sensors and a Python service, then tried to do CRA-style vulnerability management on it.

</div>

<div class="mt-4 table-tight">

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

<div v-click class="callout mt-4">

Then the analysis was still wrong. The conversion **dropped the Linux kernel's CPEs**,
so zero CVEs were reported for the kernel.

</div>

<!--
Slides are CC-BY-4.0, credit her clearly.
The kernel line is the punchline. A green dashboard that means nothing.
-->

---

# Where the packaging options stand

<div class="table-tight mt-5">

| | Rebuild it in 2029 | Build-time SBOM | Vuln tracking | Support story |
|---|---|---|---|---|
| **Debian / `rosdep`** | ✗ no lockfile | ~ deb metadata | ✓ mature tracker | ✓ but on Debian's clock |
| **Docker** | ~ digest pinning | ~ inferred from layers | ~ image scanning | ✗ image is not a commitment |
| **Nix** | ✓✓ strongest | ~ thin tooling | ~ improving | ~ no product framing |
| **Yocto** | ✓ recipes + patches | ✓✓ SPDX by default | ✓ `cve-check` | ✓ **4-year LTS** |
| **conda / Pixi** | ✓✓ `pixi.lock` | ~ via Syft on the env | ~ CPE-not-PURL gap | ~ channel-based |

</div>

<div v-click class="mt-5 text-sm">

**Nobody is finished.** Yocto has the best SBOM story and admits vendor patches break it.
Nix has the best reproducibility and the thinnest tooling. Conda has a real
cross-language lockfile and an incomplete PURL story.

</div>

<div v-click class="mt-3 text-sm text-[var(--tufte-muted)]">

The consistent finding at FOSDEM: **a good SBOM starts from a lockfile.**

</div>

<!--
Vendor-neutral on purpose. Naming Pixi's own gap out loud is what makes
the rest of the row credible.
-->

---

# What's being built

<div class="grid grid-cols-2 gap-10 mt-8">

<div>

### In the tooling

<v-clicks>

- **PURLs for conda packages**, `conda/ceps#63`
- **Syft** now reads `conda-meta`, merged
- **Sigstore attestations** for published artifacts, CEP 27
- **`cargo-auditable`**, so Rust deps stop hiding inside binaries

</v-clicks>

</div>

<div>

### In the ecosystems

<v-clicks>

- Yocto, Zephyr, SwiftPM, BuildStream and pkgconf are all adding **build-time SBOMs**
- Erlang/OTP ships source SBOMs, OSV scanning and OpenVEX, and their foundation became a **CNA**
- Metadata is being curated at scale by Maven Heaven and Nixpkgs Clarity

</v-clicks>

</div>

</div>

<div v-click class="mt-10 pt-5 border-t border-[var(--tufte-rule)] border-opacity-20">

There is no ROS entry in that second list yet.

</div>

<!--
Erlang is the template worth pointing at: the foundation became a CNA so the
ecosystem could issue and map CVEs. Directly applicable to OSRA.
-->

---
layout: center
class: text-center
---

# What ROS needs to figure out together

<div class="text-left max-w-2xl mx-auto mt-10">

<v-clicks>

- **Build-time SBOMs** from `colcon`, rather than bolted on afterwards
- **Stable identifiers**, one PURL per ROS package, across apt, conda and source
- **A steward** who can issue advisories. Erlang's foundation became a CNA. Ours could.
- **Better metadata**: licenses, origins, `rosdep` mappings

</v-clicks>

</div>

<div v-click class="mt-14 text-lg">

Two things you can start today. **Write down your support period**, and
**pin something you can rebuild in five years.**

</div>

<div v-click class="mt-10 text-sm text-[var(--tufte-muted)]">

Slides, sources and the full research notes:
<a href="https://github.com/prefix-dev/roscon-2026-cra-talk">github.com/prefix-dev/roscon-2026-cra-talk</a>

</div>

<!--
Land on the two actions. They cost nothing and both are genuinely useful
regardless of which packaging tool anyone picks.
-->

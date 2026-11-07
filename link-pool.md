# iOS Sideloading Links

## Index

- [Guides & learning](#guides--learning)
  - [General sideloading guides](#general-sideloading-guides)
  - [Anti-revoke DNS guides](#anti-revoke-dns-guides)
  - [VPN with custom DNS](#vpn-with-custom-dns)
- [DNS profiles & testing](#dns-profiles--testing)
- [Tools & utilities](#tools--utilities)
  - [Certificate & provisioning tools](#certificate--provisioning-tools)
  - [Source & repository tools](#source--repository-tools)
  - [Other tools](#other-tools)
  - [Toolkit sites](#toolkit-sites)
- [Signer apps & installers](#signer-apps--installers)
  - [Signer and installer projects](#signer-and-installer-projects)
  - [Signer app installers](#signer-app-installers)
  - [Web signers](#web-signers)
- [Certificates & signing services](#certificates--signing-services)
  - [Free/shared certificate resources](#freeshared-certificate-resources)
  - [Paid certificate providers](#paid-certificate-providers)
- [App sources & repositories](#app-sources--repositories)
- [Shortcuts](#shortcuts)
- [Communities](#communities)

[Main cheatsheet](README.md#contents) · [Compare the five routes](routes.md#compare-the-routes)

**Link labels matter:** **Official** = maintained by the named project/platform; **community** = independent guide/tool; **provider** = third-party signing/DNS/certificate service. Listing a provider or IPA source is **not an endorsement**. Prices, trustworthiness, URLs and supported iOS versions can change.

## Guides & learning

### General sideloading guides

- [TechyBuff — sideloading guides](https://techybuff.com/category/sideload/) — community how-tos and troubleshooting.
- [WSF — guides and toolkit](https://wsfstore.cc) — provider-hosted free/paid sideloading guides.
- [Apple — free vs. paid developer account rules](https://developer.apple.com/help/account/basics/about-your-developer-account) — official limits and Personal Team rules.
- [Apple — signing certificate types](https://developer.apple.com/help/account/certificates/certificates-overview) — official technical reference.
- [Apple — PPQ verification](https://developer.apple.com/help/account/provisioning-profiles/provisioning-profile-updates/) — official development/ad hoc check-in rules.

### Anti-revoke DNS guides

- [Custom NextDNS profile — sideloading.vercel.app](https://sideloading.vercel.app/guide) — community guide (some content requires an interactive page).
- [NextDNS DNS method — surrel14 (GitHub)](https://github.com/surrel14/dnsmethod.github.io/blob/main/README.md) — detailed community walkthrough.
- [Cloudflare Gateway DNS method — DanielTheManiel (GitHub)](https://github.com/Dan1elTheMan1el/Antirevoke-Cloudflare-DNS/blob/main/README.md) — self-managed Cloudflare alternative.
- [TechyBuff — anti-revoke articles](https://techybuff.com/?s=anti+revoke) — community explanations and workarounds.

### VPN with custom DNS

- [VPN + DNS guide — sideloading.vercel.app](https://sideloading.vercel.app/vpn) — community reference.
- [TechyBuff — VPN with custom DNS](https://techybuff.com/vpn-setup-custom-dns/) — resolver and DNS-leak considerations.
- [TechyBuff — VPN article search](https://techybuff.com/?s=VPN) — additional community posts.

**Reminder:** DNS blocking does not undo a certificate revocation; VPN/DNS compatibility and PPQ instructions differ by setup. See [the shared DNS notes](README.md#dns-ppq--vpn).

## DNS profiles & testing

**Self-managed DNS:**

- [NextDNS — dashboard/setup](https://my.nextdns.io/) — configure your **own** profile; avoid sharing account/profile-specific URLs publicly.
- [Cloudflare Zero Trust](https://one.dash.cloudflare.com/) — custom DNS gateway option (see the Cloudflare guide above).

**Third-party DNS/profile pages** (inspect a profile's source, ownership and permissions before installing):

- [FlareStore — DNS profiles and DIY guidance](https://flarestore.app/dns/)
- [Khoindvn — sideloading site/profiles](https://khoindvn.io.vn/)
- [WSF — configuration profiles](https://wsfteam.xyz/configprofiles)
- [TechyBuff — ESign/DNS resources](https://techybuff.com/esign/)

**Testing:** [NextDNS test endpoint](https://test.nextdns.io) — useful for confirming NextDNS is the active resolver; it **does not** by itself prove all verification traffic is blocked.

## Tools & utilities

### Certificate & provisioning tools

- [FlareStore — certificate & provisioning checker](https://mobilepro.vision/)
- [WSF — certificate checker](https://wsfstore.cc/tools/checker)
- [WSF — change `.p12` password](https://wsfstore.cc/tools/cert-password)
- [FlareStore — IPSW signing status](https://flarestore.app/ipsw/) — firmware-signing information, **not** an IPA certificate check.

**Privacy:** Uploading a `.p12` and its password to a third-party checker may expose the private signing key. Prefer non-sensitive serial/profile checks or a trusted local tool when possible.

### Source & repository tools

- [Alt Direct](https://altdirect.app/) — simplify adding a repository source to supported managers.
- [WSF — repository browser](https://wsfstore.cc/tools/repo-browser) — browse source contents.
- [FlareStore — repository creator](https://flarestore.app/repo-creator/)
- [WSF — repository creator](https://wsfstore.cc/tools/repository-creator)
- [FlareStore — repo decoder/converter](https://flarestore.app/repo-decoder/)

### Other tools

- [FlareStore — device UDID tool](https://udid.website/)
- [WSF — UDID/device details](https://wsfstore.cc/tools/udid)
- [WSF — `.plist` installation-manifest creator](https://wsfstore.cc/tools/repo-creator) — *different* tool from its repository creator.
- [FlareStore — signing badge studio](https://flarestore.app/badges/)

A **UDID** identifies a device for provisioning. Only submit it to a site that needs it and that you trust.

### Toolkit sites

- [FlareStore toolkit](https://flarestore.app/)
- [WSF toolkit](https://wsfstore.cc/)

## Signer apps & installers

### Signer and installer projects

**Original project references where available:**

- [Feather — GitHub](https://github.com/claration/feather) — on-device IPA signer/installer.
- [SideStore — documentation](https://docs.sidestore.io/) · [GitHub](https://github.com/SideStore/SideStore) — Apple Account signing and device-side refresh.
- [AltStore Classic — official FAQ](https://faq.altstore.io/altstore-classic/your-altstore) — Apple Account signing through the AltStore ecosystem.
- [Sideloadly — official site](https://sideloadly.io/) — desktop signing/installation and refresh.
- [Scarlet — official docs](https://docs.usescarlet.com/) — multiple installation arrangements.
- [LiveContainer — GitHub](https://github.com/LiveContainer/LiveContainer) · [installation docs](https://livecontainer.github.io/docs/installation) — **guest-app launcher**, not a standalone signing route.
- [TrollStore — original GitHub](https://github.com/opa334/TrollStore) · [installation guidance](https://ios.cfw.guide/installing-trollstore/) — persistent installation on compatible older iOS builds.
- [Accretion — website](https://getaccretion.app/) · [source code](https://github.com/GETAccretion/getaccretion.github.io) — app/source discovery and associated services.
- [WSF Portal](https://wsfteam.xyz/) · [project GitHub](https://github.com/WSF-Team/WSF) — provider's portal and resources.
- [FlareStore app](https://flarestore.app/app/) — provider's on-device signing/app management offering.

**Related, version-specific tools:** [Lara](https://github.com/rooootdev/lara) · [SparseBox](https://github.com/khanhduytran0/SparseBox) — not separate signing routes; check upstream version/hardware support.

### Signer app installers

- [FlareStore — signer app installer](https://flarestore.app/installer/) — third-party distribution.
- [WSF — signer app installer](https://wsfstore.cc/tools/apps) — Feather, ESign, KSign and Scarlet.
- [Khoindvn — ESign/KSign installer resources](https://khoindvn.io.vn/) — third-party distribution.
- [TechyBuff — ESign](https://techybuff.com/esign/) · [KSign](https://techybuff.com/ksign/) · [Feather](https://techybuff.com/feather/) — third-party installation guides/downloads.
- [SideInstaller — project website](https://sideinstaller.net/) · [GitHub](https://github.com/FrizzleM/SideInstaller) — **independent** on-device sideloading/setup tool; **not the official SideStore project**.

Where no verified original ESign/KSign project download is established here, these links are labeled as **third-party distribution**, not “official.”

### Web signers

- [WSF web signer](https://wsfstore.cc/tools/signer)
- [FlareStore web signer](https://flarestore.app/signer/)
- [Arctic web signer](https://sign.arcticsign.app/)
- [DaiSign](https://daisign.lol/)
- [KravaSigner Web](https://www.kravasign.com/sign)

A web signer may receive your **IPA, `.p12`, password and provisioning profile**. Check where signing happens and what is stored before uploading private assets.

## Certificates & signing services

### Free/shared certificate resources

- [FlareStore — shared certificates](https://flarestore.app/certs/)
- [WSF — shared certificates](https://wsfteam.xyz/certificates)
- [TechyBuff — ESign certificate resources](https://techybuff.com/esign/)
- [TechyBuff — Feather certificate resources](https://techybuff.com/feather/)

Shared certificates can already be revoked, expire unexpectedly, or fail device verification. These are **community/provider resources**, not legitimate Apple public-distribution certificates.

### Paid certificate providers

- [ArcticSign](https://arcticsign.app/)
- [FlareStore](https://flarestore.app/) (shop linked from its site)
- [RustSign](https://rustsign.com/)
- [Signulous](https://www.signulous.com/register) — clean link without a referral parameter.
- [WSF](https://wsfstore.cc/purchase)
- [KravaSign](https://www.kravasign.com/) — also offers the web signer above.

Review **device coverage, profile/entitlements, PPQ conditions, delivery time, revocation-replacement terms, and refunds**; a seller's “no revoke” wording is not an Apple guarantee. [What paid signing actually means](routes.md#route-4--paid-signing-certificates--services).

## App sources & repositories

- [Accretion — source index](https://getaccretion.app/) — browse many AltStore/SideStore sources; some downloads require an account.
- [Accretion — curated source URL](https://getaccretion.app/curated.json) · [full source URL](https://getaccretion.app/source.json) — original links preserved; **confirm they still work** before importing (direct JSON endpoints weren't independently verified).
- [Repo Installer ++ — GitHub](https://github.com/Kleo08s/RepoInstaller) · [Shortcut](https://browse.shortcuty.app/shortcut/46d02ab5-97d9-42ba-a042-e0bdc6d731f2) · [changelog](https://routinehub.co/shortcut/21714/changelog)
- [iOSBoom IPA library](https://iosboom.com/ipa-library) — third-party IPA listing; individual downloads require independent trust checks.

A repo being listed does **not** mean every app is signed, maintained, available or safe. [How sources and updates work](README.md#app-sources--ipa-safety).

## Shortcuts

- [Sideloading category — Shortcuty](https://browse.shortcuty.app/category/Sideloading)
- [NextDNS Manager](https://browse.shortcuty.app/shortcut/b7ca6055-9d7d-4b86-a5a9-151f0e6b811c)
- [IPA Updates — Discord notifier](https://browse.shortcuty.app/shortcut/6da7750d-aeab-4747-bd0d-35ca4c99a3b8)
- [Sideloading Helper](https://browse.shortcuty.app/shortcut/631a6113-3b13-430e-a159-0765b2ea54b2)
- [Sign & Install IPAs](https://browse.shortcuty.app/shortcut/70953c10-fffb-4c82-9614-eb270d7b69d5)
- [Quick Refresh — SideStore](https://browse.shortcuty.app/shortcut/43a01d47-8ba5-494e-bdc4-d746d465a356)
- [LiveContainer Repo Installer](https://browse.shortcuty.app/shortcut/e9035be5-0dbd-41d0-99c6-3b2f46bcfa45)
- [LiveContainer Repo Browser & Installer](https://browse.shortcuty.app/shortcut/2915d5fe-df56-4da7-abd8-080824b14917)

Review permissions, network requests and any account/API tokens used by a shortcut before running it.

## Communities

- [FlareStore Discord](https://discord.gg/FPpzcpQ2JJ)
- [Khoindvn Discord](https://discord.com/invite/cQjuP9TEp3)
- [RustSign Discord](https://discord.com/invite/rustsign)
- [ArcticSign Discord](https://discord.com/invite/arcticsign)
- [WSF Discord](https://discord.com/invite/kwvwkCBhxT)
- [r/sideloaded Discord](https://discord.gg/pUHBuxYzSG)
- [r/Sideload Discord](https://discord.gg/eZNNH4fSHb)
- [r/sideloaded on Reddit](https://www.reddit.com/r/sideloaded/)


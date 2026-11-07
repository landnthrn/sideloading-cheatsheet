# Pick Your Route

**Five main ways to sideload iOS apps.** The route determines your app limits, how often you must refresh, and what can make the apps stop working. The signer app is only one part of the setup.

For unfamiliar terms, use the [cheatsheet](cheatsheet.md#the-basics). For official project links, tools, and guides, see the [link pool](link-pool.md#guides--learning).

## On this page

- [Compare the routes](#compare-the-routes)
- [Route 1 — Free Apple Account signing](#route-1--free-apple-account-signing)
  - [Overview](#overview)
  - [Pros](#pros)
  - [Cons](#cons)
  - [Example setups](#example-setups)
- [Route 2 — Paid Apple Developer Account signing](#route-2--paid-apple-developer-account-signing)
  - [Overview](#overview-1)
  - [Pros](#pros-1)
  - [Cons](#cons-1)
  - [Example setup](#example-setup)
- [Route 3 — Free shared/revoked enterprise certificates + DNS](#route-3--free-sharedrevoked-enterprise-certificates--dns)
  - [Overview](#overview-2)
  - [Pros](#pros-2)
  - [Cons](#cons-2)
  - [Example setup](#example-setup-1)
- [Route 4 — Paid signing certificates & services](#route-4--paid-signing-certificates--services)
  - [Overview](#overview-3)
  - [Pros](#pros-3)
  - [Cons](#cons-3)
  - [Example setup](#example-setup-2)
- [Route 5 — TrollStore & compatible exploit-based methods](#route-5--trollstore--compatible-exploit-based-methods)
  - [Overview](#overview-4)
  - [Pros](#pros-4)
  - [Cons](#cons-4)
  - [Example setup](#example-setup-3)

## Compare the routes

| Route | Cost | Installed apps & refreshing | Main drawback |
| --- | --- | --- | --- |
| [**1. Free Apple Account**](#route-1--free-apple-account-signing) | Free | Normally **3 installed apps**, **10 App IDs**, and a refresh **every 7 days** | Regular refreshing; some app capabilities unavailable |
| [**2. Own Apple Developer Account**](#route-2--paid-apple-developer-account-signing) | **US$99/year** (varies by region) | No free-account 3-app cap; profiles/certificates often last around **1 year** | Annual cost and signing setup |
| [**3. Shared/revoked certificate + DNS**](#route-3--free-sharedrevoked-enterprise-certificates--dns) | Free | No normal 3-app cap or weekly refresh | Can break abruptly; DNS/VPN complications |
| [**4. Paid signing certificate/service**](#route-4--paid-signing-certificates--services) | Seller-dependent | No normal 3-app cap; commonly sold as a **~1-year** service | Depends on seller and certificate remaining valid |
| [**5. TrollStore**](#route-5--trollstore--compatible-exploit-based-methods) | Free | No ordinary app-slot cap or seven-day refresh | Only works on certain iOS versions/builds |

**Quick distinction:** **SideStore, Sideloadly, Feather, and ESign are tools.** The route is the kind of signing/installation behind them. **LiveContainer** works alongside some routes to host guest apps; it isn't a sixth signing route.

The setups below are **realistic examples**, not the only possible combinations. Tool compatibility and iOS requirements change; check the project's current instructions before setting one up.

## Route 1 — Free Apple Account signing

**What it is:** Use a normal Apple Account to authorize IPAs for personal testing. Usually done with **SideStore, AltStore Classic, or Sideloadly**.

### Overview

- **Cost:** Free.
- **App limit:** Normally **3 directly installed apps per device**, **10 registered App IDs**, and **3 registered devices** with free Personal Team signing. A signing helper may use a slot, and app extensions can use additional App IDs. [Apple’s rules](https://developer.apple.com/help/account/basics/about-your-developer-account).
- **Refreshing:** Apps need signing renewed every **7 days**, before they stop opening. Tools can automate this, but only if the refresh completes.
- **Notifications:** Remote push notifications and some advanced features may not work; local notifications are different.
- **Good for:** People who want a free signing route and don't mind maintaining it.
- **DNS / VPN:** No anti-revoke DNS is needed. **SideStore uses LocalDevVPN for installing and refreshing**, which serves a different purpose. [SideStore setup](https://docs.sidestore.io/docs/installation/prerequisites).

### Pros

- Free, with no need for shared/leaked enterprise certificates.
- Widely supported by current Apple Account signing tools.
- **LiveContainer** makes it practical to run multiple compatible guest apps without each taking a normal installed-app slot.

### Cons

- Miss a refresh and your app may stop opening until it's signed again.
- App IDs, installed-app slots, and entitlements are restricted.
- Setup may involve a computer, pairing file, local VPN, or desktop helper, depending on the tool.

### Example setups

- **SideStore + LiveContainer + refresh shortcut:** Run compatible guest IPAs inside LiveContainer, use SideStore to keep the installed host signed, and optionally use an iOS Shortcut to trigger refreshes. The official **LiveContainer + SideStore** package combines both into one of the three free app slots. **Verify that refreshes actually succeed.**
- **Sideloadly + desktop auto-refresh:** Install a few IPAs from Windows or macOS using your Apple Account. Sideloadly's desktop helper handles refreshing when the required computer connection is available. This is an alternative setup, **not the recommended way to install LiveContainer**.

**Good to know:** LiveContainer doesn’t remove the need to refresh the *container*. Guests can have feature limitations, including remote push and extensions. See [LiveContainer explained](cheatsheet.md#livecontainer). Other app-limit workarounds such as [Lara](https://github.com/rooootdev/lara) and [SparseBox](https://github.com/khanhduytran0/SparseBox) only support certain iOS builds and don’t automatically remove the 10-App-ID restriction. Emulators needing JIT require a separate compatible method. [JIT basics](cheatsheet.md#jit-just-in-time-compilation).

## Route 2 — Paid Apple Developer Account signing

**What it is:** Pay Apple for **your own Apple Developer Program membership**, giving you control of the developer team, certificates, provisioning profiles, and registered devices.

### Overview

- **Cost:** Usually **US$99/year**, with regional price differences.
- **App limit:** No free-account **3-app / 10-App-ID cap**. For ad hoc signing, Apple generally allows up to **100 registered devices per product family per membership year**, subject to its [device rules](https://developer.apple.com/help/account/devices/devices-overview/).
- **Refreshing:** No seven-day cycle. Certificates and provisioning profiles have **separate expiry dates**; many common arrangements are valid for roughly **one year**.
- **Notifications:** Remote push, app groups, and extensions are possible **when the app and signing configuration support them**.
- **Good for:** Frequent sideloaders who want to manage their own signing credentials.
- **DNS / VPN:** No anti-revoke DNS is needed. If your profile requires PPQ verification, `ppq.apple.com` must be reachable at first launch; a VPN is fine if it does not block that check.

### Pros

- You control the developer team and signing files instead of depending on a reseller.
- No weekly app refreshing or ordinary free-account slot cap.
- More flexibility with legitimate app capabilities and provisioning.

### Cons

- An annual Apple membership fee.
- You handle setup, device registration, certificate/profile renewals, and related problems.
- Apple can still revoke certificates or restrict a developer account.

### Example setup

- **Your Apple Developer Account + ad hoc profile + Feather:** Register your iPhone's **UDID** under your developer team, create suitable signing files, import them into Feather, then sign and install your IPAs. You can also use Xcode or a compatible desktop tool for the signing/installing part.

**Good to know:** **Paying Apple for your own developer membership is different from buying a certificate from a seller** (Route 4). A paid membership doesn’t automatically make push notifications work in every IPA. Development and ad hoc profiles serve different purposes, and certain profiles require Apple’s [PPQ verification](https://developer.apple.com/help/account/provisioning-profiles/provisioning-profile-updates/). Check [Apple’s signing certificates](https://developer.apple.com/help/account/certificates/certificates-overview) and [membership rules](https://developer.apple.com/help/account/membership/program-enrollment/).

## Route 3 — Free shared/revoked enterprise certificates + DNS

**What it is:** Sign IPAs with publicly shared signing files, often from enterprise certificates. **Shared doesn’t necessarily mean revoked**, but Apple may revoke those credentials. If a certificate is revoked, some users rely on **anti-revoke DNS** to block certain verification requests. **DNS doesn’t make a revoked certificate valid again.**

### Overview

- **Cost:** Free.
- **App limit:** No normal free-account three-app cap.
- **Refreshing:** No ordinary seven-day refresh. Certificate expiry and verification can still end access.
- **Notifications:** May work with certain certificates and apps; not guaranteed.
- **Good for:** People who prefer free, on-device installation and accept that their apps could stop working without warning.
- **DNS / VPN:** Anti-revoke DNS is mainly used when relying on revoked signing credentials. A VPN that overrides the blocking DNS can break the workaround. PPQ-blocking advice is method-specific, not a universal step.

### Pros

- No Apple Developer membership fee or weekly app refresh routine.
- Can install many IPAs while the signing arrangement remains usable.
- Common tools include **ESign, KSign, and Feather**, often without needing a computer after setup.

### Cons

- **Fragile:** certificate revocation, an iOS verification check, or a network/DNS change can break apps.
- VPNs and reboots may interfere with DNS-dependent installs.
- Shared credentials and third-party profiles bring trust and security risks; app entitlements may be limited.

### Example setup

- **NextDNS anti-revoke profile + ESign or Feather + shared certificate:** Configure a DNS profile, obtain a working signer, import the certificate and matching provisioning profile, then sign and install an IPA. Some community methods temporarily change a `ppq.apple.com` rule while installing; **whether that applies depends on the actual signing arrangement**.

**Good to know:** A certificate that says it expires next year **can still be revoked today**. “Blacklisted” doesn’t automatically mean a device is permanently banned from every signing method. Community advice about toggling `ppq.apple.com` or airplane mode is **not a universal requirement or guarantee**; Apple documents [PPQ checks](https://developer.apple.com/help/account/provisioning-profiles/provisioning-profile-updates/) for certain development/ad hoc profiles. See [DNS/VPN essentials](cheatsheet.md#anti-revoke-dns-ppq--vpn) and [DNS guides](link-pool.md#anti-revoke-dns-guides).

Apple’s [Enterprise Developer Program](https://developer.apple.com/programs/enterprise/) is intended for organizations’ internal apps, not public distribution using leaked certificates.

## Route 4 — Paid signing certificates & services

**What it is:** Pay a **third-party provider** to register your device and supply signing files or an app installation service. The seller normally controls the Apple developer team, while you receive access for your registered device.

### Overview

- **Cost:** Depends on provider, device, and plan.
- **App limit:** Normally no free-account three-app limit.
- **Refreshing:** No weekly free-account refresh; many services advertise **about a year**. The service period, certificate expiry, profile expiry, and replacement warranty may end on different dates.
- **Notifications:** Can work when the app and provisioning allow remote push; don't assume all paid plans support it.
- **Good for:** People who want a convenient signing arrangement without running their own developer team.
- **DNS / VPN:** A working paid certificate normally needs **no anti-revoke DNS**. Ordinary VPN use is generally fine if required Apple verification still works. A PPQ-required profile must reach `ppq.apple.com` for its first-launch check; a genuinely PPQ-less arrangement does not require that particular check.

### Pros

- Usually simpler than managing an Apple Developer Program membership.
- No weekly seven-day refresh; many apps can be signed with a working plan.
- Some providers include a replacement policy if a signing certificate gets revoked.

### Cons

- Costs money, and **certificates can still be revoked**.
- Relies on the provider's account, delivery, policies, and customer support.
- App features, certificate export, device coverage, and replacement terms vary by plan. A replacement certificate can require re-signing and reinstalling apps, so protect your [app data](cheatsheet.md#what-happens-to-app-data).

### Example setup

- **Paid certificate provider + device UDID + Feather:** Buy a plan from a provider (for example, **ArcticSign** or **Signulous**), register your device, obtain the `.p12` and `.mobileprovision` if the plan provides them, import them into Feather, then sign and install IPAs. Some providers instead offer an installer or web signer.

**Before buying:** Check **UDID registration, delivery time, exportable `.p12`/profile files (if you want to use Feather or ESign), notifications/entitlements, PPQ requirements, renewal, revocation replacement, waiting periods, and refunds**. Some plans only provide an installer or web signer. “Revoke protection” usually describes a seller’s replacement policy—not immunity from revocation.

**“PPQ” and “PPQ-less”** are seller/community labels, not official Apple certificate classes. If Apple requires a PPQ check, **blocking `ppq.apple.com` may prevent first launch**. If a profile really does not require that check, there is no PPQ-related reason to block it. Neither label guarantees protection from revocation.

## Route 5 — TrollStore & compatible exploit-based methods

**What it is:** **TrollStore** uses a CoreTrust vulnerability to install compatible IPAs persistently, without ordinary renewable Apple Account signing. **You don't need a jailbreak to use it.**

### Overview

- **Cost:** Free.
- **App limit:** No ordinary Apple Account three-app or ten-App-ID cap.
- **Refreshing:** No seven-day refresh while the supported installation mechanism remains in place.
- **Notifications:** Feature support varies by app; the exploit doesn’t guarantee every entitlement or service.
- **Compatibility:** The [official project](https://github.com/opa334/TrollStore) lists iOS/iPadOS **14.0 beta 2–16.6.1**, **16.7 RC (20H18)**, and **17.0**. Final 16.7.x builds other than that RC and 17.0.1+ aren’t supported by those CoreTrust bugs. Installation may be narrower; use the [version-specific guide](https://ios.cfw.guide/installing-trollstore/).
- **Good for:** Someone already using a compatible older iOS version who wants persistent apps.
- **DNS / VPN:** TrollStore does not require anti-revoke DNS or a special VPN arrangement to keep its apps installed.

### Pros

- Persistent installation, without regular app refreshing or annual signing fees.
- No ordinary free-account app-slot limit.
- Greater flexibility than conventional signing for some apps and capabilities.

### Cons

- Only available on certain older iOS versions/builds and compatible devices.
- Getting TrollStore installed may involve a device-specific procedure.
- Updating or restoring iOS can remove the vulnerability needed to keep using TrollStore.

### Example setup

- **Supported iOS build + TrollStore + IPA:** Check your exact device/iOS build, use its supported method to install TrollStore, then install compatible IPA files through TrollStore itself. There's no separate ESign/Feather certificate workflow or seven-day refresh for those apps.

**Good to know:** **Check version compatibility before updating.** If you use a supported iOS version specifically for TrollStore, an ordinary iOS update may permanently remove that option. TrollStore isn’t a jailbreak, and persistent installation doesn’t mean every app works or that an untrusted IPA is safe.

---

**More practical details:** [Signing, app updates, and data](cheatsheet.md#installing-updating--keeping-app-data) · **Find tools:** [Link pool](link-pool.md#signer-apps--installers)

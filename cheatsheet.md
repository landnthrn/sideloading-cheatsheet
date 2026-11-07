# Sideloading Cheatsheet

**The practical side of iOS sideloading:** what the files and tools are for, how apps stay installed, what can go wrong, and how to keep your data. To compare methods, see [Pick your route](pick-your-route.md#compare-the-routes). For downloads and guides, use the [link pool](link-pool.md#guides--learning).

## On this page

- [The basics](#the-basics)
  - [IPA (`.ipa`)](#ipa-ipa)
  - [Signer and installer](#signer-and-installer)
  - [Signing certificate and `.p12` file](#signing-certificate-and-p12-file)
  - [Provisioning profile (`.mobileprovision`)](#provisioning-profile-mobileprovision)
  - [Entitlements](#entitlements)
  - [Bundle ID and App ID](#bundle-id-and-app-id)
  - [UDID](#udid)
  - [Developer trust and revocation](#developer-trust-and-revocation)
- [Tools & what they do](#tools--what-they-do)
  - [Common tools](#common-tools)
    - [On-device signers for certificate-based setups](#on-device-signers-for-certificate-based-setups)
    - [Apple Account signing tools](#apple-account-signing-tools)
    - [Discovery and distribution](#discovery-and-distribution)
  - [LiveContainer](#livecontainer)
  - [TrollStore](#trollstore)
  - [On-device vs. web signing](#on-device-vs-web-signing)
- [Signing files, certificates & capabilities](#signing-files-certificates--capabilities)
  - [Signing arrangements & certificate labels](#signing-arrangements--certificate-labels)
    - [Actual signing arrangements](#actual-signing-arrangements)
    - [Labels you'll encounter in sideloading communities](#labels-youll-encounter-in-sideloading-communities)
  - [Notifications, extensions & JIT](#notifications-extensions--jit)
    - [Push notifications (remote notifications)](#push-notifications-remote-notifications)
    - [Local notifications](#local-notifications)
    - [Widgets, extensions, and app groups](#widgets-extensions-and-app-groups)
    - [JIT (Just-in-Time compilation)](#jit-just-in-time-compilation)
  - [Signer settings & installation options](#signer-settings--installation-options)
    - [Common switches you might see in Feather, ESign, or other signers](#common-switches-you-might-see-in-feather-esign-or-other-signers)
- [Installing, updating & keeping app data](#installing-updating--keeping-app-data)
  - [What each action actually does](#what-each-action-actually-does)
  - [What happens to app data?](#what-happens-to-app-data)
  - [Removing signing components](#removing-signing-components)
- [Expiration, revocation & app failures](#expiration-revocation--app-failures)
  - [What expires?](#what-expires)
  - [Revocation, blacklisting & account restrictions](#revocation-blacklisting--account-restrictions)
  - [When something stops working](#when-something-stops-working)
- [Anti-revoke DNS, PPQ & VPN](#anti-revoke-dns-ppq--vpn)
- [App sources & IPA safety](#app-sources--ipa-safety)
- [Quick workflows & good habits](#quick-workflows--good-habits)
  - [Quick process flows](#quick-process-flows)
  - [Good habits](#good-habits)

## The basics

<details>
<summary>Summary</summary>
<p>An IPA needs acceptable signing and authorization before iOS can run it. Certificates, provisioning profiles, and DNS profiles have different jobs; which rules apply depends on the sideloading route.</p>
</details>

**Sideloading** means installing an iOS app outside the usual App Store process. Having an app's file isn't enough: iOS must accept how that app was signed or installed.

**Typical certificate-based flow:** `Get IPA → Sign IPA → Install → Trust/verify if required → Open`

### IPA (`.ipa`)

- The app package you download/import into a signer.
- It can contain the original app or a modified version. Being an `.ipa` doesn't mean it's safe, compatible, or correctly signed.

### Signer and installer

- A **signer** applies signing credentials to an IPA. Code signing lets iOS check the app’s signing identity and whether its signed contents have changed. **ESign, KSign, and Feather** are familiar signers.
- An **installer** gets the authorized app onto the device. Many signer apps do both, but signing a file and installing it are separate steps.

### Signing certificate and `.p12` file

- A signing certificate identifies a developer/signing identity; its corresponding **private key** is needed to actually sign the app.
- A `.p12` (or `.pfx`) commonly holds the certificate **and private key**, protected by a password. Some tools import it directly; Apple Account-based tools often handle credentials for you.
- **Keep your private `.p12` and password secure.** Sharing them can let someone else sign apps with your identity.

### Provisioning profile (`.mobileprovision`)

- Describes which app identity, signing team, capabilities, and (when applicable) registered devices are authorized.
- Must match the signing arrangement. A valid certificate alone does **not** guarantee an app can install or open.

### Entitlements

- Signing-related permissions for capabilities such as remote push notifications, app groups, and certain extensions.
- The app must request the capability, and its provisioning/signing arrangement must permit it. Installing successfully doesn't mean every feature will work.

### Bundle ID and App ID

- A **Bundle ID** identifies an app, for example `com.example.app`. Changing it can create a separate app identity and interfere with updates or saved data.
- An **App ID** is registered under an Apple developer team, generally associating a Team ID with a Bundle ID. The **10 App IDs** limit on free signing is *not* the same as the **3 installed apps** limit.

### UDID

- A unique device identifier used to register specific iPhones or iPads for signing methods such as ad hoc provisioning.
- A seller may ask for your UDID to add your device to its signing profile. A UDID is **not** a certificate or an Apple Account.

### Developer trust and revocation

- **Developer trust:** Some installed apps need you to approve or verify their developer in iOS settings. Trusting the developer does not verify the IPA's safety.
- **Revocation:** Apple invalidates signing authorization *before* its listed expiry. Apps depending on it may stop opening, even if the certificate's expiry date is months away.

**Don't confuse these:** a **provisioning profile** helps authorize an app; a **DNS configuration profile** changes which DNS service the phone uses. They are unrelated kinds of profiles.

## Tools & what they do

**Pick your route before picking a tool.** SideStore uses Apple Account signing; Feather and ESign commonly work with imported signing certificates. Changing signer apps doesn't remove the rules attached to your Apple Account or certificate.

### Common tools

#### On-device signers for certificate-based setups

<details>
<summary>Summary</summary>
<p>ESign, KSign, Feather, and similar on-device tools let you sign and install IPAs using imported certificate files. The signing credentials and installation method still determine what works.</p>
</details>

- **ESign / KSign:** Import an IPA and signing files (`.p12` and provisioning profile), sign the IPA, then install it. Common in free shared-certificate and paid-certificate setups.
- **[Feather](https://github.com/claration/feather):** Does the same core signing/installing job, with app-source browsing and certificate management. Its local installation method can depend on DNS and HTTPS working correctly.
- **[FlareStore](https://flarestore.app/app/):** Offers an app/signing manager as well as separate certificate, installer, and web-signing services. Those are different offerings, even when they're on the same site.
- **[Scarlet](https://docs.usescarlet.com/):** An app manager/installer with several ways to install apps. Which certificate or method it uses determines reliability and limitations.

#### Apple Account signing tools

<details>
<summary>Summary</summary>
<p>SideStore, AltStore Classic, and Sideloadly use Apple Account signing. Their main differences are how you install apps and how you keep them refreshed.</p>
</details>

- **[SideStore](https://docs.sidestore.io/):** Free Apple Account signing with **refreshing from the device** after setup. Uses pairing information and a local VPN (LocalDevVPN); often paired with LiveContainer.
- **[AltStore Classic](https://faq.altstore.io/altstore-classic/your-altstore):** Free Apple Account signing, app refreshing, and app activation/deactivation. Supports AltServer and newer Remote AltServer workflows; availability and requirements depend on the version.
- **[Sideloadly](https://sideloadly.io/):** Desktop app for Windows/macOS that signs and installs IPAs, with a desktop helper for automatic refreshing. Useful if you're comfortable periodically relying on a computer.

#### Discovery and distribution

- **[Accretion](https://getaccretion.app/) / [WSF Portal](https://wsfteam.xyz/):** Help discover apps, sources, downloads, and related services. Check the specific feature: *listing* an IPA isn't the same as *signing* or *installing* it.
- **Signer installers / web signers:** Services that help you install signer apps or submit IPAs for signing. See the [link pool](link-pool.md#signer-apps--installers) for actual providers and tools.

Two other names deserve their own explanation because they work differently from ordinary signer apps.

### LiveContainer

<details>
<summary>Summary</summary>
<p>LiveContainer hosts multiple guest apps inside one signed app. SideStore is a common way to keep that host refreshed, but guests have limitations and aren’t equivalent to standalone installations.</p>
</details>

**[LiveContainer](https://github.com/LiveContainer/LiveContainer) lets you run multiple compatible IPAs inside one installed app**, instead of installing every guest app separately on your Home Screen.

- **Why people use it:** Free Apple Account signing normally allows only **3 directly installed apps**. LiveContainer takes one installed-app slot while holding multiple *guest* apps inside it. The host uses imported signing credentials to prepare and run those guest apps in JIT-less mode.
- **Typical pairing:** **SideStore + LiveContainer**, with the host refreshed before its seven-day authorization expires. The official **[LiveContainer + SideStore package](https://livecontainer.github.io/docs/installation/lc_sidestore)** puts both into one free app slot and supports refreshing through Shortcuts. Check that any scheduled refresh succeeds.
- **The catch:** The host still needs signing/refreshing; guests aren’t independent iOS installations. You can make Home Screen shortcuts for guests, but push notifications, extensions, and some integrations may not work. Guest data isn’t isolated like data from separately installed apps.
- **Compatibility & safety:** Requires iOS/iPadOS **15+**. Follow [LiveContainer’s official installation guide](https://livecontainer.github.io/docs/installation); it doesn’t support installation through Sideloadly or most web signers. Avoid untrusted modified builds, which may access guest-app data.

### TrollStore

<details>
<summary>Summary</summary>
<p>TrollStore gives compatible older iOS builds persistent IPA installation without weekly signing refreshes. Its strict version requirements make checking compatibility before updating essential.</p>
</details>

**[TrollStore](https://github.com/opa334/TrollStore) installs compatible IPAs persistently by using an iOS CoreTrust vulnerability.** It's a different installation method, not simply a signer that renews Apple certificates.

- **Why people use it:** No normal **3-app free-account limit**, **seven-day refresh**, or annual signing subscription for apps installed through TrollStore.
- **The catch:** It only works on **specific iOS/iPadOS builds**: the official project lists **14.0 beta 2–16.6.1**, **16.7 RC (20H18)**, and **17.0**. Later builds such as 17.0.1+ don’t support those CoreTrust bugs; actual installation options may be narrower. Check the [official installation guide](https://ios.cfw.guide/installing-trollstore/).
- **Jailbreak required?** **No.** TrollStore can work without jailbreaking, although its installation steps depend on the device and iOS build.
- **Important precautions:** Updating or restoring iOS may remove the vulnerability TrollStore depends on. Check compatibility **before** updating, and only install IPAs from sources you trust: persistent installation doesn’t make an app safe.

### On-device vs. web signing

<details>
<summary>Summary</summary>
<p>Both can sign IPAs, but an on-device signer keeps the signing process on your device. A web signer may require uploading your IPA and sensitive signing files.</p>
</details>

| | On-device signer (such as Feather) | Web signer |
| --- | --- | --- |
| **What happens** | Import credentials and sign within an installed app. | Submit an IPA through a website; signing often runs on its server. |
| **Convenience** | Good for repeatedly signing and managing apps. | Can work without first installing a signer app. |
| **Main concern** | The signer itself must remain usable. | The site may receive your IPA, **private key, password, and provisioning profile**. |

**Using your own private signing key?** Prefer a trusted local signer. If a website says signing is done in your browser rather than on its server, verify that claim before submitting your files.

## Signing files, certificates & capabilities

**Typical signing files:** `IPA + matching .p12/private key + .mobileprovision → Signed IPA → Install`

Some tools get the credentials automatically using your Apple Account. Certificate-based signers usually ask you to import them. **Importing a replacement certificate does not update apps you’ve already signed.** Two apps signed with the same certificate can still have different capabilities because their provisioning profiles and entitlements differ.

**How many apps can you sign?** There isn't one certificate-wide number. Free Apple Account signing normally allows **3 installed apps, 10 App IDs, and 3 registered devices**, with **7-day provisioning**; paid teams and enterprise signing use different rules. See [the route comparison](pick-your-route.md#compare-the-routes).

### Signing arrangements & certificate labels

<details>
<summary>Summary</summary>
<p>A certificate, provisioning profile, and signer app do different jobs. The signing arrangement determines device limits and capabilities; community labels such as “PPQ-less” or “private” don’t override Apple’s rules.</p>
</details>

#### Actual signing arrangements

<details>
<summary>Summary</summary>
<p>Development, ad hoc, and enterprise signing are used for different purposes. They have different rules about which apps and devices are authorized.</p>
</details>

- **Development signing:** Used for app development and testing. A free Apple Account uses restricted development provisioning; a paid developer team has more options.
- **Ad hoc signing:** A paid Apple Developer team authorizes apps for **registered device UDIDs**. It's useful for installing outside the App Store on specified devices; it isn't an unlimited device registration method.
- **Enterprise / in-house signing:** Intended for a company's internal apps. Shared or leaked enterprise certificates used for public sideloading can be revoked and aren't intended as a general public distribution route.

#### Labels you'll encounter in sideloading communities

<details>
<summary>Summary</summary>
<p>Terms like “private certificate,” “PPQ-less,” and “certificate with JIT” describe seller claims or particular setups, not separate Apple certificate types. Check the actual signing requirements.</p>
</details>

- **Private / shared certificate:** Describes who has the signing credentials. It's not a separate Apple-issued certificate category, and private credentials can still be revoked.
- **PPQ / PPQ-less:** Seller terms used for differences in provisioning or Apple verification behavior. They're **not two official Apple certificate types**. See [PPQ and anti-revoke DNS](#anti-revoke-dns-ppq--vpn).
- **“Certificate with JIT”:** Some sellers describe certificates this way, but **a certificate by itself does not enable JIT**. JIT requires a compatible enabling method on the device.
- **SSL/TLS certificate:** Secures a website or HTTPS connection. It's unrelated to Apple code-signing certificates, though some local installer tools also use HTTPS.

**A signer app has its own signing authorization.** For example, Feather may be installed using one certificate but sign other IPAs using a different `.p12`. If Feather’s own certificate fails, separately signed apps can remain usable under *their* credentials.

**Your own paid Developer Account vs. a paid certificate seller:** With your own membership, you manage the Apple developer team. With a third-party service, the seller generally controls the team and provides signing access. [Compare the routes](pick-your-route.md#compare-the-routes).

### Notifications, extensions & JIT

<details>
<summary>Summary</summary>
<p>Notifications and app extensions need the right app capabilities and signing permissions. JIT is a separate compatibility issue that depends on the device, iOS version, and enabling method.</p>
</details>

#### Push notifications (remote notifications)

- These are notifications sent to your device through Apple's Push Notification service (**APNs**), for example when someone messages you while an app isn't open.
- They require the app's push capability, appropriate signing entitlements, and a working setup behind the app. **Buying a paid certificate alone doesn't guarantee push notifications will work.**

#### Local notifications

- Alerts scheduled by the app on the device itself, such as a reminder timer.
- They are different from remote push notifications, so “this signing route doesn't support push” doesn't mean all alerts are impossible.

#### Widgets, extensions, and app groups

- Extra features such as widgets and share extensions often have their own identifiers and signing requirements; shared data between the app and extension can require app groups.
- Removing extensions may make signing easier **but disables those features**. Extensions may also use up App IDs under free signing.

#### JIT (Just-in-Time compilation)

- Lets software create executable machine code while running. Some emulators and runtimes need it for full speed or compatibility.
- iOS restricts JIT. Whether you can enable it depends on **your iOS version, device, tool, and app**; developers' debugging services and some exploits are common enabling paths.
- **It isn't something a special `.p12` automatically unlocks.**

### Signer settings & installation options

<details>
<summary>Summary</summary>
<p>Signer settings can change app identity, features, or installation behavior. Check what a setting actually does in your signer before changing it, particularly when DNS or HTTPS is involved.</p>
</details>

#### Common switches you might see in Feather, ESign, or other signers

- **Change/randomize Bundle ID:** Changes the app's identity. Can be useful for a second copy, but it may stop an app from updating in place or accessing its previous data and sign-in features.
- **Remove extensions / provisioning:** Alters the signed app or its signing setup. May fix a specific incompatibility, but can also remove widgets, shares, or capabilities that the app needs. Not a universal fix.
- **Inject tweaks:** Adds modifications to an IPA. Only use tweaks you trust and understand; they can break the app or introduce unwanted behavior.
- **Force Localize:** The behavior is tool/version-specific and isn’t consistently documented as a general signing option. Check that signer’s own description instead of assuming it translates or repairs an app. Don’t copy unfamiliar setting choices from another signer’s guide.
- **Install via localhost / local server:** The signer may temporarily serve the IPA and installation manifest through a local HTTPS address. Some local installers use a hostname that still needs DNS resolution and valid HTTPS. **This doesn’t mean you’re hosting your own app repository.** DNS/VPN problems can interrupt installation even if signing succeeded. See [Feather troubleshooting](https://github.com/claration/Feather/wiki/Troubleshooting).

## Installing, updating & keeping app data

**Refreshing keeps an existing app's signing authorization valid. Updating installs a newer version of the app.** They're separate things; one doesn't automatically do the other.

### What each action actually does

<details>
<summary>Summary</summary>
<p>Signing, installing, refreshing, updating, and deleting are different actions. Refreshing renews authorization, updating changes the app version, and deleting can remove its saved data.</p>
</details>

| Action | What actually happens |
| --- | --- |
| **Sign** | Prepares an IPA with valid credentials and permitted capabilities; the app isn't installed yet. |
| **Install** | Adds the signed app to the device when iOS accepts it. |
| **Refresh / re-sign** | Renews time-limited signing/provisioning, usually for the *same app version*. |
| **Update** | Installs a newer IPA/build, with the signing requirements still satisfied. |
| **Install over the app** | Replaces an existing installation; **data may stay** if iOS allows an in-place replacement. |
| **Delete, then reinstall** | Removes the previous installation first, usually including its local data. |
| **Offload** | Removes the app binary but normally keeps its documents/data; getting a sideloaded app back may need valid signing. |

**If your certificate expires or is revoked:** you generally need to **re-sign and reinstall the affected apps** with a working identity. Adding a new `.p12` to Feather or ESign doesn't change installed apps by itself.

**If only your signer stops working:** apps signed with other identities don't automatically stop opening. You'll still need a working tool to update or refresh them.

### What happens to app data?

<details>
<summary>Summary</summary>
<p>Compatible in-place refreshes and updates generally preserve data; deleting an app usually doesn’t. Check backups and signing compatibility before reinstalling or changing certificates.</p>
</details>

| Situation | What usually happens |
| --- | --- |
| **Successful refresh or compatible update** | Data stays when the app can be replaced in place. |
| **Certificate expiry or revocation** | Data often remains on the device, **even if the app won't open**. |
| **Bundle ID or signing-team change** | Can prevent replacement or access to the same keychain/app-group data. |
| **Delete the app** | Removes its local app container and usually its unsynced data. |
| **Offload the app** | Normally keeps documents/data, but reinstalling may require fresh signing. |

**Don’t delete an app just because it won’t launch.** First check signing and backup/export options. Deleting it may turn a fixable signing problem into lost saves or settings. Even with the same Bundle ID, a new signing team or changed keychain/app groups can affect data access. Prefer the app’s own export or cloud sync; a full-device backup may not restore every sideloaded app correctly.

**AltStore Classic’s [Deactivate/Activate](https://faq.altstore.io/altstore-classic/activating-apps)** helps preserve app data while freeing an active app slot. It isn’t the same as manually deleting an app.

### Removing signing components

<details>
<summary>Summary</summary>
<p>Removing a signer or its imported files doesn’t automatically invalidate other installed apps. Removing developer trust, DNS rules, or the installed app itself can have different consequences.</p>
</details>

- **Delete a signer app:** its locally stored imported files may disappear, but separately installed apps don't become unsigned just because the signer was removed.
- **Remove a `.p12` or `.mobileprovision` from a signer:** removes that copy of the file; it doesn't revoke Apple's certificate or alter apps already installed.
- **Remove enterprise developer trust:** apps depending on that trusted developer may stop launching. It's different from Apple revoking the certificate.
- **Remove a DNS configuration profile:** changes which DNS rules apply. This can break apps relying on anti-revoke DNS.
- **Delete an installed app:** normally deletes its local data too; it won't automatically uninstall separate signers or DNS profiles.

You can inspect installed configuration profiles in **Settings → General → VPN & Device Management**. Don't remove an organization-managed profile without checking what it controls. [Apple's profile guidance](https://support.apple.com/en-ca/guide/personal-safety/ips327569a75/web).

## Expiration, revocation & app failures

**Expiry = authorization reaches its listed end date. Revocation = Apple invalidates it early.** Both can stop apps from opening, but they're not the same event.

### What expires?

<details>
<summary>Summary</summary>
<p>Certificates, provisioning profiles, developer memberships, and seller plans can expire on different dates. An installed app usually stops working because its signing authorization is no longer valid.</p>
</details>

- **Signing certificate:** has an expiry date. Replacing it can mean signing the affected apps again.
- **Provisioning profile:** has its own expiry. Free Apple Account profiles are normally valid for **7 days**, even if the underlying certificate has a later date.
- **Paid Apple Developer membership:** annual membership and services need renewal; the effect on apps depends on their actual signing arrangement.
- **Third-party signing service / replacement coverage:** the seller's plan or warranty ends on its terms, not necessarily on the signing certificate's date.
- **Installed app's authorization:** people say an app “expired” when its signing/provisioning is no longer valid. An IPA saved in Files doesn't have a separate seven-day refresh timer; the installed app's signing authorization is what expires.

### Revocation, blacklisting & account restrictions

<details>
<summary>Summary</summary>
<p>A failed app can be caused by revocation, an expired or mismatched profile, account restrictions, or installation/network problems. The error alone doesn’t prove a device-wide ban.</p>
</details>

- **Certificate revoked:** apps depending on that signing identity may stop working. That does not automatically revoke unrelated certificates on the device.
- **Provisioning or registration failure:** an incorrect UDID, App ID, team, entitlement, or expired profile can cause failure *without* the certificate being revoked.
- **“Blacklisted” device:** community shorthand often used when a device can't install or verify an app under a particular certificate. It does **not** necessarily mean the phone is banned from every signing method.
- **Apple Account or developer team restricted:** Apple may restrict development privileges or terminate a developer team. That's different from one app's certificate failing; community reports don't establish a universal ban rate.
- **Installer/network failure:** a corrupt IPA, incompatible iOS version, missing developer trust, or DNS/VPN interference can look like revocation.

**“No-revoke” isn't a guarantee.** Certificate-checker tools can inspect profiles, dates, and sometimes known revocation status, but they can't promise your particular app will keep opening.

### When something stops working

<details>
<summary>Summary</summary>
<p>Installation errors, verification prompts, crashes, and missing features can have different causes. Identify whether the problem is signing, app compatibility, trust, or networking before deleting anything.</p>
</details>

**Check the symptom before deleting or re-signing anything.**

| What you see | Start by checking |
| --- | --- |
| **“Integrity could not be verified” / install fails** | IPA, certificate/profile, device UDID, bundle ID conflict, installation method. |
| **“Unable to verify app” / internet required** | Developer trust, Apple's required verification, applicable PPQ checks, DNS/VPN blocking. |
| **App opens and immediately closes** | Expired/revoked signing **or** a real app crash, incompatible build, bad tweak, missing entitlement. |
| **Only the signer stops opening** | Signer's own authorization; apps signed separately may still work. |
| **App opens but notifications or another feature fail** | App-specific permissions, entitlements, extensions, or its backend, not just the certificate. |

**Best recovery habit:** work out whether the problem is the **app**, the **signature/profile**, the **installer**, or the **network**. Change one thing at a time so you know what fixed it.

## Anti-revoke DNS, PPQ & VPN

<details>
<summary>Summary</summary>
<p>Anti-revoke DNS may help some revoked-certificate setups keep working, but it cannot undo a revocation or guarantee protection. PPQ rules and VPN compatibility depend on the actual signing and network setup.</p>
</details>

**Anti-revoke DNS** blocks selected Apple verification domains in an attempt to keep apps signed with revoked credentials running. **It doesn't make the revoked certificate valid again** and it can stop working unexpectedly.

- **Who uses it?** Mostly people installing apps with publicly shared or revoked certificates. Free Apple Account signing and ordinary valid paid signing do not require this DNS workaround.
- **What’s PPQ?** An Apple [provisioning verification requirement](https://developer.apple.com/help/account/provisioning-profiles/provisioning-profile-updates/) for certain development/ad hoc profiles. It’s separate from ordinary certificate revocation lookups and can involve `ppq.apple.com`. “PPQ-less” is a seller label, not an official certificate type.
- **Should `ppq.apple.com` be blocked?** **It depends.** Some community methods briefly allow it while installing or first opening an app, then block it afterward. Don't treat this as a rule for every signing arrangement.
- **Can a VPN work with anti-revoke DNS?** Sometimes, but many VPNs set their own DNS resolver. Cached DNS answers or other network paths can also bypass the intended blocking. If the effective DNS no longer blocks verification domains, apps may stop working. Check the resolver on both Wi-Fi and cellular.
- **Are restarts or DNS changes risky?** For DNS-dependent revoked-certificate setups, yes. Rebooting, switching networks, changing VPNs, or removing a DNS profile can change how iOS verifies signing.
- **Does toggling airplane mode protect everything?** No. Cycling Wi-Fi/cellular can refresh network behavior, but it isn't a guarantee against verification.

**A DNS profile being installed doesn’t prove its rules are active.** Check the effective DNS after network/VPN changes; the [DNS tools and guides](link-pool.md#dns-profiles--testing) can help. Blocking Apple domains may also interrupt unrelated services or the DNS/HTTPS used by some local app installers. See the [NextDNS and Cloudflare guides](link-pool.md#anti-revoke-dns-guides).

## App sources & IPA safety

<details>
<summary>Summary</summary>
<p>A repository helps you find IPA downloads and updates; it doesn’t automatically sign apps or confirm they’re safe. Check each IPA’s origin and use valid signing to install new versions.</p>
</details>

**An app source (or repo) lists apps and their downloads.** It doesn't automatically sign or install anything. Most use a JSON manifest with app names, versions, and IPA links; the actual files may live somewhere else.

- **Do sources show updates?** Only if their maintainer or automated feed publishes new versions and your app manager refreshes the listing. A source may be stale, your app manager may cache an older listing, or a listed IPA download link may no longer work.
- **Does an update listing install the new app?** No. Your signer still needs to download the new IPA and install it with valid signing.
- **Can you trust an IPA because a source lists it?** No. Prefer original developer releases; check the project, release history, compatible iOS version, and any hashes provided. Modified or pre-signed IPAs can contain unwanted code. A valid code signature doesn’t prove the original developer approved a modified app.

For source browsers, repo tools, and directories, use the [link pool](link-pool.md#app-sources--repositories).

## Quick workflows & good habits

### Quick process flows

```text
Certificate signing:   IPA + .p12/key + matching profile → Sign → Install → Open
Apple Account signing: IPA → Tool manages signing → Install → Refresh before 7-day expiry
Refresh:               Installed app → Renew signing/profile → Refresh in place if supported
Update:                New IPA version → Check app identity → Sign → Install over old version if allowed
Replace certificate:   New signing files → Re-sign affected apps → Install without deleting if compatible
Uninstall:             Back up/export app data → Delete app → Local data usually removed
```

### Good habits

<details>
<summary>Summary</summary>
<p>Know how each app was installed, track its signing expiry, keep trustworthy IPAs and backups, and confirm refreshes succeed. When troubleshooting, change one thing at a time.</p>
</details>

- **Know your route.** App limits, refresh schedules, and feature support come from the signing arrangement, not just the signer icon.
- **Write down the essentials:** app/source/version, Bundle ID, signing identity, profile expiration, and backup status.
- **Keep a way back in:** clean IPAs, trustworthy installer links, working credentials, and backups in case a signer stops opening.
- **Back up before changing things:** especially bundle IDs, signing teams, certificates, DNS settings, or installed apps.
- **Change one thing at a time:** network, signature, and app compatibility problems can show similar errors.
- **Check compatibility for special tools and workarounds:** LiveContainer, Lara, SparseBox, JIT-enabling methods, and TrollStore do not work the same way or support the same iOS builds.
- **Check that auto-refresh really happened.** With SideStore or a Shortcuts-based refresh, verify the app's remaining validity rather than assuming the automation succeeded.

**Next:** [Compare the five routes](pick-your-route.md#compare-the-routes) · [Find tools and guides](link-pool.md#guides--learning)

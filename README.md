# iOS Sideloading Cheatsheets

A complete sideloading guide. Understand all the elements, the tools & their specifics, know what to expect, and compare the most popular routes people take for their setup of sideloading.  

You don't need to know everything, this isn't study book. 
Find and read what you want to know. 

# Index

- ## [Cheatsheet](cheatsheet.md)
  Practical knowledge shared across sideloading routes.
  - [The basics](cheatsheet.md#the-basics)  
    Core sideloading terms and what an IPA needs to run on iOS.
    - [IPA (`.ipa`)](cheatsheet.md#ipa-ipa)  
      What an IPA contains and why an IPA download alone is not enough.
    - [Signer and installer](cheatsheet.md#signer-and-installer)  
      Why signing an app and installing it are separate steps.
    - [Signing certificate and `.p12` file](cheatsheet.md#signing-certificate-and-p12-file)  
      Signing identities, private keys, and the files used by signers.
    - [Provisioning profile (`.mobileprovision`)](cheatsheet.md#provisioning-profile-mobileprovision)  
      How iOS determines which app, device, and capabilities are authorized.
    - [Entitlements](cheatsheet.md#entitlements)  
      Permissions that determine whether advanced app features work.
    - [Bundle ID and App ID](cheatsheet.md#bundle-id-and-app-id)  
      How apps are identified and why the two IDs have different limits.
    - [UDID](cheatsheet.md#udid)  
      Device identification for registration and signing.
    - [Developer trust and revocation](cheatsheet.md#developer-trust-and-revocation)  
      Developer verification versus early loss of signing authorization.
  - [Tools & what they do](cheatsheet.md#tools--what-they-do)  
    What signing, installation, and app-management tools actually do.
    - [Common tools](cheatsheet.md#common-tools)  
      Recognizable apps and services, grouped by what they do.
      - [On-device signers for certificate-based setups](cheatsheet.md#on-device-signers-for-certificate-based-setups)  
        ESign, KSign, Feather, and similar certificate-based signers.
      - [Apple Account signing tools](cheatsheet.md#apple-account-signing-tools)  
        SideStore, AltStore Classic, and Sideloadly and how they refresh apps.
      - [Discovery and distribution](cheatsheet.md#discovery-and-distribution)  
        Sources, portals, and services for finding and distributing IPAs.
    - [LiveContainer](cheatsheet.md#livecontainer)  
      How guest apps work inside one installed host, plus limitations.
    - [TrollStore](cheatsheet.md#trollstore)  
      Persistent IPA installation and the iOS versions that support it.
    - [On-device vs. web signing](cheatsheet.md#on-device-vs-web-signing)  
      Convenience, signing-file handling, and privacy differences.
  - [Signing files, certificates & capabilities](cheatsheet.md#signing-files-certificates--capabilities)  
    The signing assets, account rules, and features that apps can use.
    - [Signing arrangements & certificate labels](cheatsheet.md#signing-arrangements--certificate-labels)  
      How real Apple signing methods differ from community terminology.
      - [Actual signing arrangements](cheatsheet.md#actual-signing-arrangements)  
        Development, ad hoc, and enterprise authorization.
      - [Labels you'll encounter in sideloading communities](cheatsheet.md#labels-youll-encounter-in-sideloading-communities)  
        What private, PPQ-less, JIT, and SSL/TLS certificate labels mean.
    - [Notifications, extensions & JIT](cheatsheet.md#notifications-extensions--jit)  
      Why notifications, widgets, app extensions, and JIT may or may not work.
      - [Push notifications (remote notifications)](cheatsheet.md#push-notifications-remote-notifications)  
        Requirements for notifications delivered through Apple’s push service.
      - [Local notifications](cheatsheet.md#local-notifications)  
        Alerts generated on the device rather than sent remotely.
      - [Widgets, extensions, and app groups](cheatsheet.md#widgets-extensions-and-app-groups)  
        Additional signing and identifier requirements for app features.
      - [JIT (Just-in-Time compilation)](cheatsheet.md#jit-just-in-time-compilation)  
        What JIT does and why enabling it is separate from normal signing.
    - [Signer settings & installation options](cheatsheet.md#signer-settings--installation-options)  
      Effects of app-identity changes, tweak injection, and local installing.
      - [Common switches you might see in Feather, ESign, or other signers](cheatsheet.md#common-switches-you-might-see-in-feather-esign-or-other-signers)  
        What common signer options change and what they can break.
  - [Installing, updating & keeping app data](cheatsheet.md#installing-updating--keeping-app-data)  
    What signing, refreshing, updating, and reinstalling change.
    - [What each action actually does](cheatsheet.md#what-each-action-actually-does)  
      A side-by-side view of common app actions.
    - [What happens to app data?](cheatsheet.md#what-happens-to-app-data)  
      When saved data stays, when it may be lost, and how to protect it.
    - [Removing signing components](cheatsheet.md#removing-signing-components)  
      The effects of removing signers, certificates, profiles, or apps.
  - [Expiration, revocation & app failures](cheatsheet.md#expiration-revocation--app-failures)  
    Why a working installation can stop running.
    - [What expires?](cheatsheet.md#what-expires)  
      Which certificates, profiles, memberships, and service periods end.
    - [Revocation, blacklisting & account restrictions](cheatsheet.md#revocation-blacklisting--account-restrictions)  
      Different causes of certificate, device, or account failures.
    - [When something stops working](cheatsheet.md#when-something-stops-working)  
      Common symptoms and the first things to troubleshoot.
  - [Anti-revoke DNS, PPQ & VPN](cheatsheet.md#anti-revoke-dns-ppq--vpn)  
    How certificate checks, DNS blocking, PPQ, and VPNs interact.
  - [App sources & IPA safety](cheatsheet.md#app-sources--ipa-safety)  
    How repositories list updates and how to evaluate IPA downloads.
  - [Quick workflows & good habits](cheatsheet.md#quick-workflows--good-habits)  
    Short process references and ways to avoid preventable mistakes.
    - [Quick process flows](cheatsheet.md#quick-process-flows)  
      At-a-glance flows for signing, refreshing, replacing, and removing apps.
    - [Good habits](cheatsheet.md#good-habits)  
      Backups, expiry tracking, and maintaining a recovery path.

- ## [Pick Your Route](pick-your-route.md)  
  Compare five sideloading methods and their trade-offs.
  - [Compare the routes](pick-your-route.md#compare-the-routes)  
    Compare cost, app limits, refresh schedules, and drawbacks.
  - [Route 1 — Free Apple Account signing](pick-your-route.md#route-1--free-apple-account-signing)  
    Free signing with app-slot limits and seven-day refreshes.
    - [Overview](pick-your-route.md#overview)  
      Cost, limits, refreshing, and setup requirements at a glance.
    - [Pros](pick-your-route.md#pros)  
      Main advantages of the free Apple Account route.
    - [Cons](pick-your-route.md#cons)  
      Free-signing restrictions and ongoing maintenance.
    - [Example setups](pick-your-route.md#example-setups)  
      SideStore with LiveContainer, or Sideloadly with desktop refreshes.
  - [Route 2 — Paid Apple Developer Account signing](pick-your-route.md#route-2--paid-apple-developer-account-signing)  
    Control your own Apple developer team and signing credentials.
    - [Overview](pick-your-route.md#overview-1)  
      Cost, registration, capabilities, and renewal requirements.
    - [Pros](pick-your-route.md#pros-1)  
      Why managing your own developer team can be worthwhile.
    - [Cons](pick-your-route.md#cons-1)  
      Cost and technical maintenance trade-offs.
    - [Example setup](pick-your-route.md#example-setup)  
      A realistic signing workflow using your own paid team.
  - [Route 3 — Free shared/revoked enterprise certificates + DNS](pick-your-route.md#route-3--free-sharedrevoked-enterprise-certificates--dns)  
    Use shared certificates, with revocation and DNS-related risks.
    - [Overview](pick-your-route.md#overview-2)  
      Cost, certificate validity, DNS, and compatibility basics.
    - [Pros](pick-your-route.md#pros-2)  
      Where shared-certificate setups offer convenience.
    - [Cons](pick-your-route.md#cons-2)  
      Revocation, DNS, VPN, and trust risks.
    - [Example setup](pick-your-route.md#example-setup-1)  
      A typical shared-certificate and NextDNS setup.
  - [Route 4 — Paid signing certificates & services](pick-your-route.md#route-4--paid-signing-certificates--services)  
    Third-party signing plans, provisioning, and provider policies.
    - [Overview](pick-your-route.md#overview-3)  
      Price, plan limitations, renewal, and PPQ requirements.
    - [Pros](pick-your-route.md#pros-3)  
      Conveniences offered by third-party signing services.
    - [Cons](pick-your-route.md#cons-3)  
      Cost, seller dependence, and revocation risk.
    - [Example setup](pick-your-route.md#example-setup-2)  
      A paid certificate provider and Feather installation example.
  - [Route 5 — TrollStore & compatible exploit-based methods](pick-your-route.md#route-5--trollstore--compatible-exploit-based-methods)  
    Persistent installs on specific supported iOS builds.
    - [Overview](pick-your-route.md#overview-4)  
      Compatibility, persistence, and feature support.
    - [Pros](pick-your-route.md#pros-4)  
      Advantages of persistent TrollStore installs.
    - [Cons](pick-your-route.md#cons-4)  
      Limited compatibility and update risks.
    - [Example setup](pick-your-route.md#example-setup-3)  
      Installing IPAs through TrollStore on supported devices.

- ## [Link Pool](link-pool.md)
  - [Guides & learning](link-pool.md#guides--learning)
    - [General sideloading guides](link-pool.md#general-sideloading-guides)
    - [Anti-revoke DNS guides](link-pool.md#anti-revoke-dns-guides)
    - [VPN with custom DNS](link-pool.md#vpn-with-custom-dns)
  - [DNS profiles & testing](link-pool.md#dns-profiles--testing)
  - [Tools & utilities](link-pool.md#tools--utilities)
    - [Certificate & provisioning tools](link-pool.md#certificate--provisioning-tools)
    - [Source & repository tools](link-pool.md#source--repository-tools)
    - [Other tools](link-pool.md#other-tools)
    - [Toolkit sites](link-pool.md#toolkit-sites)
  - [Signer apps & installers](link-pool.md#signer-apps--installers)
    - [Signer and installer projects](link-pool.md#signer-and-installer-projects)
    - [Signer app installers](link-pool.md#signer-app-installers)
    - [Web signers](link-pool.md#web-signers)
  - [Certificates & signing services](link-pool.md#certificates--signing-services)
    - [Free/shared certificate resources](link-pool.md#freeshared-certificate-resources)
    - [Paid certificate providers](link-pool.md#paid-certificate-providers)
  - [App sources & repositories](link-pool.md#app-sources--repositories)
  - [Shortcuts](link-pool.md#shortcuts)
  - [Communities](link-pool.md#communities)

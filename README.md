# Android Security Assessment Tools

A curated list of tools for vulnerability analysis, reverse engineering and penetration testing of
Android applications.

This is not a comprehensive list, and many tools overlap in purpose. Most are community-maintained
open source projects, so coverage varies over time — using several tools together is usually
necessary for a thorough assessment.

**Last reviewed: August 2026.** Every link below was checked for liveness, and every GitHub-hosted
project was checked against the GitHub API for its archived flag and most recent push. Dead links
have been removed or repointed, and projects that are no longer maintained are marked as such rather
than quietly left in place.

### Status legend

| Status | Meaning |
| --- | --- |
| ✅ | Actively maintained — pushed to within roughly the last year |
| ⚠️ | Usable but stale — no activity for 1+ years; expect friction on modern Android |
| 🗄️ | Archived or unmaintained — listed for historical reference only |

The date in each Status cell is the project's last observed push as of this review.

---

## Contents

- [Standards and methodology](#standards-and-methodology)
- [All-in-one assessment frameworks](#all-in-one-assessment-frameworks)
- [Static analysis and SAST](#static-analysis-and-sast)
- [Reverse engineering and decompilation](#reverse-engineering-and-decompilation)
- [Dynamic analysis and runtime instrumentation](#dynamic-analysis-and-runtime-instrumentation)
- [Rooting, hooking and device preparation](#rooting-hooking-and-device-preparation)
- [Emulation and virtual devices](#emulation-and-virtual-devices)
- [Network interception and TLS testing](#network-interception-and-tls-testing)
- [Fuzzing](#fuzzing)
- [APK acquisition and corpora](#apk-acquisition-and-corpora)
- [Privacy, trackers, dependencies and SBOM](#privacy-trackers-dependencies-and-sbom)
- [Malware analysis and mobile forensics](#malware-analysis-and-mobile-forensics)
- [Vulnerable apps and training labs](#vulnerable-apps-and-training-labs)
- [Platform utilities](#platform-utilities)
- [Archived and historical tools](#archived-and-historical-tools)

---

## Standards and methodology

Start here. The OWASP Mobile Application Security project replaced the old MSTG naming and is the
reference most assessments are structured around.

| Tool | Status | Description |
| --- | --- | --- |
| [OWASP MAS](https://mas.owasp.org/) | ✅ | Umbrella project covering the MASVS standard, the MASTG testing guide and the MAS Checklist. Supersedes the older "MSTG" material. |
| [OWASP MASVS](https://mas.owasp.org/MASVS/) | ✅ | Mobile Application Security Verification Standard — the requirements baseline used to scope an assessment. |
| [OWASP MASTG](https://mas.owasp.org/MASTG/) | ✅ | Mobile Application Security Testing Guide — concrete test cases, techniques and reversing walkthroughs. Source at [OWASP/mastg](https://github.com/OWASP/mastg). |
| [OWASP MAS Checklist](https://mas.owasp.org/checklists/) | ✅ | MASVS requirements as a checklist mapped to MASTG tests, for tracking assessment coverage. |
| [Android Security Bulletins](https://source.android.com/docs/security/bulletin) | ✅ | Monthly platform vulnerability disclosures — needed to judge whether a target device or OS build is patched. |
| [Android security guidance](https://developer.android.com/privacy-and-security/security-tips) | ✅ | Google's official app security and privacy guidance; the baseline that findings are argued against. |
| [android-security-awesome](https://github.com/ashishb/android-security-awesome) | ✅ | Long-running companion list of Android security resources, papers and tools. |

## All-in-one assessment frameworks

| Tool | Status | Description |
| --- | --- | --- |
| [MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF) | ✅ 2026-07 | Mobile Security Framework — automated static, dynamic and malware analysis for Android, iOS and Windows apps, from binaries or source. The most common starting point for a first pass. [Docs](https://mobsf.github.io/docs/). |
| [drozer](https://github.com/ReversecLabs/drozer) | ✅ 2026-04 | Assesses attack surface by assuming the role of an app and interacting with IPC endpoints, the Android runtime and the underlying OS. Includes intent fuzzing modules. Formerly MWR/F-Secure/WithSecure Labs. |
| [Reversense (Dexcalibur)](https://github.com/reversenseorg/dexcalibur) | ✅ 2026-07 | Reverse engineering platform that automates Frida hook generation and tracks dynamically loaded/decrypted DEX. A full rewrite of the original Dexcalibur. |
| [APKLab](https://github.com/APKLab/APKLab) | ✅ 2026-07 | Android reverse engineering workbench for VS Code, wiring apktool, jadx and patching into the editor. |
| [apk.sh](https://github.com/ax/apk.sh) | ✅ 2026-01 | Shell tool that automates the repetitive parts of APK work — pulling, decoding, rebuilding and injecting the Frida gadget. |

## Static analysis and SAST

| Tool | Status | Description |
| --- | --- | --- |
| [Semgrep](https://semgrep.dev/) | ✅ 2026-07 | Fast pattern-based static analysis with Java/Kotlin support; the general-purpose SAST engine most mobile rules are now written for. |
| [mobsfscan](https://github.com/MobSF/mobsfscan) | ✅ 2026-03 | Finds insecure code patterns in Android and iOS source. MobSF's rule set as a standalone CLI, suitable for CI. |
| [Androguard](https://github.com/androguard/androguard) | ✅ 2026-06 | Python library and toolset for parsing and analysing DEX, APK and manifests. The foundation many other tools build on. |
| [Quark-Engine](https://github.com/ev-flow/quark-engine) | ✅ 2026-07 | Scores app behaviour against a weighted set of malicious behaviour rules rather than just permissions. Moved from the `quark-engine` org to `ev-flow`. |
| [APKiD](https://github.com/rednaga/APKiD) | ✅ 2026-07 | Identifies packers, protectors, obfuscators and compiler oddities in an APK — "PEiD for Android". Run it first to know what you're up against. |
| [apkleaks](https://github.com/dwisiswant0/apkleaks) | ✅ 2025-08 | Scans an APK for URIs, endpoints and hardcoded secrets. |
| [FlowDroid](https://github.com/secure-software-engineering/FlowDroid) | ✅ 2026-07 | Context-, flow-, field- and object-sensitive, lifecycle-aware static taint analysis for Android. Still the reference academic taint engine. [Project page](https://blogs.uni-paderborn.de/sse/tools/flowdroid/). |
| [RiskInDroid](https://github.com/ClaudiuGeorgiu/RiskInDroid) | ✅ 2026-07 | Quantitative permission-based risk scoring (0–100) using machine learning over declared vs. actually used permissions. |
| [diffuse](https://github.com/JakeWharton/diffuse) | ✅ 2026-07 | Diffs APKs, AABs, AARs and JARs — useful for spotting what changed between releases, including added SDKs. |
| [apkanalyzer](https://developer.android.com/tools/apkanalyzer) | ✅ | Official CLI (and Android Studio GUI) for inspecting APK contents, manifest, DEX and resource size. |
| [Android Lint](https://developer.android.com/privacy-and-security/security-tips) | ✅ | Built into the Android Gradle plugin, with a set of security checks; the cheapest static coverage for teams that own the source. |
| [QARK](https://github.com/linkedin/qark) | ⚠️ 2024-01 | Quick Android Review Kit — finds common app vulnerabilities in source or APKs and can generate proof-of-concept exploit APKs and ADB commands. No longer actively developed by LinkedIn. |
| [Androwarn](https://github.com/maaaaz/androwarn) | ⚠️ 2020-01 | Detects and reports potentially malicious behaviour by statically analysing Dalvik bytecode via Smali, producing a report at a chosen detail level. |
| [Argus-SAF](https://github.com/arguslab/Argus-SAF) | ⚠️ 2023-07 | Static analysis framework for Android apps, including inter-component data flow. The former Amandroid. |
| [FSquaDRA](https://github.com/zyrikby/FSquaDRA) | ⚠️ 2023-04 | Detects repackaged applications by comparing resource files, on the premise that repackaged apps preserve the original look and feel. |
| [StaDynA](https://github.com/zyrikby/StaDynA) | ⚠️ 2023-04 | Combines static and dynamic analysis to cover dynamic class loading and reflection, which pure static analysis misses. |

## Reverse engineering and decompilation

| Tool | Status | Description |
| --- | --- | --- |
| [jadx](https://github.com/skylot/jadx) | ✅ 2026-07 | DEX to Java decompiler with an excellent GUI, deobfuscation support and scripting. The default first look at any APK. |
| [Apktool](https://apktool.org/) | ✅ 2026-08 | Decodes resources to near-original form and rebuilds them after modification, including manifest and smali round-tripping. [Repo](https://github.com/iBotPeaches/Apktool). |
| [smali/baksmali](https://github.com/google/smali) | ✅ 2026-07 | Assembler/disassembler for the DEX format. Google's fork is the maintained continuation of JesusFreke's now-archived original. |
| [Ghidra](https://github.com/NationalSecurityAgency/ghidra) | ✅ 2026-07 | Full-featured SRE framework with a strong decompiler — the standard free option for the native (JNI/`.so`) side of an app. |
| [Rizin](https://github.com/rizinorg/rizin) / [Cutter](https://github.com/rizinorg/cutter) | ✅ 2026-08 | UNIX-style reverse engineering framework and its Qt GUI. The actively-governed fork of the radare lineage. |
| [radare2](https://github.com/radareorg/radare2) | ✅ 2026-08 | Scriptable command-line reversing toolkit and hex editor for binaries, with wide architecture support. |
| [r2frida](https://github.com/nowsecure/r2frida) | ✅ 2026-07 | Marries radare2's analysis with Frida's runtime access, so you can inspect and patch live memory from r2. |
| [Bytecode Viewer](https://github.com/Konloch/bytecode-viewer) | ✅ 2026-07 | Java/Android bytecode viewer and editor that bundles several decompilers (Procyon, CFR, FernFlower, Krakatau) plus smali and APK support behind one GUI. |
| [CFR](https://www.benf.org/other/cfr/) | ✅ 2026-06 | Java decompiler, particularly good at modern language constructs. [Repo](https://github.com/leibnitz27/cfr). |
| [FernFlower](https://github.com/JetBrains/fernflower) | ✅ 2026-07 | Analytical Java decompiler; the engine behind IntelliJ IDEA's and Android Studio's decompiled view. |
| [Krakatau](https://github.com/Storyyeller/Krakatau) | ✅ 2026-04 | Java decompiler and disassembler that copes well with obfuscated or hand-crafted bytecode. |
| [LIEF](https://github.com/lief-project/LIEF) | ✅ 2026-07 | Library for parsing and modifying executable formats, including ELF and DEX — the scripting workhorse for native library surgery. |
| [blutter](https://github.com/worawit/blutter) | ✅ 2026-04 | Reverse engineering for Flutter applications, recovering Dart classes and function names from release builds. |
| [reFlutter](https://github.com/Impact-I/reFlutter) | ✅ 2026-06 | Patches the Flutter engine so a Flutter app's traffic can be intercepted and its internals traced. Maintained fork of the archived ptswarm original. |
| [frida-il2cpp-bridge](https://github.com/vfsfitvnm/frida-il2cpp-bridge) | ✅ 2026-04 | Frida module for dumping, tracing and hijacking IL2CPP (Unity) applications at runtime. |
| [JEB Decompiler](https://www.pnfsoftware.com/) | ✅ Commercial | Commercial Android decompiler with strong deobfuscation and a scripting API; often the fallback when jadx fails on protected apps. |
| [IDA Pro](https://hex-rays.com/ida-pro) | ✅ Commercial | Commercial disassembler/decompiler, widely used for the native components of Android apps. |
| [dex2jar](https://github.com/pxb1988/dex2jar) | ⚠️ 2024-07 | Converts DEX to JAR so Java-oriented tooling can be used. Largely superseded by jadx for reading code. |
| [JD-GUI](https://github.com/java-decompiler/jd-gui) | ⚠️ 2024-07 | Standalone graphical Java decompiler. |
| [Procyon](https://github.com/mstrobel/procyon) | ⚠️ 2022-06 | Java decompiler, still bundled inside Bytecode Viewer. |
| [Il2CppDumper](https://github.com/Perfare/Il2CppDumper) | ⚠️ 2024-08 | Recovers metadata and symbols from Unity IL2CPP builds, restoring names for use in Ghidra or IDA. |
| [Simplify](https://github.com/CalebFenton/simplify) | ⚠️ 2022-04 | Virtually executes an app to understand behaviour, then rewrites the code into an equivalent but readable form. Generic across obfuscation types. |
| [apkx](https://github.com/muellerberndt/apkx) | ⚠️ 2021-01 | One-step APK decompilation wrapper with multiple decompiler backends. |
| [Emacs-Smali](https://github.com/strazzere/Emacs-Smali) | ✅ 2026-07 | Smali/baksmali major mode for Emacs. |
| [ProGuard](https://github.com/Guardsquare/proguard) | ✅ 2026-07 | The open source Java/Android optimizer and obfuscator. Worth knowing from the defensive side, and for reading its mapping files. |

## Dynamic analysis and runtime instrumentation

| Tool | Status | Description |
| --- | --- | --- |
| [Frida](https://frida.re/) | ✅ 2026-08 | Dynamic instrumentation toolkit — inject scripts into black-box processes to hook functions, trace crypto APIs and modify behaviour with no source and no restart. The centre of gravity for modern mobile testing. [Android guide](https://frida.re/docs/android/), [repo](https://github.com/frida/frida), [CLI tools](https://github.com/frida/frida-tools). |
| [objection](https://github.com/sensepost/objection) | ✅ 2026-07 | Frida-powered runtime mobile exploration: bypass pinning and root detection, dump keychains and storage, and patch APKs to embed the Frida gadget, all without a rooted device. |
| [RMS — Runtime Mobile Security](https://github.com/m0bilesecurity/RMS-Runtime-Mobile-Security) | ✅ 2026-07 | Web UI over Frida for hooking and manipulating a running app's classes and methods, with live output. |
| [Medusa](https://github.com/Ch0pin/medusa) | ✅ 2026-07 | Modular Frida framework with a large library of ready-made hook modules, so common tasks don't need custom scripts. |
| [MVT](https://github.com/mvt-project/mvt) | ✅ 2026-07 | Mobile Verification Toolkit — forensic triage of Android and iOS devices for signs of compromise. |
| [House](https://github.com/nccgroup/house) | ⚠️ 2021-06 | Frida-backed runtime analysis toolkit with a web GUI, written in Python. |
| [Inspeckage](https://github.com/ac-pm/Inspeckage) | ⚠️ 2020-09 | Dynamic analysis via Android API hooks with a web dashboard, exposing storage, crypto, IPC and network activity. Requires the legacy Xposed framework. |

## Rooting, hooking and device preparation

Getting a usable test device is often the hardest part of an Android assessment. The Xposed
ecosystem in particular has churned heavily — the original Xposed, EdXposed, and LSPosed are all
now unmaintained.

| Tool | Status | Description |
| --- | --- | --- |
| [Magisk](https://github.com/topjohnwu/Magisk) | ✅ 2026-08 | Systemless root and module framework; the standard way to root a test device while keeping the system partition intact. |
| [KernelSU](https://github.com/tiann/KernelSU) | ✅ 2026-07 | Kernel-based root solution, useful where a userspace approach is detected or unavailable. Requires a compatible or custom kernel. |
| [APatch](https://github.com/bmax121/APatch) | ✅ 2026-08 | Kernel and system patching for root, an alternative to Magisk and KernelSU with its own module format. |
| [Vector](https://github.com/JingMatrix/Vector) | ✅ 2026-08 | Actively maintained Xposed framework, continuing from the archived LSPosed. The current route to running Xposed modules on modern Android. |
| [LSPatch](https://github.com/JingMatrix/LSPatch) | ✅ 2026-04 | Non-root Xposed framework that patches modules directly into a target APK. Maintained fork of the archived original. |
| [Shizuku](https://github.com/RikkaApps/Shizuku) | ✅ 2025-06 | Lets ordinary apps call system APIs with adb or root privileges via a Java process, enabling some testing without full root. |
| [AlwaysTrustUserCerts](https://github.com/NVISOsecurity/AlwaysTrustUserCerts) | ✅ 2025-06 | Magisk/KernelSU module that promotes user-installed CA certificates into the system store on boot — the standard fix for Android 7+ refusing user CAs for app traffic. |
| [apkpatcher](https://github.com/badadaf/apkpatcher) | ✅ 2025-07 | Automates patching an APK with the Frida gadget, for instrumenting apps on non-rooted devices. |
| [strongR-frida-android](https://github.com/hzzheyang/strongR-frida-android) | ✅ 2026-07 | Anti-detection build of frida-server, for targets that check for stock Frida. |
| [Florida](https://github.com/Ylarod/Florida) | ✅ 2026-07 | Another anti-detection frida-server build with the same purpose. |
| [JustTrustMe](https://github.com/Fuzion24/JustTrustMe) | ⚠️ 2024-09 | Xposed module that disables certificate validation across common HTTP libraries. Convenient, but Frida-based approaches are more reliable now. |
| [Airgate](https://github.com/heyaibi/airgate) | ✅ 2026-08 | Offline Device-Owner watchdog that hardens dedicated Android devices for cold-storage — monitors 14 wireless/USB/system-tamper signals and triggers reactive hardening / factory-reset wipe; 100% offline (no INTERNET permission, Dhizuku), GPL-3.0. Hardened target for testing DevicePolicyManager and airgap bypass. |

## Emulation and virtual devices

| Tool | Status | Description |
| --- | --- | --- |
| [Android Emulator](https://developer.android.com/studio) | ✅ | Official emulator shipped with Android Studio. Google API (non-Play) images allow `adb root`, which is usually enough for app testing. |
| [Waydroid](https://github.com/waydroid/waydroid) | ✅ 2026-08 | Runs a full Android system in a container on a Linux host using Wayland, with near-native performance. |
| [redroid](https://github.com/remote-android/redroid-doc) | ✅ 2026-05 | Multi-arch, GPU-enabled Android in Docker — convenient for scaling analysis across many containerised instances. |
| [android-emulator-container-scripts](https://github.com/google/android-emulator-container-scripts) | ✅ 2026-07 | Google's scripts for running the emulator inside containers, for CI and headless analysis pipelines. |
| [scrcpy](https://scrcpy.org/) | ✅ 2026-07 | Displays and controls a physical device over USB or TCP/IP with no app installed. Invaluable for driving a device during testing. [Repo](https://github.com/Genymobile/scrcpy). |
| [Corellium](https://www.corellium.com/) | ✅ Commercial | Virtualised ARM Android and iOS devices with kernel-level introspection and snapshots; the practical way to test rooted devices at scale. |
| [Genymotion](https://www.genymotion.com/) | ✅ Commercial | Commercial Android emulator with rooted images, cloud instances and pentest-friendly device configuration. |
| [gnirehtet](https://github.com/Genymobile/gnirehtet) | ⚠️ 2024-08 | Reverse tethering — gives a device network access through the host, which also routes its traffic through host-side tooling. |

## Network interception and TLS testing

Since Android 7 apps no longer trust user-installed CAs by default, and modern apps frequently pin.
Expect to combine a proxy with either a system-CA module or runtime pinning bypass.

| Tool | Status | Description |
| --- | --- | --- |
| [Burp Suite](https://portswigger.net/burp) | ✅ | Intercepting proxy and web security testing platform. Sits as a man-in-the-middle between app and server, allowing inspection and modification of traffic in both directions. |
| [Caido](https://caido.io/) | ✅ 2026-07 | Modern lightweight web security proxy with a Rust backend and project-based workflow; increasingly used as a Burp alternative. |
| [OWASP ZAP](https://www.zaproxy.org/) | ✅ 2026-08 | Free, actively maintained intercepting proxy and scanner, usable in the same role as Burp. Now maintained as ZAP by Checkmarx; the old OWASP wiki page is gone. |
| [mitmproxy](https://github.com/mitmproxy/mitmproxy) | ✅ 2026-08 | Interactive, TLS-capable intercepting HTTP proxy with a strong Python scripting API for automated traffic manipulation. |
| [HTTP Toolkit](https://httptoolkit.com/) | ✅ 2026-02 | Open source tool for intercepting and debugging HTTP(S), with one-click Android setup including ADB-based interception. |
| [frida-interception-and-unpinning](https://github.com/httptoolkit/frida-interception-and-unpinning) | ✅ 2026-07 | Maintained Frida scripts that disable pinning and redirect app traffic to a proxy. The most current answer to certificate pinning. |
| [PCAPdroid](https://github.com/emanuele-f/PCAPdroid) | ✅ 2026-07 | No-root network monitor, firewall and PCAP capture on the device itself, with per-app TLS decryption support. Ideal when you cannot root. |
| [SSLyze](https://github.com/nabla-c0d3/sslyze) | ✅ 2026-07 | Fast SSL/TLS configuration scanner and library, for checking the server side of an app's connections. Replaces the dead NCC Group links. |
| [testssl.sh](https://github.com/testssl/testssl.sh) | ✅ 2026-07 | Shell-based TLS/SSL testing against any port, covering protocols, ciphers and known flaws. |
| [Wireshark](https://www.wireshark.org/) | ✅ | The standard network protocol analyser, for capturing and inspecting traffic for insecure protocols or cleartext payloads. |
| [nmap](https://nmap.org/) | ✅ | Network discovery and security auditing — used against the backend and device services rather than the app itself. |
| [Nuclei](https://github.com/projectdiscovery/nuclei) | ✅ 2026-07 | Template-driven vulnerability scanner, useful for sweeping the API endpoints an app talks to. |
| [OpenVAS / Greenbone](https://openvas.org/) | ✅ | Vulnerability scanning and management framework for the supporting infrastructure. |
| [apk-mitm](https://github.com/niklashigi/apk-mitm) | ⚠️ 2024-07 | Automatically rewrites an APK to remove pinning and permit user CAs, producing an interceptable build. Still widely used despite the pause in development. |

## Fuzzing

The Android-specific fuzzers from the 2010s are effectively dead. Current practice is to point
general-purpose, coverage-guided fuzzers at native libraries and JVM code, and to use drozer for
intent-level testing.

| Tool | Status | Description |
| --- | --- | --- |
| [AFL++](https://github.com/AFLplusplus/AFLplusplus) | ✅ 2026-08 | Coverage-guided fuzzer with QEMU and frida modes; the mainstream choice for fuzzing native `.so` libraries extracted from an APK. |
| [Honggfuzz](https://github.com/google/honggfuzz) | ✅ 2026-06 | Security-oriented, feedback-driven fuzzer with software- and hardware-based coverage and useful analysis options. Supports Android targets. |
| [Jazzer](https://github.com/CodeIntelligenceTesting/jazzer) | ✅ 2026-07 | Coverage-guided, in-process fuzzing for the JVM — the route to fuzzing an app's Java/Kotlin parsing logic. |
| [syzkaller](https://github.com/google/syzkaller) | ✅ 2026-08 | Unsupervised coverage-guided kernel fuzzer, used against Android kernels and drivers. |
| [OSS-Fuzz](https://github.com/google/oss-fuzz) | ✅ 2026-07 | Continuous fuzzing service for open source projects; the model to copy for sustained fuzzing of app dependencies. |
| [drozer](https://github.com/ReversecLabs/drozer) | ✅ 2026-04 | Its intent fuzzing modules are the maintained replacement for the various standalone intent fuzzers. |

## APK acquisition and corpora

| Tool | Status | Description |
| --- | --- | --- |
| [apkeep](https://github.com/EFForg/apkeep) | ✅ 2026-05 | EFF's command-line APK downloader supporting several sources. The current answer to the dead Play crawler scripts. |
| [Aurora Store](https://github.com/whyorean/AuroraStore) | ✅ 2026-08 | Open source Play Store client that can fetch APKs, including anonymously. |
| [AndroZoo](https://androzoo.uni.lu/) | ✅ | Research dataset of millions of APKs with metadata, available to academics — the standard corpus for large-scale study. |
| [APKMirror](https://www.apkmirror.com/) | ✅ | Widely used repository of signature-verified APKs and historical versions. |
| [APKCombo](https://apkcombo.com/) | ✅ | Alternative APK and split-APK source, useful for pulling specific versions or architectures. |
| [googleplay-api](https://github.com/NoMore201/googleplay-api) | ⚠️ 2023-08 | Unofficial Python Play Store API for searching and downloading apps. The most recently maintained of the Play API libraries. |
| [gplaycli](https://github.com/matlink/gplaycli) | ⚠️ 2021-10 | Command-line Play Store downloader built on the Python API. |

Verify the signature and hash of anything pulled from a third-party mirror before analysing or
installing it.

## Privacy, trackers, dependencies and SBOM

| Tool | Status | Description |
| --- | --- | --- |
| [Exodus Privacy](https://reports.exodus-privacy.eu.org/) | ✅ 2026-05 | Static analysis platform that identifies trackers and permissions in Android apps, with a public report database. [Repo](https://github.com/Exodus-Privacy/exodus). |
| [TrackerControl](https://github.com/TrackerControl/tracker-control-android) | ✅ 2026-08 | On-device monitoring and blocking of trackers and ads, exposing where an app actually sends data at runtime. |
| [AppCensus](https://www.appcensus.io/) | ✅ | Dynamic-analysis-backed database of the personal data apps actually transmit. |
| [Play SDK Index](https://play.google.com/sdks) | ✅ | Google's index of commercial SDKs with policy and safety signals — useful context on third-party code found in an app. |
| [Play Integrity API](https://developer.android.com/google/play/integrity) | ✅ | Google's device and app integrity attestation. Understand it both as a control to test and as an obstacle to instrumentation. |
| [osv-scanner](https://github.com/google/osv-scanner) | ✅ 2026-08 | Scans dependency manifests and lockfiles against the OSV vulnerability database. Straightforward to run against Gradle projects. |
| [Trivy](https://github.com/aquasecurity/trivy) | ✅ 2026-08 | Broad scanner for vulnerabilities, misconfiguration, secrets and SBOM generation, including JVM dependencies. |
| [OWASP dependency-check](https://github.com/dependency-check/DependencyCheck) | ✅ 2026-08 | Identifies known-vulnerable dependencies via CPE matching, with a Gradle plugin. Moved from the `jeremylong` org. |
| [Syft](https://github.com/anchore/syft) | ✅ 2026-07 | Generates SBOMs from images and filesystems, usable to inventory what actually ships inside a build. |
| [CycloneDX Gradle plugin](https://github.com/CycloneDX/cyclonedx-gradle-plugin) | ✅ 2026-07 | Produces CycloneDX SBOMs directly from Gradle projects. |

## Malware analysis and mobile forensics

| Tool | Status | Description |
| --- | --- | --- |
| [MVT](https://github.com/mvt-project/mvt) | ✅ 2026-07 | Mobile Verification Toolkit — consent-based forensic triage of devices and backups for indicators of compromise. |
| [capa](https://github.com/mandiant/capa) | ✅ 2026-07 | Identifies capabilities in executables from static features; applicable to the native components of Android malware. |
| [Volatility 3](https://github.com/volatilityfoundation/volatility3) | ✅ 2026-07 | Memory forensics framework, including Android/Linux memory support. Replaces the old standalone memory dumpers. |
| [VirusTotal](https://www.virustotal.com/) | ✅ | Multi-engine file and URL analysis with behavioural reports; the quickest triage of a suspicious APK. |
| [Koodous](https://koodous.com/) | ✅ | Collaborative platform for Android malware analysis, with a shared APK corpus and YARA rulesets. |
| [APKiD](https://github.com/rednaga/APKiD) | ✅ 2026-07 | Also belongs here — packer and obfuscator identification is usually the first step in malware triage. |
| [DroidBox](https://github.com/pjlantz/droidbox) | ⚠️ 2023-06 | Dynamic analysis reporting hashes, network data, file operations, `DexClassLoader` activity, information leaks, circumvented permissions and crypto operations. Targets old API levels. |
| [CuckooDroid](https://github.com/idanr1986/cuckoo-droid) | ⚠️ 2020-11 | Android extension for Cuckoo Sandbox, automating execution and analysis of Android applications. |

## Vulnerable apps and training labs

| Tool | Status | Description |
| --- | --- | --- |
| [OWASP MASTG crackmes](https://mas.owasp.org/crackmes/) | ✅ | Reversing challenges maintained alongside the MASTG, covering root detection, anti-tampering and obfuscation. The best-maintained practice material. |
| [OVAA](https://github.com/oversecured/ovaa) | ⚠️ 2024-07 | Oversecured Vulnerable Android App — a compact collection of realistic, modern vulnerability classes. |
| [InsecureBankv2](https://github.com/dineshshetty/Android-InsecureBankv2) | ⚠️ 2024-04 | Vulnerable banking app covering a wide range of common Android flaws, with a companion backend. |
| [Damn Vulnerable Bank](https://github.com/rewanthtammana/Damn-Vulnerable-Bank) | ⚠️ 2023-12 | Intentionally vulnerable banking application, including root and tamper detection to practise bypassing. |
| [DIVA Android](https://github.com/payatu/diva-android) | ⚠️ 2023-05 | Damn Insecure and Vulnerable App — small, focused exercises on insecure storage, logging and IPC. |

## Platform utilities

| Tool | Status | Description |
| --- | --- | --- |
| [adb](https://developer.android.com/tools/adb) | ✅ | Android Debug Bridge — the base interface to a device for shell access, app install, file transfer and port forwarding. |
| [apksigner](https://developer.android.com/tools/apksigner) | ✅ | Signs APKs and verifies signatures and signature scheme versions. Required after any repackaging. |
| [bundletool](https://developer.android.com/tools/bundletool) | ✅ | Builds and inspects Android App Bundles, and generates the split APK sets a device would actually install. |
| [adb autocomplete](https://github.com/mbrubeck/android-completion) | ✅ 2025-11 | Bash completion for `adb`, `fastboot`, `emulator` and other SDK tools. |
| [Dalvik bytecode reference](https://source.android.com/docs/core/runtime/dalvik-bytecode) | ✅ | Official DEX instruction reference — the replacement for the various dead opcode listing pages. |

---

## Archived and historical tools

These were on the original list and are kept here for provenance. All are archived, abandoned, or
have dead download links, and most target Android versions no longer worth testing. Use the
replacement where one is given.

### Static analysis

| Tool | Fate | Replacement |
| --- | --- | --- |
| ApkAnalyser (Sony) | 🗄️ Archived 2023 | [apkanalyzer](https://developer.android.com/tools/apkanalyzer), [jadx](https://github.com/skylot/jadx) |
| [APKInspector](https://github.com/honeynet/apkinspector/) | 🗄️ Last push 2013 | [jadx](https://github.com/skylot/jadx), [MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF) |
| DidFail (CMU) | 🗄️ Dead link | [FlowDroid](https://github.com/secure-software-engineering/FlowDroid) |
| DroidLegacy | 🗄️ Bitbucket 404 | [Quark-Engine](https://github.com/ev-flow/quark-engine) |
| PSU tools (Dare, Ded, Kirin, JLift) | 🗄️ Site offline | [jadx](https://github.com/skylot/jadx), [Androguard](https://github.com/androguard/androguard) |
| Smali-CFGs | 🗄️ Repo deleted | [Ghidra](https://github.com/NationalSecurityAgency/ghidra), [jadx](https://github.com/skylot/jadx) |
| PSCout | 🗄️ Site offline | [Androguard](https://github.com/androguard/androguard) |
| Amandroid | 🗄️ Renamed | [Argus-SAF](https://github.com/arguslab/Argus-SAF) (itself stale since 2023) |
| [smalisca](https://github.com/dorneanu/smalisca) | 🗄️ Archived 2019 | [Androguard](https://github.com/androguard/androguard) |
| [CFGScanDroid](https://github.com/TACIXAT/CFGScanDroid) | 🗄️ Last push 2015 | [Quark-Engine](https://github.com/ev-flow/quark-engine) |
| [maldrolyzer](https://github.com/maldroid/maldrolyzer) | 🗄️ Last push 2015 | [Quark-Engine](https://github.com/ev-flow/quark-engine), [capa](https://github.com/mandiant/capa) |
| SPARTA | 🗄️ Site 404 | [Checker Framework](https://checkerframework.org/) still exists |
| [ConDroid](https://github.com/JulianSchuette/ConDroid) | 🗄️ Last push 2016 | [Reversense](https://github.com/reversenseorg/dexcalibur) |
| [DroidRA](https://github.com/serval-snt-uni-lu/DroidRA) | 🗄️ Last push 2020 | [Reversense](https://github.com/reversenseorg/dexcalibur) |
| [SUPER](https://github.com/SUPERAndroidAnalyzer/super) | 🗄️ Archived 2020 | [mobsfscan](https://github.com/MobSF/mobsfscan), [Semgrep](https://semgrep.dev/) |
| [ClassyShark](https://github.com/google/android-classyshark) | 🗄️ Archived 2023 | [jadx](https://github.com/skylot/jadx), [diffuse](https://github.com/JakeWharton/diffuse) |
| [droidstatx](https://github.com/devoteam-cybertrust/droidstatx) | 🗄️ Archived 2018 | [MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF) |
| [AndroBugs](https://github.com/AndroBugs/AndroBugs_Framework) | 🗄️ Archived 2019 | [mobsfscan](https://github.com/MobSF/mobsfscan) |
| [JAADAS](https://github.com/flankerhqd/JAADAS) | 🗄️ Archived 2017 | [FlowDroid](https://github.com/secure-software-engineering/FlowDroid) |
| Devknox | 🗄️ Product discontinued | [Semgrep](https://semgrep.dev/), Android Lint |
| [Nogotofail](https://github.com/google/nogotofail) | 🗄️ Archived 2022 | [testssl.sh](https://github.com/testssl/testssl.sh), [mitmproxy](https://github.com/mitmproxy/mitmproxy) |
| Pithus | 🗄️ Marked unmaintained; site unreachable | [MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF) |

### Reverse engineering

| Tool | Fate | Replacement |
| --- | --- | --- |
| [JesusFreke/smali](https://github.com/JesusFreke/smali) | 🗄️ Archived 2024 | [google/smali](https://github.com/google/smali) |
| [enjarify](https://github.com/google/enjarify) | 🗄️ Archived 2019 | [jadx](https://github.com/skylot/jadx) |
| Dedexer | 🗄️ SourceForge, dead | [baksmali](https://github.com/google/smali) |
| Dare | 🗄️ Site offline | [jadx](https://github.com/skylot/jadx) |
| Jad | 🗄️ Unmaintained since 2001 | [CFR](https://www.benf.org/other/cfr/), [FernFlower](https://github.com/JetBrains/fernflower) |
| Smali viewer | 🗄️ Download and manual links dead | [jadx](https://github.com/skylot/jadx) |
| [AndBug](https://github.com/swdunlop/AndBug) | 🗄️ Last push 2016 | [Frida](https://frida.re/) |
| [Fino](https://github.com/sysdream/fino) | 🗄️ Last push 2014 | [objection](https://github.com/sensepost/objection) |
| [Introspy-Android](https://github.com/iSECPartners/Introspy-Android) | 🗄️ Last push 2014 | [objection](https://github.com/sensepost/objection) |
| [AFE](https://github.com/appknox/AFE) | 🗄️ Last push 2015 | [drozer](https://github.com/ReversecLabs/drozer) |
| [Android-KillPermAndSigChecks](https://github.com/iSECPartners/Android-KillPermAndSigChecks) | 🗄️ Cydia Substrate era | [Frida](https://frida.re/) |
| [Android-OpenDebug](https://github.com/iSECPartners/Android-OpenDebug) | 🗄️ Cydia Substrate era | [Frida](https://frida.re/), [apkpatcher](https://github.com/badadaf/apkpatcher) |
| [Redexer](https://github.com/plum-umd/redexer) | 🗄️ Last push 2021 | [LIEF](https://github.com/lief-project/LIEF), [Apktool](https://apktool.org/) |
| [diff-gui](https://github.com/antojoseph/diff-gui) | 🗄️ Last push 2016 | [RMS](https://github.com/m0bilesecurity/RMS-Runtime-Mobile-Security) |
| [frida-dexdump](https://github.com/hluwa/frida-dexdump) | 🗄️ Archived 2023 | [Reversense](https://github.com/reversenseorg/dexcalibur) |
| [Wallbreaker](https://github.com/hluwa/Wallbreaker) | 🗄️ Archived 2023 | [objection](https://github.com/sensepost/objection) |
| Indroid | 🗄️ Bitbucket, thread injection PoC | [Frida](https://frida.re/) |
| smalihook | 🗄️ Blogspot post from 2011 | [Frida](https://frida.re/) |
| AXMLPrinter2 | 🗄️ Google Code archive | [Apktool](https://apktool.org/), [apkanalyzer](https://developer.android.com/tools/apkanalyzer) |

### Dynamic analysis, distros and devices

| Tool | Fate | Replacement |
| --- | --- | --- |
| Xposed (original) | 🗄️ Abandoned; XDA thread 403s | [Vector](https://github.com/JingMatrix/Vector), [LSPatch](https://github.com/JingMatrix/LSPatch) |
| [adbi](https://github.com/crmulliner/adbi) | 🗄️ Last push 2017 | [Frida](https://frida.re/) |
| [Android Hooker](https://github.com/AndroidHooker/hooker) | 🗄️ Archived 2016 | [Frida](https://frida.re/), [Medusa](https://github.com/Ch0pin/medusa) |
| [ProbeDroid](https://github.com/ZSShen/ProbeDroid) | 🗄️ Last push 2018 | [Frida](https://frida.re/) |
| [AppMon](https://github.com/dpnishant/appmon) | 🗄️ Archived 2023 | [RMS](https://github.com/m0bilesecurity/RMS-Runtime-Mobile-Security), [objection](https://github.com/sensepost/objection) |
| [Aurasium](https://github.com/xurubin/aurasium) | 🗄️ Last push 2015 | [Frida](https://frida.re/) |
| [android-lkms](https://github.com/strazzere/android-lkms) | 🗄️ Last push 2014 | [KernelSU](https://github.com/tiann/KernelSU) |
| [Mem](https://github.com/MobileForensicsResearch/mem) | 🗄️ Last push 2015 | [Volatility 3](https://github.com/volatilityfoundation/volatility3), [MVT](https://github.com/mvt-project/mvt) |
| [AuditdAndroid](https://github.com/drwhomphd/AuditdAndroid) | 🗄️ Last push 2013 | Platform audit logging |
| [DECAF / DroidScope](https://github.com/decaf-project/DECAF) | 🗄️ Effectively dormant | [Frida](https://frida.re/), [MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF) |
| ASEF | 🗄️ Google Code archive | [MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF) |
| [android-vts](https://github.com/AndroidVTS/android-vts) | 🗄️ Last push 2019 | [Android Security Bulletins](https://source.android.com/docs/security/bulletin) |
| [MARA Framework](https://github.com/xtiankisutsa/MARA_Framework) | 🗄️ Last push 2019 | [MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF) |
| [Androl4b](https://github.com/sh4hin/Androl4b) | 🗄️ Last push 2023 | [Waydroid](https://github.com/waydroid/waydroid) plus tools installed directly |
| [Vezir Project](https://github.com/oguzhantopgul/Vezir-Project) | 🗄️ Last push 2016 | As above |
| [CobraDroid](https://github.com/jakev/CobraDroidBeta/) | 🗄️ Last push 2020 | [Corellium](https://www.corellium.com/), rooted AVD |
| Android Tamer | 🗄️ Site unreachable | [Waydroid](https://github.com/waydroid/waydroid), [redroid](https://github.com/remote-android/redroid-doc) |
| AppUse | 🗄️ Redirects to a legacy page | As above |
| Appie | 🗄️ Unmaintained | As above |
| Android Malware Analysis Toolkit | 🗄️ Download link dead | [MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF) |
| Android Reverse Engineering VM | 🗄️ Honeynet redmine offline | [Waydroid](https://github.com/waydroid/waydroid) |
| [GoatDroid](https://github.com/nvisium-jack-mannino/OWASP-GoatDroid-Project) | 🗄️ Archived 2014 | [OWASP MASTG crackmes](https://mas.owasp.org/crackmes/) |
| [InjuredAndroid](https://github.com/B3nac/InjuredAndroid) | 🗄️ Archived 2021 | [OVAA](https://github.com/oversecured/ovaa) |
| ExploitMe Android Labs | 🗄️ Unmaintained | [OWASP MASTG crackmes](https://mas.owasp.org/crackmes/) |

### Fuzzing, crawlers and network

| Tool | Fate | Replacement |
| --- | --- | --- |
| IntentFuzzer / IntentSniffer (NCC) | 🗄️ nccgroup.trust dead | [drozer](https://github.com/ReversecLabs/drozer) |
| [radamsa-android](https://github.com/anestisb/radamsa-android) | 🗄️ Last push 2019 | [AFL++](https://github.com/AFLplusplus/AFLplusplus) |
| [melkor-android](https://github.com/anestisb/melkor-android) | 🗄️ Last push 2014 | [AFL++](https://github.com/AFLplusplus/AFLplusplus) |
| [MFFA](https://github.com/fuzzing/MFFA) | 🗄️ Last push 2016 (Stagefright era) | [AFL++](https://github.com/AFLplusplus/AFLplusplus), [Honggfuzz](https://github.com/google/honggfuzz) |
| [AndroFuzz](https://github.com/jonmetz/AndroFuzz) | 🗄️ Last push 2014 | [AFL++](https://github.com/AFLplusplus/AFLplusplus) |
| [google-play-crawler](https://github.com/Akdeniz/google-play-crawler) | 🗄️ Archived 2023 | [apkeep](https://github.com/EFForg/apkeep) |
| [googleplay-api (egirault)](https://github.com/egirault/googleplay-api) | 🗄️ Archived 2017 | [apkeep](https://github.com/EFForg/apkeep) |
| [node-google-play](https://github.com/dweinstein/node-google-play) | 🗄️ Archived 2021 | [apkeep](https://github.com/EFForg/apkeep) |
| [node-aptoide](https://github.com/dweinstein/node-aptoide) | 🗄️ Last push 2015 | [apkeep](https://github.com/EFForg/apkeep) |
| [node-appland](https://github.com/dweinstein/node-appland) | 🗄️ Last push 2015 | [apkeep](https://github.com/EFForg/apkeep) |
| APK-Downloader | 🗄️ Site returns 522 | [apkeep](https://github.com/EFForg/apkeep) |
| SSLyze (NCC link) | 🗄️ Old URL dead | [nabla-c0d3/sslyze](https://github.com/nabla-c0d3/sslyze) |
| ZAP (OWASP wiki link) | 🗄️ Wiki page 404 | [zaproxy.org](https://www.zaproxy.org/) |
| [dockerfile-androguard](https://github.com/dweinstein/dockerfile-androguard) | 🗄️ Last push 2019 | Official [Androguard](https://github.com/androguard/androguard) packaging |

---

## Contributing

Pull requests are welcome. When adding a tool, please include a direct link, a one- or two-sentence
description of what it actually does, and check that the project has seen activity recently. When a
tool dies, prefer moving it to the archived section with a replacement over deleting it outright —
the mapping from old tool to current equivalent is often the most useful part of a list like this.

# LeapDroid

LeapDroid is being rewritten as a native WSA-like Android subsystem for Apple Silicon Macs. The new generation targets macOS 14+ on ARM64, with separate Android Desktop and Android TV editions and shared runtime/protocol contracts with Trinity for Windows.

> **Current status:** architecture and repository reset. The complete historical LeapGL implementation is preserved on the [`legacy`](https://github.com/LamPPKK/leapdroid/tree/legacy) branch. The `master` branch is the clean foundation for the macOS ARM64 rewrite and is not yet runnable.

The canonical joint plan is maintained in Trinity: [Trinity and LeapDroid Development Plan](https://github.com/LamPPKK/TrinityEmulator/tree/main/docs/devel/trinity-leapdroid-development-plan).

## Product targets

| Product | Host | Guest ABI | Virtualization | Graphics baseline |
|---|---|---|---|---|
| LeapDroid Desktop | macOS 14+ ARM64 | arm64 | QEMU HVF | Gfxstream + ANGLE Metal; SwiftShader fallback |
| LeapDroid TV | macOS 14+ ARM64 | arm64 | QEMU HVF | Gfxstream + ANGLE Metal; SwiftShader fallback |

## Development checklist

Checkboxes describe implementation and release status on `master`. An item is checked only after its phase exit criteria and tests pass.

### Repository and native foundation

- [x] Preserve the complete previous LeapGL implementation and history on `legacy`.
- [x] Publish the joint Trinity/LeapDroid architecture and phased development plan.
- [ ] Complete Phase 0 macOS HVF, renderer, sandbox, signing, launcher identity, and performance gates.
- [ ] Fix repository and licensing boundaries between the shared GPL runtime and native macOS application components.
- [ ] Add reproducible Apple Silicon Xcode, Swift Package Manager, CMake, and CI toolchains.
- [ ] Implement the LeapDroid daemon and native Swift/SwiftUI/AppKit control center.
- [ ] Add signed and notarized development packages, entitlements, SBOMs, provenance, license reports, and rollback-capable updates.

### Runtime, Android guest, and compatibility

- [ ] Integrate the pinned shared QEMU runtime with HVF for an arm64 guest.
- [ ] Implement the versioned shared control, presentation, input, clipboard, notification, surface, and bulk-data protocols.
- [ ] Boot the shared current AOSP/GKI arm64 Desktop and Android TV products.
- [ ] Implement lifecycle, suspend/resume, snapshots, storage quotas, multi-instance support, APK management, deep links, and opt-in ADB.
- [ ] Do not bundle GApps, Google Play Store, Google Play Services, or other proprietary Google system packages.
- [ ] Preinstall verified F-Droid and add an independently controlled, opt-in microG compatibility profile.
- [ ] Maintain CTS, VTS, compatibility, upgrade, and real-application test matrices for both LeapDroid editions.

### Graphics and native macOS experience

- [ ] Deliver compatibility-first accelerated GLES through Gfxstream and ANGLE Metal.
- [ ] Add Vulkan support after conformance and isolation gates pass.
- [ ] Keep SwiftShader as a tested software fallback.
- [ ] Present every Android task in a real AppKit `NSWindow` with SwiftUI/AppKit-native menus, sheets, commands, title bars, resizing, Retina, multi-display, fullscreen, and PiP behavior.
- [ ] Integrate app identity, icons, state restoration, Spotlight/`NSUserActivity` activation, URL/file opening, accessibility, and native keyboard shortcuts.
- [ ] Decide the signed/notarized per-app launcher-shim gate; otherwise publish the one-Dock-identity fallback while retaining native per-window identity.
- [ ] Add per-app `Auto`, `Phone`, and `Tablet` profiles with orientation, adaptive/fixed form factor, size, density, aspect/letterbox, placement, fullscreen/PiP, and safe reset.
- [ ] Apply Android configuration per task/display without changing unrelated applications or the whole guest.

### Input, host integration, and Android TV

- [ ] Implement `NSEvent`/`NSTextInputClient` keyboard, host-layout, shortcut, and IME handling.
- [ ] Implement absolute pointer, relative capture, cursor synchronization, focus recovery, touch/gesture, and stylus paths.
- [ ] Implement GameController hot-plug, multiple players, stable mappings, battery state, and haptics.
- [ ] Synchronize `NSPasteboard` text, sanitized HTML, and bounded images bidirectionally with loop and sensitive-content protection.
- [ ] Mirror Android notification post/update/remove/open/action/reply/dismiss lifecycle through UserNotifications with per-app/channel policy.
- [ ] Broker selected host folders, drag/drop, audio, microphone, camera, location, and host link/open-with flows through explicit permissions.
- [ ] Deliver a 10-foot Android TV launcher/SystemUI, D-pad/gamepad/media-key navigation, push-to-talk, 1080p and 4K SDR profiles, and TV-aware app discovery.
- [ ] Keep TV clipboard off by default and make notification previews opt-in and allowlisted.

### Privacy, sandboxing, and release quality

- [ ] Enforce a production zero-telemetry profile with no analytics, automatic crash upload, remote logging, experiments, stable tracking ID, or hidden metrics endpoint.
- [ ] Provide bounded local diagnostics with redaction preview and explicit manual export only.
- [ ] Enforce a checked-in first-party egress manifest and packet-capture tests across startup, idle, crash, update, suspend/resume, and app launch.
- [ ] Preserve Android per-UID isolation and harden SELinux, seccomp, spawning, memory safety, dynamic-code policy, and per-app network/sensor controls using reviewed GrapheneOS patterns.
- [ ] Separate UI, VM, renderer, media, updater, diagnostics, and integration helpers with App Sandbox, Hardened Runtime, least-privilege entitlements, authenticated XPC/IPC, resource limits, and no ambient network access.
- [ ] Audit Waydroid guest/HAL, LineageOS microG/TV, GrapheneOS security, and relevant WSA-kernel concepts with pinned provenance and explicit `import`, `reimplement`, or `drop` decisions.
- [ ] Meet boot, resume, frame pacing, input latency, memory, CPU, energy, notification, and TV responsiveness budgets.
- [ ] Pass security review, fuzzing, long-run stability, upgrade/rollback, accessibility, localization, signing/notarization, and release gates for Desktop and TV.

## Legacy source and licensing

Use the `legacy` branch to inspect the historical LeapGL renderer:

```shell
git clone --branch legacy https://github.com/LamPPKK/leapdroid.git
```

LeapGL is a historical reference, not the shipping renderer architecture. Reuse only justified behavior or tests with provenance and a license review. The retained root `LICENSE` continues to describe the repository's Apache-2.0 licensing; GPL runtime derivatives must remain in a clearly separate component/process with their own notices and source obligations.

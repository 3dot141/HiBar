# HiBar

A menu bar manager for **macOS 27** — hide menu bar icons, show them back with one click. Works on notched **and non-notched** displays.

![HiBar](docs/icon.png)

HiBar is a fork of [Ice](https://github.com/jordanbaird/Ice) with a different hiding backend and a friendlier default behavior, adapted for macOS 27.0.1 and non-notched / multi-display setups.

## What's different from upstream Ice

- **Assessment-mode hiding backend** — hidden items are removed through `MenuBarAgent`'s assessment-mode assertions (private `MenuBarClientCore`), which works on non-notched displays where the spacer/overflow approach breaks. Adapted from Ice PR [#995](https://github.com/jordanbaird/Ice/pull/995) / [#1001](https://github.com/jordanbaird/Ice/pull/1001) (RabenkoYevhenii, Carlos Santos et al.).
- **Zero-configuration hiding** — items left of HiBar's button are hidden automatically, like Ice's pre-27 behavior. A layout saved in the editor overrides this per app. (See `Concealer27.layoutInferredByPosition`.)
- **Ad-hoc signing is not supported for hiding** — the assertion must be held by a signed app bundle. Build with a real signing identity.

## Building

Xcode 27+ with the macOS 27 SDK is required (SwiftUI's `@State` macro plugin only ships with Xcode).

```sh
xcodebuild -project Ice.xcodeproj -scheme Ice -configuration Release \
  -derivedDataPath build \
  CODE_SIGN_IDENTITY="Apple Development: …" DEVELOPMENT_TEAM=… \
  ENABLE_HARDENED_RUNTIME=YES \
  build
```

See `MACOS27.md` for the approach, limitations and troubleshooting.

## Credits & License

HiBar is licensed under the **GNU GPL-3.0** (see `LICENSE`), inherited from Ice.

- [jordanbaird/Ice](https://github.com/jordanbaird/Ice) — the original project and its authors
- Ice PR [#995](https://github.com/jordanbaird/Ice/pull/995) & [#1001](https://github.com/jordanbaird/Ice/pull/1001) — the macOS 27 assessment-mode backend (itself adapted from [Barometer](https://github.com/mackid1993/Barometer) and [Thaw](https://github.com/thaw-app/Thaw), GPLv3)
- Ice PR [#950](https://github.com/jordanbaird/Ice/pull/950) — ad-hoc signed build support for `MenuBarItemService`
- Ice PR [#997](https://github.com/jordanbaird/Ice/pull/997) — the alternative public-API macOS 27 approach this fork investigated before switching backends

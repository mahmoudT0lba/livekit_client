# Local patches to livekit_client 2.8.1

Vendored from pub.dev `livekit_client` 2.8.1 (Apache-2.0, see `LICENSE` / `NOTICE`),
wired in through `dependency_overrides` in the app `pubspec.yaml`. The `example/`
app and `coverage/` were dropped; everything else is upstream.

## 1. `lib/src/participant/local.dart` — Dart 3.13 compile fix

`publishVideoTrack`'s nested `negotiate()` used `publishOptions!` as an argument and
then read `publishOptions.videoCodec` / `.degradationPreference` after an `await`.
Dart 3.13 no longer keeps the null-promotion across that suspension, so compiling
fails with "Property 'videoCodec' cannot be accessed on 'VideoPublishOptions?'".

Fix: capture `final VideoPublishOptions negotiatedOptions = publishOptions!;` before
the first `await` and use it for the rest of the closure. Behaviour (codec choice,
degradation preference, SVC bitrate info) is unchanged. The audio `negotiate()` only
passes the value as an argument, so it needs no change.

## Removing this fork

Drop the `livekit_client` entry in `dependency_overrides` once the app moves to a
LiveKit release that compiles on the supported Dart SDK. That upgrade is gated on
the flutter_webrtc fork rebase (see `docs/LIVEKIT_UPGRADE_AUDIT.md`).

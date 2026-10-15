# Local patches

Upstream: `livekit_client` 2.8.1, copied from the published pub.dev package.

## Dart 3.13 video publishing closure

In `lib/src/participant/local.dart`, `_publishVideoTrack.negotiate()` captures the
resolved `publishOptions` in a final non-null local before its first await. The
nullable parameter is reassigned during codec selection, so Dart cannot promote
it inside the closure. All negotiation reads use the same resolved options;
default, supported-codec and server-codec selection remain unchanged.

The app selects this package through a dependency override. Native sources,
SDK versions and public APIs are unchanged. Remove the override when upgrading
to an upstream release containing the equivalent fix.

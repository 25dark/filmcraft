# OVERLORD Studio authenticated FilmCraft control (development branch only)

Base FilmCraft revision: 7b6c134472287db4367fca80bcb498240ba36567.
Studio integration source: 25dark/overlord-studio, feat/imaging-export-video-workspace-20261009.

## Control hardening

When FILMCRAFT_CONTROL_TOKEN is unset, FilmCraft retains its existing 127.0.0.1 JSON-lines control behavior. When set to a nonempty value, every request must include the exact string in params._opStudioToken. Failed requests return {"ok":false,"error":"unauthorized"}. The token is stripped before dispatching requests to the engine/UI handler.

OVERLORD Studio generates a new random token per launched FilmCraft process, chooses a random ephemeral loopback port, sets FILMCRAFT_CONTROL_TOKEN in the child's environment, checks that an unauthenticated UI inspect call is denied, and then connects with the token. Old unprotected FilmCraft binaries are intentionally refused.

The control port is local only but authentication is not a sandbox against malicious same-user native applications able to inspect process state. Do not send this token in renderer IPC payloads or logs.

## Validation and distribution

The Windows workflow compiles/tests the native app but intentionally does not publish a forked binary. Upstream FilmCraft branding and assets are trademarked: rebrand the fork and conduct attribution/codec licensing review before redistribution in OVERLORD Studio.

Smoke acceptance on Windows: launch Studio Video for an existing session; require unauthenticated probe to fail; open/create Video/OVERLORD-Edit.fcproj; import one MP4/MOV; play, stop, undo/redo, save, restart and reopen; verify native UI playback, proper project recovery and no changes to Studio's photo catalog. Native preview and timeline remain in FilmCraft's own window until a separate GPU-embedding project is completed.

# Kybotos apps

The app list of the online store for [Kybotos](https://github.com/kybotos/kybotos) devices, published with GitHub Pages
at `https://kybotos.github.io/apps/index.txt`.

A device opens it from **Add apps → Online**. It installs an app only when **both the list and the app are signed**
with a key built into its firmware, and the app matches the list (size, SHA-512, app id and version).
Nothing here is executed by the server; these are static files.

| File | Contents |
|---|---|
| `index.txt` | The app list (text). Format: `shared/app_index.h` in the kybotos repository |
| `index.sig` | Ed25519 signature of `index.txt` (key id and signature) |
| `apps/<id>/<version>/<name>.wasm` | The apps. Each `.wasm` ends with a `kybotos.signature` custom section (Ed25519 over everything before it). Format: `shared/app_sign.h` |

The list has a serial number; a device remembers the highest one it has seen and refuses older lists.

## Signing key

| Key | Id | Fingerprint (first 16 hex digits of SHA-256 of the public key) |
|---|---|---|
| release-1 | 16 | `c948ce43e1d41919` |

The public key is in `shared/app_keys.h` of the kybotos repository. The private key is not in any repository.
Lists and apps are made with `tools/store/kbstore.py` there (`sign-wasm`, `build-index`).

## License

The apps are built from the kybotos repository and are under its license (Apache-2.0).

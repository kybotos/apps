# Kybotos apps

The app list of the online store for Kybotos devices. A device opens it from **Add apps → Online** and installs an app
only when **both the list and the app are signed** with a key built into its firmware, and the app matches the list.
These are static files for the devices; there is no source code here.

| File | Contents |
|---|---|
| `index.txt` | The app list |
| `index.sig` | The signature of `index.txt` |
| `apps/<id>/<version>/<name>.wasm` | The apps (signed WebAssembly modules) |

## The list (`index.txt`)

Text, one item per line. The first line is `kybotos-index 1`, then `serial <n>`, then one block per app:

```
app
id kybotos.metronome          app id (same as in the app's manifest)
name Metronome
version 1.0.0
color d98a1f                  optional: tile color (RGB)
icon 0000…                    optional: 32 x 32 one-bit icon, 256 hex digits
size 7627                     bytes of the .wasm
sha512 …                      SHA-512 of the .wasm (128 hex digits)
url apps/kybotos.metronome/1.0.0/metronome.wasm   relative to index.txt
end
```

A device remembers the highest `serial` it has seen and refuses an older list. It shows an app as new when the app id is
not installed, and as an update when the listed version is newer than the installed one.

## Signatures (Ed25519)

- `index.sig`: `kybotos-index-sig 1`, `key <key id>`, `sig <signature of index.txt, 128 hex digits>`.
- Each `.wasm` ends with a custom section named `kybotos.signature` (72 bytes: `KBS`, format `1`, key id, three zero bytes,
  then the 64-byte Ed25519 signature of every byte of the file before this section). WebAssembly runtimes skip custom
  sections, so the signature does not change how the app runs.
- A device installs a downloaded app only when its size and SHA-512 match the list, its signature is valid, and its
  manifest has the listed id and version.

| Key | Id | Fingerprint (first 16 hex digits of SHA-256 of the public key) |
|---|---|---|
| release-1 | 16 | `c948ce43e1d41919` |

## License

The app binaries are distributed under the Apache License 2.0.

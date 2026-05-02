# nix-config-secrets

Encrypted secrets for my [NixOS / home-manager configuration](https://github.com/mtaku3/nix-config), managed with [agenix](https://github.com/ryantm/agenix).

This repo is included as a git submodule from the main config and is safe to publish: every `.age` file is encrypted to a fixed set of host SSH keys and user identities. Without the corresponding private key, the contents cannot be decrypted.

## Layout

```
common/         # secrets shared across hosts (e.g. k8s PKI roots)
helios/         # per-host secrets
  home/<user>/  #   home-manager scope
  system/       #   NixOS system scope
m5p01/
xanthus/
```

Each `*.age` blob is decrypted on the target host at activation time by agenix. Recipients are declared in the parent flake's `secrets.nix`.

## Working with secrets

```sh
# edit (requires a recipient identity)
agenix -e path/to/secret.age

# rekey after changing recipient list
agenix -r
```

## License

[MIT](./LICENSE). The license covers the repository structure and metadata; the encrypted payloads carry no implied license to their plaintext.

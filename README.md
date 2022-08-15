# BOUF - Building OBS Updates Fast(er)

`bouf` is the next-generation utility for preparing and building OBS Studio Windows release binaries and updater delta patches.

The main binary `bouf` automates the entire process based on the rules laid out in the config file and command line.

Additionally, the `bouf-sign` utility is provided to sign files verified by the OBS updater (manifest, updater.exe, whatsnew, branches, etc.)

The generated output has the following structure:

* `install/` - OBS install files used to build installer/zip files (signed)
* `updater/`
  + `patches_studio/<branch>/{core,obs-browser}` - delta patches for upload to server 
  + `update_studio/<branch>/{core,obs-browser}` - files split into packages for upload to server
* `pdbs/` - Full PDBs
* `manifest_<branch>.json` and `manifest_<branch>.json.sig` for updater
* `added.txt`, `changed.txt`, and `removed.txt` for manual checks 
* `OBS-Studio-<version>-Installer.exe` - NSIS installer (signed)
* `OBS-Studio-<version>.zip` - ZIP file of `install/`
* `OBS-Studio-<version>-pdbs.zip` - Archive of unstripped PDBs

## Usage:

```
bouf

USAGE:
    bouf.exe [OPTIONS] --config <Config file> --version <Major.Minor.Patch[-(rc|beta)Num]>

OPTIONS:
        --beta <Beta number>
        --branch <Beta branch>
    -c, --config <Config file>
        --clear-output                                  Clear existing output directory
    -h, --help                                          Print help information
    -i, --input <new build>
        --note-file <file.rtf>                          File containing release notes
    -o, --output <output dir>
    -p, --previous <old builds>
        --private-key <file.pem>                        Falls back to "UPDATER_PRIVATE_KEY" env var
        --rc <RC number>
        --skip-codesigning                              Skip codesigning
        --skip-installer                                Skip creating NSIS installer
        --skip-manifest-signing                         Skip signing manifest
        --skip-patches                                  Skip creating delta patches
        --skip-preparation                              Skip creating NSIS installer
    -v, --version <Major.Minor.Patch[-(rc|beta)Num]>    OBS main version
```


**Note:** A valid configuration file based on `extra/config.example.toml` is required (see `extra/ci` for an example).

Some parameters can be set via environment variables (e.g. secrets):
- `UPDATER_PRIVATE_KEY` - updater signing key (PEM, encoded as base64)

## ToDo

- Figure out a license
- Cleanup, bugfixes, rewrites...
  + See "ToDo"s in source code and `rustc` warnings (it angry)
- Also probably more tests.

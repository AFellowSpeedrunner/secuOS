# secuOS
> "SecureROM? More like InSecureROM."

A work-in-progress operating system for iPhones with an A6 to A11 chip built from pongoOS using checkra1n for booting.

## Building on macOS

- Install Xcode's command-line utilities
- Run `make all`

## Building on Linux

- Install clang (if in doubt, from [apt.llvm.org](https://apt.llvm.org))
- Install `ld64` and cctools' `strip`.
  - On Debian-based distros these can be installed from the checkra1n repo:
    ```
    echo 'deb https://assets.checkra.in/debian /' | sudo tee /etc/apt/sources.list.d/checkra1n.list
    sudo apt-key adv --fetch-keys https://assets.checkra.in/debian/archive.key
    sudo apt-get update
    sudo apt-get install -y ld64 cctools-strip
    ```
  - On other distros these can be downloaded from [this repo](https://github.com/Siguza/ld64/releases). They can be self-compiled but it's more recommended to use the releases and copy binaries to /usr/bin.
- Run `make all`

### Building PongoTerm

- Navigate to scripts
- Run make

If `clang`, `ld64` or `cctools-strip` don't have their default names/paths, you'll want to change their invocation. For reference, the default variables are equivalent to:

    EMBEDDED_CC=clang EMBEDDED_LDFLAGS=-fuse-ld=/usr/bin/ld64 STRIP=cctools-strip make all

## Build artifacts

The Makefile will create binaries in `build/`:

- `secuOS` - A Mach-O of the main secuOS
- `secuOS.bin` - Same as the above, but as a bare metal binary that can be jumped to
- `vmacho` - I actually don't know this one, sorry

## Usage with checkra1n

    checkra1n -k secuOS.bin                  # Boots to secuOS
    scripts/pongoterm                        # Used to interact with secuOS (read 'Building PongoTerm') but sudo may be required

## Contributions

By submitting a pull request, you agree to license your contributions under the [MIT License](https://github.com/checkra1n/pongoOS/blob/master/LICENSE.md) and you certify that you have the right to do so.
If you want to import third-party code that cannot be licensed as such, that shall be noted prominently for us to evaluate accordingly.

## Structure

- secuOS and its drivers are in `src/`.
  - Build-time helper tools are in `tools/`.
- The stdlib used by secuOS (aka Newlib) is in `aarch64-none-darwin`.
  - This includes a custom patch for Newlib to work with the Darwin ABI.
- An example module exists in `example/` which can be used for secuOS development.
- Scripts to communicate with the secuOS shell (secuShell) are in `scripts/`.
  - This includes `pongoterm`, an interactive shell client for macOS and Linux. Read the 'Building PongoTerm' section.

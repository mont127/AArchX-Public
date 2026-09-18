<p align="center">
  <h1 align="center"><em>AArchX</em></h1>
</p>
<p align="center">
  <b>An x86-64 to arm64 userspace binary translator for macOS.</b><br>
  <sub>Run Intel macOS software on Apple Silicon without using Rosetta's translator at runtime.</sub>
</p>
<p align="center">
  <img alt="platform" src="https://img.shields.io/badge/platform-macOS%20Apple%20Silicon-lightgrey.svg">
  <img alt="architecture" src="https://img.shields.io/badge/guest-x86--64-orange.svg">
  <img alt="host" src="https://img.shields.io/badge/host-arm64-blue.svg">
  <img alt="status" src="https://img.shields.io/badge/status-experimental-yellow.svg">
</p>

[!WARNING]
AArchX is experimental software. Compatibility is incomplete and crashes, incorrect behavior, or performance issues are possible. Do not rely on it for production workloads or important data.

About

AArchX is a from-scratch userspace binary translator for Apple Silicon Macs.

It can load Intel x86-64 Mach-O executables and execute them on arm64 using its own:

* x86 decoder
* interpreter
* arm64 JIT compiler
* Mach-O loader
* dynamic linker
* syscall compatibility layer
* threading and signal support

AArchX does not use Rosetta’s binary translator at runtime.

The Rosetta package is currently still required in the default compatibility mode because macOS distributes its x86-64 shared system cache as part of Rosetta. AArchX maps and uses that cache itself.

AArchX was previously called Ocerz, and the executable and some configuration names still use ocerz.

Downloads

Precompiled builds are available from the Releases section of this repository.

Download the latest release for Apple Silicon macOS, extract it, and make the binary executable if necessary:

chmod +x ocerz

Check that it runs:

./ocerz version

Usage

Run an x86-64 executable by passing it to AArchX:

./ocerz /path/to/program

For example:

./ocerz /Applications/SomeApp.app/Contents/MacOS/SomeApp

Arguments can be passed normally:

./ocerz /path/to/program arg1 arg2

Basic options:

usage: ocerz [-v] [-trace] [-strace] [-no-jit] [-native|-cache] [-path file] [--] program [args...]
       ocerz version

Option	Description
-v	Enable additional logging
-trace	Trace guest x86 instructions
-strace	Trace guest syscalls
-no-jit	Run using the interpreter instead of the JIT
-cache	Use Apple’s x86-64 shared cache
-native	Experimental native-framework bridge mode
--	End AArchX options

Requirements

AArchX currently requires:

* an Apple Silicon Mac
* a supported version of macOS
* Apple’s Rosetta package installed when using the default -cache mode

Rosetta can normally be installed with:

softwareupdate --install-rosetta

AArchX does not use Rosetta to translate the application itself. The package is currently required because it provides the x86-64 macOS shared cache used by AArchX.

Compatibility

AArchX is under active development and does not yet run every Intel application.

Software that has been successfully brought up during development includes:

* macOS command-line programs
* several built-in macOS applications
* Steam’s x86-64 macOS client
* Wine
* i386 Windows applications through Wine WoW64
* Ollama’s x86-64 command-line tools and application

Compatibility varies between applications and AArchX releases.

A program opening successfully does not necessarily mean every feature of that program works correctly.

Performance

AArchX includes an arm64 JIT compiler and supports a growing range of modern x86 instructions, including significant SSE, AVX2, FMA, BMI and related instruction support.

On some internal microbenchmarks AArchX performs close to Rosetta, and some workloads run faster.

These results should not be interpreted as a claim that AArchX is generally faster than Rosetta. Real-world performance depends heavily on the application and on which parts of the x86 instruction set and macOS runtime it uses.

Native mode

AArchX also contains an experimental mode that can run x86-64 programs against synthesized x86 system libraries which bridge into the Mac’s native arm64 frameworks.

Run it with:

./ocerz -native /path/to/program

Native mode is significantly more limited than the default shared-cache mode and is intended primarily for development and experimentation.

Reporting issues

If an application does not work, please open an issue and include:

* your Mac model / Apple Silicon generation
* macOS version
* AArchX release version
* application name and version
* the exact command used to launch it
* terminal output or crash information
* whether the problem also occurs with -no-jit

For additional diagnostics, you can try:

./ocerz -v /path/to/program

or:

./ocerz -strace /path/to/program

Please avoid including passwords, authentication tokens, personal file paths, or other sensitive information in issue reports.

Source code

This repository contains public release builds of AArchX, not the main development source tree.

The AArchX source code is maintained separately.

License

AArchX is source-available software distributed under the AArchX Proprietary License.

The software may be used for permitted personal, educational, academic and research purposes. Commercial use, redistribution, bundling, and distribution of modified versions may require explicit permission.

See the license distributed with each release for the exact terms.

⸻

AArchX is experimental and evolving quickly. Compatibility, performance and behavior may change substantially between releases.

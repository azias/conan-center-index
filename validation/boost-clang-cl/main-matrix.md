# Boost clang-cl validation results

Each row records execution, not an inference from another configuration.
Maximum concurrency: one case, four build jobs.

Components: JSON, Container, Exception and Test; Boost 1.91 also explicitly enables
Regex because Test -> Algorithm -> Regex is not represented in recipe dependency data.
Extended cases additionally enable Atomic, Chrono, DateTime, Filesystem, Regex,
Serialization and Thread. No claim is made that every Boost module was compiled.

| Compiler | Boost | Build | Link | Layout | Selection | Status |
| --- | --- | --- | --- | --- | --- | --- |
| clang-cl | 1.89.0 | Debug | shared | system | minimal | passed |
| clang-cl | 1.89.0 | Debug | static | system | minimal | passed |
| clang-cl | 1.89.0 | Release | shared | system | minimal | passed |
| clang-cl | 1.89.0 | Release | static | system | minimal | passed |
| clang-cl | 1.91.0 | Debug | shared | system | minimal | passed |
| clang-cl | 1.91.0 | Debug | static | system | minimal | passed |
| clang-cl | 1.91.0 | Release | shared | system | minimal | passed |
| clang-cl | 1.91.0 | Release | static | system | minimal | passed |
| clang-cl | 1.91.0 | Debug | shared | tagged | minimal | passed |
| clang-cl | 1.91.0 | Release | static | tagged | minimal | passed |
| clang-cl | 1.91.0 | Debug | shared | versioned | minimal | passed |
| clang-cl | 1.91.0 | Release | static | versioned | minimal | passed |
| clang-cl | 1.90.0 | Release | shared | system | minimal | passed |
| clang-cl | 1.89.0 | Debug | shared | system | extended | passed |
| clang-cl | 1.91.0 | Release | static | system | extended | passed |
| msvc | 1.89.0 | Release | shared | system | minimal | passed |
| msvc | 1.89.0 | Release | static | system | minimal | passed |
| msvc | 1.91.0 | Release | shared | system | minimal | passed |
| msvc | 1.91.0 | Release | static | system | minimal | passed |
| linux-gcc | 1.89.0 | Release | shared | system | minimal | passed |
| linux-gcc | 1.89.0 | Release | static | system | minimal | passed |
| linux-gcc | 1.91.0 | Release | shared | system | minimal | passed |
| linux-gcc | 1.91.0 | Release | static | system | minimal | passed |
| linux-clang | 1.91.0 | Release | shared | system | minimal | passed |

Counts: passed=24

## Acceptance performed for successful rows

- Conan installation and package_info completed without missing/unused-library errors.
- The CMake JSON round-trip and always-static-component consumers built and ran.
- The upstream recipe test_package built and ran.
- Windows archive directives and JSON DLL dependencies match the requested dynamic CRT.
- Linux consumers were also inspected with ldd.
- Exact argument arrays, profiles, graph and detailed logs are retained under out/<case>/.

## Independent findings

- The official 1.89 baseline was reproduced without the consuming application dependencies.
- clang-cl ignores -fPIC and -stdlib=None; lld-link also ignores the latter.
  They did not cause the demonstrated package_info failure and remain outside the naming diff.
- VS 2022 headers were confirmed in actual object CodeView records, rather than inferred
  from discovery messages. Selected components do not qualify every assembler/Context path.
- Boost 1.91's implicit Regex dependency is tracked separately as CCI issue #31040.
- The static/shared naming patch and the Microsoft CRT follow-up remain separate.

## Limits

MSVC and Linux regression rows use Release. The required clang-cl system-layout rows
cover both Debug and Release. Only the listed configurations were compiled; the 1.90
intermediate-version check is a single Release/shared case. MinGW, legacy
compiler=msvc/toolset=ClangCL, other architectures and all optional Boost modules
are not qualified by this matrix.

## Additional executed checks

Current b2 smoke: passed; official failure: reproduced.
Four MSVC-consumer/clang-cl-Boost profile probes reuse binaries with --build=never.
Two original CRT mismatches and two successful separate runtime-follow-up cases are recorded separately.
The follow-up rejects shared/static-CRT before compilation.

The main Windows matrix uses the dynamic CRT with runtime_type matching build_type.
The combined runtime-follow-up recipe did not run the entire main matrix.

# Boost clang-cl validation artifacts

The naming and runtime recipe corrections have separate diffs and bug reports. These files contain the full build logs, profiles and the additional compatibility results used during local validation.

- [Naming issue: full build output and profiles](naming-issue-logs.zip)
- [Runtime issue: full build output, archive directives and profiles](runtime-issue-logs.zip)
- [Standalone reproducer, profiles and scripts](boost-clang-cl-reproducer.zip)
- [Original 24-case matrix](main-matrix.md)
- [Additional old-version, CRT and MSYS2 checks](additional-checks.md)

Paths in the logs have been neutralized. No Conan cache or prebuilt Boost package is included. Successful and deliberately failing configurations are distinguished in the reports. The two recipe PRs contain only their respective changes to conanfile.py; these validation files are kept on a separate branch.

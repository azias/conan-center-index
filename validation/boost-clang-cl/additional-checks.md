# Additional compatibility results

The original 24-case matrix is unchanged. These results are additional executed cases.

| Recipe change | Compiler | Boost | Configuration | Layout | Result |
| --- | --- | --- | --- | --- | --- |
| naming | clang-cl | 1.78.0 | Debug, shared | versioned | passed |
| naming | clang-cl | 1.78.0 | Release, static | versioned | passed |
| naming | clang-cl | 1.85.0 | Release, shared | tagged | passed |
| naming + runtime | clang-cl | 1.78.0 | Debug, shared, dynamic Release CRT | tagged | passed |
| naming + runtime | clang-cl | 1.78.0 | Release, static, static Release CRT | versioned | passed |
| naming + runtime | msys2-gcc | 1.89.0 | Release, shared | system | passed |

Every successful row passed package_info(), two CMake programs and the recipe test_package. Windows clang-cl rows also passed archive CRT checks and applicable JSON DLL dependency checks. The MSYS2 GCC row uses GNU/UCRT runtime libraries rather than MSVC CRT flags.

## MSYS2 CLANG64 comparison

Clang 23.1.4 targets x86_64-w64-windows-gnu. The official recipe, naming correction, and combined runtime correction all fail before packaging because the recipe selects clang-win and sends MSVC-style arguments to clang++. This is a reproduced existing failure, not a successful MSYS2 Clang build. UCRT64 GCC 16.2.0 passed with the combined corrections.

## Other evidence

The two CRT cases on Boost 1.78 use the combined corrections. They extend the separate runtime validation from 1.91 to the oldest supported version. The official Boost 1.91 static/MT reproducer was also run without either correction: it produces MSVCRT archive directives despite static CRT settings.

Metadata for all 14 recipe-supported versions was reviewed for the always-static component list. That is not a claim that all versions/configurations were compiled. No Boost 1.92 test is claimed.

## Isolation

The original MSYS2 installation has no toolchain packages and was not updated. A portable MSYS2 root and a dedicated Conan cache were prepared under the investigation directory. Compiler-version extensions for Clang 23 and GCC 16 were installed through settings_user.yml in that dedicated cache only. Personal profiles and the consuming application sources were preserved.


## Recent Conan client

Conan 2.34.0 consumed the exact qualified Boost 1.78 static/MT binary from a separate cache with --build=never. The two CMake programs and the recipe test_package passed. The binary itself was built with Conan 2.22.2; this was a consumption check, not another Boost build. The system Conan installation and the original 2.22.2 cache were not migrated.


## Runtime correction tested independently

The exact source from PR #31349 was built without the naming correction. Boost 1.91.0, clang-cl 20.1.2, Release, static libraries, static CRT and system layout passed installation/package_info(), archive inspection, both standalone CMake programs and the recipe test_package. The archive requests LIBCMT as configured. The naming PR is not required to merge this runtime correction.

# Ubuntu - 22.04

{% hint style="info" %}
Xahaud now supports building using Conan. We recommend using Conan to build the repository. The detailed build instructions for Conan can be found in the [BUILD.md](https://github.com/Xahau/xahaud/blob/dev/BUILD.md) file in the source code.
{% endhint %}

## Conan Requirements

| Dependency | Minimum Version |
| ---------- | --------------- |
| Python     | 3.7             |
| Conan      | 1.55            |
| CMake      | 3.16            |
| GCC        | 10              |
| Clang      | 13              |

## Quick Start with Conan

### 1. Set Up Conan Profile

```bash
conan profile new default --detect
conan profile update settings.compiler.cppstd=20 default
conan profile update settings.compiler.libcxx=libstdc++11 default
```

### 2. Export Custom Recipes (Required)

```bash
conan export external/snappy snappy/1.1.10@xahaud/stable
conan export external/soci soci/4.0.3@xahaud/stable
```

### 3. Build

```bash
mkdir .build && cd .build
conan install .. --output-folder . --build missing --settings build_type=Release
cmake -DCMAKE_TOOLCHAIN_FILE:FILEPATH=build/generators/conan_toolchain.cmake -DCMAKE_BUILD_TYPE=Release ..
cmake --build .
```

### 4. Test

```bash
./rippled --unittest
```

## Troubleshooting

For detailed troubleshooting, build options, and advanced configuration, see the [BUILD.md](https://github.com/Xahau/xahaud/blob/dev/BUILD.md) file.

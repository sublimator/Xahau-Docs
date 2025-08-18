# Mac OS - 15.3.2 (24D81)

Xahaud uses Conan for dependency management. For comprehensive build instructions, see the [BUILD.md](https://github.com/Xahau/xahaud/blob/dev/BUILD.md) file in the source code.

## Requirements

| Dependency  | Working Version |
| ----------- | --------------- |
| Apple Clang | 13.1.6+        |
| Clang       | 13+             |
| CMake       | 3.16+           |
| Ninja       | 1.10+ (recommended) |
| Python      | 3.7+            |
| Conan       | 1.55+           |

## Dependency Management with Conan

### Set Up Conan Profile

```bash
conan profile new default --detect
conan profile update settings.compiler.cppstd=20 default
```

### Export Custom Recipes (Required)

Xahaud requires custom Conan recipes for Snappy and SOCI:

```bash
conan export external/snappy snappy/1.1.10@xahaud/stable
conan export external/soci soci/4.0.3@xahaud/stable
```

### Install Dependencies

```bash
mkdir .build && cd .build
conan install .. --output-folder . --build missing --settings build_type=Release
```

## Build with CMake

### Set Build Environment Variables

```bash
export CC=clang
export CXX=clang++
export CFLAGS="-DBOOST_ASIO_HAS_STD_INVOKE_RESULT"
export CXXFLAGS="-DBOOST_ASIO_HAS_STD_INVOKE_RESULT"
```

### Configure and Build (Ninja - Recommended)

```bash
cmake -G Ninja -DCMAKE_TOOLCHAIN_FILE:FILEPATH=build/generators/conan_toolchain.cmake -DCMAKE_BUILD_TYPE=Release ..
ninja
```

### Alternative: Configure and Build (Make)

```bash
cmake -DCMAKE_TOOLCHAIN_FILE:FILEPATH=build/generators/conan_toolchain.cmake -DCMAKE_BUILD_TYPE=Release ..
make -j$(sysctl -n hw.logicalcpu)
```

## Test

```bash
./rippled --unittest
```

## Troubleshooting

For detailed troubleshooting, build options, and advanced configuration, see the [BUILD.md](https://github.com/Xahau/xahaud/blob/dev/BUILD.md) file.

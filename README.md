# enum-flags

> Bit flags for C++ scoped enums

C++ 11 added a new type of enums — *enum classes*.
These are strongly typed enumerations, they do not cast implicitly to or from
the underlying integer type. They also do not leak enumeration values to
enclosing scope (thus the names of this values do not clash and you no longer
need to prefix/suffix them with something to disambiguate).

The only drawback to type safety is you cannot treat variables of *enum class*
types as sets of flags. That is because *enum classes* do not cast to integers
and there are no bitwise operators overloads defined for them.

This library brings a `flags` class template which provides bit flags for
scoped enums.

 > [!TIP]
 > It's a revision to demostrate building of head-only library and should-not-compile testing of SideCMake.
 > The original repository utilizes Conan for building; however, I have replaced Conan with CMake for easier maintenance, as my familiarity with Conan is limited.

## Install

### Using CMake and SideCMake 

```bash
vcpkg install boost
git clone https://github.com/grisumbras/enum-flags.git
cd enum-flags
cp ./enum-flags/LocalPresets.json.empty ./enum-flags/LocalPresets.json
mkdir _build
cmake "-S./enum-flags" 
      -G "Ninja" 
      "-B./_build" 
      "-DCMAKE_INSTALL_PREFIX=./install" 
      --preset unixlike-clang-release
      "-DSIDECMAKE_DIR=/path/to/SideCMake/"
      "-DBUILD_TESTING=ON"
      "-DCMAKE_TOOLCHAIN_FILE=/path/to/vcpkg/scripts/buildsystems/vcpkg.cmake"
cmake --build "./_build" --target install --config "Release"
```
> [!TIP]
> - Pick an options for `--preset` in `CMakePresets.json` section : `configurePresets`
> - Boost is required only if you build or run tests (when `BUILD_TESTING` is `ON`).
> - We recommend vcpkg for package management, but you may use any package manager you prefer.
## Usage

``` c++
#include <flags/flags.hpp>

enum class MyEnum { Value1 = 1 << 0, Value2 = 1 << 1 };
ALLOW_FLAGS_FOR_ENUM(MyEnum)

int main() {
  auto mask1 = MyEnum::Value1 | MyEnum::Value2; // set flags Value1 and Value 2
  if (mask1 & MyEnum::Value2) { // if Value2 flag is set
    /* ... */
  }
}
```

More info can be found in the [docs](http://grisumbras.github.io/enum-flags/).


## Maintainer

Dmitry Arkhipov <grisumbras@gmail.com>


## Contributing

Patches welcome!


## License

[MIT (C) Dmitry Arkhipov.](LICENSE)

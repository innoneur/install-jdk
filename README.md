# install-jdk

[![Codacy Badge](https://app.codacy.com/project/badge/Grade/309b149bb42643bbb08e01e6d0c553f9)](https://www.codacy.com/gh/jyksnw/install-jdk/dashboard?utm_source=github.com&utm_medium=referral&utm_content=jyksnw/install-jdk&utm_campaign=Badge_Grade) [![PyPI Downloads Badge](https://img.shields.io/pypi/dm/install-jdk.svg)](https://pypi.org/project/install-jdk/) [![PyPI Version Badge](https://img.shields.io/pypi/v/install-jdk.svg)](https://pypi.org/project/install-jdk/)

The `install-jdk` library is a Python package that simplifies the process of installing JDK (Java Development Kit) on Windows, macOS, Linux and other supported operating systems. The library provides a simple interface for downloading and installing the appropriate version of an OpenJDK build. `install-jdk` is a useful tool for users, developers, and system administrators who need to set up Java development environment or runtime. It simplifies the process of downloading and installing the appropriate JDK version, saving time and effort.

`install-jdk` has no third-party dependencies and depends solely on the standard libraries found in Python 3. This means that users can easily install and use the library without having to install any additional dependencies. This makes it more lightweight and simpler to use.

`install-jdk` aims to provide as many options as possible to install an OpenJDK Java version across a wide array of operating systems and architectures. Please see each vendors OpenJDK documentation to see what operating systems and architectures they support.

## Supported OpenJDK Build Vendors

| OpenJDK Build      | Status      | Vendor Tags                               | Vendor Documentation                                        | `install-jdk` Documentation                                                                    | Source Code                                                                                           |
| ------------------ | ----------- | ----------------------------------------- | ----------------------------------------------------------- | ---------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| Adoptium (default) | Implemented | Adoptium, Temurin, AdoptOpenJDK, eclipse | [Adoptium](https://adoptium.net/docs/)                      | [Adoptium Build Client docs](https://github.com/jyksnw/install-jdk/wiki/Adoptium-Build-Client) | [Adoptium Build Client src](https://github.com/jyksnw/install-jdk/blob/master/jdk/client/adoptium.py) |
| Corretto           | Implemented | Corretto, Amazon, AWS                     | [Corretto](https://docs.aws.amazon.com/corretto/index.html) | [Corretto Build Client docs](https://github.com/jyksnw/install-jdk/wiki/Corretto-Build-Client) | [Corretto Build Client src](https://github.com/jyksnw/install-jdk/blob/master/jdk/client/corretto.py) |
| Microsoft          | Planning    | Microsoft                                 | ...coming soon                                              |                                                                                                |                                                                                                       |
| Azul               | Implemented    | Azul, Zulu                                | [Azul Zulu](https://www.azul.com/downloads-new/?package=jdk#zulu)                                             |                                                                                                |                                                                                                       | [Zulu Build Client src](https://github.com/jyksnw/install-jdk/blob/master/jdk/client/zulu.py

install-jdk will do its best to detect the operating system and architecture that it is running on. Currently is able to detect:

- Operating Systems

  - Windows
  - Linux
  - MacOS
  - AIX

- Architecture
  - arm
  - aarch64
  - ppc64
  - x64
  - x86

## Installation

To install the install-jdk library, simply run the following command:

```bash
pip install install-jdk
```

## Usage

To use the `install-jdk` library, import it into your Python code:

```python
import jdk
```

The library provides an `install` function, which takes the following parameters:

- `version` - The major version of the Java OpenJDK build to install (e.g. 8, 11, 17, etc.).
- `operating_system` - The target operating system. If not specified, will use the user's detected operating system if possible.
- `arch` - The target architecure. If not specified, will use the user's detected architecture if possible.
- `impl` - The Java implementation to use. Currently only supports `HOTSPOT` and dependent on the OpenJDK Build Vendor.
- `jre` - A boolean value indicating that the Java Runtime Environment should be installed. Defaults to false, which will install the Java Development Kit.
- `path` - The location to install the downloaded OpenJDK build. If not specified, will install into `$HOME/.jdk/<VERSION>` for the Java Development Kit and `$HOME/.jre/<VERSION>` for the Java Runtime Environment.
- `vendor` - The vendor to download the OpenJDK build from. If not specified, defaults to [Adoptium](https://adoptium.com). This is a named argument so must be provided like `vendor='Corretto'`. To find a list of available supported OpenJDK vendors available see the list of [Supported OpenJDK Build Vendors](#supported-openjdk-build-vendors)

Here are some example code snippet:

```python
jdk.install('11)
# Platform dependent install of Java JDK 11 into $HOME/.jdk/<VERSION>

jdk.install('11', jre=True)
# Platform dependent install of Java JRE 11 into $HOME/.jre/<VERSION>

jdk.install('17', vendor='Corretto')
# Installs a Corretto build of Java 17 JDK. Defualt vendor is Adoptium

jdk.install('17', vendor='Corretto', path='/usr/local/jdk')
# Installs a Corretto build of Java 17 JDK. Defualt vendor is Adoptium
```

The library also has a `get_download_url` function that returns the URL for the given version, it takes the following parameters:

- `version` - The major version of the Java OpenJDK build to install (e.g. 8, 11, 17, etc.).
- `operating_system` - The target operating system. If not specified, will use the user's detected operating system if possible.
- `arch` - The target architecure. If not specified, will use the user's detected architecture if possible.
- `impl` - The Java implementation to use. Currently only supports `HOTSPOT` and dependent on the OpenJDK Build Vendor.
- `jre` - A boolean value indicating that the Java Runtime Environment should be installed. Defaults to false, which will install the Java Development Kit.
- `vendor` - The vendor to download the OpenJDK build from. If not specified, defaults to [Adoptium](https://adoptium.com). This is a named argument so must be provided like `vendor='Corretto'`. To find a list of available supported OpenJDK vendors available see the list of [Supported OpenJDK Build Vendors](#supported-openjdk-build-vendors)

Here are some example code snippets:

```python
from jdk.enums import OperatingSystem, Architecture

download_url = jdk.get_download_url('17', jre=True)
print(download_url)
# Obtains the platform dependent JRE download url

download_url = jdk.get_download_url('17', operating_system=OperatingSystem.LINUX, arch=Architecure.AARCH64, vendor='Corretto')
print(download_url)
# Obtains OpenJDK 17 from Corretto for Linux running on aarch64
```

The library has a `download` function that will download the requested version and returns back the path to where it was downloaded. This function does not currently support overriding the default download path which is the operating systems specific TMP directory. It takes the following parameters.

- `download_url` - The URL to the file to be downloaded. Defaults to None.
  - - `version` - Required when `download_version` is None and must be provided as a named parameter. The major version of the Java OpenJDK build to install (e.g. 8, 11, 17, etc.).
- `operating_system` - Must be provided as a named parameter. The target operating system. If not specified, will use the user's detected operating system if possible.
- `arch` - Must be provided as a named parameter. The target architecure. If not specified, will use the user's detected architecture if possible.
- `impl` - Must be provided as a named parameter. The Java implementation to use. Currently only supports `HOTSPOT` and dependent on the OpenJDK Build Vendor.
- `jre` - Must be provided as a named parameter. A boolean value indicating that the Java Runtime Environment should be installed. Defaults to false, which will install the Java Development Kit.
- `vendor` - Must be provided as a named parameter. The vendor to download the OpenJDK build from. If not specified, defaults to [Adoptium](https://adoptium.com). This is a named argument so must be provided like `vendor='Corretto'`. To find a list of available supported OpenJDK vendors available see the list of [Supported OpenJDK Build Vendors](#supported-openjdk-build-vendors)

Here are some example code snippets:

```python
from jdk.enums import OperatingSystem, Architecture

download_file = jdk.download('https://api.adoptium.net/v3/binary/latest/17/ga/windows/x64/jdk/hotspot/normal/eclipse')
print(download_file)
# Downloads the requested file and returns back the TMP locations it was stored in.

download_file = jdk.download(version='17', operating_system=OperatingSystem.LINUX, arch=Architecure.AARCH64, vendor='Corretto')
print(download_file)
# Downloads the a Linux aarch64 build of Java 17 from Corretto and returns back the TMP location it was stored in.
```

The library has an `uninstall` function that will remove provided version if present. This function does not currently work if the path parameter was overriden during the install. It looks for JDK installs in `$HOME/.jdk/<VERSION>` and JRE installs in `$HOME/.jre/<VERSION>`. It takes the following parameters.

- `version` - The major version of the Java OpenJDK build to install (e.g. 8, 11, 17, etc.).
- `jre` - A boolean value indicating that the Java Runtime Environment should be installed. Defaults to false, which will install the Java Development Kit.

Here are some example code snippets:

```python
jdk.uninstall('11')
# Removes the Java 11 JDK if installed

jdk.uninstall('11', jre=True)
# Removes the Java 11 JRE if installed
```

The library also provided two helper properties that can be used to see what it detected as the user's operating system and architecture.

```python
print(jdk.OS)
print(jdk.ARCH)
```

## Credits

The install-jdk library uses OpenJDK builds, and is created and maintained by [jyksnw](https://github.com/jyksnw).

_This was originally a port of the GitHub Action [`installjdk`](https://github.com/AdoptOpenJDK/install-jdk) but has morphed into something much more._

### Vendor Credits

- [Adoptium](https://adoptium.net/)
- [Corretto](https://aws.amazon.com/corretto/)
  - [Corretto Downloads](https://github.com/corretto/corretto-downloads)

## License

The `install-jdk` library is open-source and is distributed under the [MIT license]. See the [LICENSE](LICENSE.md) file for more information.

## Contribution

See [CONTRIBUTING](CONTRIBUTING.MD)

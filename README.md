# Lingua Franca West Template
This is a west-centric project template for Lingua Franca applications targeting the Zephyr RTOS.

## Prerequisites
- Linux or macOS operation system
- [reactor-uc](https://github.com/lf-lang/reactor-uc) cloned to your system and `REACTOR_UC_PATH` environment variable pointing to it.

### Getting started
Press `Use template` in the upper right corner and choose `Create a new repository`. Then clone this new repository to your machine.
To start developing in your new repo, you must first install the Zephyr dependencies, toolchains and SDK.
This requires you to follow selected parts of the Zephyr official Getting Started guide.

1. Install the dependencies used by Zephyr by following the steps in [Install Dependencies](https://docs.zephyrproject.org/3.7.0/develop/getting_started/index.html#install-dependencies)

2. Then install the Zephyr toolchains and SDK by following the steps in [Install the Zephyr SDK](https://docs.zephyrproject.org/3.7.0/develop/getting_started/index.html#install-the-zephyr-sdk)

Create and activate a virtual environment for this project. 

```sh
python3 -m venv ./venv
source ./venv/bin/activate
```
**IMPORTANT**: Remember to always activate the virtual environment before using the template.

Install the west, the Zephyr build tool

```sh
pip install west
```

Pull down the Zephyr RTOS sources using west (this can take a while)

```sh
west update
```

Configure environment variables

```sh
source deps/zephyr/zephyr-env.sh
```

Install Python dependencies
```sh
pip install -r deps/zephyr/scripts/requirements.txt
```

Export a CMake package
```sh
west zephyr-export
```


## HelloWorld
To build and emulate the provided [!](./src/HelloWorld.lf) using the `native_posix` target, do:
```sh
west build -t run
```

## Changing the target board

To build for a different board, e.g. the `qemu_cortex_m3` emulation. Either change the `BOARD` variable in [!](./CMakeLists.txt), or using `west`:

```sh
west build -b qemu_cortex_m3 -p always -t run
```

Note the `-p always` which is a `west` option for cleaning the build directory. This must be used when changing the target board.


## Changing the LF application
The `LF_MAIN` CMake variable decides which LF application to build. This can either be modified in [!](/CMakeLists.txt) or from the command line. To build [!](/src/Blinky.lf) for Adafruit Feather do:
```sh
west build -b adafruit_feather -p always -- -DLF_MAIN=Blinky
```

With `west`, CMake arguments are separated from `west` arguments with a `--`.


## Log level

The log level of the LF app can be changed by setting the variable `LOG_LEVEL` in [](./CMakeLists.txt) or by modifying it on the command line:
```sh
west build -t run -p always -- -DLOG_LEVEL=LF_LOG_LEVEL_DEBUG
```

## Cleaning all build artifacts 

By passing `-p always` to `west`, the `build` folder is cleaned and the project is reconfigured by CMake. However, this does not clean the files generated by `lfc`. For this we provide a custom west command `west clean` defined in [clean.py](./scripts/clean.py) which also cleans the files generated by LFC. Use it to do a complete reset.

```sh
west clean
```

## Flashing to a board

To flash an application onto a board, simply use `west flash`. This may require the installation of additional, vendor-specific tools. See the [official docs](https://docs.zephyrproject.org/3.7.0/develop/west/build-flash-debug.html#west-flashing) for more information.


## West-centric development
This template integrates the Lingua Franca compiler `lfc` into a `west`-based project. This requires that the user understands how to use `west` and zephyr. Please refer to the [official docs](https://docs.zephyrproject.org/3.7.0/index.html) for more information.



## Troubleshooting

```sh
Command 'west' not found, did you mean:
```

or

```sh
Traceback (most recent call last):
  File "/home/erling/dev/lf-west-template/deps/zephyr/scripts/build/gen_kobject_list.py", line 62, in <module>
    import elftools
ModuleNotFoundError: No module named 'elftools'
```

Activate the virtual environment where the Zephyr dependencies are installed.



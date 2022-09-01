USAGE - Docker image with HOL Light preinstalled
========================================

## Install
Follow the <INSTALL.md> first. Esspeciall the the part [Further and simpler usage](INSTALL.md#furtherandsimplerusage)

## Help
```bash
hol-light --help
```
Prints the possible ways to run hol-light.

## Run interactivly
```bash
hol-light
```

## Run interactivly, loading a file first
```bash
hol-light --interactive --file FILENAME
```

## Load a file and terminate after execution
```bash
hol-light --file FILENAME
```

## Using Podman instead of Docker
When you need to use podman, you can simply add `--podman` to use it instead of docker. For instance:
```bash
hol-light --file --podman FILENAME
```

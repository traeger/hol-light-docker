Scripts for HOL Light (installation, deployment, checkpointing) on Docker or Podman.
========================================

## Introduction
HOL Light needs 'camlp5'. This package is sometimes hard to install (e.g. Fedora). 
Therefor we provied a set of scriptes using docker/podman and dmtcp (for checkpoints) to install it anywhere and hopefully with less pain.

After the installion it allows you to run [HOL Light](https://www.cl.cam.ac.uk/~jrh13/hol-light/) the typical linux way.

### Run interactivly
```bash
hol-light
```

### Load a file and terminate after execution
```bash
hol-light --file FILENAME
```

## Remark
Simplified version of <https://github.com/maggesi/hol-light-docker>. This version only supports the HOL Light core library but makes the installion simpler overall. If you need Multivariate analysis, Complex analysis or Quaternions than please use maggesi's version.

## Install
Follow the [install instructions](INSTALL.md).

## Usage
We provide [further ways](USAGE.md) running HOL Light.

INSTALL - Docker image with HOL Light preinstalled
========================================

## Notes

- Based on Debian and use OCaml from the Debian distribution.
- Dmtcp is used for checkpointing the ocaml execution to be able to skip the hol-light loading on each start.
- Dmtcp refuse to start under root: we use the "opam" user.

## Targets of the multistage build

| Image tag         | Description                            |
|----------------   |-------------------------------------   |
| `holbox-base`     | Debian + ocaml/opam                    |
| `holbox-build`    | Build tools + HOL Light + Dmtcp        |
| `hol-light-base`  | Only HOL Light installed               |
| `hol-light-ckpt`  | HOL Light + Dmtcp                      |

## Manually built images with checkpointed binaries

| Checkpoint                | Preloaded with                 |
|------------------------   |-----------------------------   |
| `hol-light-core`          | Core library                   |

## How to build the images

Build and test HOL Light "base" (no Dmtcp, no checkpoint)
```
docker build --target hol-light-base -t hol-light-base hol-light-base
```
if you want to use cache and already downloaded images, or use options
`--pull` and `--no-cache` redownload images and updates
```
docker build --pull --no-cache --target hol-light-base -t hol-light-base hol-light-base
```
To run the container
```
docker run --rm -it hol-light-base
```

Build the other images (prepare checkpointing)
```
docker build --target holbox-build -t holbox-build hol-light-base
docker build --target hol-light-base -t hol-light-base hol-light-base
docker build --target hol-light-ckpt -t hol-light-ckpt hol-light-base
```

## Create HOL-Light Executable Checkpoints

To start the container for building the checkpoints:
```
docker run --name build-ckpt -h holbox -it -m 6G holbox-build screen
```

In one screen terminal
```
dmtcp_coordinator -q -p 7779
s
```

In another screen terminal (`C-a c`)
```
dmtcp_launch -q -j -p 7779 ocaml -I `camlp5 -where` -init make.ml
```
Once finished loading (before checkpoint)
```
load_path := "/src" :: !load_path;;
Gc.compact();;
```

Back into the first terminal (`C-a n`)
```
c
```

Back to the ocaml process (`C-a n`).
Kill the ocaml process (`C-d`).

Then open another screen tab and move the checkpoint in a separate
directory:
```
mkdir ckpt_core
mv ckpt*.dmtcp dmtcp_restart_script* ckpt_core
```

Close the image.

## Build checkpoint images
Commit the build-ckpt container into an image called holbox-ckpt
```
docker container commit build-ckpt holbox-ckpt
```

Build the images with the ckeckpointed binaries
```
docker build --target hol-light-core -t hol-light-core hol-light-core
```

## Run the checkpointed images
```
docker container run --rm -h holbox -it hol-light-core
```

## Further and simpler usage
We provided a even more straight forward way to use hol-light, by making it available as a common executable.
To set this up follow the following instructions:

### Setup a simple shell-script wrapper
Add the contributed shell-script to you user executables to make it avialable as `hol-light`.
```bash
ln -s hol-light ~bin/hol-light
```

### Use it
See <USAGE.md>.

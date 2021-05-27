# Building with only a Makefile

If only building for native systems, it is possible to significantly reduce
the complexity of the build by removing Bazel (and Docker). This simple
approach builds only what is needed - all that is required is a TensorFlow
repo to point it to.

To prepare your system, you'll need the following packages (both available on
Debian Bullseye)

```
sudo apt install libabsl-dev libflatbuffers-dev
```

To build

```
TFROOT=/tmp/tensorflow make -j$(nproc) libedgetpu
```

This is very much a draft, the following steps still need to be done:
* Generate the firmware header, instead of copying it in to the repo.
* Fix output so .d and .o don't wind up in the main tree.
* Clean up the file.
* Possibly add cross compilation support, although this likely requires
coral crosstool.

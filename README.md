# rpi-resize-stb
Raspberry PI utility for image resizing
- Source: https://github.com/ImageProcessing-ElectronicPublications/stb-image-resize.git


## Functionality

The image is scaled using interpolation:
- bicubic (4x4)
- biakima (6x6) 
- biline (2x2) 

The image is resampled using:
- gsample (3x3)

Include prefilter:
- Gauss (for downsample)
- "Reverse Interpolate Scale (RIS)"

Added features:
- Saving into `.jpg` format


## Build

### Load submodules

Submodules:
- [stb](https://github.com/nothings/stb.git) -> [src/stb](src/stb)

```shell
$ git submodule init
$ git submodule update
```

### Install dependencies

Build dependencies:
```shell
$ sudo apt-get install build-essential cmake
```
Cross-compiling toolchain: https://github.com/SanyaOgr/rpi-crosscompile-template#setup

### Compilation

```shell
$ mkdir build
$ cd build
$ cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_TOOLCHAIN_FILE=rpi-toolchain.cmake ..
$ cmake --build .
```


## Use

The first and second parameters specify the paths to the image and the result {PNG}. The `-H`, `-W` and `-r` options set the resulting dimensions. If `-H` or `-W` is given, then `-r` is ignored. The `-m` option specifies the scaling method {0 - bicubic, 1 - biakima, -1 - biline, -2 - gsample}. The `-p` option specifies the "Reverse Interpolate Scale (RIS)" (if equal to 0, then RIS is disabled).
```shell
./stbresize -r 4 ${IMAGE_PATH} ${IMAGE_PATH}.out.png
```

## Structure

- `biakima.h` - biakima image scaling
- `bicubic.h` - bicubic image scaling
- `biline.h` - biline image scaling
- `dependencies.c` - API [stb](https://github.com/nothings/stb.git)
- `gauss.h` - prefilter: Gauss
- `gsample.h` - gsample image sampler
- `hris.h` - HRIS/mean scale: Reverse Interpolate Scale (RIS)
- `ris.h` - prefilter: Reverse Interpolate Scale (RIS)
- `stb/` - [stb](https://github.com/nothings/stb.git)
- `stbresize.c` - CLI program.

## Image scaling

Use bicubic or biakima interpolation method to scale the image. The principle is: for each pixel in the scaled image, find the nearest 4x4 or 6x6 pixel grid at the corresponding position in the original image, and use this grid to perform interpolation to obtain RGB of this pixel. See `bicubic.h` or `biakima.h` for details.

Regarding the principle of zooming by the bicubic interpolation method, it is not expanded here, so you can ignore it.

Use the [stb image library](https://github.com/nothings/stb.git) for processing, you can ignore it.

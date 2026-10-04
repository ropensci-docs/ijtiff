# The \_ImageJ\_ Problem

## Introduction

The *ImageJ* software (<https://imagej.net/ij/>) is a widely-used image
viewing and processing software, particularly popular in microscopy and
life sciences. It supports the TIFF image format (and many others). It
reads TIFF files perfectly, however it can sometimes write them in a
peculiar way, meaning that when other softwares try to read TIFF files
written by *ImageJ*, mistakes can be made.

One goal of the `ijtiff` R package is to correctly import TIFF files
that were saved from *ImageJ*.

### Frames and Channels in TIFF files

- In a volumetric image, *frames* are typically the different z-slices.
  In a time-stack of images (i.e. a video), each frame represents a
  time-point.
- There is one *channel* per colour. A conventional colour image is made
  up of 3 colour channels: red, green and blue. A grayscale (black and
  white) image has just one channel. It’s possible to acquire two
  channels (e.g. red an blue but not green), five channels
  (e.g. infrared, red, green, blue and ultraviolet), or any number at
  all, but these cases are seen mostly in specialist imaging fields like
  microscopy.

### The Peculiarity of *ImageJ* TIFF files

It is common to use `TIFFTAG_SAMPLESPERPIXEL` to record the number of
channels in a TIFF image, however *ImageJ* sometimes leaves
`TIFFTAG_SAMPLESPERPIXEL` with a value of 1 and instead encodes the
number of channels in `TIFFTAG_IMAGEDESCRIPTION` which might look
something like  
`"ImageJ=1.51 images=16 channels=2 slices=8"`.

A conventional TIFF reader would miss this channel information (because
it is in an unusual place). `ijtiff` does not miss it. We’ll see an
example below.

*Note*: These peculiar *ImageJ*-written TIFF files are still bona fide
TIFF files according to the TIFF specification. They just break with
common conventions of encoding channel information.

## Reading *ImageJ* TIFF files

``` r

path_2ch_ij <- system.file("img", "Rlogo-banana-red_green.tif",
  package = "ijtiff"
)
```

`path_2ch_ij` is the path to a TIFF file which was made in *ImageJ* from
the R logo dancing banana GIF used in the README of Jeroen Ooms’
`magick` package. The TIFF is a time-stack containing only the red and
green channels of the first and third frames of the original GIF. Here’s
the full gif:

![](../../../../github/home/R/x86_64-pc-linux-gnu-library/4.6/ijtiff/img/Rlogo-banana.gif)

Here are the red and green channels of the first and third frames of the
TIFF:

![](the-imagej-problem_files/figure-html/red%20and%20green%20banana-1.png)

### The original `tiff` package

When we import it with the original `tiff` package:

``` r

img <- tiff::readTIFF(path_2ch_ij, all = TRUE)
str(img)
#> List of 4
#>  $ : num [1:155, 1:200, 1:3] 0.996 0.996 0.996 0.996 0.996 ...
#>  $ : num [1:155, 1:200, 1:3] 0 0 0 0 0 0 0 0 0 0 ...
#>  $ : num [1:155, 1:200, 1:3] 0.996 0.996 0.996 0.996 0.996 ...
#>  $ : num [1:155, 1:200, 1:3] 0 0 0 0 0 0 0 0 0 0 ...
img[[1]][100:105, 50:55, 1] # print a section of the first image in the series
#>           [,1]      [,2]      [,3]      [,4]      [,5]      [,6]
#> [1,] 0.6601663 0.6601663 0.6523537 0.6601663 0.7031357 0.9179828
#> [2,] 0.6718853 0.6406348 0.6718853 0.6406348 0.6406348 0.6718853
#> [3,] 0.6523537 0.6601663 0.6406348 0.6601663 0.6601663 0.6406348
#> [4,] 0.6406348 0.6406348 0.6601663 0.6406348 0.6601663 0.6406348
#> [5,] 0.6718853 0.6718853 0.6406348 0.6601663 0.6406348 0.6601663
#> [6,] 0.6718853 0.6406348 0.6406348 0.6406348 0.6523537 0.6523537
```

- We just get a list of 4 frames, with wrong information about the
  channels (it looks like there are 3 channels per frame).
- The numbers in the image array(s) are (by default) normalized to the
  range \[0, 1\].

### The `ijtiff` package

When we import the same image with the `ijtiff` package:

``` r

img <- ijtiff::read_tif(path_2ch_ij)
#> Reading image from /github/home/R/x86_64-pc-linux-gnu-library/4.6/ijtiff/img/Rlogo-banana-red_green.tif
#> Reading an 8-bit, integer image with dimensions 155x200x2x2 (y,x,channel,frame) . . .
dim(img) # 2 channels, 2 frames
#> [1] 155 200   2   2
img[100:105, 50:55, 1, 1] # print a section of the first channel, first frame
#>      [,1] [,2] [,3] [,4] [,5] [,6]
#> [1,]  169  169  167  169  180  235
#> [2,]  172  164  172  164  164  172
#> [3,]  167  169  164  169  169  164
#> [4,]  164  164  169  164  169  164
#> [5,]  172  172  164  169  164  169
#> [6,]  172  164  164  164  167  167
```

- We see the image nicely represented as an array of channels of frames.
- The numbers in the image are integers, the same as would be seen if
  one opened the image with ImageJ.

## Note

The original `tiff` package reads several types of TIFFs correctly,
including many that are saved from *ImageJ*. This is just an example of
a TIFF type that it doesn’t perform so well with.

## Advice for all *ImageJ* users

Base *ImageJ* (similar to the `tiff` R package) does not properly open
some perfectly good TIFF files[^1] (including some TIFF files written by
the `tiff` and `ijtiff` R packages). Instead it often gives you the
error message: *imagej can only open 8 and 16 bit/channel images*. These
images in fact can be opened in *ImageJ* using the wonderful
*Bio-Formats* plugin.

[^1]: I think native *ImageJ* only likes 1, 3 and 4-channel images and
    complains about the rest, but I’m not sure about this.

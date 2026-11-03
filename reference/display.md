# Basic image display.

Display an image that has been read in by
[`read_tif()`](https://docs.ropensci.org/ijtiff/reference/read_tif.md)
as it would look in 'ImageJ'. This function wraps
[`graphics::plot.raster()`](https://rdrr.io/r/graphics/plot.raster.html).

## Usage

``` r
display(img, ...)
```

## Arguments

- img:

  An
  [ijtiff_img](https://docs.ropensci.org/ijtiff/reference/ijtiff_img.md)
  object.

- ...:

  Passed to
  [`graphics::plot.raster()`](https://rdrr.io/r/graphics/plot.raster.html).

## Examples

``` r
img <- read_tif(system.file("img", "Rlogo.tif", package = "ijtiff"))
#> Reading image from /github/home/R/x86_64-pc-linux-gnu-library/4.6/ijtiff/img/Rlogo.tif
#> Reading an 8-bit, integer image with dimensions 76x100x4x1 (y,x,channel,frame) . . .
display(img)

display(img[, , 1, 1]) # first (red) channel, first frame
display(img[, , 2, ]) # second (green) channel, first frame

display(img[, , 3, ]) # third (blue) channel, first frame

display(img, basic = TRUE) # displays first (red) channel, first frame
#> Warning: "basic" is not a graphical parameter

```

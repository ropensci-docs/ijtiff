# Convert an ijtiff_img object to a raster object for plotting

This function converts an
[ijtiff_img](https://docs.ropensci.org/ijtiff/reference/ijtiff_img.md)
object to a `raster` object that can be used with base R graphics
functions. The function extracts the first frame of the image and
converts it to an RGB raster representation.

## Usage

``` r
# S3 method for class 'ijtiff_img'
as.raster(x, ...)
```

## Arguments

- x:

  An
  [ijtiff_img](https://docs.ropensci.org/ijtiff/reference/ijtiff_img.md)
  object. This should be a 4D array with dimensions representing (y, x,
  channel, frame).

- ...:

  Passed to
  [`graphics::plot.raster()`](https://rdrr.io/r/graphics/plot.raster.html).

## Value

A `raster` object compatible with
[`graphics::plot.raster()`](https://rdrr.io/r/graphics/plot.raster.html).
The raster will represent the first frame of the input image.

## Details

The function performs the following operations:

- Extracts the first frame of the image

- Checks for invalid values (all NA or negative values)

- Determines the appropriate color scaling based on the image bit depth

- Creates an RGB representation using the available channels

For single-channel images, a grayscale representation is created. For
RGB images (3 channels), a full-color representation is created.

## Examples

``` r
# Read a TIFF image
img <- read_tif(system.file("img", "Rlogo.tif", package = "ijtiff"))
#> Reading image from /github/home/R/x86_64-pc-linux-gnu-library/4.6/ijtiff/img/Rlogo.tif
#> Reading an 8-bit, integer image with dimensions 76x100x4x1 (y,x,channel,frame) . . .

# Convert to raster and plot
raster_img <- as.raster(img)
plot(raster_img)

```

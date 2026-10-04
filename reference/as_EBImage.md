# Convert an [ijtiff_img](https://docs.ropensci.org/ijtiff/reference/ijtiff_img.md) to an [EBImage::Image](https://rdrr.io/pkg/EBImage/man/Image.html).

This is for interoperability with the the `EBImage` package.

## Usage

``` r
as_EBImage(img, colormode = NULL, scale = TRUE, force = TRUE)
```

## Arguments

- img:

  An
  [ijtiff_img](https://docs.ropensci.org/ijtiff/reference/ijtiff_img.md)
  object (or something coercible to one).

- colormode:

  A numeric or a character string containing the color mode which can be
  either `"Grayscale"` or `"Color"`. If not specified, a guess is made.
  See 'Details'.

- scale:

  Scale values in an integer image to the range `[0, 1]`? Has no effect
  on floating-point images.

- force:

  This function is designed to take
  [ijtiff_img](https://docs.ropensci.org/ijtiff/reference/ijtiff_img.md)s
  as input. To force any old array through this function, use
  `force = TRUE`, but take care to check that the result is what you'd
  like it to be.

## Value

An [EBImage::Image](https://rdrr.io/pkg/EBImage/man/Image.html).

## Details

The guess for the `colormode` is made as follows: \* If `img` has an
attribute `color_space` with value `"RGB"`, then `colormode` is set to
`"Color"`. \* Else if `img` has 3 or 4 channels, then `colormode` is set
to `"Color"`. \* Else `colormode` is set to "Grayscale".

## Examples

``` r
if (rlang::is_installed("EBImage")) {
  img <- read_tif(system.file("img", "Rlogo.tif", package = "ijtiff"))
  str(img)
  str(as_EBImage(img))
}
#> Reading image from /github/home/R/x86_64-pc-linux-gnu-library/4.6/ijtiff/img/Rlogo.tif
#> Reading an 8-bit, integer image with dimensions 76x100x4x1 (y,x,channel,frame) . . .
#>  'ijtiff_img' num [1:76, 1:100, 1:4, 1] 0 0 0 0 0 0 0 0 0 0 ...
#>  - attr(*, "ImageWidth")= int 100
#>  - attr(*, "ImageLength")= int 76
#>  - attr(*, "ImageDepth")= int 1
#>  - attr(*, "BitsPerSample")= int 8
#>  - attr(*, "SamplesPerPixel")= int 4
#>  - attr(*, "SampleFormat")= chr "unsigned integer data"
#>  - attr(*, "PlanarConfiguration")= chr "contiguous"
#>  - attr(*, "RowsPerStrip")= int 76
#>  - attr(*, "Compression")= chr "LZW"
#>  - attr(*, "Threshholding")= int 1
#>  - attr(*, "XResolution")= num 300
#>  - attr(*, "YResolution")= num 300
#>  - attr(*, "ResolutionUnit")= chr "inch"
#>  - attr(*, "Orientation")= chr "top_left"
#>  - attr(*, "PhotometricInterpretation")= chr "RGB"
#>  - attr(*, "tags_by_frame")=List of 1
#>   ..$ frame1:List of 26
#>   .. ..$ ImageWidth               : int 100
#>   .. ..$ ImageLength              : int 76
#>   .. ..$ ImageDepth               : int 1
#>   .. ..$ BitsPerSample            : int 8
#>   .. ..$ SamplesPerPixel          : int 4
#>   .. ..$ SampleFormat             : chr "unsigned integer data"
#>   .. ..$ PlanarConfiguration      : chr "contiguous"
#>   .. ..$ RowsPerStrip             : int 76
#>   .. ..$ TileWidth                : NULL
#>   .. ..$ TileLength               : NULL
#>   .. ..$ Compression              : chr "LZW"
#>   .. ..$ Threshholding            : int 1
#>   .. ..$ XResolution              : num 300
#>   .. ..$ YResolution              : num 300
#>   .. ..$ XPosition                : NULL
#>   .. ..$ YPosition                : NULL
#>   .. ..$ ResolutionUnit           : chr "inch"
#>   .. ..$ Orientation              : chr "top_left"
#>   .. ..$ Copyright                : NULL
#>   .. ..$ Artist                   : NULL
#>   .. ..$ DocumentName             : NULL
#>   .. ..$ DateTime                 : NULL
#>   .. ..$ ImageDescription         : NULL
#>   .. ..$ Software                 : NULL
#>   .. ..$ PhotometricInterpretation: chr "RGB"
#>   .. ..$ ColorMap                 : NULL
#> Formal class 'Image' [package "EBImage"] with 2 slots
#>   ..@ .Data    : num [1:100, 1:76, 1:4, 1] 0 0 0 0 0 0 0 0 0 0 ...
#>   ..@ colormode: int 2
#>   ..$ dim: int [1:4] 100 76 4 1
```

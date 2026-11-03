# `ijtiff_img` class.

A class for images which are read or to be written by the `ijtiff`
package.

## Usage

``` r
ijtiff_img(img, ...)

as_ijtiff_img(img, ...)
```

## Arguments

- img:

  An array representing the image.

  - For a multi-plane, grayscale image, use a 3-dimensional array
    `img[y, x, plane]`.

  - For a multi-channel, single-plane image, use a 4-dimensional array
    with a redundant 4th slot `img[y, x, channel, ]` (see ijtiff_img
    'Examples' for an example).

  - For a multi-channel, multi-plane image, use a 4-dimensional array
    `img[y, x, channel, plane]`.

- ...:

  Named arguments which are set as attributes.

## Value

A 4 dimensional array representing an image, indexed by
`img[y, x, channel, frame]`, with selected attributes.

## Examples

``` r
img <- matrix(1:4, nrow = 2) # to be a single-channel, grayscale image
ijtiff_img(img, description = "single-channel, grayscale")
#> 2x2 pixel ijtiff_img with 1 channel and 1 frame.
#> Preview (top left of first channel of first frame):
#>      [,1] [,2]
#> [1,]    1    3
#> [2,]    2    4
#> ── TIFF tags ───────────────────────────────────────────────────────────────────
img <- array(seq_len(2^3), dim = rep(2, 3)) # 1 channel, 2 frame
ijtiff_img(img, description = "blah blah blah")
#> 2x2 pixel ijtiff_img with 1 channel and 2 frames.
#> Preview (top left of first channel of first frame):
#>      [,1] [,2]
#> [1,]    1    3
#> [2,]    2    4
#> ── TIFF tags ───────────────────────────────────────────────────────────────────
img <- array(seq_len(2^3), dim = c(2, 2, 2, 1)) #  2 channel, 1 frame
ijtiff_img(img, description = "blah blah")
#> 2x2 pixel ijtiff_img with 2 channels and 1 frame.
#> Preview (top left of first channel of first frame):
#>      [,1] [,2]
#> [1,]    1    3
#> [2,]    2    4
#> ── TIFF tags ───────────────────────────────────────────────────────────────────
img <- array(seq_len(2^4), dim = rep(2, 4)) # 2 channel, 2 frame
ijtiff_img(img, software = "R")
#> 2x2 pixel ijtiff_img with 2 channels and 2 frames.
#> Preview (top left of first channel of first frame):
#>      [,1] [,2]
#> [1,]    1    3
#> [2,]    2    4
#> ── TIFF tags ───────────────────────────────────────────────────────────────────
```

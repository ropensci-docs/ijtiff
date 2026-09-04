# Rejig linescan images.

`ijtiff` has the fourth dimension of an
[ijtiff_img](https://docs.ropensci.org/ijtiff/reference/ijtiff_img.md)
as its time dimension. However, some linescan images (images where a
single line of pixels is acquired over and over) have the time dimension
as the y dimension, (to avoid the need for an image stack). These
functions allow one to convert this type of image into a conventional
[ijtiff_img](https://docs.ropensci.org/ijtiff/reference/ijtiff_img.md)
(with time in the fourth dimension) and to convert back.

## Usage

``` r
linescan_to_stack(linescan_img)

stack_to_linescan(img)
```

## Arguments

- linescan_img:

  A 4-dimensional array in which the time axis is the first axis.
  Dimension 4 must be 1 i.e. `dim(linescan_img)[4] == 1`.

- img:

  A conventional
  [ijtiff_img](https://docs.ropensci.org/ijtiff/reference/ijtiff_img.md),
  to be turned into a linescan image. Dimension 1 must be 1 i.e.
  `dim(img)[1] == 1`.

## Value

The converted image, an object of class
[ijtiff_img](https://docs.ropensci.org/ijtiff/reference/ijtiff_img.md).

## Examples

``` r
linescan <- ijtiff_img(array(rep(1:4, each = 4), dim = c(4, 4, 1, 1)))
print(linescan)
#> 4x4 pixel ijtiff_img with 1 channel and 1 frame.
#> Preview (top left of first channel of first frame):
#>      [,1] [,2] [,3] [,4]
#> [1,]    1    2    3    4
#> [2,]    1    2    3    4
#> [3,]    1    2    3    4
#> [4,]    1    2    3    4
#> ── TIFF tags ───────────────────────────────────────────────────────────────────
stack <- linescan_to_stack(linescan)
print(stack)
#> 1x4 pixel ijtiff_img with 1 channel and 4 frames.
#> Preview (top left of first channel of first frame):
#> [1] 1 2 3 4
#> ── TIFF tags ───────────────────────────────────────────────────────────────────
linescan <- stack_to_linescan(stack)
print(linescan)
#> 4x4 pixel ijtiff_img with 1 channel and 1 frame.
#> Preview (top left of first channel of first frame):
#>      [,1] [,2] [,3] [,4]
#> [1,]    1    2    3    4
#> [2,]    1    2    3    4
#> [3,]    1    2    3    4
#> [4,]    1    2    3    4
#> ── TIFF tags ───────────────────────────────────────────────────────────────────
```

# Count the number of frames in a TIFF file.

TIFF files can hold many frames. Often this is sensible, e.g. each frame
could be a time-point in a video or a slice of a z-stack.

## Usage

``` r
count_frames(path)

frames_count(path)
```

## Arguments

- path:

  A string, the path to the tiff file to read. `path` may also be a raw
  vector of TIFF bytes, read in-memory.

## Value

A number, the number of frames in the TIFF file. This has an attribute
`n_dirs` which holds the true number of directories in the TIFF file,
making no allowance for the way ImageJ may write TIFF files.

## Details

For those familiar with TIFF files, this function counts the number of
directories in a TIFF file. There is an adjustment made for some
ImageJ-written TIFF files.

## Examples

``` r
count_frames(system.file("img", "Rlogo.tif", package = "ijtiff"))
#> [1] 1
#> attr(,"n_dirs")
#> [1] 1
```

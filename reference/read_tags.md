# Read TIFF tag information without actually reading the image array.

TIFF files contain metadata about images in their *TIFF tags*. This
function is for reading this information without reading the actual
image.

## Usage

``` r
read_tags(path, frames = "all", translate_tags = TRUE)

tags_read(path, frames = 1)
```

## Arguments

- path:

  A string, the path to the tiff file to read. `path` may also be a raw
  vector of TIFF bytes, read in-memory.

- frames:

  Which frames do you want to read. Default all. To read the 2nd and 7th
  frames, use `frames = c(2, 7)`.

- translate_tags:

  Logical. Should the TIFF tags be translated to human-readable strings?
  E.g. `Compression = 1` becomes `Compression = "none"`.

## Value

A list of lists.

## See also

[`read_tif()`](https://docs.ropensci.org/ijtiff/reference/read_tif.md)

## Author

Simon Urbanek, Kent Johnson, Rory Nolan.

## Examples

``` r
read_tags(system.file("img", "Rlogo.tif", package = "ijtiff"))
#> $frame1
#> $frame1$ImageWidth
#> [1] 100
#> 
#> $frame1$ImageLength
#> [1] 76
#> 
#> $frame1$ImageDepth
#> [1] 1
#> 
#> $frame1$BitsPerSample
#> [1] 8
#> 
#> $frame1$SamplesPerPixel
#> [1] 4
#> 
#> $frame1$SampleFormat
#> [1] "unsigned integer data"
#> 
#> $frame1$PlanarConfiguration
#> [1] "contiguous"
#> 
#> $frame1$RowsPerStrip
#> [1] 76
#> 
#> $frame1$TileWidth
#> NULL
#> 
#> $frame1$TileLength
#> NULL
#> 
#> $frame1$Compression
#> [1] "LZW"
#> 
#> $frame1$Threshholding
#> [1] 1
#> 
#> $frame1$XResolution
#> [1] 299.99
#> 
#> $frame1$YResolution
#> [1] 299.99
#> 
#> $frame1$XPosition
#> NULL
#> 
#> $frame1$YPosition
#> NULL
#> 
#> $frame1$ResolutionUnit
#> [1] "inch"
#> 
#> $frame1$Orientation
#> [1] "top_left"
#> 
#> $frame1$Copyright
#> NULL
#> 
#> $frame1$Artist
#> NULL
#> 
#> $frame1$DocumentName
#> NULL
#> 
#> $frame1$DateTime
#> NULL
#> 
#> $frame1$ImageDescription
#> NULL
#> 
#> $frame1$Software
#> NULL
#> 
#> $frame1$PhotometricInterpretation
#> [1] "RGB"
#> 
#> $frame1$ColorMap
#> NULL
#> 
#> 
```

# Working with TIFF Tags

## Introduction to TIFF Tags

TIFF (Tagged Image File Format) files are structured around a set of
metadata elements called “tags”. These tags contain information about
the image data, such as dimensions, color space, compression method, and
other properties. The `ijtiff` package provides functions to work with
these tags, allowing you to both read existing tags from TIFF files and
understand which tags are supported.

## Supported TIFF Tags

The
[`get_supported_tags()`](https://docs.ropensci.org/ijtiff/reference/get_supported_tags.md)
function returns a named integer vector of all TIFF tags that are
supported by the `ijtiff` package. Let’s see what tags are available:

``` r

library(ijtiff)
print(supported_tags <- get_supported_tags())
#>                ImageWidth               ImageLength                ImageDepth 
#>                       256                       257                     32997 
#>             BitsPerSample           SamplesPerPixel              SampleFormat 
#>                       258                       277                       339 
#>       PlanarConfiguration              RowsPerStrip                 TileWidth 
#>                       284                       278                       322 
#>                TileLength               Compression             Threshholding 
#>                       323                       259                       263 
#>               XResolution               YResolution                 XPosition 
#>                       282                       283                       286 
#>                 YPosition            ResolutionUnit               Orientation 
#>                       287                       296                       274 
#>                 Copyright                    Artist              DocumentName 
#>                     33432                       315                       269 
#>                  DateTime          ImageDescription                  Software 
#>                       306                       270                       305 
#> PhotometricInterpretation                  ColorMap 
#>                       262                       320
```

The names in this vector are the human-readable tag names, and the
values are the corresponding tag codes defined in the TIFF
specification.

## Reading Tags from TIFF Files

To read the tags from an existing TIFF file, you can use the
[`read_tags()`](https://docs.ropensci.org/ijtiff/reference/read_tags.md)
function. Let’s read tags from a sample TIFF file included with the
package:

``` r

sample_tiff <- system.file("img", "Rlogo.tif", package = "ijtiff")
tags <- read_tags(sample_tiff)
tags[[1]]
#> $ImageWidth
#> [1] 100
#> 
#> $ImageLength
#> [1] 76
#> 
#> $ImageDepth
#> [1] 1
#> 
#> $BitsPerSample
#> [1] 8
#> 
#> $SamplesPerPixel
#> [1] 4
#> 
#> $SampleFormat
#> [1] "unsigned integer data"
#> 
#> $PlanarConfiguration
#> [1] "contiguous"
#> 
#> $RowsPerStrip
#> [1] 76
#> 
#> $TileWidth
#> NULL
#> 
#> $TileLength
#> NULL
#> 
#> $Compression
#> [1] "LZW"
#> 
#> $Threshholding
#> [1] 1
#> 
#> $XResolution
#> [1] 299.99
#> 
#> $YResolution
#> [1] 299.99
#> 
#> $XPosition
#> NULL
#> 
#> $YPosition
#> NULL
#> 
#> $ResolutionUnit
#> [1] "inch"
#> 
#> $Orientation
#> [1] "top_left"
#> 
#> $Copyright
#> NULL
#> 
#> $Artist
#> NULL
#> 
#> $DocumentName
#> NULL
#> 
#> $DateTime
#> NULL
#> 
#> $ImageDescription
#> NULL
#> 
#> $Software
#> NULL
#> 
#> $PhotometricInterpretation
#> [1] "RGB"
#> 
#> $ColorMap
#> NULL
```

The
[`read_tags()`](https://docs.ropensci.org/ijtiff/reference/read_tags.md)
function returns a list where each element corresponds to the tags from
one frame of the TIFF file.

## Understanding Tag Values

Different tags have different data types and interpretations. Let’s
examine the values of some common tags:

``` r

tags[[1]]$ImageWidth
#> [1] 100
tags[[1]]$ImageLength # Height of the image
#> [1] 76
tags[[1]]$XResolution
#> [1] 299.99
tags[[1]]$YResolution
#> [1] 299.99
tags[[1]]$ResolutionUnit
#> [1] "inch"
```

## Tags in Multi-Frame TIFF Files

TIFF files can contain multiple frames, and each frame can have
different tag values. Let’s examine a multi-frame TIFF file:

``` r

multi_frame_tiff <- system.file("img", "Rlogo-banana.tif", package = "ijtiff")
multi_frame_tags <- read_tags(multi_frame_tiff)
length(multi_frame_tags)
#> [1] 8
```

We can compare tags across frames to see if they differ:

``` r

dimensions <- data.frame(
  Frame = character(),
  Width = integer(),
  Height = integer(),
  stringsAsFactors = FALSE
)
for (i in seq_along(multi_frame_tags)) {
  frame_name <- names(multi_frame_tags)[i]
  dimensions <- rbind(
    dimensions,
    data.frame(
      Frame = frame_name,
      Width = multi_frame_tags[[i]]$ImageWidth,
      Height = multi_frame_tags[[i]]$ImageLength,
      stringsAsFactors = FALSE
    )
  )
}
dimensions
#>    Frame Width Height
#> 1 frame1   100     78
#> 2 frame2   100     78
#> 3 frame3   100     78
#> 4 frame4   100     78
#> 5 frame5   100     78
#> 6 frame6   100     78
#> 7 frame7   100     78
#> 8 frame8   100     78
```

## Conclusion

TIFF tags provide rich metadata about image files. Using the `ijtiff`
package, you can both read these tags from existing files and understand
which tags are supported. This can be particularly useful for scientific
image analysis, where metadata like resolution, units, and creation time
can be crucial for proper interpretation of results.

For more information about TIFF tags, you can refer to the TIFF 6.0
specification or visit the Library of Congress’s [TIFF Tag
Reference](https://www.loc.gov/preservation/digital/formats/content/tiff_tags.shtml).

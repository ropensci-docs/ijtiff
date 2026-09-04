# Write images in TIFF format

Write images into a TIFF file.

## Usage

``` r
write_tif(
  img,
  path,
  bits_per_sample = "auto",
  compression = "none",
  overwrite = FALSE,
  msg = TRUE,
  tags_to_write = NULL
)

tif_write(
  img,
  path,
  bits_per_sample = "auto",
  compression = "none",
  overwrite = FALSE,
  msg = TRUE,
  tags_to_write = NULL
)
```

## Arguments

- img:

  An array representing the image.

  - For a multi-plane, grayscale image, use a 3-dimensional array
    `img[y, x, plane]`.

  - For a multi-channel, single-plane image, use a 4-dimensional array
    with a redundant 4th slot `img[y, x, channel, ]` (see
    [ijtiff_img](https://docs.ropensci.org/ijtiff/reference/ijtiff_img.md)
    'Examples' for an example).

  - For a multi-channel, multi-plane image, use a 4-dimensional array
    `img[y, x, channel, plane]`.

- path:

  Path to the TIFF file to write to. Pass `path = NULL` to write to
  memory; `write_tif` then returns the TIFF file as a raw vector instead
  of writing to disk.

- bits_per_sample:

  Number of bits per sample (numeric scalar). Supported values are 8,
  16, and 32. The default `"auto"` automatically picks the smallest
  workable value based on the maximum element in `img`. For example, if
  the maximum element in `img` is 789, then 16-bit will be chosen
  because 789 is greater than 2 ^ 8 - 1 but less than or equal to 2 ^
  16 - 1.

- compression:

  A string, the desired compression algorithm. Must be one of `"none"`,
  `"LZW"`, `"PackBits"`, `"RLE"`, `"JPEG"`, `"deflate"` or `"Zip"`. If
  you want compression but don't know which one to go for, I recommend
  `"Zip"`, it gives a large file size reduction and it's lossless. Note
  that `"deflate"` and `"Zip"` are the same thing. Avoid using `"JPEG"`
  compression in a TIFF file if you can; I've noticed it can be buggy.

- overwrite:

  If writing the image would overwrite a file, do you want to proceed?

- msg:

  Print an informative message about the image being written?

- tags_to_write:

  A named list of TIFF tags to write. Tag names are case-insensitive and
  hyphens/underscores are ignored (e.g., "X_Resolution", "x-resolution",
  and "xresolution" are all equivalent).

  If `NULL` (the default), any supported TIFF tags present as attributes
  on `img` (e.g., from
  [`read_tif()`](https://docs.ropensci.org/ijtiff/reference/read_tif.md))
  will be automatically extracted and written. If `tags_to_write` is
  specified, it is merged with auto-detected tags, with explicit tags
  taking precedence. This enables round-trip editing where metadata from
  a read image is preserved on re-export unless explicitly overridden.

  Supported tags are:

  - `xresolution` - Numeric value for horizontal resolution in pixels
    per unit

  - `yresolution` - Numeric value for vertical resolution in pixels per
    unit

  - `resolutionunit` - Integer: 1 (none), 2 (inch), or 3 (centimeter)

  - `orientation` - Integer 1-8 for image orientation

  - `xposition` - Numeric value for horizontal position in resolution
    units

  - `yposition` - Numeric value for vertical position in resolution
    units

  - `copyright` - Character string for copyright notice

  - `artist` - Character string for creator name

  - `documentname` - Character string for document name

  - `datetime` - Date/time (character, Date, or POSIXct)

  - `imagedescription` - Character string for image description

  - `photometric` - `"RGB"`, `"BlackIsZero"`, or `"WhiteIsZero"` (or
    integer code 0/1/2). Default is `"BlackIsZero"`.

## Value

The input `img` (invisibly), or — when `path = NULL` — a raw vector of
the TIFF bytes.

## See also

[`read_tif()`](https://docs.ropensci.org/ijtiff/reference/read_tif.md)

## Author

Simon Urbanek wrote most of this code for the 'tiff' package. Rory Nolan
lifted it from there and changed it around a bit for this 'ijtiff'
package. Credit should be directed towards Lord Urbanek.

## Examples

``` r
img <- read_tif(system.file("img", "Rlogo.tif", package = "ijtiff"))
#> Reading image from /github/home/R/x86_64-pc-linux-gnu-library/4.6/ijtiff/img/Rlogo.tif
#> Reading an 8-bit, integer image with dimensions 76x100x4x1 (y,x,channel,frame) . . .
temp_dir <- tempdir()

# Basic write
write_tif(img, paste0(temp_dir, "/", "Rlogo"))
#> Writing /tmp/RtmpX8vkBW/Rlogo.tif: an 8-bit, 76x100 pixel image of unsigned integer type with 4 channels and 1 frame . . .
#>  Done.

# Round-trip with automatic metadata preservation
# Metadata from read_tif is automatically preserved
img_with_metadata <- read_tif(system.file("img", "Rlogo.tif", 
                                          package = "ijtiff"))
#> Reading image from /github/home/R/x86_64-pc-linux-gnu-library/4.6/ijtiff/img/Rlogo.tif
#> Reading an 8-bit, integer image with dimensions 76x100x4x1 (y,x,channel,frame) . . .
write_tif(img_with_metadata, paste0(temp_dir, "/", "Rlogo_preserved"))
#> Writing /tmp/RtmpX8vkBW/Rlogo_preserved.tif: an 8-bit, 76x100 pixel image of unsigned integer type with 4 channels and 1 frame . . .
#>  Done.

# Write with additional/overridden tags
write_tif(img_with_metadata, paste0(temp_dir, "/", "Rlogo_with_tags"),
          tags_to_write = list(
            artist = "R Core Team",
            copyright = "(c) 2024",
            imagedescription = "The R logo"
          ))
#> Writing /tmp/RtmpX8vkBW/Rlogo_with_tags.tif: an 8-bit, 76x100 pixel image of unsigned integer type with 4 channels and 1 frame . . .
#>  Done.

img <- matrix(1:4, nrow = 2)
write_tif(img, paste0(temp_dir, "/", "tiny2x2"))
#> Writing /tmp/RtmpX8vkBW/tiny2x2.tif: an 8-bit, 2x2 pixel image of unsigned integer type with 1 channel and 1 frame . . .
#>  Done.
list.files(temp_dir, pattern = "tif$")
#> [1] "Rlogo.tif"           "Rlogo_preserved.tif" "Rlogo_with_tags.tif"
#> [4] "tiny2x2.tif"        
```

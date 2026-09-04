# Read/write an image array to/from disk as text file(s).

Write images (arrays) as tab-separated `.txt` files on disk. Each
channel-frame pair gets its own file.

## Usage

``` r
write_txt_img(img, path, rds = FALSE, msg = TRUE)

read_txt_img(path, msg = TRUE)

txt_img_write(img, path, rds = FALSE, msg = TRUE)

txt_img_read(path, msg = TRUE)
```

## Arguments

- img:

  An image, represented by a 4-dimensional array, like an
  [ijtiff_img](https://docs.ropensci.org/ijtiff/reference/ijtiff_img.md).

- path:

  The name of the input/output output file(s), *without* a file
  extension.

- rds:

  In addition to writing a text file, save the image as an RDS (a single
  R object) file?

- msg:

  Print an informative message about the image being read?

## Examples

``` r
img <- read_tif(system.file("img", "Rlogo.tif", package = "ijtiff"))
#> Reading image from /github/home/R/x86_64-pc-linux-gnu-library/4.6/ijtiff/img/Rlogo.tif
#> Reading an 8-bit, integer image with dimensions 76x100x4x1 (y,x,channel,frame) . . .
tmptxt <- tempfile(pattern = "img", fileext = ".txt")
write_txt_img(img, tmptxt)
#> Writing img8037bef6152_ch1.txt, img8037bef6152_ch2.txt, img8037bef6152_ch3.txt and img8037bef6152_ch4.txt: a 76x100 pixel text image with 4 channels and 1 frame . . .
#>  Done.
tmptxt_ch1_path <- paste0(strex::str_before_last_dot(tmptxt), "_ch1.txt")
print(tmptxt_ch1_path)
#> [1] "/tmp/RtmpX8vkBW/img8037bef6152_ch1.txt"
txt_img <- read_txt_img(tmptxt_ch1_path)
#> Reading 76x100 pixel text image 'img8037bef6152_ch1.txt' . . .
#>  Done.
```

# Reading and Writing Images

## Reading TIFF files

Check out the following video:

![](../../../../github/home/R/x86_64-pc-linux-gnu-library/4.6/ijtiff/img/Rlogo-banana.gif)

As you can see, it’s a colour video of a banana dancing in front of the
R logo. Hence, it has colour channel (red, green and blue) and frame (a
video is comprised of several *frames*) information inside. I have this
video saved in a TIFF file.

``` r

path_dancing_banana <- system.file("img", "Rlogo-banana.tif",
  package = "ijtiff"
)
print(path_dancing_banana)
#> [1] "/github/home/R/x86_64-pc-linux-gnu-library/4.6/ijtiff/img/Rlogo-banana.tif"
```

To read it in, you just need
[`read_tif()`](https://docs.ropensci.org/ijtiff/reference/read_tif.md)
and the path to the image.

``` r

library(ijtiff)
img_dancing_banana <- read_tif(path_dancing_banana)
#> Reading image from /github/home/R/x86_64-pc-linux-gnu-library/4.6/ijtiff/img/Rlogo-banana.tif
#> Reading an 8-bit, integer image with dimensions 78x100x3x8 (y,x,channel,frame) . . .
```

Let’s take a peek inside of `img_dancing_banana`.

``` r

print(img_dancing_banana)
#> 78x100 pixel ijtiff_img with 3 channels and 8 frames.
#> Preview (top left of first channel of first frame):
#>      [,1] [,2] [,3] [,4] [,5] [,6]
#> [1,]  255  255  255  255  255  255
#> [2,]  255  255  255  255  255  255
#> [3,]  255  255  255  255  255  255
#> [4,]  255  255  255  255  255  255
#> [5,]  255  255  255  255  255  255
#> [6,]  255  255  255  255  255  255
#> ── TIFF tags ───────────────────────────────────────────────────────────────────
#> • ImageWidth: 100
#> • ImageLength: 78
#> • ImageDepth: 1
#> • BitsPerSample: 8
#> • SamplesPerPixel: 3
#> • SampleFormat: unsigned integer data
#> • PlanarConfiguration: contiguous
#> • RowsPerStrip: 78
#> • Compression: Deflate
#> • Threshholding: 1
#> • ResolutionUnit: inch
#> • Orientation: top_left
#> • Software: ijtiff package, R 4.0.0
#> • PhotometricInterpretation: BlackIsZero
```

You can see it’s a 4-dimensional array. The last two dimensions are 3
and 8; this is because these are the channel and frame slots
respectively: the image has 3 channels (red, green and blue) and 8
frames. The first two dimensions tell us that the images in the video
are 78 pixels tall and 100 pixels wide. The image object is of class
`ijtiff_img`. This guarantees that it is a 4-dimensional array with this
structure. The attributes of the `ijtiff_img` give information on the
various TIFF tags that were part of the TIFF image. You can read more
about various TIFF tags at
<https://www.loc.gov/preservation/digital/formats/content/tiff_tags.shtml>.
To read just the tags and not the image, use the
[`read_tags()`](https://docs.ropensci.org/ijtiff/reference/read_tags.md)
function.

Let’s visualize the constituent parts of that 8-frame, colour TIFF.

![](reading-and-writing-images_files/figure-html/red-blue-green-banana-1.png)

There you go: 8 frames in 3 colours.

### Reading only certain frames

It’s possible to read only certain frames. This can be a massive time
and memory saver when working with large images.

Suppose we only want frames 3, 5 and 7 from the image above.

``` r

img_dancing_banana357 <- read_tif(path_dancing_banana, frames = c(3, 5, 7))
#> Reading image from /github/home/R/x86_64-pc-linux-gnu-library/4.6/ijtiff/img/Rlogo-banana.tif
#> Reading an 8-bit, integer image with dimensions 78x100x3x3 (y,x,channel,frame) . . .
```

Let’s visualize again.

![](reading-and-writing-images_files/figure-html/red-bblue-green-banana357-1.png)

Just in case you’re wondering, it’s not currently possible to read only
certain channels.

### More examples

If you read an image with only one frame, the frame slot (4) will still
be there:

``` r

path_rlogo <- system.file("img", "Rlogo.tif", package = "ijtiff")
img_rlogo <- read_tif(path_rlogo)
#> Reading image from /github/home/R/x86_64-pc-linux-gnu-library/4.6/ijtiff/img/Rlogo.tif
#> Reading an 8-bit, integer image with dimensions 76x100x4x1 (y,x,channel,frame) . . .
dim(img_rlogo) # 4 channels, 1 frame
#> [1]  76 100   4   1
class(img_rlogo)
#> [1] "ijtiff_img" "array"
display(img_rlogo)
```

![](reading-and-writing-images_files/figure-html/one-frame-1.png)

You can also have an image with only 1 channel:

``` r

path_rlogo_grey <- system.file("img", "Rlogo-grey.tif", package = "ijtiff")
img_rlogo_grey <- read_tif(path_rlogo_grey)
#> Reading image from /github/home/R/x86_64-pc-linux-gnu-library/4.6/ijtiff/img/Rlogo-grey.tif
#> Reading a 32-bit, float image with dimensions 76x100x1x1 (y,x,channel,frame) . . .
dim(img_rlogo_grey) # 1 channel, 1 frame
#> [1]  76 100   1   1
display(img_rlogo_grey)
```

![](reading-and-writing-images_files/figure-html/one-channel-1.png)

## Writing TIFF files

To write an image, you need an object in the style of an `ijtiff_img`
object (see
[`help("ijtiff_img", package = "ijtiff")`](https://docs.ropensci.org/ijtiff/reference/ijtiff_img.md)).
The basic idea is to have your image in a 4-dimensional array with the
structure `img[y, x, channel, frame]`. Then, to write this image to the
location `path`, you just type `write_tif(img, path)`.

``` r

path <- tempfile(pattern = "dancing-banana", fileext = ".tif")
print(path)
#> [1] "/tmp/Rtmp0pvDTj/dancing-bananac9e83e349c.tif"
write_tif(img_dancing_banana, path)
#> Writing /tmp/Rtmp0pvDTj/dancing-bananac9e83e349c.tif: an 8-bit, 78x100 pixel image of unsigned integer type with 3 channels and 8 frames . . .
#>  Done.
```

## Reading text images

Note: if you don’t know what text images are, see
[`vignette("text-images", package = "ijtiff")`](https://docs.ropensci.org/ijtiff/articles/text-images.md).

You may have a text image that you want to read (but realistically, you
might never).

``` r

path_txt_img <- system.file("img", "Rlogo-grey.txt", package = "ijtiff")
txt_img <- read_txt_img(path_txt_img)
#> Reading 76x100 pixel text image 'Rlogo-grey.txt' . . .
#>  Done.
```

## Writing text images

Writing a text image works as you’d expect.

``` r

write_txt_img(txt_img, path = tempfile(pattern = "txtimg", fileext = ".txt"))
#> Writing txtimgc9e39ec42bd.txt: a 76x100 pixel text image with 1 channel and 1 frame . . .
#>  Done.
```

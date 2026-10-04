# TIFF tag reference.

A dataset containing the information on all known baseline and extended
TIFF tags.

## Usage

``` r
tif_tags_reference()
```

## Source

<https://www.awaresystems.be>

## Details

A data frame with 96 rows and 10 variables:

- code_dec:

  decimal numeric code of the TIFF tag

- code_hex:

  hexadecimal numeric code of the TIFF tag

- name:

  the name of the TIFF tag

- short_description:

  a short description of the TIFF tag

- tag_type:

  the type of TIFF tag: either "baseline" or "extended"

- url:

  the URL of the TIFF tag at <https://www.awaresystems.be>

- libtiff_name:

  the TIFF tag name in the libtiff C library

- c_type:

  the C type of the TIFF tag data in libtiff

- count:

  the number of elements in the TIFF tag data

- default:

  the default value of the data held in the TIFF tag

## Examples

``` r
tif_tags_reference()
#> # A tibble: 96 × 10
#>    code_dec code_hex name      short_description type  url   libtiff_name c_type
#>       <dbl> <chr>    <chr>     <chr>             <chr> <chr> <chr>        <chr> 
#>  1      254 00FE     NewSubfi… A general indica… base… http… TIFFTAG_SUB… LONG  
#>  2      255 00FF     SubfileT… A general indica… base… http… TIFFTAG_OSU… SHORT 
#>  3      256 0100     ImageWid… The number of co… base… http… TIFFTAG_IMA… SHORT…
#>  4      257 0101     ImageLen… The number of ro… base… http… TIFFTAG_IMA… SHORT…
#>  5      258 0102     BitsPerS… Number of bits p… base… http… TIFFTAG_BIT… SHORT 
#>  6      259 0103     Compress… Compression sche… base… http… TIFFTAG_COM… SHORT 
#>  7      262 0106     Photomet… The color space … base… http… TIFFTAG_PHO… SHORT 
#>  8      263 0107     Threshho… For black and wh… base… http… TIFFTAG_THR… SHORT 
#>  9      264 0108     CellWidth The width of the… base… http… TIFFTAG_CEL… SHORT 
#> 10      265 0109     CellLeng… The length of th… base… http… TIFFTAG_CEL… SHORT 
#> # ℹ 86 more rows
#> # ℹ 2 more variables: count <chr>, default <chr>
```

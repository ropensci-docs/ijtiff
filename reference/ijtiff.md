# `ijtiff`: TIFF I/O for *ImageJ* users

This is a general purpose TIFF I/O utility for R. The [`tiff`
package](https://cran.r-project.org/package=tiff) already exists for
this purpose but `ijtiff` adds some functionality and overcomes some
bugs therein.

## Details

- `ijtiff` can write TIFF files whose pixel values are real
  (floating-point) numbers; `tiff` cannot.

- `ijtiff` can read and write *text images*; `tiff` cannot.

- `tiff` struggles to interpret channel information and gives cryptic
  errors when reading TIFF files written by the *ImageJ* software;
  `ijtiff` works smoothly with these images.

## See also

Useful links:

- <https://docs.ropensci.org/ijtiff/>

- <https://github.com/ropensci/ijtiff>

- Report bugs at <https://github.com/ropensci/ijtiff/issues>

## Author

**Maintainer**: Rory Nolan <rorynoolan@gmail.com>
([ORCID](https://orcid.org/0000-0002-5239-4043))

Authors:

- Kent Johnson <kjohnson@akoyabio.com>

Other contributors:

- Simon Urbanek <Simon.Urbanek@r-project.org> \[contributor\]

- Sergi Padilla-Parra <spadilla@well.ox.ac.uk>
  ([ORCID](https://orcid.org/0000-0002-8010-9481)) \[thesis advisor\]

- Jeroen Ooms ([ORCID](https://orcid.org/0000-0002-4035-0289))
  \[reviewer, contributor\]

- Jon Clayden ([ORCID](https://orcid.org/0000-0002-6608-0619))
  \[reviewer\]

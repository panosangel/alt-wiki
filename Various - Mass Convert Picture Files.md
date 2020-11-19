# Mass Convert Picture Files

## Install Software
Look for [ImageMagick - Convert, Edit, or Compose Bitmap Images](https://www.imagemagick.org/)

## Script
Example to set size `-resize` proportionally and quality `-quality` to all `jpg` files in a folder. It does _not_ work recursively.
```shell script
for file in *.jpg; do convert $file -resize 70% -quality 90 $NEW_FOLDER/${file}_mod; done
```
# Manage PDF Files

## Use Ghostscript to shrink a pdf
```shell script
gs -sDEVICE=pdfwrite -dPDFSETTINGS=/ebook -dCompatibilityLevel=1.4 -dNOPAUSE -dQUIET -dBATCH -sOutputFile=output.pdf merged.pdf
```
Source: [Compress PDF files with ghostscript](https://gist.github.com/firstdoit/6390547) 

## Merge files
```shell script
sudo apt-get install poppler-utils
```
```shell script
pdfunite source1.pdf source2.pdf merged_output.pdf
```
Source: [How to easily merge PDF documents under Linux](https://tuxbyte.com/how-to-easily-merge-pdf-documents-under-linux/)

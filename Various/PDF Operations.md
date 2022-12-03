# PDF Operations

## Compress a file

1. Install Ghostscript 
   ```shell
   sudo apt install ghostscript
   ```
2. Run 
   ```shell
   gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/ebook -dNOPAUSE -dQUIET -dBATCH -sOutputFile=OUTPUT.pdf INPUT.pdf
   ```


## Appendix A - Sources
- [How to Compress PDF in Linux [GUI & Terminal]](https://itsfoss.com/compress-pdf-linux/)

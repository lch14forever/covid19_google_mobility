## Download all pdfs

Download index

```sh
wget https://www.google.com/covid19/mobility/
```

Download all pdfs

```sh
grep -o  "https://.*pdf" index.html | while read l;do wget $l;done
```

## Extract the first two pages

Create new working directory

```sh
mkdir separated && cd separated
```

Use poppler to split pages

```sh
for i in ../*pdf; do bs=`basename $i`; pdfseparate -f 1 -l 2 $i ${bs%%.pdf}.%d.pdf;done
```

## Crop the pages

Crop Using pdfcrop in texlive. Crop twice -- first time roughly only keeping the figures and then crop automatically to the content

Create new working directory

```sh
cd .. && mkdir cropped && cd cropped
```

Crop the first page figure

```sh
for i in ../separated/*1.pdf;do  pdfcrop --margins '-120 -300 -190 0' $i tmp.pdf ; bs=`basename $i`; pdfcrop tmp.pdf $bs; done
```

Crop the seconda page figure

```sh
for i in ../separated/*2.pdf;do  pdfcrop --margins '-120 0 -190 0' $i tmp.pdf ; bs=`basename $i`; pdfcrop tmp.pdf $bs; done
```

Clean up

```sh
rm tmp.pdf
```

## Crop out each pannel 

Create new working directory

```sh
cd .. && mkdir pannels && cd pannels
```

Top

```sh
for i in ../cropped/*1.pdf; do pdfcrop --margins '0 0 0 -200' $i tmp.pdf ; bs=`basename $i`; pdfcrop tmp.pdf ${bs%%?.pdf}1.pdf; done
for i in ../cropped/*2.pdf; do pdfcrop --margins '0 0 0 -200' $i tmp.pdf ; bs=`basename $i`; pdfcrop tmp.pdf ${bs%%?.pdf}4.pdf; done
```

Middle

```sh
for i in ../cropped/*1.pdf; do pdfcrop --margins '0 -100 0 -100' $i tmp.pdf ; bs=`basename $i`; pdfcrop tmp.pdf ${bs%%?.pdf}2.pdf; done
for i in ../cropped/*2.pdf; do pdfcrop --margins '0 -100 0 -100' $i tmp.pdf ; bs=`basename $i`; pdfcrop tmp.pdf ${bs%%?.pdf}5.pdf; done
```

Bottom

```sh
for i in ../cropped/*1.pdf; do pdfcrop --margins '0 -200 0 0' $i tmp.pdf ; bs=`basename $i`; pdfcrop tmp.pdf ${bs%%?.pdf}3.pdf; done
for i in ../cropped/*2.pdf; do pdfcrop --margins '0 -200 0 0' $i tmp.pdf ; bs=`basename $i`; pdfcrop tmp.pdf ${bs%%?.pdf}6.pdf; done
```

```sh
rm tmp.pdf
```

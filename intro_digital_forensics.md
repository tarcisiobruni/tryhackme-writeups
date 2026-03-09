# TryHackMe Writeup: Introduction to Digital Forensics

## Digital Forensics Phases

According to the National Institute of Justice, the digital forensics process consists of the following phases:

- Collection:  Identifying all the devices from which the data can be collected (personal computers, laptops, digital cameras, USBs, etc.). It is also necessary to ensure the original data is not tampered with while collecting the evidence.
- Examination: The evidence data usually needs to be filtered, and the data of interest needs to be extracted.
- Analysis: The extracted data is analyzed to determine its relevance and potential legal implications.
- Reporting: A detailed report is created summarizing the findings and the methodology used in the investigation.

A formal document named Chain of Custody is used to document the collection. For each evidence item, the document should include the following information:

- Model
- Author
- Date and time of collection
- Location of collection
- Description of the evidence
- Signature of the person who collected the evidence

## Practical Exercise

Two files are provided to practice digital forensics in this room: a image file (JPG) and a PDF document.

### Question 1: What is the name of the author of the pdf document?

Using tool like pdfinfo is easy to find the author of a PDF document. The following command can be used:

```bash
pdfinfo document.pdf
```

Some usefull information about the PDF document will be displayed, including the author of the document and the program used to create the document, e.g, Microsoft Word.

### Question 2: What is the name of street where the photo was taken?

Using a tool like exiftool, we can extract a huge amount of metadata from the image file, including geographical information about where the photo was taken. The following command can be used:

```bash
exiftool image.jpg
```

Now, using some online mapping tool like Bing ou Google Map, we can search for Longitude and Latitude coordinates to find the name of the street where the photo was taken.

```bash
exiftool image.jpg | grep 'GPS Position'
```

### Question 3: What is the model of camera used to take the photo?

Again, using the exiftool command, we can find about the came model used to take the photo. The following command can be used:

```bash
exiftool image.jpg | grep 'Camera Model Name'
```

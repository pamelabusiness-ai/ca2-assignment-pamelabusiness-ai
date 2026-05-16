# Multi-modal Document Understanding for CA2

This project presents a complete document‑processing pipeline that combines Computer Vision, OCR, and Natural Language Processing (NLP) and other techniques to analyse different types of real‑world documents, including:

- an academic PDF page
- two receipt images
- a logo image
- structured and semi‑structured visual regions (tables, signatures, contours)

The goal is to demonstrate how multiple AI techniques can be integrated into a single system capable of understanding documents across both text and image modalities.


## Project Overview

The aim of this project is to explore how different CV and NLP techniques can be combined into a unified pipeline for document understanding. The focus is not on building a highly optimised or production‑ready system, but on understanding:
- how each stage works
- how each technique contributes to the final output
- how different document types behave under the same processing steps
- The system processes a small set of heterogeneous documents:
- Academic PDF (dense text, structured layout)
- Thermal receipts (noisy, uneven lighting, compressed fonts)
- Logo image (non‑text visual content)

Each document is processed using the same multi‑stage pipeline, allowing direct comparison of results.


## Pipeline / Framework 
The pipeline is designed as a multi‑modal system, integrating both text‑based and image‑based analysis.

1. Image Preprocessing (OpenCV)
Techniques applied: grayscale conversion, noise reduction, CLAHE contrast enhancement, Otsu thresholding, adaptive thresholding for difficult images (receipts).
These steps significantly improve OCR accuracy and prepare images for downstream CV tasks.

2. OCR Text Extraction (Tesseract): PDF page converted to image using Poppler, receipts processed after adaptive thresholding, OCR applied to extract raw text, structured OCR output (bounding boxes) used for section classification

3. Text Preprocessing (NLTK)
A full NLP pipeline was applied:
- cleaning and lowercasing
- tokenisation
- removal of non‑alphabetic tokens
- stopword removal
- stemming
- lemmatisation

This normalises the text and prepares it for feature extraction.

4. Text Feature Extraction
Techniques used: TF‑IDF to identify the most meaningful terms, Bag‑of‑Words for raw frequency counts, RAKE for key phrase extraction, Sentiment analysis using TextBlob, Named Entity Recognition using spaCy (ORG, GPE, DATE, PERSON, MONEY)

5. Document Structure Classification
Using Tesseract’s bounding box heights:
- Title
- Subtitle
- Body text
- This provides insight into the layout and hierarchy of the PDF.

6. Visual Feature Detection (OpenCV)
Multiple CV techniques were applied: contour detection, region classification (text block, figure, table region), 
signature detection using shape descriptors (solidity, complexity), table detection using morphological operations, 
connected components segmentation, Watershed segmentation for precise region boundaries, Canny edge detection, 
ORB keypoint detection for logo analysis.

These steps reveal the visual structure of the documents beyond text.

8. Image Segmentation
Two segmentation methods were used:
- Connected Components — groups all distinct regions
- Watershed Algorithm — separates touching regions and identifies boundaries
- This helps identify meaningful visual segments such as blocks, tables, and signatures.

8. Multi‑Modal Integration
All extracted information is combined into a unified report: OCR text, NLP features, named entities, sentiment, visual regions, segmentation results, table structures, signature candidates, cross‑modal references (e.g., text mentioning “figure”, “table”, etc.).

A document complexity score summarises the richness and structure of the document.

10. Final Output Dashboard
- A 3×3 dashboard visualises the entire pipeline:
- PDF preprocessing
- table detection
- receipt preprocessing
- contour analysis
- Watershed segmentation
- TF‑IDF terms
- NER distribution
- sentiment comparison

This provides a clear overview of the system’s multi‑modal understanding.
**Key Observations:**
1. OCR accuracy depends heavily on preprocessing quality
2. Adaptive thresholding is essential for receipts
3. Academic documents produce richer NLP features
4. Receipts are highly structured and numeric
5. TF‑IDF is less effective for short documents
6. Contour‑based region detection works but is not precise
7. Watershed segmentation improves boundary detection
8. Table detection works well with morphological operations
9. NER performance is affected by OCR noise
10. Complex layouts produce many detected regions

### Technologies Used
- Python
- OpenCV
- Tesseract OCR
- NLTK
- spaCy
- TextBlob
- Scikit‑learn
- Pandas
- Matplotlib

### Limitations
Small dataset — not suitable for ML training
OCR noise affects NLP accuracy
Contour‑based detection is heuristic and imprecise
Watershed can over‑segment if markers are noisy
NER accuracy depends on OCR quality
Complex layouts produce many false regions
- extraction of raw text from document images  

### Conclusion
This project demonstrates how Computer Vision, OCR, and NLP can be combined into a single multi‑modal pipeline for document understanding. The addition of:
- enhanced NER (spaCy)
- structured region detection (tables, signatures)
- segmentation (connected components + Watershed)
- feature detection (ORB, Canny)
- and a final integration dashboard
- provides a comprehensive view of both the textual and visual structure of documents.

The system works well as a complete pipeline and highlights real‑world challenges such as OCR quality, layout complexity, and document variability.

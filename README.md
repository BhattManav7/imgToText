# imgToText
# 🪪 ID Card OCR Extraction using Tesseract and OpenCV

This project implements an end-to-end Optical Character Recognition (OCR) pipeline for extracting structured text data from ID cards using Python, OpenCV, Tesseract, and Pandas.

---

## 📌 Features

- ✅ Image preprocessing using grayscale conversion and thresholding (`cv2.THRESH_TRUNC`)
- ✅ Text extraction using Tesseract (`pytesseract.image_to_data` & `image_to_string`)
- ✅ Confidence-based text filtering and bounding box visualization
- ✅ Output structured as a Pandas DataFrame with word positions and confidence scores
- ✅ Character-level accuracy evaluation using Levenshtein distance
- ✅ Ready for analyzing scanned IDs, student cards, and structured documents

---

## 🧠 Example Workflow

```python
img = cv2.imread("id.jpg")
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
_, threshed = cv2.threshold(gray, 127, 255, cv2.THRESH_TRUNC)

# Text data with bounding boxes and confidence
text_df = pytesseract.image_to_data(threshed, output_type='data.frame')
filtered_text = text_df[text_df.conf != -1]

# Draw boxes for words with conf > 60
for i in range(len(filtered_text)):
    if int(filtered_text['conf'].iloc[i]) > 60:
        (x, y, w, h) = (filtered_text['left'].iloc[i], filtered_text['top'].iloc[i],
                        filtered_text['width'].iloc[i], filtered_text['height'].iloc[i])
        cv2.rectangle(img, (x, y), (x + w, y + h), (0, 255, 0), 2)

# PDF OCR and Structured Data Extraction

This project is a Python pipeline that uses Optical Character Recognition (OCR) to extract text and structured data from scanned PDF documents. It processes each page, cleans the recognized text, identifies key information based on keywords, and exports the findings into a structured JSON file.

This notebook is ideal for batch processing scanned documents, such as mortgage deeds or invoices, to pull out key fields.


*(**Note:** To add an image here, take a screenshot of one of the matplotlib outputs from your notebook, upload it to your GitHub repository, and change the line above to `![OCR Bounding Box Visualization](your-image-filename.png)`)*

##  Features

* **PDF to Image Conversion:** Uses `PyMuPDF (fitz)` to render each PDF page as a high-resolution image.
* **Image Preprocessing:** Applies `OpenCV` filters (grayscale, binary threshold, and median blur) to improve OCR accuracy.
* **OCR Engine:** Leverages `Tesseract` via the `pytesseract` wrapper to extract text and its bounding box coordinates from the preprocessed images.
* **Text Cleaning:** A custom `clean_text_overall` function uses regular expressions (`re`) to fix common OCR errors (e.g., `1s` -> `is`, `Oate` -> `Date`) and normalize data.
* **Keyword Filtering:** Intelligently filters the raw OCR output to keep only text that contains relevant keywords (like "borrower", "loan", "date") or digits, reducing noise.
* **Data Visualization:** Uses `Matplotlib` to display the original PDF pages with green bounding boxes and red text labels drawn over the detected words.
* **Structured Export:** Parses the cleaned text to find key fields (like "Lender", "Loan Amount") and exports them, along with their bounding box, into a clean `mortgage_structured.json` file.

##  Technologies Used

* **Python 3**
* **Tesseract OCR Engine:** The core system-level OCR engine.
* **Python Libraries:**
    * `pymupdf`: For PDF reading and rendering.
    * `pytesseract`: As the Python wrapper for Tesseract.
    * `opencv-python` (cv2): For all image preprocessing.
    * `pillow` (PIL): For image object manipulation.
    * `numpy`: For numerical and image array operations.
    * `matplotlib`: For visualizing the final output.
    * `json`: For exporting the structured data.

##  Setup and Installation

**1. Install Tesseract OCR Engine**

This is a system-level dependency and **must be installed first**.

* **On Ubuntu/Debian:**
    ```bash
    sudo apt update
    sudo apt install tesseract-ocr
    ```
* **On macOS:**
    ```bash
    brew install tesseract
    ```
* **On Windows:**
    Download and run the installer from the [Tesseract at UB Mannheim](https://github.com/UB-Mannheim/tesseract/wiki) page. Ensure you add the Tesseract installation directory to your system's `PATH` environment variable.

**2. Clone the Repository**

```bash
git clone [https://github.com/](https://github.com/)[your-username]/[your-repository-name].git
cd [your-repository-name]
````

**3. Set up a Python Environment and Install Dependencies**

It is highly recommended to use a virtual environment.

```bash
# Create a virtual environment
python3 -m venv venv

# Activate it
source venv/bin/activate  # On Unix/macOS
.\venv\Scripts\activate    # On Windows

# Create a 'requirements.txt' file with the content below
# and then run the install command
pip install -r requirements.txt
```

**`requirements.txt` file content:**

```
pymupdf
pytesseract
opencv-python
pillow
numpy
matplotlib
```

## Usage

1.  **Add a PDF:** Place a sample scanned PDF (like the `MTG_10009588.pdf` you used) into the repository's root folder, or in a `data/` subfolder.
2.  **Update Path:** Change the `pdf_path` variable in the notebook to match your file's name (e.g., `pdf_path = "MTG_10009588.pdf"`).
3.  **Run the Notebook:** Open and run the `Analyze_a_scanned_PDF.ipynb` notebook.
4.  **View Output:**
      * The notebook will display the images of each page with the detected text and bounding boxes overlaid.
      * A new file, `mortgage_structured.json`, will be created in the root directory containing the extracted key-value pairs.

## Example JSON Output

The final cell of the notebook produces the following structured data file (`mortgage_structured.json`):

```json
{
    "Borrower": {
        "value": "Borrower",
        "bbox": [
            349,
            954,
            111,
            19
        ]
    },
    "Lender": {
        "value": "Lender",
        "bbox": [
            547,
            979,
            64,
            18
        ]
    },
    "Loan Amount": {
        "value": "AMOUNT",
        "bbox": [
            1178,
            311,
            71,
            30
        ]
    },
    "Property Address": {
        "value": "ADDRESS",
        "bbox": [
            531,
            256,
            95,
            16
        ]
    },
    "Dates": {
        "value": "dated",
        "bbox": [
            766,
            1204,
            44,
            15
        ]
    },
    "Document Title": {
        "value": "MORTGAGE",
        "bbox": [
            747,
            130,
            207,
            55
        ]
    }
}
```

## Limitations and Future Work

  * **Simple Extraction Logic:** The current structured data export is very simple. It finds the *first* piece of text that contains a keyword (e.g., "borrower"). This could be improved by using regex to find the *value* associated with the keyword (e.g., find "Loan Amount", then find the `$XXX,XXX.XX` value near it).
  * **Page Chunking:** The notebook splits the page into three horizontal chunks for faster processing. This is a naive approach and could cut words or lines in half, leading to missed data. A more robust solution might involve processing the full page or using `pytesseract`'s page segmentation modes.
  * **Script Conversion:** This proof-of-concept notebook could be refactored into a command-line `.py` script for easier batch processing.



```

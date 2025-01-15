# QR-Code-Bar-Code-MICR
To create a Python program that handles barcode, QR code, OCR, and MICR reading, writing, and scanning, you will need to use various libraries. Below is an overview of the libraries and how to implement them for each task.
1. Barcode and QR Code:

    For barcode and QR code generation and scanning, the pyzbar and qrcode libraries are useful.

2. OCR (Optical Character Recognition):

    For OCR, the popular library is pytesseract, which is a wrapper for Tesseract OCR.

3. MICR (Magnetic Ink Character Recognition):

    MICR is often used in banking and might need specialized handling, but a general OCR method can be used for reading MICR characters. In real-world applications, you may need to use specific libraries or tools provided by financial institutions for accurate results.

Installation:

To install the required libraries, you can use pip:

pip install pyzbar qrcode pytesseract Pillow

For Tesseract OCR, you will also need to install the Tesseract engine:

    For Ubuntu: sudo apt install tesseract-ocr
    For Windows: Download from Tesseract GitHub and add it to your system path.

Python Code:

import qrcode
from pyzbar.pyzbar import decode
from PIL import Image
import pytesseract
import cv2

# Function to generate a QR code
def generate_qr(data, filename='qrcode.png'):
    qr = qrcode.QRCode(
        version=1,
        error_correction=qrcode.constants.ERROR_CORRECT_L,
        box_size=10,
        border=4,
    )
    qr.add_data(data)
    qr.make(fit=True)
    img = qr.make_image(fill='black', back_color='white')
    img.save(filename)
    print(f"QR code saved as {filename}")

# Function to scan QR code from an image
def scan_qr(image_path):
    img = Image.open(image_path)
    decoded_objects = decode(img)
    for obj in decoded_objects:
        if obj.type == 'QRCODE':
            print(f"QR Code Data: {obj.data.decode('utf-8')}")
            return obj.data.decode('utf-8')
    print("No QR code found")
    return None

# Function to generate a barcode
def generate_barcode(data, filename='barcode.png'):
    from barcode import Code128
    from barcode.writer import ImageWriter
    barcode = Code128(data, writer=ImageWriter())
    barcode.save(filename)
    print(f"Barcode saved as {filename}")

# Function to scan barcode from an image
def scan_barcode(image_path):
    img = cv2.imread(image_path)
    barcodes = decode(img)
    for barcode in barcodes:
        barcode_data = barcode.data.decode('utf-8')
        barcode_type = barcode.type
        print(f"Barcode Data: {barcode_data}, Barcode Type: {barcode_type}")
        return barcode_data
    print("No barcode found")
    return None

# Function to perform OCR (optical character recognition)
def ocr_scan(image_path):
    img = cv2.imread(image_path)
    text = pytesseract.image_to_string(img)
    print(f"OCR Text: {text}")
    return text

# Function to scan MICR (similar to OCR but focused on font used in MICR)
def micr_scan(image_path):
    img = cv2.imread(image_path)
    # This is just a generic OCR scan for MICR. You might need specialized tools for accurate results.
    text = pytesseract.image_to_string(img)
    print(f"MICR Text: {text}")
    return text

# Example Usage
if __name__ == '__main__':
    # Generate QR Code
    generate_qr("https://www.example.com", "example_qrcode.png")
    # Scan QR Code
    scan_qr("example_qrcode.png")

    # Generate Barcode
    generate_barcode("123456789", "example_barcode.png")
    # Scan Barcode
    scan_barcode("example_barcode.png")

    # Perform OCR
    ocr_scan("example_image.png")

    # Perform MICR Scan (same as OCR for general purposes)
    micr_scan("example_micr_image.png")

Explanation:

    Generate QR Code: Using qrcode library, this function generates a QR code from the given data and saves it as an image.

    Scan QR Code: The pyzbar library is used to decode QR codes from an image.

    Generate Barcode: Using python-barcode library, this function generates a barcode of the specified data and saves it.

    Scan Barcode: The pyzbar library can decode different types of barcodes (like Code128) from an image.

    OCR: pytesseract is used to extract text from an image using optical character recognition.

    MICR: Since MICR is similar to OCR but uses specific fonts (like the E-13B font), it can be read with pytesseract. However, for more accurate MICR scanning, you would need specialized MICR readers or software. This code will work for basic OCR on MICR fonts as well.

Notes:

    This code requires Tesseract OCR to be installed for OCR functionality.
    You can tweak the OCR and MICR methods based on the quality of the images you're working with (e.g., pre-processing the images for better recognition).

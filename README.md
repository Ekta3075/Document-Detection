# 🪪 ID Card Boundary Detection

This project contains a **generalized identity-card boundary detection algorithm** implemented using **OpenCV** inside a Jupyter Notebook.  
No training data is used.

The final output draws a colored boundary around each detected card and displays the processed result.

---

# 📁 Project Files
The project is organized as follows:
```markdown
Project/
│
├── Document Detection.ipynb # Main Jupyter Notebook
│
├── Images/ # Sample input images
│ ├── sample1.jpg
│ ├── sample2.jpg
│ ├── sample3.jpg
│ └── ... 
│
├── Outputs/ # Auto-generated output images
│ ├── multiple_cards_detected_1.jpg
│ ├── multiple_cards_detected_2.jpg
│ └── ... (images saved after running notebook)
│
├── README.md # Documentation file
└── requirements.txt # Python dependencies
```
Place your test images in the **same folder** as the notebook before running.

---

# 🛠 Installation

Install required packages using:
 ```python
   pip install -r requirements.txt
 ```
**Dependencies:**

- opencv-python  
- numpy  
- matplotlib  

---

# 🚀 How to Run the Notebook

Follow these steps to execute the ID-card detection system:

---

### **1️⃣ Open the notebook**

Open Jupyter Notebook in your project folder

### **2️⃣ Update the image file names if needed**
Inside the notebook, find this list:
```python
sample_images = ["sample1.jpg", "sample2.jpg", "sample3.jpg"]
```

Replace these file names with your own images if required.

Make sure all images are placed in the same directory as the notebook.

### **3️⃣ Run all cells from top to bottom**

The notebook will perform the following steps:

- **Load each input image**
- **Detect ID card boundaries** using edge detection and contour approximation
- **Draw colored contours** around all detected ID-card-like shapes
- **Label each detected card** (e.g., *Card 1*, *Card 2*, etc.)
- **Display the processed output** inside the notebook
- **Save the final result image** in the project folder

Here is the exact code used to process all images:
```python

sample_images = [
    "sample2.jpg",
    "sample1.jpg",
    "sample.jpg",
    "sample4.jpg",
    "sample5.jpg",
    "sample6.jpg",
    "sample7.jpg"
]

for i, img_name in enumerate(sample_images, start=1):
    output, contours = detect_multiple_id_cards(img_name, show=True)
    if output is not None:
        out_name = f"multiple_cards_detected_{i}.jpg"
        cv2.imwrite(out_name, output)
        print(f"Saved: {out_name} | Detected {len(contours)} card(s)")
    else:
        print(f"Skipped {img_name} (image not found or unreadable)")

```
# 🧠 Algorithm Overview

The detection system uses a fully classical computer vision pipeline (no training data).  
Below is the complete step-by-step workflow used inside the notebook:

---

### ✔ 1. Preprocessing
To prepare the image for edge detection:
- Convert the image to **grayscale**
- Apply **Gaussian blur** to smooth the image and reduce noise

These steps improve the quality of edge detection.

---

### ✔ 2. Edge Detection
Edges are extracted from the preprocessed image using **Canny edge detection**:

```python
edges = cv2.Canny(gray, 70, 200)
```
This highlights the strong boundaries of ID cards.

---


### ✔ 3. Contour Detection

Contours are extracted from the edge map using:

```python
cnts, _ = cv2.findContours(edges, cv2.RETR_LIST, cv2.CHAIN_APPROX_SIMPLE)
```
Each contour represents a potential shape in the image.

---

### ✔ 4. Card-Shaped Filtering

From the detected contours, the algorithm selects shapes that resemble ID cards based on:

 - **4 corners** (quadrilateral shape)

 - **Valid area range** (to avoid tiny or huge shapes)

 - **Correct aspect ratio** (similar to typical ID cards)

 - **Not nested inside another contour** (removes duplicates and noise)

Only contours passing all conditions are treated as ID-card candidates.

---

### ✔ 5. Output Rendering

Once valid ID-card shapes are detected, the following actions are performed:

 - **Colored boundaries** are drawn around each detected card

 - **Labels** such as Card 1, Card 2, etc. are added

 - The **output is displayed inside the notebook**

 - The **annotated result is saved** as an image file:
   
   ```python-repl
   multiple_cards_detected_1.jpg
   multiple_cards_detected_2.jpg
   ```
These saved results appear in the same folder as the notebook.
  


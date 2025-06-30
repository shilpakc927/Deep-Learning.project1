# 🩺 Skin Disease Detector

This project is an end-to-end skin disease classification system built with:

- **Jupyter Notebook**: Training a Deep Neural Network (DNN) model on a skin disease dataset.
- **Flask Web Application**: Serving the trained model for predictions.
- **HTML Frontend**: A 2-page interface for users to interact with the classifier.

---

## 📚 Table of Contents

- [Project Overview](#project-overview)
- [Project Structure](#project-structure)
- [Dataset](#dataset)
- [Model](#model-training)
- [Flask Application](#flask-application)
- [HTML Templates](#html-templates)
- [How To Run](#how-to-run)
- [Workflow Summery](#workflow-summery)
- [Technologies Used](#technologies-used)
- [License](#license)
- [Files Included](#files-included)

---
## 🎯 Project Overview

Skin Disease Detector provides a user-friendly way to identify skin conditions from images.  
**Workflow:**

1. **Model Training** (in the notebook):
   - Load a labeled dataset of skin disease images.
   - Preprocess the data.
   - Train a DNN classifier.
   - Save the trained model (`skin_disease_model.h5`).

2. **Flask Application**:
   - Loads the saved model.
   - Provides a web interface for uploading images.
   - Returns predictions.
   - Shows a thank-you page after prediction.

---

## 🗂️ Project Structure

```
skin_disease_detector/
├── app.py                      # Flask backend
├── dnn skin disease dataset.ipynb   # Notebook for training the model
├── skin_disease_model.h5       # Trained model file
├── static/
│   └── uploads/                # Uploaded images stored here
├── templates/
│   ├── dnnfront.html           # Landing dashboard page
│   └── dnnresult.html          # Upload + prediction + thank you
├── requirements.txt            # Python dependencies
└── README.md                   # Project documentation
```

---

## 📊 Dataset
The project uses a **skin disease dataset** containing images labeled by disease type.  
- Images are preprocessed (resized, normalized).  
- Labels are one-hot encoded for multiclass classification.

You can use public datasets such as:
- HAM10000
- Dermnet
- Or any other skin disease image dataset.

**Note:** Make sure your dataset directory is correctly referenced in the notebook.
The dataset contains labeled images of the following skin diseases:
- Actinic keratoses
- Basal cell carcinoma
- Benign keratosis
- Dermatofibroma
- Melanocytic nevi
- Melanoma
- Vascular lesions
- Intraepithelial carcinoma

All images are resized to 224×224 pixels and normalized before training.

---

## 🧠 Model Training (`dnn skin disease dataset.ipynb`)

The Deep Neural Network (DNN) is implemented in Keras.  
Typical architecture includes:

- **Input Layer** matching image dimensions.
- **Hidden Layers** with `Dense` layers and ReLU activations.
- **Dropout Layers** to prevent overfitting.
- **Output Layer** with softmax activation for multiclass classification.

The notebook performs:

1. **Data Preprocessing**
   - Rescale pixel values to [0,1]
   - Resize images to 224×224
   - Split dataset into training and validation

2. **Model Architecture**
   - Flatten input
   - Dense layers with ReLU
   - Dropout layers
   - Final softmax layer

3. **Training**
   ```python
   model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])
   model.fit(train_data, epochs=20, validation_data=val_data)
   ```
4. **Saving**
   ```python
   model.save('skin_disease_model.h5')
   ```
This saved model is loaded by Flask for predictions.

---
## 🌐 Flask Application (`app.py`)

The Flask app manages routing and predictions.

**Routes:**

- `/`  
  Displays `dnnfront.html`, the dashboard introduction.

- `/predict`  
  Handles:  
  - **GET:** Displays upload form in `dnnresult.html`.  
  - **POST:** Receives uploaded file, runs prediction, shows results in `dnnresult.html`.

**Key Steps:**

- **Load saved model:**
  ```python
  model = load_model('skin_disease_model.h5')
  ```

- **Preprocess image:**
  ```python
  img = image.load_img(filepath, target_size=(224, 224))
  img_array = image.img_to_array(img) / 255.0
  img_array = np.expand_dims(img_array, axis=0)
  ```

- **Predict:**
  ```python
  prediction = model.predict(img_array)
  pred_index = np.argmax(prediction)
  predicted_class = class_names[pred_index]
  ```
---

## 🖥️ HTML Templates

**`dnnfront.html`**

This is the introduction dashboard page.

- Title
- Welcome message
- Link/button to go to the upload page (`/predict`)


**`dnnresult.html`**

This page handles:

- Upload form
- Showing prediction results
- Displaying "Thank you" after exit

**Behavior:**

- **When GET:** shows upload form
- **When POST:** shows prediction + exit button
- **When exit button clicked:** shows blank page with thank you message

This template supports:

- Uploading an image
- Displaying prediction results
- Displaying a thank you message after exiting

---
## ⚙️ How to Run
1. **Clone the repository:**
    ```python
   git clone https://github.com/your-username/skin_disease_detector.git
   cd skin_disease_detector
     ```

3. **Create a virtual environment:**
    ```python
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
    ```
    
5. **Install dependencies:**
     ```python 
    pip install -r requirements.txt
      ```

7. **Train model (if needed):**
   - Open the notebook.
   - Run all cells to create skin_disease_model.h5.

8. **Run Flask app:**
     ```python
    python app.py
     ```
     
10. **Open your browser:**
    ```
    http://127.0.0.1:5000/
    ```
---
## 🧪 Workflow Summary
```
✅ Step 1: Open dashboard (/) – welcome and start diagnosis button.
✅ Step 2: Go to upload page (/predict) – browse and upload image.
✅ Step 3: Prediction result displayed with image and exit button.
✅ Step 4: Click exit – shows thank you message.
```
---
## 🛠️ Technologies Used
- Python 3
- TensorFlow/Keras
- Flask
- HTML/CSS

 ---

## 📄 License
   MIT License
 
---

✅ **How to use this:**
- Copy everything **inside the fences above** (including the triple backticks at the start and end).
- Save it as:
  README.md
- Place it in your project folder.
- Commit and push to GitHub.

✅ This is **one single README file** describing:
- Notebook
- Flask
- Two HTML pages
- Complete workflow

---
## 📁 Files Included
- `dnn skin disease dataset.ipynb` – jupyter nootebook  file with  raw data
- `dnnfront.html` – html introduction page
- `dnnresult.html` - html second page for uploading image and prediction
- `hmnist_8_8_L.csv` - dataset
- `skin_disease_model.h5` - saved model for flask working
---

## 📸 Project Screenshots

Here are some screenshots demonstrating the application workflow:

### 🟢 Dashboard Page

![Dashboard](Screenshot-1.png)

### 🖼️ Uploading an Image

![Upload](Screenshot-2.png)

### ✅ Prediction Result

![Prediction](Screenshot-3.png)

### ✅ Exit

![completion message](Screenshot-4.png)

## 📩 Contact

**Shilpa K C**  
[LinkedIn](https://www.linkedin.com/in/shilpa-kc) | [Email](shilpakcc@gmail.com)

For questions or suggestions, feel free to reach out.


   

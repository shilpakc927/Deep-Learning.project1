# 🩺 Skin Disease Detector

This project is an end-to-end Deep Learning application to classify skin diseases from images using a trained DNN model and a Flask web interface.

---

## 🎯 Project Objective

To provide a simple web app that allows:
- Uploading an image of a skin lesion.
- Predicting the disease type.
- Displaying results with an option to exit and see a thank you message.

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

4. **Saving**
   model.save('skin_disease_model.h5')
   
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
   git clone https://github.com/your-username/skin_disease_detector.git
   cd skin_disease_detector

2.**Create a virtual environment:**
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate

3.**Install dependencies:**  
    pip install -r requirements.txt

4.**Train model (if needed):**
   - Open the notebook.
   - Run all cells to create skin_disease_model.h5.

5.**Run Flask app:**
    python app.py

6.**Open your browser:**
    http://127.0.0.1:5000/

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



   

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

skin_disease_detector/
├── app.py # Flask backend
├── dnn skin disease dataset.ipynb # Notebook for training the model
├── skin_disease_model.h5 # Trained model file
├── static/
│ └── uploads/ # Uploaded images stored here
├── templates/
│ ├── dnnfront.html # Landing dashboard page
│ └── dnnresult.html # Upload + prediction + thank you
├── requirements.txt # Python dependencies
└── README.md # Project documentation

---

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
 ## 🌐 Flask Application (app.py)
The Flask app manages routing and predictions.

**Routes**:
- /
Displays dnnfront.html, the dashboard introduction.
- /predict
Handles:
   - GET: Displays upload form in dnnresult.html.
   - POST: Receives uploaded file, runs prediction, shows results in dnnresult.html.

**Key Steps**:

- Load saved model:
        model = load_model('skin_disease_model.h5')

- Preprocess image:
       img = image.load_img(filepath, target_size=(224,224))
       img_array = image.img_to_array(img)/255.0
       img_array = np.expand_dims(img_array, axis=0)

- Predict:
      prediction = model.predict(img_array)
      pred_index = np.argmax(prediction)
      predicted_class = class_names[pred_index]

---
## 🖥️ HTML Templates
**dnnfront.html**
This is the introduction dashboard page.
- Title
- Welcome message
- Link/button to go to the upload page (/predict)

- Example:
  <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Skin Disease Classifier</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            background: linear-gradient(135deg, #6a11cb, #2575fc);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
        }

        .card {
            background-color: #000000;
            color: #ffffff;
            padding: 50px 60px;
            border-radius: 25px;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.5);
            text-align: center;
            animation: fadeIn 1s ease-in-out;
            transition: transform 0.3s ease;
        }

        .card:hover {
            transform: scale(1.02);
        }

        h1 {
            font-size: 36px;
            margin-bottom: 20px;
            color: #bb86fc;
        }

        p {
            font-size: 18px;
            margin-bottom: 35px;
            color: #e0e0e0;
        }

        .button {
            padding: 14px 34px;
            font-size: 18px;
            border: none;
            border-radius: 10px;
            background: linear-gradient(to right, #8e2de2, #4a00e0);
            color: white;
            text-decoration: none;
            font-weight: bold;
            cursor: pointer;
            transition: background 0.3s ease, transform 0.3s ease;
            box-shadow: 0 8px 15px rgba(0, 0, 0, 0.2);
        }

        .button:hover {
            background: linear-gradient(to right, #4a00e0, #8e2de2);
            transform: translateY(-2px);
        }

        @keyframes fadeIn {
            0% { opacity: 0; transform: scale(0.95); }
            100% { opacity: 1; transform: scale(1); }
        }
    </style>
</head>
<body>

    <div class="card">
        <h1>🩺 Skin Disease Classifier</h1>
        <p>Click below to analyze a  image and predict the type of skin disease.</p>
        <a href="/predict" class="button">🔍 Predict Disease</a>
    </div>

</body>
</html>

---
**dnnresult.html**
This page handles:
- Upload form
- Showing prediction results
- Displaying "Thank you" after exit

**Behavior:**
- When GET: shows upload form
- When POST: shows prediction + exit button
- When exit button clicked: shows blank page with thank you

- Example:
  <!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Prediction Result</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      background: linear-gradient(135deg, #6a11cb, #2575fc);
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      min-height: 100vh;
      color: #fff;
    }

    h1 {
      margin-top: 40px;
      color: #fff;
      font-size: 36px;
      text-shadow: 1px 1px 4px #000;
    }

    .card {
      background-color: #000;
      color: #fff;
      padding: 30px 35px;
      border-radius: 20px;
      text-align: center;
      box-shadow: 0 20px 50px rgba(0, 0, 0, 0.5);
      margin-top: 20px;
      width: 340px;
      animation: fadeIn 0.8s ease-in-out;
    }

    input[type="file"] {
      background-color: #fff;
      color: #000;
      padding: 10px;
      border-radius: 8px;
      font-size: 14px;
      margin-bottom: 10px;
      cursor: pointer;
    }

    .filename {
      font-size: 13px;
      color: #ccc;
      margin: 5px 0 15px;
      font-style: italic;
    }

    button {
      padding: 10px 25px;
      font-size: 15px;
      border: none;
      border-radius: 8px;
      background: linear-gradient(to right, #8e2de2, #4a00e0);
      color: white;
      font-weight: bold;
      margin: 8px 5px;
      cursor: pointer;
      transition: 0.3s;
    }

    button:hover {
      transform: translateY(-2px);
      background: linear-gradient(to right, #4a00e0, #8e2de2);
    }

    img {
      width: 280px;
      border-radius: 12px;
      margin: 15px 0;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.4);
    }

    .prediction {
      background-color: #fff;
      color: #0b3d91;
      padding: 14px;
      border-radius: 12px;
      font-size: 18px;
      font-weight: bold;
      margin-top: 15px;
      border: 2px solid #4a00e0;
      box-shadow: 0 4px 15px rgba(74, 0, 224, 0.4);
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: scale(0.95); }
      to { opacity: 1; transform: scale(1); }
    }

    footer {
      margin-top: 30px;
      font-size: 13px;
      color: #eee;
    }
  </style>

  <script>
    function handleExit() {
      document.open();
      document.write('<!DOCTYPE html><html><head><title>Exit</title><style>body{display:flex;justify-content:center;align-items:center;height:100vh;font-family:sans-serif;font-size:20px;background-color:#111;color:#0f0;}</style></head><body>✅ Prediction completed. Thank you!</body></html>');
      document.close();
    }

    function showFileName(input) {
      const fileInfo = document.getElementById('filename');
      fileInfo.textContent = input.files.length > 0 ? `Selected: ${input.files[0].name}` : '';
    }
  </script>
</head>

<body>
  <h1>🧪 Prediction Result</h1>

  <div class="card">
    <form action="{{ url_for('predict') }}" method="post" enctype="multipart/form-data">
      <input type="file" name="file" accept="image/*" onchange="showFileName(this)" required><br>
      <div id="filename" class="filename"></div>
      <button type="submit">🔍 Predict</button>
    </form>

    {% if image_file %}
      <img src="{{ url_for('static', filename=image_file) }}" alt="Predicted Image">
      <div class="prediction">🩺 Predicted Skin Disease: <span style="color:#4a00e0">{{ prediction }}</span></div>
    {% endif %}

    <button onclick="handleExit()">🚪 Exit</button>
  </div>

  <footer>
    &copy; 2025 Skin Classifier AI | Built with 💙
  </footer>
</body>
</html>

This template supports:
- Upload
- Showing results
- Displaying thank you message after exiting

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
✅ Step 1: Open dashboard (/) – welcome and start diagnosis button.
✅ Step 2: Go to upload page (/predict) – browse and upload image.
✅ Step 3: Prediction result displayed with image and exit button.
✅ Step 4: Click exit – shows thank you message.

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



   

# Weather Data Analysis and Rain Prediction

**MCA Mini Project**

This project studies past weather records and predicts whether it may rain tomorrow. It uses Python, Pandas, a Random Forest model written for this project, and a small Flask website.

## Student details

- **Student:** Mohammad Yusuf
- **Roll number:** Y26MC20061
- **Course:** Master of Computer Applications (MCA)
- **University:** Acharya Nagarjuna University, Guntur

## 1. What can I do in this project?

1. View a summary of the weather dataset.
2. See EDA charts (simple graphs that help us understand the data).
3. Enter weather details and get a Rain / No Rain result.
4. Try sample weather records from the dataset.
5. View model test scores and a confusion matrix.

**Please note:** The dataset's output column is `RainTomorrow`. So the current model predicts Rain or No Rain. It does not give a temperature number. A separate regression model would be needed for temperature prediction.

## 2. Project files

```text
Weather_Rain_Prediction_MCA/
│
├── app.py                         # Opens the Flask website
├── train.py                       # Trains and tests the model
├── weather_data_shuffled.csv      # Main dataset used by the project
├── model_results.json             # Model test results
├── requirements.txt               # Python packages
├── README.md                      # Project guide
├── start_project.bat              # Start the website on Windows
│
├── models/
│   ├── random_forest.py           # Decision Tree and Random Forest code
│   └── rain_prediction_model.pkl  # Trained model
│
├── templates/
│   ├── index.html                 # Main website page
│   └── field.html                 # Weather input field design
│
└── static/
    ├── style.css                  # Website design
    └── charts/                    # EDA graphs (created by app.py)
```

## 3. Dataset details

The main dataset is `weather_data_shuffled.csv`.

- Rows: **34,850**
- Columns: **21**
- Input features: **20**
- Output column: **RainTomorrow**
- No Rain (`0`): **20,910 records**
- Rain (`1`): **13,940 records**

The records are randomly shuffled. Shuffling changes the order of rows; it does not change the original values or class counts. The dataset is not forced to a 50:50 ratio.

### What are 0 and 1?

- `0` means No Rain tomorrow.
- `1` means Rain tomorrow.

### Main input features

- Temperature: `MinTemp`, `MaxTemp`, `Temp9am`, `Temp3pm`
- Rain and sunshine: `Rainfall`, `RainToday`, `Sunshine`, `Evaporation`
- Humidity and clouds: `Humidity9am`, `Humidity3pm`, `Cloud9am`, `Cloud3pm`
- Wind: `WindGustDir`, `WindGustspeed`, `WindDir9am`, `WindDir3pm`, `WindSpeed9am`, `WindSpeed3pm`
- Air pressure: `Pressure9am`, `Pressure3pm`

The three wind direction columns use numeric codes from the dataset. The original data does not provide a compass-direction lookup, so the website keeps the same codes used during model training.

## 4. How to run the project on Windows 10 or 11

### Before you start

1. Install **Python 3.10 or newer** from [python.org](https://www.python.org/downloads/).
2. During installation, select **Add Python to PATH**.
3. Download or copy this ZIP file and extract it. Do not run the project from inside the ZIP.
4. Open the extracted `weather-data-analyse-main` folder in File Explorer.
5. Open Git Bash or PowerShell in this project folder.

The project already includes the trained model. You normally do **not** need to run `train.py` before starting the website.

### Option A: Run using Git Bash

Install [Git for Windows](https://git-scm.com/download/win) if Git Bash is not already available.

1. Open the extracted project folder in File Explorer.
2. Right-click an empty area inside the folder and choose **Open Git Bash here**.
3. Check that Python is available:

```bash
python --version
```

If `python` is not recognised, try:

```bash
py --version
```

4. Create a virtual environment:

```bash
py -m venv .venv
```

If the `py` command is unavailable but `python` works, use `python -m venv .venv`.

5. Activate the environment:

```bash
source .venv/Scripts/activate
```

6. Install the required packages:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

7. Start the Flask website:

```bash
python app.py
```

8. Open this address in Chrome or Edge:

**http://127.0.0.1:5000**

Keep Git Bash open while using the website. To stop the server, press `Ctrl + C`.

### Option B: Run using Windows PowerShell

1. Open the extracted project folder in File Explorer.
2. Click the address bar, type `powershell`, and press Enter. PowerShell opens in that folder.
3. Check Python:

```powershell
py --version
```

4. Create the virtual environment:

```powershell
py -m venv .venv
```

5. Install packages using the virtual-environment Python directly:

```powershell
.\.venv\Scripts\python.exe -m pip install --upgrade pip
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

6. Start the Flask website:

```powershell
.\.venv\Scripts\python.exe app.py
```

7. Open **http://127.0.0.1:5000** in Chrome or Edge.

Keep PowerShell open while using the website. Press `Ctrl + C` to stop Flask.

**Important:** This PowerShell method does not require activating the virtual environment or changing the PowerShell execution policy.

### Option C: Quick start using the Windows batch file

You can also double-click `start_project.bat` in File Explorer. It creates/uses the virtual environment, installs the packages, and starts the website. Keep its Command Prompt window open while using the website.

### Optional: Train the model again

The trained model is already included. Only retrain if you change the dataset or intentionally want to create a new model.

- Git Bash (after activating `.venv`):

```bash
python train.py
```

- PowerShell:

```powershell
.\.venv\Scripts\python.exe train.py
```

Training updates `models/rain_prediction_model.pkl` and `model_results.json`.

## 5. How to use the website

1. **Home:** Check the number of records and weather inputs.
2. **Data Analysis:** Look at the four EDA graphs.
3. **Try Prediction:** Enter the six main weather values.
4. If you want, open **More weather details** to edit the other 14 inputs.
5. You can click **Load No Rain example** or **Load Rain example** to fill the form with a real dataset example.
6. Click **Predict Rain Tomorrow**.
7. Read the result: `0 = No Rain` or `1 = Rain`.

The extra 14 fields already have example values from the dataset. You do not have to open that section for a basic test. The model still receives all 20 values.

## 6. What is EDA?

EDA means Exploratory Data Analysis. It is the step where we study the dataset before building a machine-learning model.

The website shows:

- **Rain / No Rain chart:** Counts how many records belong to each class.
- **Rainfall chart:** Shows how rainfall values are distributed.
- **Humidity chart:** Compares afternoon humidity for Rain and No Rain records.
- **Correlation chart:** Shows which numeric features have a stronger relationship with `RainTomorrow`. Correlation does not mean that a feature causes rain.

## 7. How the Random Forest works

1. The dataset is separated into training data (80%) and testing data (20%).
2. Ten decision trees are trained using random samples of the training data.
3. Each tree uses Gini impurity to choose splits.
4. The trees make their own predictions.
5. The Random Forest uses majority voting to give the final result.
6. The model is tested using accuracy, precision, recall, F1 score and a confusion matrix.

The Random Forest code is written in `models/random_forest.py`. Scikit-learn is used for splitting the dataset and calculating evaluation scores; the model itself is our custom implementation.

## 8. Model results

The model included in this project was trained using `weather_data_shuffled.csv` with an 80:20 train/test split and `random_state=42`.

| Measure | Result from this training run |
|---|---:|
| Accuracy | 81.91% |
| Precision | 80.85% |
| Recall | 71.77% |
| F1 score | 76.04% |

These are results from one train/test split. They are not a promise that every new prediction will be correct.

## 9. Train the model again (optional)

The trained model is already included, so you can open the website without training again.

If you change the dataset or want to repeat training, open Command Prompt in the project folder and run:

```bat
.venv\Scripts\python.exe train.py
```

Or, after first-time setup, double-click `train_model.bat`. Training replaces `models/rain_prediction_model.pkl` and updates `model_results.json`.

## 10. Common problems

**Python is not recognised**

Install Python again and select **Add Python to PATH**. Close and reopen the terminal.

**A package is missing**

Run:

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

**The model file is missing**

Run `train.py` once to create it.

**Port 5000 is already in use**

Close the other program using port 5000, then start this project again.

---

**Project note:** This is an educational demonstration using historical weather records. It is not connected to a live weather station or official forecast service.

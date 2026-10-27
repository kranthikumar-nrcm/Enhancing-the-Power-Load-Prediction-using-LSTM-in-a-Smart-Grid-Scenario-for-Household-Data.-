# Enhancing-the-Power-Load-Prediction-using-LSTM-in-a-Smart-Grid-Scenario-for-Household-Data.-

# Power Quality Issues Prediction Using Machine Learning — Smart Grid

<div align="center">

![Django](https://img.shields.io/badge/Django-5.2.12-092E20?style=for-the-badge&logo=django&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-1.6+-F9AB00?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Random Forest](https://img.shields.io/badge/Random%20Forest-ML-4285F4?style=for-the-badge&logo=google-brain&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-3.10-0072B5?style=for-the-badge&logo=python&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-0.13-FF9900?style=for-the-badge&logo=python&logoColor=white)

</div>

---

## 📖 Overview

**Power Quality Issues Prediction Using Machine Learning** is a Django-based web platform that predicts power quality faults in smart grid household data using machine learning. The system analyzes electrical parameters — **RMS Voltage, Peak Voltage, THD (Total Harmonic Distortion), and Duration** — to classify power quality disturbances into five categories:

| Fault Type | Label Code | Description |
|------------|------------|-------------|
| **Normal** | 0 | Stable power supply within acceptable limits |
| **Voltage Sag** | 1 | Short-duration decrease in voltage |
| **Voltage Swell** | 2 | Short-duration increase in voltage |
| **Harmonics** | 3 | Distortions in the normal voltage waveform |
| **Transient** | 4 | Sudden, brief spike in voltage or current |

The platform features a **dual-role access model** (Admin & User), full user registration with **password hashing**, admin-controlled activation, a **multi-model training pipeline** (Random Forest, Gradient Boosting, SVM, Logistic Regression, KNN), an extensive **Exploratory Data Analysis (EDA)** module with 11+ charts, and a real-time **prediction interface**.

> **Note:** Despite the project title mentioning **LSTM**, the current implementation uses **traditional machine learning classifiers** (Random Forest achieved 99.37% accuracy). The LSTM approach appears to be the research intent documented in the Jupyter notebook.

---

## 🎯 Key Features

### Core ML Capabilities
- **Multi-Model Training:** Trains and compares 5 classifiers on the power quality dataset:
  - Random Forest (✅ Best: **99.37%** accuracy)
  - Gradient Boosting (**99.16%**)
  - Logistic Regression (**91.77%**)
  - SVM (**87.34%**)
  - KNN (**84.60%**)
- **Best Model Persistence:** The top-performing model and label encoder are saved as `.pkl` files in `media/models/`
- **Real-Time Prediction:** Users input RMS Voltage, Peak Voltage, THD, and Duration to get instant fault type predictions with human-readable descriptions
- **Comprehensive EDA:** 11 visualizations generated on-the-fly:
  - Fault Type Distribution (bar + pie)
  - Avg RMS Voltage by Fault Type (bar)
  - Avg THD by Fault Type (bar)
  - Phase Distribution (pie)
  - RMS Voltage Boxplot per Fault
  - Feature Correlation Heatmap
  - Duration Distribution per Fault (histogram)
  - RMS Voltage Distribution (histogram + KDE)
  - Peak Voltage Distribution (histogram + KDE)
  - THD Distribution (histogram + KDE)
  - Duration Distribution (histogram + KDE)
- **Statistical Summary Table:** Min, Max, Mean, Std Dev for all numerical features

### User Features
- Register with name, login ID, password (hashed), mobile, email, locality, address, city, state
- Login with credentials (account must be activated by admin)
- Access the **Training** page to train models and view accuracy comparison
- Access the **EDA** page to view all exploratory data analysis charts and statistics
- Access the **Prediction** page to predict fault types from electrical parameters
- Logout functionality with session flush

### Admin Features
- Admin login (default: `admin`/`admin` or `Admin`/`Admin`)
- Session-based authentication with guard checks on all admin routes
- View dashboard with total users, active users, and pending approvals count
- View all registered users in a styled table
- Activate or delete users
- Admin logout with session flush

---

## 🏗️ Tech Stack

| Component | Technology |
|-----------|------------|
| **Backend** | Python 3.11, Django 5.2.12 |
| **ML Framework** | Scikit-learn 1.6+ (Random Forest, Gradient Boosting, SVM, Logistic Regression, KNN) |
| **Data Handling** | Pandas, NumPy |
| **Visualization** | Matplotlib (Agg backend), Seaborn |
| **Model Persistence** | Joblib (`.pkl` files) |
| **Database** | SQLite3 |
| **Frontend** | HTML5, CSS3 (glassmorphism), Bootstrap 5.3.2 (EDA + Training pages), Google Fonts (Inter) |
| **Auth** | Django sessions + `make_password`/`check_password` hashing |
| **Background** | Custom CSS glassmorphism with `backdrop-filter: blur()` |

### Key Files in `media/`
| Path | Contents |
|------|----------|
| `power_quality_fault_dataset.csv` | 500+ records, 12 columns: ID, Fault_Type, Phase, RMS_Voltage, Peak_Voltage, THD, Duration_ms, DWT_Energy_Level1, DWT_Energy_Level2, DWT_Entropy, Signal_Noise_Ratio_dB, Label |
| `models/power_quality_model.pkl` | Best trained model (Random Forest, saved after training) |
| `models/label_encoder.pkl` | LabelEncoder mapping fault type strings to integers |
| `eda_charts/*.png` | 11 EDA charts generated on-demand by the EDA view |
| `graphs/*.png` | Duplicate copies of fault_bar.png, heatmap.png, rms_voltage.png, peak_voltage.png, thd.png, duration.png |

---

## 📁 Project Structure

```
CODE/
├── manage.py                        # Django CLI entry point
├── vishnus.ipynb                    # Jupyter notebook: ML training & evaluation (reference)
├── db.sqlite3                       # SQLite database
│
├── PowerQualityPrediction/         # Main Django project
│   ├── settings.py                 # Django 5.2 settings, template/static dirs, media config
│   ├── urls.py                     # All URL routing (main, admin, user)
│   ├── views.py                    # Landing page, login/register form renderers
│   ├── asgi.py
│   ├── wsgi.py
│   └── __init__.py
│
├── Admins/                          # Admin app
│   ├── models.py                   # (empty — no custom admin models)
│   ├── views.py                    # AdminLoginCheck, AdminHome, ViewRegisteredUsers, AdminActivaUsers, DeleteUsers, AdminLogout
│   ├── admin.py
│   ├── apps.py
│   └── migrations/
│
├── Users/                           # User app (core ML logic lives here)
│   ├── models.py                   # UserRegistrationModel, Dataset
│   ├── forms.py                    # UserRegistrationForm with validation patterns
│   ├── views.py                    # Auth, UserHome, UserLogout, training(), eda(), prediction()
│   ├── admin.py
│   ├── apps.py
│   └── migrations/
│
├── templates/                       # All Django templates (shared dir)
│   ├── base.html                   # Master template: glassmorphism navbar, background image, footer
│   ├── index.html                  # Landing page (minimal)
│   ├── AdminLogin.html             # Admin login (glass card)
│   ├── UserLogin.html              # User login (glass card)
│   ├── UserRegistrations.html      # Registration form (glass card, custom CSS)
│   ├── admins/
│   │   ├── adminbase.html          # Admin base layout (extends base.html)
│   │   ├── AdminHome.html          # Admin dashboard (welcome card + stats)
│   │   └── RegisteredUsers.html    # User list table (glassmorphism, activate/delete buttons)
│   └── users/
│       ├── userbase.html           # User base layout (extends base.html)
│       ├── UserHome.html           # User dashboard (hero + 4 feature cards, full HTML document)
│       ├── training.html           # Model training results (Bootstrap 5 cards + accuracy table)
│       ├── eda.html                # EDA dashboard (11 charts + stat cards + stats table)
│       └── prediction.html         # Fault prediction form + result card (glassmorphism)
│
├── static/
│   └── imgs/
│       └── pw.png                   # Background image used in base.html and UserHome.html
│
└── media/
    ├── power_quality_fault_dataset.csv  # Main dataset (500+ rows, 12 columns)
    ├── models/
    │   ├── power_quality_model.pkl      # Best ML model (Random Forest)
    │   └── label_encoder.pkl            # Fault type label encoder
    ├── eda_charts/                      # Generated EDA chart PNGs
    └── graphs/                          # Duplicate chart copies
```

---

## 🚀 Installation & Setup

### Prerequisites
- Python 3.10 or higher
- pip (Python package manager)

### Step 1: Navigate to Project
```bash
cd "D:/AIML C/B12/110. Enhancing the Power Load Prediction using LSTM in a Smart Grid Scenario for Household Data/CODE"
```

### Step 2: Create & Activate Virtual Environment
```bash
python -m venv venv
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate
```

### Step 3: Install Dependencies
```bash
pip install django scikit-learn pandas numpy matplotlib seaborn joblib
```

### Step 4: Run Migrations
```bash
python manage.py migrate
```

### Step 5: (Optional) Create a Django Superuser
```bash
python manage.py createsuperuser
```

### Step 6: Run the Development Server
```bash
python manage.py runserver
```

Visit **[http://127.0.0.1:8000](http://127.0.0.1:8000)** in your browser.

---

## 🔐 Default Credentials

| Role   | Username      | Password   |
|--------|---------------|------------|
| Admin  | `admin`       | `admin`    |
| Admin  | `Admin`       | `Admin`    |

> Users must register and be **activated by the admin** before they can log in.
> Passwords are **hashed** using Django's `make_password` before storage.

---

## 🔄 How It Works

### Training Pipeline

```
┌──────────────────────────────────────┐
│  GET/POST /training/                 │
│  training() view                     │
└──────────┬───────────────────────────┘
           │
           ▼
┌──────────────────────────────────────┐
│  Load dataset                        │
│  media/power_quality_fault_dataset.csv│
│  - Drop NaN values                    │
│  - Drop duplicates                    │
└──────────┬───────────────────────────┘
           │
           ▼
┌──────────────────────────────────────┐
│  Feature Selection                   │
│  X = [RMS_Voltage, Peak_Voltage,     │
│       THD, Duration_ms]              │
│  y = Fault_Type (5 classes)          │
│  LabelEncoder: Normal=0, Sag=1,      │
│    Swell=2, Harmonics=3, Transient=4 │
└──────────┬───────────────────────────┘
           │
           ▼
┌──────────────────────────────────────┐
│  Train/Test Split (80/20)            │
│  random_state=42                     │
└──────────┬───────────────────────────┘
           │
           ▼
┌──────────────────────────────────────┐
│  Train 5 Models                      │
│  ┌──────────────────────────────┐    │
│  │ Random Forest    → 99.37% ✅ │    │
│  │ Gradient Boosting → 99.16%   │    │
│  │ Logistic Regression → 91.77% │    │
│  │ SVM               → 87.34%   │    │
│  │ KNN               → 84.60%   │    │
│  └──────────────────────────────┘    │
└──────────┬───────────────────────────┘
           │
           ▼
┌──────────────────────────────────────┐
│  Save best model + encoder           │
│  media/models/power_quality_model.pkl│
│  media/models/label_encoder.pkl      │
└──────────┬───────────────────────────┘
           │
           ▼
┌──────────────────────────────────────┐
│  Render training.html                │
│  - Stats cards (records, columns,    │
│    best model)                       │
│  - Accuracy comparison table         │
│  - Best accuracy badge               │
└──────────────────────────────────────┘
```

### Prediction Pipeline

```
┌──────────────────────────────────────┐
│  User enters 4 parameters            │
│  - RMS Voltage (e.g., 229.83)        │
│  - Peak Voltage (e.g., 325.02)       │
│  - THD (e.g., 1.70)                  │
│  - Duration (e.g., 0.25)             │
└──────────┬───────────────────────────┘
           │
           ▼
┌──────────────────────────────────────┐
│  POST /prediction/                   │
│  prediction() view                   │
└──────────┬───────────────────────────┘
           │
           ▼
┌──────────────────────────────────────┐
│  Load model + encoder                │
│  joblib.load(power_quality_model.pkl)│
│  joblib.load(label_encoder.pkl)      │
└──────────┬───────────────────────────┘
           │
           ▼
┌──────────────────────────────────────┐
│  Predict                             │
│  model.predict([[rms, peak, thd,    │
│                 duration]])          │
│  encoder.inverse_transform() → label │
└──────────┬───────────────────────────┘
           │
           ▼
┌──────────────────────────────────────┐
│  Map to description                  │
│  Normal     → "Power supply stable…" │
│  Sag        → "Voltage decrease…"    │
│  Swell      → "Voltage increase…"    │
│  Harmonics  → "Waveform distortion…" │
│  Transient  → "Sudden spike…"        │
└──────────┬───────────────────────────┘
           │
           ▼
┌──────────────────────────────────────┐
│  Render prediction.html              │
│  Green result card with fault type   │
│  and human-readable description      │
└──────────────────────────────────────┘
```

### EDA Pipeline

```
┌──────────────────────────────────────┐
│  GET /eda/                           │
│  eda() view                          │
└──────────┬───────────────────────────┘
           │
           ▼
┌──────────────────────────────────────┐
│  Load dataset                        │
│  Compute statistics:                 │
│  - Total records, features, classes  │
│  - Missing values, duplicates        │
│  - Feature stats (min/max/mean/std)  │
└──────────┬───────────────────────────┘
           │
           ▼
┌──────────────────────────────────────┐
│  Generate 11 charts                  │
│  Save to media/eda_charts/          │
│  Also copy to media/graphs/          │
│  - fault_bar.png                     │
│  - fault_pie.png                     │
│  - rms_bar.png                       │
│  - phase_pie.png                     │
│  - thd_bar.png                       │
│  - rms_box.png                       │
│  - heatmap.png                       │
│  - duration_hist.png                 │
│  - rms_voltage.png                   │
│  - peak_voltage.png                  │
│  - thd.png                           │
│  - duration.png                      │
└──────────┬───────────────────────────┘
           │
           ▼
┌──────────────────────────────────────┐
│  Render eda.html                     │
│  - 4 stat cards (records/features/   │
│    classes/missing)                  │
│  - 6 chart rows (11 charts total)    │
│  - Feature statistics table          │
└──────────────────────────────────────┘
```

### Jupyter Notebook (`vishnus.ipynb`)

The notebook contains a single executed code cell that:
1. Loads and displays the dataset
2. Trains all 5 models (Random Forest, Gradient Boosting, SVM, KNN, Logistic Regression)
3. Prints full classification reports and confusion matrices for each
4. Identifies **Random Forest as the best model at 99.37% accuracy**
5. Saves the model

> The notebook uses the same 4 features (RMS_Voltage, Peak_Voltage, THD, Duration_ms) and the same 5-class target as the Django app.

---

## 📡 API & URL Endpoints

| Method   | URL                          | View                      | Description                          |
|----------|------------------------------|---------------------------|--------------------------------------|
| GET      | `/`                          | `index`                   | Landing page                         |
| GET      | `/index/`                    | `index`                   | Landing page (alternate)            |
| GET      | `/AdminLogin/`               | `AdminLogin`              | Admin login form                     |
| GET      | `/UserLogin/`                | `UserLogin`               | User login form                      |
| GET      | `/UserRegister/`             | `UserRegister`            | User registration form               |
| POST     | `/AdminLoginCheck/`          | `AdminLoginCheck`         | Admin authentication (session-based) |
| GET      | `/AdminHome/`                | `AdminHome`               | Admin dashboard (stats: total/active/pending) |
| GET      | `/ViewRegisteredUsers/`      | `ViewRegisteredUsers`     | User list table (admin, session-guarded) |
| GET      | `/AdminActivaUsers/?uid=<id>`| `AdminActivaUsers`        | Activate user by ID (admin only)    |
| GET      | `/DeleteUsers/?uid=<id>`     | `DeleteUsers`             | Delete user by ID (admin only)      |
| GET      | `/AdminLogout/`              | `AdminLogout`             | Admin logout (session flush)        |
| POST     | `/UserRegisterActions/`      | `UserRegisterActions`     | Handle registration (password hashed) |
| POST     | `/UserLoginCheck/`           | `UserLoginCheck`          | Handle user login (password verified via `check_password`) |
| GET      | `/UserHome/`                 | `UserHome`                | User dashboard (session-guarded)    |
| GET      | `/UserLogout/`               | `UserLogout`              | User logout (session flush)         |
| GET/POST | `/training/`                 | `training`                | Train 5 ML models + view accuracy   |
| GET/POST | `/prediction/`               | `prediction`              | Predict fault type from 4 parameters |
| GET      | `/eda/`                      | `eda`                     | Exploratory Data Analysis (11 charts + stats) |

---

## 📊 Dataset

### `power_quality_fault_dataset.csv`

| Column | Type | Description |
|--------|------|-------------|
| `ID` | int | Row identifier (1–500+) |
| `Fault_Type` | string | Target: Normal, Sag, Swell, Harmonics, Transient |
| `Phase` | string | Electrical phase: A, B, or C |
| `RMS_Voltage` | float | Root Mean Square voltage (V) |
| `Peak_Voltage` | float | Peak voltage (V) |
| `THD` | float | Total Harmonic Distortion (%) |
| `Duration_ms` | int | Fault duration in milliseconds |
| `DWT_Energy_Level1` | float | Discrete Wavelet Transform energy level 1 |
| `DWT_Energy_Level2` | float | Discrete Wavelet Transform energy level 2 |
| `DWT_Entropy` | float | DWT entropy |
| `Signal_Noise_Ratio_dB` | float | Signal-to-noise ratio in dB |
| `Label` | int | Numeric encoding of Fault_Type (0–4) |

**Dataset statistics (from EDA):**
- ~500 records
- 12 columns (11 features + 1 target)
- 5 fault classes
- Balanced distribution across classes
- No missing values after cleaning

### Feature-Target Relationship

| Fault Type | Typical RMS Voltage | Typical THD | Label |
|------------|--------------------|-------------|-------|
| Normal | ~229–235V | ~1.5–2.5% | 0 |
| Sag | ~135–175V | ~1.5–5.0% | 1 |
| Swell | ~250–290V | ~2.0–5.0% | 2 |
| Harmonics | ~225–238V | ~6.0–16.7% | 3 |
| Transient | ~220–240V | ~3.0–8.0% | 4 |

---

## 🎨 UI/UX Design

The interface uses a **glassmorphism dark theme**:

| Element | Detail |
|---------|--------|
| **Background** | `static/imgs/pw.png` — full-screen background image with dark overlay (`rgba(0,0,0,0.55)`) |
| **Navbar** | Sticky, semi-transparent white (`rgba(255,255,255,0.1)`), blur(12px), rounded links with blue hover |
| **Glass cards** | `rgba(255,255,255,0.1)` background, `backdrop-filter: blur(10-18px)`, white border, box shadow |
| **Buttons** | Gradient blue (`#00c6ff` → `#0072ff`) for primary, red gradient for danger/reset |
| **Typography** | Segoe UI / Inter (Google Fonts on EDA + Prediction pages) |
| **Color accents** | Blue (`#0072ff`, `#1e88e5`), cyan (`#00bcd4`), green for success, red for danger |
| **Animations** | Hover transforms (translateY, scale), fade-in keyframes, slide-up result animation |
| **Responsive** | Media queries at 768px and 900px for mobile layouts |

**Page-specific styling:**
- **Login/Register:** Centered glass cards, form validation with green/red border feedback
- **Admin dashboard:** Stat cards with glassmorphism, user table with gradient header and status badges
- **Training page:** Bootstrap 5 cards with gradient headers, accuracy comparison table with green highlight for best model
- **EDA page:** Full Bootstrap 5 dashboard with stat cards, chart cards with hover lift, feature stats table
- **Prediction page:** White glass card on dark background, input fields with blue focus ring, green result card with slide-up animation

---

## ⚠️ Security Notes

- **Passwords are hashed** using Django's `make_password` / `check_password` — good practice ✅
- **Session-based auth** with guards on admin and user routes — admin routes check `admin_logged_in` session flag
- **`DEBUG = True`** in settings — set to `False` for production
- **`ALLOWED_HOSTS = []`** — configure for production deployment
- **CSRF protection** is enabled via Django middleware
- The `Dataset` model in `users/models.py` allows file uploads but is not currently used in any view
- Background image path (`static/imgs/pw.png`) must exist for proper UI rendering

---

## 🛠️ Key Implementation Details

### Password Hashing
```python
# Registration: hash before save
user.password = make_password(form.cleaned_data['password'])
user.save()

# Login: verify hashed password
if check_password(pswd, check.password):
    # proceed with login
```

### Model Training & Selection
```python
models = {
    "Random Forest": RandomForestClassifier(n_estimators=100, random_state=42),
    "Gradient Boosting": GradientBoostingClassifier(random_state=42),
    "SVM": SVC(),
    "Logistic Regression": LogisticRegression(max_iter=1000),
}

# Train all, compare accuracy, save best
for name, model in models.items():
    model.fit(X_train, y_train)
    pred = model.predict(X_test)
    acc = accuracy_score(y_test, pred) * 100
    if acc > best_accuracy:
        best_model = model
        best_name = name

joblib.dump(best_model, "media/models/power_quality_model.pkl")
joblib.dump(encoder, "media/models/label_encoder.pkl")
```

### Prediction Input Features
The prediction uses only **4 of the 12 dataset columns**:
```python
X = df[['RMS_Voltage', 'Peak_Voltage', 'THD', 'Duration_ms']]
```
The other 8 columns (Phase, DWT_Energy_Level1/2, DWT_Entropy, Signal_Noise_Ratio_dB, Label, ID) are **not used** for training or prediction in the Django app.

### EDA Chart Generation
All 11 charts are generated dynamically on each EDA page load using `matplotlib` (Agg backend) and saved as PNG files. Charts use a consistent color palette:
```python
COLORS = ['#1e88e5', '#43a047', '#fb8c00', '#e53935', '#8e24aa']
FAULT_ORDER = ['Normal', 'Sag', 'Swell', 'Harmonics', 'Transient']
```

### Session Management
- **Admin:** `request.session['admin_logged_in'] = True`, checked via `request.session.get('admin_logged_in')`
- **User:** `request.session['id']`, `request.session['loggeduser']`, checked via `'id' not in request.session`
- **Logout:** `request.session.flush()` on both admin and user logout

---

## 🌱 Future Enhancements

- [ ] **Implement actual LSTM model** as mentioned in the project title — currently only traditional ML classifiers are used
- [ ] **Include all 12 features** in training/prediction (currently only 4 are used; DWT features, SNR, Phase could improve accuracy)
- [ ] **Add model versioning** — compare LSTM vs Random Forest vs other deep learning models
- [ ] **Add confusion matrix visualization** to the training results page
- [ ] **Add real-time data stream simulation** for live power quality monitoring
- [ ] **Add user prediction history** — save past predictions per user
- [ ] **Add email notifications** for admin when new users register
- [ ] **Production deployment:** PostgreSQL + Gunicorn + Nginx, set `DEBUG=False`, configure `ALLOWED_HOSTS`
- [ ] **Security:** Add rate limiting on login attempts, implement proper password reset flow
- [ ] **Dataset management:** Allow users/admins to upload new datasets via the `Dataset` model
- [ ] **API layer:** Add Django REST Framework for headless prediction API
- [ ] **Dashboard improvements:** Add trend charts, historical fault analysis, per-phase breakdown

---

## 📄 License

This project is developed for academic/research purposes in the domain of smart grid power quality prediction.

---

## ✍️ Acknowledgments

- **Scikit-learn** for ML classifiers and evaluation metrics
- **Pandas & NumPy** for data manipulation
- **Matplotlib & Seaborn** for comprehensive EDA visualizations
- **Django** for the web application framework with session-based authentication
- **Jupyter Notebook** for the initial ML experimentation and model comparison

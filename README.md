# ASR Using Whisper (Arabic)

A simple end-to-end project demonstrating **Arabic Automatic Speech Recognition (ASR)** using **OpenAI’s Whisper** model. The final interface is a **Flask** web app (with an HTML/JS frontend) to test the model via **file uploads** or **microphone recording**, and it’s suitable for running on **Google Colab** with **ngrok** for public access.

## Table of Contents
1. [Overview](#overview)
2. [Features](#features)
3. [Demo (Screenshots)](#demo)
4. [Installation & Setup](#installation)
5. [Model Usage](#model-usage)
6. [Run the Web Interface](#run-the-web-interface)
7. [References](#references)
8. [License](#license)

---

<a name="overview"></a>
## 1. Overview

- **Goal**: Provide a user-friendly **Arabic speech recognition** tool based on **Whisper**.
- **Environment**: Designed for **Google Colab** (but can be adapted locally if you have a GPU and the same packages installed).
- **Approach**:
  1. **Train or Fine-tune** Whisper model for Arabic (optionally).
  2. **Load** the pretrained model from Google Drive or a local directory.
  3. **Serve** a simple Flask website that transcribes uploaded/recorded audio.

---

<a name="features"></a>
## 2. Features

- **Upload Audio** (.mp3, .wav) or **Record** via browser mic.
- Real-time display of **transcription** in Arabic.
- Minimal external dependencies: `Flask`, `pyngrok`, `transformers`, `librosa`, `pydub`, etc.
- Easy to **share** by generating a public URL via **ngrok** in Colab.

---

<a name="demo"></a>
## 3. Demo (Screenshots)

*(Optional: Include screenshots showing the Colab environment, the website UI, and sample transcriptions.)*

---

<a name="installation"></a>
## 4. Installation & Setup

### 4.1 Clone or Download This Repo

```bash
git clone https://github.com/YourUsername/arabic-whisper-asr.git
cd arabic-whisper-asr
```

### 4.2 Environment

- **Google Colab** is highly recommended for easy GPU usage.  
- Alternatively, create a local conda/virtual environment with Python 3.8+ and install dependencies.

#### 4.2.1 For Google Colab

1. Open the `.ipynb` notebook (or create a new one) in Colab.  
2. Run cells to install dependencies (see below or your notebook code).

#### 4.2.2 Dependencies

Within Colab or locally:

```bash
pip install -r requirements.txt
```

If you don’t have a `requirements.txt`, you can install packages manually:
```bash
pip install flask pyngrok pydub librosa transformers
```

---

<a name="model-usage"></a>
## 5. Model Usage

### 5.1 Using a Pretrained Model

We provide a **trained Whisper model** (for Arabic) stored on Google Drive:

- **Link**: [Google Drive Folder](https://drive.google.com/drive/folders/1BGmT2ZsGCWkXhMQYHPUTx7qCcFpq53xs?usp=drive_link)

**Steps** in Colab:

```python
from google.colab import drive
drive.mount('/content/drive')

import shutil
import os

# Path in Drive where model is stored
model_dir = "/content/drive/MyDrive/ASR"
local_model_dir = "./model"

# Copy the model to local directory
shutil.copytree(model_dir, local_model_dir)
print(f"Model copied to {local_model_dir}")
```

### 5.2 Optional: Re-train or Fine-tune

If you prefer to **fine-tune** on another dataset or re-train:
1. You’ll have a separate training notebook with Whisper & HF datasets.  
2. **Mount** Drive, train, and then **save** the trained model similarly.

---

<a name="run-the-web-interface"></a>
## 6. Run the Web Interface

Below is the typical **workflow** in Google Colab:

1. **Install** packages & create `templates` and `static` folders:
   ```python
   !pip install flask pyngrok pydub librosa transformers
   !mkdir templates static
   ```
2. **Place** the provided `app.py` (or the code snippet) in the same directory. This contains:
   - The HTML/JS in `templates/index.html`
   - The CSS in `static/styles.css`
   - The Flask endpoints
3. **Load** your model from `./model`.
4. **Start** the Flask app and open the ngrok tunnel:
   ```python
   from pyngrok import ngrok
   # ...
   public_url = ngrok.connect(5000)
   app.run(port=5000)
   print("Public URL:", public_url)
   ```
5. **Click** the `public_url` to open your website.  
6. **Test**:
   - Click “Record” or upload an audio file (Arabic).  
   - The transcript appears after analysis.

*(For reference, see the example code cells in your `Colab` or in `app.py`.)*

---

<a name="references"></a>
## 7. References

1. **A Comparative Study of LLM-based ASR and Whisper in Low Resource and Code Switching Scenario**  
   [arXiv:2412.00721v1](https://arxiv.org/html/2412.00721v1#bib.bib19)

2. **A Transfer Learning End-to-End Arabic Text-To-Speech (TTS) Deep Architecture**  
   [(PDF) ResearchGate Link](https://www.researchgate.net/publication/344018193_A_Transfer_Learning_End-to-End_Arabic_Text-To-Speech_TTS_Deep_Architecture)

*(These inspired the approach for Arabic speech tasks.)*

---

<a name="license"></a>
## 8. License

*(Include any license you’re using, e.g., MIT, Apache 2.0, etc.)*

```
MIT License

Copyright (c) 2024 ...

Permission is hereby granted, free of charge, ...
```

*(Adjust license text as needed.)*

---

## That’s It!

By following the instructions above, anyone (e.g., your supervisor, classmates) can:

1. **Clone** this repo or open your Colab notebook.
2. **Mount** Google Drive and copy the pretrained Whisper model.
3. **Run** the Flask + ngrok code.
4. **Access** the ASR website at the provided public URL and test **Arabic speech recognition** in real time.

Feel free to reach out if you have any questions or suggestions for improvements!

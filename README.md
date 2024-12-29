![Alt text describing the image](Hamza-Waseem-Nasser/
arabic_ai_project/images)

# ASR Using Whisper (Arabic) — Simple Instructions

## 1. Overview
This project demonstrates **Arabic Speech Recognition** using OpenAI’s **Whisper** model. You can **retrain** the model (using Colab and your dataset) or **use** the already-trained model stored in **Google Drive**. The final system is a **Flask**-based website that lets users record or upload audio for transcription in Arabic.

## 2. Running the Project

### A. Retraining or Fine-tuning the Model (Optional)
1. **Mount Google Drive** in your Colab notebook:
   ```python
   from google.colab import drive
   drive.mount('/content/drive')
   ```
2. **Train** or **fine-tune** your Whisper model:
   ```python
   # (example) trainer.train()
   # after training, save the model:
   trainer.save_model(training_args.output_dir)
   processor.save_pretrained(training_args.output_dir)
   ```
3. **Copy** the trained model to Google Drive:
   ```python
   import shutil, os
   drive_output_dir = "/content/drive/MyDrive/whisper-small-hi"

   # Remove old directory if exists
   if os.path.exists(drive_output_dir):
       shutil.rmtree(drive_output_dir)

   shutil.copytree(training_args.output_dir, drive_output_dir)
   print(f"Model saved at {drive_output_dir}")
   ```
> **In most cases**, you won’t need to do this again unless you want to **update** or **retrain** the model.

### B. Using the Already-Trained Model
If you do **not** plan to retrain, you can **use** the model from this link (example):  
**[Google Drive Link](https://drive.google.com/drive/folders/1BGmT2ZsGCWkXhMQYHPUTx7qCcFpq53xs?usp=drive_link)**

1. **Mount Google Drive** in Colab:
   ```python
   from google.colab import drive
   drive.mount('/content/drive')
   ```
2. **Copy** the model from Drive to your local environment (e.g. `./model`):
   ```python
   import shutil, os

   drive_model_dir = "/content/drive/MyDrive/ASR"
   local_model_dir = "./model"
   shutil.copytree(drive_model_dir, local_model_dir)
   print(f"Model copied to {local_model_dir}")
   ```
> Now your model is ready to be loaded by the Flask code.

---

## 3. Running the Flask Website
1. **Install** dependencies (in Colab):
   ```python
   !pip install pydub pyngrok transformers librosa flask
   !mkdir templates static
   ```
2. **Create** your `templates/index.html` and `static/styles.css` (the HTML and CSS are already provided in the code snippet).
3. **Load** the model in your Flask app (example snippet):
   ```python
   from transformers import WhisperProcessor, WhisperForConditionalGeneration
   processor = WhisperProcessor.from_pretrained("model/")
   model = WhisperForConditionalGeneration.from_pretrained("model/")
   ```
4. **Run** the Flask code (the same code you provided in your notebook):
   ```python
   # Start ngrok
   from pyngrok import ngrok
   public_url = ngrok.connect(5000)
   print("Public URL:", public_url)

   # Launch the app
   app.run(port=5000)
   ```
5. **Open** the `public_url` in your browser. You should see the Arabic ASR interface:
   - **Record** your voice or **upload** an audio file.
   - The webpage displays the **transcription** in Arabic after processing.
![Speech Recognition Project Screenshot](images/image1.jpg)

![Flask ngrok Tunnel Screenshot](images/image2.jpg)
images/image1.jpg
---

## 4. Additional Notes
- **Recording**: Make sure your browser allows microphone access.  
- **File Upload**: Supports `.mp3` and `.wav`.  
- **Colab GPU** recommended for faster transcriptions.

---

## 5. Related Papers & Inspiration
1. **A Comparative Study of LLM-based ASR and Whisper in Low Resource and Code Switching Scenario**  
   [arxiv.org/html/2412.00721v1#bib.bib19](https://arxiv.org/html/2412.00721v1#bib.bib19)  

2. **A Transfer Learning End-to-End Arabic Text-To-Speech (TTS) Deep Architecture**  
   [ResearchGate Link](https://www.researchgate.net/publication/344018193_A_Transfer_Learning_End-to-End_Arabic_Text-To-Speech_TTS_Deep_Architecture)

*(These guided some architecture choices and gave insight into Arabic speech tasks.)*

---

### That’s it!
This **simple** approach lets anyone **try** your Arabic ASR system:

1. **Mount** Google Drive for the pretrained model.  
2. **Install** the necessary packages.  
3. **Run** the Flask + ngrok code to launch your web interface.  
4. **Open** the public URL to transcribe Arabic speech.  

If you have any **questions** or **suggestions**, feel free to update or improve the code. Good luck!

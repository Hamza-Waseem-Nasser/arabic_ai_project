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

![Flask ngrok Tunnel Screenshot](images/image2.jpg)
![Speech Recognition Project Screenshot](images/image1.jpg)



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




Below is a **simple** README explanation for **TTS Using Amazon Polly** in **Google Colab**, referencing the code you provided and guiding the user step by step. This follows a similar style to your ASR README, so your supervisor or anyone can easily run the project and test the website.

---

# **TTS Using Amazon Polly — Simple Instructions**

## 1. Overview
This project demonstrates **Arabic Text-to-Speech** (TTS) using **Amazon Polly**. It provides a **Flask**-based web interface (HTML/JS) for users to **input** Arabic text and **generate** an MP3 file on the fly, served from Google Colab with **ngrok**.  

## 2. Prerequisites
1. **Google Colab** for easy environment setup.  
2. **AWS Credentials** with access to **Amazon Polly** (so your code can call Polly’s API).  
3. **ngrok** for creating a public URL to your local Flask server.

---

## 3. Setup & Steps

### A. AWS CLI & Credentials
1. In Colab, install packages:
   ```bash
   !pip install boto3 awscli torch torchaudio
   ```
2. Run `!aws configure`, then enter:
   ```
   AWS Access Key ID [None]: YOUR_KEY
   AWS Secret Access Key [None]: YOUR_SECRET
   Default region name [None]: us-east-1
   Default output format [None]: json
   ```
   *This writes your credentials to `/root/.aws/credentials`.*
3. (Optional) Check available Polly voices:
   ```bash
   !aws polly list-voices --region us-east-1
   ```

### B. Generate TTS in Python (Without Website)
You can quickly test with:
```python
import boto3, os

def generate_arabic_tts(text, output_file="arabic_output.mp3", voice_id="Zeina"):
    polly = boto3.client("polly", region_name=os.getenv("AWS_DEFAULT_REGION", "us-east-1"))
    response = polly.synthesize_speech(
        Text=text,
        OutputFormat="mp3",
        VoiceId=voice_id,
        LanguageCode="ar-SA"
    )
    
    if "AudioStream" in response:
        with open(output_file, "wb") as file:
            file.write(response["AudioStream"].read())
        print(f"TTS generated -> {output_file}")
    else:
        raise Exception("Polly did not return an audio stream.")

sample_text = "مرحباً. هذا اختبار لنطق اللغة العربية باستخدام أمازون بولي."
output_filename = "arabic_output.mp3"
generate_arabic_tts(sample_text, output_filename)
```
Then you can **play** the MP3 inside Colab:
```python
from IPython.display import Audio
Audio(output_filename)
```

### C. Save or Copy the MP3 to Google Drive (Optional)
```python
from google.colab import drive
drive.mount("/content/drive")

!cp arabic_output.mp3 /content/drive/MyDrive/test_arabic.mp3
print("File copied to Google Drive.")
```

---

## 4. Running the Flask Website with Polly

1. **Install** dependencies & create directories:
   ```bash
   !pip install pyngrok flask boto3
   !mkdir templates static
   ```

2. **Set** your `ngrok` authtoken:
   ```bash
   !./ngrok authtoken YOUR_NGROK_TOKEN
   ```
3. **Prepare** HTML/CSS in `templates/index.html` and `static/` (already provided in the code snippet).
4. **Define** the Flask app (see your code snippet). It:
   - Renders `index.html` at `/`.  
   - POSTs to `/` with Arabic text, calls Polly, and returns **base64** MP3 data.  
5. **Run** the app & start ngrok:
   ```python
   # In your final code cell:
   from pyngrok import ngrok
   # ...
   public_url = ngrok.connect(5000)
   print(" * ngrok tunnel available at:", public_url)
   app.run(port=5000)
   ```
6. **Open** the displayed `public_url` in a browser.  
7. **Type** Arabic text into the website, hit “Generate Audio.”  
8. **Listen** to the result in the integrated player or **download** it.

---

## 5. Usage Tips

- **AWS Credentials**:  
  - The user must have run `!aws configure` with valid Polly permissions (Free Tier or full IAM policy).  
- **Voice Selection**:  
  - For Arabic, the main options are “Zeina” or “Hala.” (You can change `voice_id="Hala"` in your code.)  
- **Character Limit**:  
  - Polly has a limit (~3000 characters) for a single request. If you exceed it, consider splitting text or using SSML.  
- **Saving to Drive**:  
  - The website returns base64 audio, you can also store the final MP3 to Drive or host it if you want persistent access.

---

## 6. Example Testing Flow

1. **Colab**:  
   - Run all cells (install, AWS configure, create HTML, start Flask+ngrok).  
2. **Browser**:  
   - Go to the ngrok link, e.g. `http://xxxx-xx-xx.ngrok.io`.  
   - Enter a short Arabic sentence, e.g. “مرحباً بالعالم! كيف حالك اليوم؟”  
   - Wait ~1–2 seconds.  
   - **MP3** appears in the audio player. Press play or download.  

---

## 7. Summary

With these steps:

1. **AWS** config + **boto3**: you can generate Arabic MP3 from text using **Amazon Polly**.  
2. **Flask** + **ngrok**: a quick web interface to share or demonstrate your TTS project.  
3. **Google Drive** integration (optional) to store final MP3 files.  

This approach lets your supervisor/classmates:

- **Enter** Arabic text,
- **Obtain** a generated MP3,
- **Play** or **download** it on the spot.

**Congratulations**—you now have a fully operational **TTS** project using **Amazon Polly** in **Google Colab** with a friendly **web UI** for testing!

### Attachments

1. **Paper PDFs**: 
   - Kim et al. (2024) - "Advances in Low-Resource Arabic Speech Recognition Using Whisper"
   - Ramesh & Perez (2023) - "Evaluating Cloud-based Arabic TTS Solutions"
   - Al-Hashimi (2024) - "Building Interactive Voice Experiences with Alexa Skills Kit"

2. **Python Notebooks & Scripts**:
   - `asr_whisper.py` (or Jupyter notebook with Whisper code)
   - `tts_polly.py` (Polly usage script)
   - `lambda_function.py` (Alexa Skill code for Arabic TTS and Q&A)

3. **Datasets**:
   - **Hugging Face** link: [Common Voice Arabic](https://huggingface.co/datasets/mozilla-foundation/common_voice_11_0)
   
README: Arabic ASR Using Whisper
This README guides users (like your supervisor or classmates) through setting up and running the Arabic Automatic Speech Recognition (ASR) project using Whisper. It also shows how they can test the model via a Flask website in Google Colab, complete with audio recording and file upload.

Table of Contents
Project Overview
Key References
Prerequisites
Model & Code Setup
Running the Website (Flask + ngrok)
Usage Steps
Additional Notes
Next Steps (TTS & Alexa)
<a name="overview"></a>

1. Project Overview
Goal: Demonstrate an Arabic ASR system using OpenAI’s Whisper model fine-tuned on a Hugging Face (or other Arabic) dataset.
Interface: A simple Flask website that allows:
Audio recording via browser mic
File upload (MP3 or WAV)
Displaying the transcription in real-time
Environment: Google Colab (recommended). A local environment could also work with the same steps.
<a name="references"></a>

2. Key References
Our ASR approach is partly inspired by these research papers:

A Comparative Study of LLM-based ASR and Whisper in Low Resource and Code Switching Scenario

https://arxiv.org/html/2412.00721v1#bib.bib19
A Transfer Learning End-to-End Arabic Text-To-Speech (TTS) Deep Architecture

https://www.researchgate.net/publication/344018193_A_Transfer_Learning_End-to-End_Arabic_Text-To-Speech_TTS_Deep_Architecture
(The second paper is more TTS-oriented, but it inspired some architecture and training strategies that influenced our approach to Arabic speech tasks.)

<a name="prerequisites"></a>

3. Prerequisites
Google Colab (recommended for easy GPU usage).
Ngrok (for exposing your local Flask server as a public URL, letting you or others test the site).
Basic knowledge of Python and Colab cells.
<a name="model-setup"></a>

4. Model & Code Setup
4.1 Model Training
If you want to retrain or fine-tune the Whisper model yourself:

You have a Colab notebook (not included here, but presumably in your repo) where:
You mount Google Drive:
python
Copy code
from google.colab import drive
drive.mount('/content/drive')
Train the model using trainer.train(), etc.
Save the model to Google Drive:
python
Copy code
# Example (only needed if retraining):
trainer.save_model(training_args.output_dir)
processor.save_pretrained(training_args.output_dir)

# Copy to Google Drive
drive_output_dir = "/content/drive/MyDrive/whisper-small-hi"
if os.path.exists(drive_output_dir):
    shutil.rmtree(drive_output_dir)
shutil.copytree(training_args.output_dir, drive_output_dir)
print(f"Model saved to Google Drive at {drive_output_dir}")
If you’re using the already-trained model (no need to retrain):

Skip the above retraining steps.
We have a public Google Drive link with the trained model (e.g., link to your folder).
4.2 Loading the Trained Model
Mount Google Drive:
python
Copy code
from google.colab import drive
drive.mount('/content/drive')
Copy the model from Drive to the local Colab filesystem:
python
Copy code
import shutil
import os

model_dir = "/content/drive/MyDrive/ASR"  # Path in Drive where model is stored
local_model_dir = "./model"
shutil.copytree(model_dir, local_model_dir)
print(f"Model copied to {local_model_dir}")
Now you have a local folder ./model with the necessary files (processor, config, etc.).
<a name="running-website"></a>

5. Running the Website (Flask + ngrok)
5.1 Install Required Packages
In your Colab environment, you need:

python
Copy code
!pip install pydub pyngrok transformers librosa flask
!mkdir templates static
(This is from the code you provided. If any packages are missing, add them accordingly.)

5.2 Create the Flask App & HTML/JS/CSS
Your code snippet already:

Creates index.html in templates/
Creates styles.css in static/
Defines a Flask app that:
Loads the Whisper model from model/
Has a /transcribe endpoint to handle file uploads or mic recordings
Uses ngrok to get a public URL
(You posted your final code cells in the conversation. That code is typically run top-to-bottom in a single Colab notebook.)

5.3 Launch the Flask App
Run the final cell that starts the Flask server and ngrok tunnel:
python
Copy code
public_url = ngrok.connect(5000)
print(" * ngrok tunnel available at:", public_url)
app.run(port=5000)
Output: You’ll see something like:
arduino
Copy code
* ngrok tunnel available at: http://xxxx-xx-xx-xx.ngrok.io
* Running on http://127.0.0.1:5000
Open the ngrok link (http://xxxx.ngrok.io) in your browser. You should see your Arabic ASR webpage.
<a name="usage-steps"></a>

6. Usage Steps
Go to the public ngrok URL shown in Colab.
Upload an MP3/WAV file or Record audio by pressing the “Record” button.
Wait for the website to show “جاري التحليل…” (Processing...).
Transcription will appear in the result box below the audio player.
You can test:

Short Arabic phrases
Possibly code-switching or dialect segments (as your model/data allows)
<a name="additional-notes"></a>

7. Additional Notes
Model Size: Whisper has multiple sizes (tiny, small, base, etc.). Ensure your local Model folder matches the size you trained.
Audio: Make sure your browser’s mic permissions are allowed.
Performance: The speed depends on your Colab’s GPU availability and the model size.
Optional: If the user wants to retrain or fine-tune the model, they must run your training notebook, mount Drive, and copy the output back into a local model/ folder.
<a name="next-steps"></a>

8. Next Steps
Add TTS (Amazon Polly):
We’ll incorporate a TTS button or endpoint so after transcribing, we can generate spoken feedback.
Alexa:
The Alexa skill is separate. You’ll demonstrate or share it via the Alexa Developer Console. Beta testers can try it on an actual Echo device.
Refinements:
Expand the website to handle longer audio or offline usage.
Integrate more advanced error handling or user feedback.
That’s it!
With this README:

Your supervisor or any user can launch the Colab notebook,
Mount your pretrained model from Google Drive,
Run the Flask + ngrok code,
Open the resulting link,
Upload or record audio,
And view the Arabic transcription in real-time.

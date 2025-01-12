# Azure-openai-Realtime-twilio-phone
Azure OpenAI Realtime integration with Twilio Phone
Azure OpenAI Realtime + Twilio Phone Integration
This repository demonstrates how to integrate Azure OpenAI Realtime (preview) with Twilio’s Media Streams to build a real-time AI voice assistant. Callers dial a Twilio phone number, and the audio is streamed to Azure OpenAI for transcription and response. The AI responses are then sent back to the caller in real time.

Features
Twilio Media Streaming: Real-time phone call audio to/from a WebSocket endpoint.
Azure OpenAI Realtime: Processes incoming audio, generates AI speech, and returns audio deltas.
FastAPI Backend: Manages WebSocket connections, forwards audio both ways.
Real-time Interaction: Users can interrupt the AI or ask follow-up questions mid-response.
Prerequisites
Python 3.9+
Twilio Account with a phone number capable of Voice calls.
Azure OpenAI Realtime (preview) access.
.env file containing:
makefile
Copy code
OPENAI_API_KEY=<Your Azure OpenAI API Key>
PORT=5050
(Optional) ngrok or another tunneling tool to expose your local server to Twilio.
Setup
Clone this repo:
bash
Copy code
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
Install dependencies:
bash
Copy code
pip install -r requirements.txt
Create .env:
bash
Copy code
# .env
OPENAI_API_KEY=<Your Azure OpenAI key>
PORT=5050
Run the app:
bash
Copy code
python main.py
or
bash
Copy code
uvicorn main:app --host 0.0.0.0 --port 5050
Twilio Configuration
Configure the Webhook for incoming calls to point to:
vbnet
Copy code
https://<your-ngrok-id>.ngrok.io/incoming-call
Media Stream: Twilio will automatically connect to wss://<your-ngrok-id>.ngrok.io/media-stream from the generated <Connect><Stream>.
How It Works
Call Flow:
User calls your Twilio number.
Twilio invokes /incoming-call, which returns TwiML to open a Media Stream.
Audio Streaming:
Twilio sends audio chunks to /media-stream via WebSocket.
The code forwards audio to Azure OpenAI Realtime.
AI Processing:
Azure OpenAI Realtime transcribes user speech and generates audio replies.
Replies are streamed back to Twilio, allowing the caller to hear the AI’s voice in real time.
Interruptions:
If the caller speaks mid-response, the code can signal the AI to truncate its current speech.
Customization
System Prompt (SYSTEM_MESSAGE) defines the AI’s personality and conversation style.
Voice (VOICE) sets the Azure OpenAI voice model.
Interrupt Logic can be tweaked or disabled if you prefer continuous AI speech.
Troubleshooting
No Audio: Check Twilio phone number settings and verify wss://<domain>/media-stream is reachable.
API Errors: Ensure your Azure OpenAI Realtime deployment name, API key, and api-version match the preview requirements.
Connection Issues: Use ngrok http 5050 to create a public tunnel for Twilio.

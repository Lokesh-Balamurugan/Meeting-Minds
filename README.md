# Meeting Minds

This project is a speech-to-text pipeline that can transcribe audio files and generate a transcript of the speech. The project can also answer default and custom question asked by the user using the GPT-3.5 Turbo model.

# Link to Live Applications
## Please Use these applications judiciously as we are running low on Free Credits, Deal ? 
- Streamlit Application - http://34.138.127.169:8051
- Airflow - http://34.138.127.169:8080
- Codelabs - https://codelabs-preview.appspot.com/?file_id=13IiASFrZKI9m-6Wwch8DBNbi6ufG1J44GHJpRr1mhos#9
- Demo of application - https://www.youtube.com/watch?v=stlX149161E

# Project Tree
```
📦 
├─ .DS_Store
├─ .gitignore
├─ Airflow
│  ├─ dags
│  │  ├─ transcribe_all_audio_dag.py -> DAG triggers on A CRON Schedule on a daily basis - 12 AM everyday
│  │  └─ transcribe_dag.py -> DAG Triggers with a REST API request, comes from a streamlit button
│  └─ docker-compose.yaml
├─ Dockerfile
├─ README.md
├─ __init__.py
├─ architechture_diagram
│  ├─ architechture_diagram.py -> Python Code to Create Diagram
│  ├─ directory.png
│  ├─ openai.png
│  └─ streamlit.png
├─ docker-compose.yml
├─ feapps3
│  └─ docker-compose.yml
├─ helper_functions
│  ├─ __init__.py
│  └─ core_helpers.py -> Core Helper Function for better reusability of code
├─ mp3_transcript_workflow.png
├─ navigation
│  ├─ adhoc.py -> Python Code with Logic to Trigger transcibe_dag.py
│  ├─ config.toml
│  ├─ mp3_uploader.py -> Python Code to Upload MP3 file to s3 Bucket
│  └─ questionnaire.py -> Python Code to ask Custom Questions
├─ requirements.txt
└─ userinterface.py -> Python Code with Routers to different Streamlit Navigation
```
©generated by [Project Tree Generator](https://woochanleee.github.io/project-tree-generator)

### Overview:

This application is designed to process and transcribe audio files, then answer questions about the transcribed audio using GPT-3.5 Turbo. The application is built using Streamlit and deployed on a Google Cloud Platform (GCP) instance. The project utilizes Amazon S3 for storage and Apache Airflow for managing the transcription workflow.

### Pages and features:

-**MP3 Uploader:** Users can upload MP3 files (up to 25 MB) which are then automatically noise-reduced using Pydub. The cleaned audio is uploaded to the 'to_be_processed' folder in S3 bucket.  

-**Adhoc Transcription and Q&A:** Users can select an audio file from the 'to_be_processed' S3 folder, then trigger an Airflow DAG to transcribe the file using Whisper API. The transcribed text is stored in the 'transcripts' S3 folder, and the audio file is archived in the 'archived' S3 folder. GPT-3.5 Turbo answers a set of default questions about the transcript, and users can also ask their own questions.  

-**Questionnaire:** Users can view and interact with existing transcripts from the 'transcripts' S3 folder. The default Q&A and ad-hoc question-answering functionality are available for these transcripts as well.

# Prerequisites

To run this project, you will need:

- Google Cloud Platform account
- Docker
- AWS access and secret keys
- OpenAI API key
- .env file containing the AWS and OpenAI keys in the same directory as the Airflow DAGs Docker Compose and the web application (Streamlit) Docker Compose files

# Installation

- Clone the repository.
- Install the required packages by running pip install -r requirements.txt.
- Create a VM instance in Google Cloud Platform.
- Set up Airflow in the VM instance:
- Create a new directory named "app" (Copy the contents of the Airflow folder from this repository into the "app" directory).
- Create two DAGs in Airflow:
1) DAG 1: transcribe_dag: This DAG is triggered when the user selects an audio file and clicks the "Transcribe to text" button in the Streamlit app. It transcribes the selected audio file using Whisper API, moves the audio file to the 'archived' S3 folder, stores the transcribed text in the 'transcripts' S3 folder, and generates answers to default questions using GPT-3.5 Turbo.
2) DAG 2: transcribe_audio_daily: This DAG is triggered daily using a CRON schedule. It processes all the audio files in the 'to_be_processed' S3 folder and performs the same tasks as the adhoc_transcription DAG.
- Run Airflow by running airflow scheduler.
- Build Docker images for the Streamlit app and push them to Docker Hub.
- Set up the Streamlit app in the VM instance:
- Create a new directory called 'feapps3' for the Streamlit app (Copy the docker-compose.yml file from the 'feapps3' folder in this repository into the new directory).
- Ensure the .env file containing the AWS and OpenAI keys is present in both the "app" directory created for Airflow and the directory created for the Streamlit app.
- Pass the AWS and OpenAI keys as environment variables in the Docker Compose file.
- Run the Docker Compose file to start the Streamlit app.



### .env file for airflow:
- AWS_ACCESS_KEY=<aws_access_key> <- should be given in double quotes ("")
- AWS_SECRET_KEY=<aws_secret_key> <- should be given in double quotes ("")
- OPENAI_API_KEY=<openai_api_key> <- should be given in double quotes ("")

### .env file for streamlit:
- AWS_ACCESS_KEY=<aws_access_key> <- should be given in double quotes ("")
- AWS_SECRET_KEY=<aws_secret_key> <- should be given in double quotes ("")
- OPENAI_API_KEY=<openai_api_key> <- should be given in double quotes ("")
- AWS_REGION=<AWS_REGION> <- should be given in double quotes ("")
- S3_BUCKET_NAME=<S3_BUCKET_NAME> <- should be given in double quotes ("")
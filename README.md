# MetaVoice-CaseStudy

## Introduction
This project focuses on designing and implementing a scalable data pre-processing pipeline for audio data. The primary goal is to transcribe and tokenize audio files and store the processed data in an organized manner. The pipeline utilizes AWS S3 for storage, the Whisper ASR (Automatic Speech Recognition) model for transcription, and custom audio tokenization.

## Prerequisites (Necessary Packages)
1. Numpy
2. PyTorch
3. Whisper (for ASR)
4. Boto3 (for AWS S3 interactions)
5. Pandas
6. PyArrow (for Parquet file handling)

## Pipeline Overview
The project consists of Python code that performs the following tasks:

**Tokenization Function:**
* 'tokenise' is a function that takes a NumPy array representing an audio file as input and returns a random 1D tensor with dtype int16 and variable length in the range (20, 1000).
*	The function includes input validation to ensure the input is a NumPy array.
*	A time delay is added to simulate model inference.

**Transcription and Tokenization Function:**
*	'transcribe_tokenize' is a function that transcribes and tokenizes an audio file.
*	It takes the file path, an AWS S3 object, a Whisper model object, and the bucket name as inputs.
*	If the file is in the .flac format, it downloads the audio file from S3, transcribes it using the Whisper model, and tokenizes the audio and if not a .flac format it returns NaN for both text and tokenized data.

**Main Processing Logic:**
*	The main processing logic iterates through a list of objects stored in the S3 bucket.
*	For each object (audio file), it calls the transcribe_tokenize function.
*	The resulting transcribed text and tokenized data are added to the DataFrame.

**Data Transformation:**
*	The DataFrame is transformed by extracting date and time information from the "LastModified" column.
*	A new column, "FolderName" is added to identify the folder containing the audio file.
*	The data is partitioned based on "FolderName", "LastModifiedDate", "LastModifiedHour" and "LastModifiedMinute."

**Data Storage:**
*	The partitioned data is saved as Parquet files.
*	Each Parquet file is named based on the partition key, which includes "FolderName", "LastModifiedDate", "LastModifiedHour" and "LastModifiedMinute."
*	This partitioning allows granular access to audio transformations based on both date and time.


**By partitioning the data based on FolderName and Date Timestamps, the pipeline allows access to audio transformations at different granularities of time, providing flexibility for various analytical needs.**

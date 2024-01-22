# Voice To Translator

## Overview
This Translation App is a Vue.js-based web application that provides real-time speech-to-text translation. It supports multiple languages and offers features like continuous speech recognition, language selection, and a user-friendly interface.

## Features
- Real-Time Translation: Translates speech to text in real time.
- Multiple Language Support: Offers a variety of languages to choose from. (
Voice recognition is currently only available in Korean, English, and Japanese.)
- Continuous Speech Recognition: Can continuously listen and transcribe speech without interruption.
- Responsive Design: Compatible with desktop and Android devices on Google Chrome.


## Installation
To set up the project on your local machine:

1. Clone the repository:
```shell
git clone [repository URL]
```
2. Navigate to the project directory:
```shell
cd [project-name]
```
3. Install dependencies:
```shell
npm install
```
4. Run the application:
```shell
npm run serve
```

## Usage
- Open the application in Google Chrome.
- Choose the input and output languages.
- Click on the 'Speech message' button to start voice input.
- Speak into your device's microphone.
- The translated text will appear in the output area.
- 
## Technologies Used
- Vue.js
- SpeechRecognition API
- Google Translate API
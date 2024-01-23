# Voice To Translator

## Overview
This Translation App is a Vue.js-based web application that provides real-time speech-to-text translation. It supports multiple languages and offers features like continuous speech recognition, language selection, and a user-friendly interface.

## Domain
https://speech-to-translator.vercel.app/ <br />
![Speech To Translator](/public/speech-to-translator.png "Screenshot")

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
git clone https://github.com/iu7726/speech-to-translator.git
```
2. Navigate to the project directory:
```shell
cd speech-to-translator.git
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
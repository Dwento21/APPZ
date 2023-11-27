import random
import speech_recognition as sr
import datetime
import pyttsx3
from gtts import gTTS
from playsound import playsound
import webbrowser
import re
import os

desktop_path = os.path.join(os.path.join(os.environ['USERPROFILE']), 'Desktop')
anekdots = [
    'У самурая наркомана немає шляху - є тільки дорожка',
    'наступив слон на колобка і каже: блін',
    'Гуляють дві зубочистки по лісу, мимо них пробігає їжачок, і одна зубочистка каже іншій, а я не знала, що тут маршрутки їздять',
    'питає програміст програміста, ось хочу пошту без спаму, що порадиш, інший відповідає - голубів'
]


def speak(text):
    tts = gTTS(text=text, lang='uk')

    # Отримання поточної дати та часу для унікальності імені файлу
now = datetime.datetime.now()
    timestamp = now.strftime("%Y%m%d_%H%M%S")  # Формат дати та часу для унікального імені файлу

    # Формування унікального імені файлу на основі дати та часу
output_file = f"C:\\Users\\DAN\\Desktop\\MyDirectory\\output_{timestamp}.mp3"

    # Збереження файлу з унікальним ім'ям
 tts.save(output_file)
    playsound(output_file)


def recognize_speech():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        print("Скажіть щось:")
        recognizer.adjust_for_ambient_noise(source)
        audio = recognizer.listen(source)
    try:
        print("Розпізнано: " + recognizer.recognize_google(audio, language="uk-UA"))
        return recognizer.recognize_google(audio, language="uk-UA")
    except sr.UnknownValueError:
        print("Не розпізнано")
        return ""
    except sr.RequestError as e:
        print("Помилка сервісу: {0}".format(e))
        return ""


while True:
    command = recognize_speech().lower()

if "привіт" in command:
        speak("Привіт, я ваш голосовий помічник!")
    elif "розкажи жарт" in command or "розкажи анекдот" in command:
        speak(random.choice(anekdots))
    elif "як справи" in command:
        speak("У мене все добре, а у вас?")
    elif "яка зараз дата" in command:
        now = datetime.datetime.now()
        current_date = now.strftime("%d-%m-%Y")
        speak(f"Сьогодні {current_date}")
    elif any(keyword in command for keyword in
             ["який зараз час", "година", "скільки годин", "що за година", "яка зараз година",
              "годинник скільки показує", "котра година зараз",
              "скажи, будь ласка, час", "котре число годин",
              "підкажи, яка година", "скажи, будь ласка, скільки часу",
              "котра зараз година", "на годиннику яке число"]):
        now = datetime.datetime.now()
        current_time = now.strftime("%H:%M")
        speak(f"Зараз {current_time}")
    elif "відкрий телеграм" in command or "відкрий месенджер телеграм" in command or "запусти телеграм" in command:
        webbrowser.open("https://web.telegram.org/")
    elif "відкрий youtube" in command:
        webbrowser.open("https://www.youtube.com/")
    elif "відкрий spotify" in command:
        webbrowser.open("https://open.spotify.com/")
    elif any(keyword in command for keyword in ["відкрий google", "гугл", "google"]):
        webbrowser.open("https://www.google.com/")
    elif "калькулятор" in command:
        speak("Для калькулятора скажіть операцію")
        operation = recognize_speech().lower()
        pattern = r"(\d+)(\s*)([\+\-\*/])(\s*)(\d+)"
        match = re.match(pattern, operation)
        if match:
            num1 = match.group(1)
            operator = match.group(3)
            num2 = match.group(5)
            result = None

if operator == '+':
                result = int(num1) + int(num2)
            elif operator == '-':
                result = int(num1) - int(num2)
            elif operator == '*':
                result = int(num1) * int(num2)
            elif operator == '/':
                if int(num2) != 0:
                    result = int(num1) / int(num2)
                else:
                    speak("Ділення на нуль не можливе")
            if result is not None:
                speak(f"Результат: {result}")
        else:
            speak("Не розпізнано операцію")
    elif "Слава Україні" in command:
        speak("Героям слава!")
    elif any(keyword in command for keyword in ["пока", "закінчити команду", "до побачення"]):
        speak("До зустрічі!")
        break

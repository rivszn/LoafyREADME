# Environment Setup for Loafy

## Prerequisites

- **Python 3.10.0** is required for this project. If you haven't installed it yet, please download and install it from [Python's official website](https://www.python.org/downloads/release/python-3100/).

## Create a Virtual Environment

1. Open your terminal/command prompt.

2. Navigate to your project directory:

```bash
cd /path/to/your/project 
```

Create a virtual environment (replace env_name with your preferred name for the environment):

```bash
python -m venv env_name
```

## Install Required Packages

After activating your virtual environment, you can install the required Python packages from the '**requirements.txt**' file:

```bash
pip install -r requirements.txt
```

This command will install all the required packages, including their specified versions, into your virtual environment.

# Loafy AI Real-time chatbot

AI that handles casual conversation locally, tuned to a personality of choice

## Built with

- Python

may contain some bloated packge (since I didn't clean the requirements YET)
so I recommend install this on venv

### The First Step: chatBot.js

First iteration, uses OpenAI api to respond intelligently to messages in discord with a custom personality
prompt

## Loafy (ver 1.0)

ver 0.0
    -> Second iteration, uses disord api to join a voice channel and play a joining audio blip
ver 1.0
    -> Joins discord calls and leaves them upon calling the command !join and !leave
    notes:  Looking to implement whisperai, however it seems as though it takes a lot of resources.
            My solution to that is to use Google colab. The paid options are quite expensive,
            so I'll keep an eye on it. (I might not need to use it)

Notes:
This iteration was discontinued because of discord.py's API not supporting audio receiving. 
I realized it would be counter productive to try and find a solution, with
the only conceivable way I can see it being able to hear is by routing the audio through my own PC
audio. 

## Loafy (ver 2.0)

I've divided this version into the following steps:

    speech-to-text
        -> language model
            -> text-to-speech

### Speech-to-text

Speech to text uses the following repo: https://github.com/mallorbc/whisper_mic 

Some features I'd like to add is the ability to listen to desktop audio as well. (This brings me to the
idea of letting the bot go on tangents if there is no input for a certain amount of time, to essentially
start a conversation). I'd also like to add the ability for it to read, but that's for another version.

Needed to make it faster, see: https://github.com/Toxotei/faster-whisper_mic/blob/main/mic.py 

### Language model

As of April 22, 2023, Open Assistant has released a couple ways to access open source models like Llama.
These were always accessible and I've already looked into attempting to run Vicuna and the other models locally,
however my hardware doesn't quite meet the expectations. I'll have to bite the bullet and sink a little bit of cash
into it and use OpenAI's API. 

Implementing the code from this repo was quite easy, so huge props to the authors: 
https://github.com/atomic14/command_line_chatgpt 

### Text-to-speech

This new crazy Text-to-speech open source AI came out, and it's pretty promising: https://github.com/suno-ai/bark/blob/main/bark/generation.py#L86
However, after messing around with it for ~30-minutes, I realized it took too long to process text and convert it at the moment. It needs a lot
of processing power to run faster. I've decided to opt out of using AI for this endevour and just use a simple TTS. 

pyttsx3:
It works. I mean, like it definitely works. It's very bad though. I've done more research, and I've taken an interest in the Google Cloud TTS API. 
It'll cost money eventually, but hopefully I don't become a hermit and use up the $300 starting credits.

Google cloud Text-to-Speech:
    voice = texttospeech.VoiceSelectionParams(
                language_code="en-US",
                name= "en-GB-Wavenet-B",
                ssml_gender=texttospeech.SsmlVoiceGender.FEMALE,
            )

Notes: 
My code is a mess. As long as it works, will clean up and optimize later. Also to note that I forgot to create
a virtual environment... Again, I'll worry about it later.

## Loafy (ver 3.0)

In this iteration, here are some things I noted about my previous versions:
1. The text-to-speech implementation is too slow, or the application is bottlenecking somewhere
2. Loafy sometimes forgets the prompt which can be problematic
3. I didn't create a virtual environment, and I lost a lot of time cleaning up my packages
4. The voice hasn't grown on me yet, I might look to other options
5. There is a lot of fluff that is unnecessary in my code, clean it and optimize it
6. The chatbot still talks too much like a chatbot, I'll do more research on LLM's and prompting
7. The audio capture is constantly running, and it captures everything and places it into a queue which is a problem

I just had my eureka moment in the shower. The single most inspirational shower thought I've ever had. 

The Idea:

### LOAFY THE DISCORD KITTEN

Hear me out.
    
    Object recognition for Valorant streams
        -> Can respond and comment on valorant streams
        -> Spike plants, kills, deaths, round score, clutches
        -> Asks to pull up the scoreboard and identify score, respond accordingly
        -> Wins and losses
        -> Comment on teammates and enemy performance
        -> Comment on seeing characters

What I need to achieve this:

    A custom trained LLM
        -> Filterless, larger memory, custom response variants
    Object recognition (both LLM and object recognition programs)
        -> Use text points on the screen
        -> Recognize characters (this is going to be a lot more difficult and expensive)
        
Proof of concept:
https://www.youtube.com/watch?v=UY7sRB60wZ4&t=10s 

MLM: 
https://github.com/WongKinYiu/yolov7 

What do discord viewers even say? Well I happen to know a couple people.
Looking into fine tuning the chatGPT model, however I feel as though this is overkill for the tasks at hand.

Here are some sample conversations:

Me:     I'm going to play ranked later. Do you know what game I'm going to play?

Loafy:  How am I supposed to know what game you're playing? Do I look like a mind reader? If you want to talk about games, go find someone else who cares.
 
Me:     I'm gonna play Valorant. Have you ever heard of it?

Loafy:  Yes, I've heard of Valorant. It's a tactical first-person shooter game developed by Riot Games. What's there to talk about it?
 
Me:     Well, surely there's a lot to talk about. It's some fun. It's very competitive. It's a game that I've been putting a lot of hours into.

Loafy:  I'm sure there is, but I'm not particularly interested in hearing about it. If you're playing it, then it must be important to you. Just don't come crying to me if you lose your matches 
        or get stuck with a bunch of incompetent teammates.

Me:     I don't get that mad

Loafy:  Good to know. It's always good to keep a level head, especially when the game gets intense. Just remember to take breaks if you start to feel overwhelmed. It's not worth getting too       
        worked up over a video game.

Me:     I have a question, let me paint you a picture.

Loafy:  Sure, go ahead and paint me your picture.

Me:     Well, it's not a picture in the literal sense, it's just a situation that I want to explain.

Loafy:  Oh, I see. Go ahead then, tell me about the situation.

### How to record desktop audio

Using the soundcard module: 
https://stackoverflow.com/questions/11509830/how-to-add-color-to-githubs-readme-md-file 

### New TTS models

https://tts.readthedocs.io/en/latest/training_a_model.html 

I've decided to give my hand at fine tuning the GPT-3 model and found an experimental Studio Voice. It's 0 to 100K bytes 
$0.00016 USD per byte ($160.00 USD per 1 million bytes). Compared to the previous voice, Neural2: 0 to 1 million bytes $0.000016 
USD per byte ($16.00 USD per 1 million bytes).

Update:
I slaved at it, but now it finally can use my VoiceMeeter output, as input. This achieves the ability to listen to voices on the desktop,
as well as from my microphone. This was less on the coding end, and heavily on figuring out how to use voicemeeter.

## References

https://discordjs.guide
https://v12.discordjs.guide
https://v12.discordjs.guide/voice/receiving-audio.html
https://discordjs.guide/voice/voice-connections.html#access
https://www.youtube.com/watch?v=8SQV-B83tPU (Colab guide to whisperai)
https://github.com/XianhaiC/Voice-Bot/blob/master/index.js 
https://www.reddit.com/r/LanguageTechnology/comments/yl31hl/how_to_install_and_deploy_openai_whisper/ 
https://arxiv.org/pdf/2303.17580.pdf 
https://github.com/mallorbc/whisper_mic 
https://pypi.org/project/SpeechRecognition/ 
https://github.com/LAION-AI/Open-Assistant 
https://github.com/atomic14/command_line_chatgpt 
https://pypi.org/project/pyttsx3/ 
https://pypi.org/project/playsound/ 
https://github.com/Toxotei/faster-whisper_mic/blob/main/mic.py 
https://github.com/guillaumekln/faster-whisper 
https://www.youtube.com/watch?v=LXA7zXVz8A4&t=28s 
https://arxiv.org/abs/1506.01497 
https://github.com/WongKinYiu/yolov7 
https://www.tensorflow.org/tutorials/images/transfer_learning 
https://www.youtube.com/watch?v=oHl8U3NccAE 
https://huggingface.co/docs/transformers/model_doc/blenderbot 
https://huggingface.co/docs/transformers/model_doc/dialogpt 
https://github.com/KoboldAI/KoboldAI-Client 
https://github.com/HRNPH/AIwaifu/blob/master/main.py 
https://stackoverflow.com/questions/18018033/how-to-stop-a-looping-thread-in-python 
https://platform.openai.com/docs/guides/fine-tuning/advanced-usage 
https://pypi.org/project/openai/ 
https://stackoverflow.com/questions/11509830/how-to-add-color-to-githubs-readme-md-file 
https://tts.readthedocs.io/en/latest/training_a_model.html 
https://github.com/coqui-ai/TTS 
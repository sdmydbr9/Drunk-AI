# Drunk-AI
# This is an integration of OPENAI chatgpt in the magicmirror. 
## This is the initial version, I am not from a technical background therefore if the codes and scripts are little off the charts or there is a way to enhance it feel free to send PR. I admit this is quite messy and little long process to set up but in the end its worth it.

Download the shortcuts linked below and configure it with the api key from MMM-RemoteControl 
 more info here ([https://github.com/Jopyth/MMM-Remote-Control.git])

ios shortcuts link 

([https://www.icloud.com/shortcuts/a0d2d12116ee4ba19ca89f684e8adc93])
## EDIT THE DOWNLOADED SHORTCUT AND REPLACE "mirrormirror" with your api key from MMM-RemoteControl. 

# you need to run this shortcuts every time you want to interact with the chatgpt, or you can use the default telegram commands but that is without voice input, hotword detection will be added soon.

## Install and configure MMM-Chat from here ([ https://github.com/sdmydbr9/MMM-Chat])

## Install MMM-NotificationTrigger ([https://github.com/MMRIZE/MMM-NotificationTrigger.git])
## Add the following lines to the config
 

       {
      module: 'MMM-NotificationTrigger',
      config: {
        triggers: [
          {
            trigger: 'SHOW_ALERT',
            fires: [
              {
                fire: 'MY_COMMAND',
                exec: (payload) => `python3 /home/pi/MagicMirror/modules/MMM-11-TTS/main.py "${payload.message}"`
              }
            ]
          }
        ]
      }
    },


## Install MMM-OpenAI from here ([https://github.com/MMRIZE/MMM-OpenAI.git])

Add the following in your config 
                      
                      
                      
                      {
        module: "MMM-OpenAI",
        position: 'top_right',
        config: {
                defaultChatInstruction: "Your name is Marvin and you are a paranoid android that reluctantly answers questions with sarcastic responses.",
                stealth: true,  // <- This is needed to hide default module view.
                postProcessing: function (handler, responseObj) {
                        if (responseObj.error) return // When the error happens, just do nothing.
                        let method = responseObj.options.method
                        let alertPayload = {
                                title: responseObj.request.prompt,
                                imageUrl: (method === 'IMAGE') ? responseObj.response.data[0].url : null,
                                message: (method === 'TEXT') ? responseObj.response.choices[0].text : 
                                ((method === 'CHAT') ? responseObj.response.choices[0].message.content : null),
                                timer: 2 * 1000
                        }
                        handler.sendNotification('SHOW_ALERT', alertPayload)
                }
        }
     },








## Clone the following respository ([https://github.com/sdmydbr9/MMM-11-TTS]) in your modules folder and install it according to instruction
## edit the main.py and add your api key and voice id, a voice id as already set by default, you can add any voice id, refer elevenlabs api doc for more details.



### Disclaimer: Even though the quality of the output if their voice is far superior to GOOGLE TTS, the character limit is very limited, 10,000 characters per month per account, i hope they will offer more in future, or you can opt for a pay account and get around 30,000 characters as well as voice cloning features,  clone any voice you want, for example clone the voice of jarvis and transform your magicmirror into jarvis. just add the voice ID in the main.py and youre good to do.

### Disclaimer 2: The above module works in my test but it is not very efficient since the script will first downlaod the audio from the api request and convert it using fmpeg and play the audio as output. 
### Depends on fmpeg. 








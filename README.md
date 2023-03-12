# Drunk-AI
This is an integration of OPENAI chatgpt in the magicmirror.
Download the shortcuts linked below and configure it with the api key from MMM-RemoteControl 
 more info here ([https://github.com/Jopyth/MMM-Remote-Control.git])

ios shortcuts link 

([https://www.icloud.com/shortcuts/a0d2d12116ee4ba19ca89f684e8adc93])

Install MMM-NotificationTrigger ([https://github.com/MMRIZE/MMM-NotificationTrigger.git])
Add the following lines to the config
 

    
    
    {
      disabled : false,
      classes:"default",
       module: "MMM-NotificationTrigger",
       config: {
         triggers:[
            {
             trigger: "SHOW_ALERT",
              fires: [
               {
                fire:"MY_COMMAND",
                exec: (payload) => `python3 /home/pi/MagicMirror/modules/MMM-11-TTS/main.py "${payload.message}"`,
               },
                     ],
             },
                  ]
              }
      },



Install MMM-OpenAI from here ([https://github.com/MMRIZE/MMM-OpenAI.git])

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



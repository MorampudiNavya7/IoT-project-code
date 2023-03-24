# IoT-project-code
This was my Virtual internship project on Medicine Remainder For Elderly People Using IBM IoT
import wiotp.sdk.device

import time

import os

import datetime

from ibm_watson import TextToSpeechV1

from ibm_cloud_sdk_core.authenticators import IAMAuthenticator

import playsound

authenticator= IAMAuthenticator('0m5T9tnGiwrLCDcUtfB5IN9RFI_cGOOeh5-6uk_Ui0q2')

text_to_speech = TextToSpeechV1(

authenticator=authenticator

)

text_to_speech.set_service_url("https://api.eu-gb.text-to-speech.watson.cloud.ibm.com/instances/603a90fe-fd35-46a1-b820-2d11112df943")

myConfig = {

"identity":{

"orgId":"vifr0r",

"typeId":"mydevice",

"deviceId":"20L213"

},

"auth": {

"token":"z&mK!-Jc)2nK)!YYo("

}

}

def myCommandCallback(cmd):

print("Message received from IBM IoT Platform: %s" % cmd.data['command'])

m=cmd.data['command']

with open ('medicine.mp3','wb') as audio_file:

audio_file.write(

text_to_speech.synthesize(

'You have to take '+m+'medicine now',

voice='en-US_MichaelV3Voice',

accept='audio/mp3'

).get_result().content)

playsound.playsound('medicine.mp3')

os.remove('medicine.mp3')

client = wiotp.sdk.device.DeviceClient(config=myConfig, logHandlers=None)

client.connect()

while True:

client.commandCallback = myCommandCallback

time.sleep(10)

client.disconnect()

# Telegram-Bot-EN
These examples demonstrate the use of TelegramBotAPI in LabVIEW. Beginners will be useful to start read
[Bots: An introduction for developers](https://core.telegram.org/bots/api/).
<br>The description in Russian  is available [by reference](https://github.com/ladikvadim/Telegram-Bot/blob/master/Docs/Readme_RU.pdf).
---
## Getting Started
---
### Software and hardware
Example **TelegramBotSimpleExample** requires installed LabVIEW 2015+ and Internet access.<br> 
Example **myRIO+TelegramBOT Demo** in addition, requires the presence of modules Real-Time and FPGA, and hardware - myRIO 1900. Instead of myRIO with a small change of the project, you can use any device on the RIO platform (sbRIO, CompactRIO).

## Methods description
A bot receives updates using method **getUpdates** (long polling) and sends messages using method **sendMessage**. Currently, only these two methods work. Each update is an array of JSON-serialized *Update* objects.
### getUpdates
<p align="center">
  <img src="https://raw.githubusercontent.com/ladikvadim/Telegram-Bot/master/Docs/ConnectorsGetUpdates.PNG">
</p>

* Input terminals:
   * Token    - bot token.
   * error in	- input error cluster.
* Output terminals:
  * Token	- bot token.
  * UpdateUniresal - array of *Update* objects.
  * ConnectionStatus - connection status indicator.
  * error out - output error cluster.

### sendMessage
<p align="center">
  <img src="https://raw.githubusercontent.com/ladikvadim/Telegram-Bot/master/Docs/ConnectorsSendMessage.PNG">
</p>

* Input terminals:
  * Token    - bot token.
  * MsgType - message type (Text, ReplyKeyboardMarkup, ReplyKeyboardRemove).
  * ChatId - active chat indicator.
  * Text - message text.
  * ReplyKeyboardMarkup - [a custom keyboard with reply options](https://core.telegram.org/bots/api/#replykeyboardmarkup).
  * ReplyToMessageId - parent message ID (in group chats).
  * error in - input error cluster.
* Output terminals:
  * error out - output error cluster.

## Bot examples
For a correct operation of each example:
* [create a bot in Telegram](https://core.telegram.org/bots#6-botfather) and write in the field "Token" a token, [which sent @Botfather](https://core.telegram.org/bots/api#authorizing-your-bot). 
The token must start with "bot", for example “**bot275887199:AAHmg4xfDwkElXKcgoITf0nrYVpOVlTc8k0**”
* On your device, start a private chat with the bot created in the previous step.
  * For testing, you can use the ***@ATISIndBot*** bot.

### TelegramBotSimpleExample
Consider the example of the simplest bot in LabVIEW. To do this, open **TelegramBotSimpleExample.vi**. The following figures show its front panel, which is simple and does not require explanations and a block diagram.

<p align="center">
  <img src="https://raw.githubusercontent.com/ladikvadim/Telegram-Bot/master/Docs/FrontPanelTelegramBotSimpleExample.PNG">
  <br>TelegramBotSimpleExample.vi Front Panel
  <img src="https://raw.githubusercontent.com/ladikvadim/Telegram-Bot/master/Docs/BlockDiagramTelegramBotSimpleExample.PNG">
 TelegramBotSimpleExample.vi Block Diagramm
</p>

Algorithm of the bot is very simple. First, an initialization takes place. Then, in an infinite loop with a specified period, occur poll the Telegram servers for new updates. SubVI BuildMsg.vi converts JSON Update objects to messages. If there are new messages, then their text is compared to variants of the Case structure. At the last stage, the reply message is sent to the active chat.

#### How to create a custom keyboard.

In order to send a keyboard to the chat you need to perform two actions:
1. Specify the type of message to send as ***ReplyKeyboardMarkup***.
2. Fill the ***ReplyKeyboardMarkup*** cluster.
<br>To delete the keyboard, you must specify the message type as ***ReplyKeyboardRemove***.

<br>The following figure shows an example of creating a custom keyboard consisting of four buttons (Red, Green, Orange, Delete) and result that is displayed in chat.

<p align="center">
  <img src="https://raw.githubusercontent.com/ladikvadim/Telegram-Bot/master/Docs/CreatingCustomKeyboard.png">
  Example of creating a custom keyboard:<br>
  А — block diagram fragment that is responsible for creating and sending the keyboard.<br>
  Б — result displayed in chat.
</p>

### myRIO+TelegramBOT Demo
This is a more complex example of implementing the logic of bot, although the meaning of his work remains the same. In this example, using messages in telegram, you can change the glow of custom LED1-3 LEDs on myRIO 1900 and perform some other functions.

To change LED glow, use next commands::<br>
`/led#,Period(ms/us),DutyCycle(%)` - changes the character of user LED (1-3) . For example, ***/led1,1000,50***. For LED1, 2 the period is set in ms, forLED3 in us.<br>
`/led#` (without parameters), # - LED number (1-3). For example, ***/led1***.As a result, bot will return user's keyboard with several buttons, allowing you to on/off the LED.

To obtain weather data in a certain city, use a command::<br>
`/getweather,City,Region`, где City — required parameter, Region — optional parameter.
For example ***/getweather,miami,us*** — bot will return a message with weather data in Miami, USA.<br>

To get the current time of myRIO, use a command:<br>
`/gettime` — the bot will return a message about myRIO current time.

## Authors

* **Vadim Ladik** - *Initial work*

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details

## Acknowledgments
* Hat tip to anyone who's code was used
* Inspiration
* Sorry for my bad English
* etc

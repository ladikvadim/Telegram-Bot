# Telegram-Bot-EN
Telegram bot implementation using LabVIEW

---
# Telegram-Bot-RU
## Быстрый старт
Представленные примеры демонстрируют использование TelegramBotAPI в LabVIEW. Новичкам будет полезно для начала прочесть 
[Bots: An introduction for developers](https://core.telegram.org/bots/api/).

### Используемое программное и аппаратное обеспечение
Для работы с примером TelegramBotSimpleExample необходимо наличие установленной LabVIEW 2015+ и выхода в Интернет.С примером myRIO+TelegramBOT Demo дополнительно необходимы модули Real-Time и FPGA. Из аппаратных средств myRIO 1900. Вместо myRIO при небольшой правке проекта можно использовать любое устройство на платформе RIO (sbRIO, CompactRIO).

## Описание реализованных методов
Для получения ботом обновлений используется метод **getUpdates** (long polling), для отправки сообщений — метод **sendMessage**. На данный 
момент реализованы только эти два метода. Каждое обновление представляет собой массив JSON—сериализованных объектов *Update*.
### getUpdates
<p align="center">
  <img src="https://raw.githubusercontent.com/ladikvadim/Telegram-Bot/master/Docs/ConnectorsGetUpdates.PNG">
</p>

* Входные терминалы:
   * Token    - token используемого бота.
   * error in	- входной кластер ошибки.
* Выходные терминалы:
  * Token	- token используемого бота.
  * UpdateUniresal - массив объектов Update.
  * ConnectionStatus - индикатор статуса соединения.
  * error out - выходной кластер ошибки.

### sendMessage
<p align="center">
  <img src="https://raw.githubusercontent.com/ladikvadim/Telegram-Bot/master/Docs/ConnectorsSendMessage.PNG">
</p>

* Входные терминалы:
  * Token - token используемого бота.
  * MsgType - тип передаваемого сообщения (Text, ReplyKeyboardMarkup, ReplyKeyboardRemove).
  * ChatId - идентификатор активного чата.
  * Text - текст передаваемого сообщения.
  * ReplyKeyboardMarkup - объект, представляющий собой пользовательскую клавиатуру.
  * error in - входной кластер ошибки.
* Выходные терминалы:
  * error out - выходной кластер ошибки.

## Примеры ботов
Для корректной работы каждого примера необходимо:
* [создать бота в Telegram](https://core.telegram.org/bots#6-botfather) и вписать в поле "Token" токен, [выданный @Botfather](https://core.telegram.org/bots/api#authorizing-your-bot). 
Токен должен начинаться с “bot”, например “**bot275887199:AAHmg4xfDwkElXKcgoITf0nrYVpOVlTc8k0**”
* На своём устройстве начать личный чат с ботом, который был создан на предыдущем шаге.
  * Для тестирования можно использовать бота ***@ATISIndBot***.

### TelegramBotSimpleExample
Рассмотрим пример простейшего бота в LabVIEW. Для этого необходимо открыть **TelegramBotSimpleExample.vi**. На следующих рисунках
представлены его лицевая панель, которая проста и не требует пояснений и блок-диаграмма.

<p align="center">
  <img src="https://raw.githubusercontent.com/ladikvadim/Telegram-Bot/master/Docs/FrontPanelTelegramBotSimpleExample.PNG">
  Лицевая панель TelegramBotSimpleExample.vi
  <img src="https://raw.githubusercontent.com/ladikvadim/Telegram-Bot/master/Docs/BlockDiagramTelegramBotSimpleExample.PNG">
  Блок-диаграмма TelegramBotSimpleExample.vi
</p>

Алгоритм работы бота очень прост. Сначала проходит инициализация. Далее в бесконечном цикле с заданным периодом происходит опрос 
серверов Telegram на наличие новых обновлений. SubVI BuildMsg.vi преобразует JSON объекты **Update** в сообщения. При наличии новых 
сообщений происходит сопоставление их текста вариантам Case-структуры. На последнем этапе выполнятся отправка ответного сообщения 
в активный чат.

#### Создание пользовательской клавиатуры.
Для передачи в чат клавиатуры необходимо выполнить два действия:
1. Указать тип сообщения для передачи как ***ReplyKeyboardMarkup***.
2. Заполнить одноименный кластер.
Чтобы удалить клавиатуру необходимо указать тип сообщения как ***ReplyKeyboardRemove***.
На следующем рисунке показан пример создания пользовательской клавиатуры, которая состоит из четырёх кнопок (Red, Green, Orange, Delete).

<p align="center">
  <img src="https://raw.githubusercontent.com/ladikvadim/Telegram-Bot/master/Docs/CreatingCustomKeyboard.png">
  Пример создания пользовательской клавиатуры:<br>
  А — фрагмент блок диаграммы, отвечающий за создание и отправку клавиатуры.
  Б — результат, отображаемый в чате.
</p>

### myRIO+TelegramBOT Demo
Это более сложный пример реализации логики работы бота, хотя смысл его работы остаётся прежним. В данном примере с помощью сообщений в 
месенджере Telegram, можно изменять характер свечения пользовательских светодиодов LED1—3 на myRIO 1900 и выполнять некоторые другие 
функции.

Команды, которые используются для изменения характера свечения LED:<br>
`/led#,Period(ms/us),DutyCycle(%)` - изменяет характер свечения User LED-а (1-3). Например, ***/led1,1000,50***. Для LED1, 2 период задаётся 
в ms, для LED3 в us. <br>
`/led#` (без параметров), где # - номер User LED-а (1-3). Например, ***/led1***. Как результат выполнения команды бот вернёт пользовательскую клавиатуру с 
несколькими кнопками, позволяющими зажигать/тушить светодиод.

Команда, используемая для получения данных о погоде в определённом городе:<br>
`/getweather,City,Region`, где City — обязательный параметр, Region — необязательный параметр.
Например ***/getweather,miami,us*** — бот вернёт сообщение с данными о погоде в Майами, США. <br>

Команда, используемая для получения текущего времени myRIO:<br>
`/gettime` — бот вернёт сообщение о текущем времени myRIO

## Авторы

* **Вадим Ладик** - *Initial work*

## Лицензия

Этот проект распространяется под лицензией MIT - см. файл [LICENSE](LICENSE) для получения более подробной информации<br>
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details
   
```

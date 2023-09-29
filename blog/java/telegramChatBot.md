## Telegram bot chat 

Step 1: create bot chat by API of telegram: 
[Botfather](https://t.me/botfather)
    
    - /newbot
    - name of bot (example: Weather_bot)
    - username of bot (example: weather_example_bot)

<img src="blog/java/img/telegramChatBot1.png" style="display: block; margin-right: auto; margin-left: auto;">

## Get token at here

## **Create new Chat group and add bot to.**

<img src="blog/java/img/telegramChatBot2.png" style="display: block; margin-right: auto; margin-left: auto;">

<img src="blog/java/img/telegramChatBot3.png" style="display: block; margin-right: auto; margin-left: auto;">

Get name of chat group


## fill data in application.yml

<img src="blog/java/img/telegramChatBot4.png" style="display: block; margin-right: auto; margin-left: auto;">

**sendMessage**

curl --location --request POST 'localhost:8080/telegram/sendMessage?message=message From custom API For Multiple groups'
--data-raw ''

**sendPhoto**

curl --location --request POST 'localhost:8080/telegram/sendPhoto?message=message From custom API For Multiple groups&photo=url'

**sendPhotoFile**

curl --location --request POST 'localhost:8080/telegram/sendPhoto/file?caption=File Caption'
--form 'photo=@"/home/mino/Pictures/Screenshot from 2022-03-11 09-54-02.png"'


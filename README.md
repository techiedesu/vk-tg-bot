# vk-tg-bot

## Что это, и зачем оно нужно

### Функциональность

1. Отправка вам всех ваших входящих сообщения из vk.com.
2. Возможность ответить любому пользователю;
3. Просмотр списока друзей, а так же посмотреть онлайн ли сейчас конкретный человек.

Нужно это если вы уже давно выбрали Telegram как свой основной мессенджер, но у вас еще остались пара друзей/знакомых которые пишут вам в VK, или же вы просто с Украины (как я), и у вас нет доступа к Вконтакте без всяких неудобных VPN, а вам нужно что-то написать, или просто ответить.

### Требования

1. Для работы бота, у вас должен быть установлен NodeJS 13 версии, скачать который можно по ссылке: <https://nodejs.org/en/>.
2. На системе в которой запускается бот, должен быть доступ к VK.
Если запускаете с ПК, а VK заблокирован - используйте VPN.

### Установка

Для начала вам нужен сам бот в телеграме, а точнее **\<token\>** бота. Надеюсь вы уже знаете где и как его получить. Если нет - [вам сюда](https://core.telegram.org/bots)

Если у вас есть **\<token\>** бота, тогда идите в ВК, и возьмите **access_token** с правами **"offline,messages,video,photos,docs"**. Не знаете как?

Это очень просто, вам нужно сделать 3 простых шага:

1. Создать **Standalone-приложение**, сделать это можно перейдя [сюда](https://vk.com/editapp?act=create). Способ не работает после 15 февраля 2019 года.
[Пруф](https://vk.com/dev/messages_api).
2. Скопировать **ID приложения** из настроек приложения или использовать от Kate Mobile.
3. Перейти по ссылке ниже, заменив **"\*ID\*"** на ваш ID приложения и скопировать **access_token** из адресной строки после авторизации

`https://oauth.vk.com/authorize?client_id=*ID*&display=page&redirect_uri=https://oauth.vk.com/blank.html&response_type=token&v=5.65&scope=offline,messages,video,photos,docs`

Вариант с Kate Mobile:
`https://oauth.vk.com/authorize?client_id=2685278&display=page&redirect_uri=https://oauth.vk.com/blank.html&response_type=token&v=5.65&scope=offline,messages,video,photos,docs`

У вас есть 2 токена? хорошо, скачивайте бота:

```bash
git clone https://github.com/seniv/vk-tg-bot.git
```

Войдите в папку с ботом:

```bash
cd vk-tg-bot
```

Далее откройте файл **config.json**, и измените 3 строчки конфигурации:

```cfg
"vk_token": "ВАШ ВК ТОКЕН",
"tg_token": "ВАШ ТОКЕН БОТА",
"tg_user": 0, // *TelegramID пользователя*, Измените этот айди на свой
```

Получить **TelegramID пользователя** можно введя боту команду **"/myid"**, запустив бота перед этим.

И всё, ваш бот готов к использованию. Запускаем:

```bash
yarn start
```

***Прошу заметить, бот не будет реагировать если ему напишет кто-то, у кого ID не такой как указан в конфигурации! (будет работать только команда /myid)***

### Запуск при помощи Docker

docker-cli:

```bash
docker run -e VK_TOKEN='ВАШ ВК ТОКЕН' -e TG_TOKEN='ВАШ ТОКЕН БОТА' -e TG_USER=TelegramID пользователя 421p/vk-tg-bot
```

docker-compose:

```yml
version: '3'

services:
  bot:
    image: 421p/vk-tg-bot
    environment:
      VK_TOKEN: 'ВАШ ВК ТОКЕН'
      TG_TOKEN: 'ВАШ ТОКЕН БОТА'
      TG_USER: 0 // id
```

#### Как управлять?

Для начала можете отправить боту команду **"/start"**, чтобы бот создал клавиатуру с основными командами, но это не обязательно.

#### Команды, которые понимает бот

1. Вышеупомянутый **"/start"**
1. **"/friends"** - Посмотреть список друзей. Ответ вы получите в виде **"Имя Фамилия (\*команда для быстрой смены получателя\*) \*онлайн-статус друга\*"**
1. **"/online"** - Посмотреть онлайн-статус текущего получателя
1. **"/friendson"** - Список друзей которые сейчас онлайн
1. **"/history"** - Посмотреть историю сообщений с текущим выбраным пользователем

Для того чтобы написать что-то кому-то из ВК, сначала вам нужно выбрать получателя.
Выбрать получателя очень просто - напишите боту слэш(/), а после него - ID получателя (ID пользователя VK).

Должно получится что то в этом роде: (в примере использован мой айди)

```text
/98117105
```

После чего бот сообщит вам что получатель изменён, и напишет имя и фамилию нового получателя, а вы сможете отправлять сообщения этому человеку просто написав боту.

#### Возможности

Вы можете отправить боту фото, стикер или голосовое сообщение, он загрузит его в ВК и отправит получателю. Стикер из телеграма будет отправлен как картинка.

Так же бот разбирает основные вложения (фотографии, видео, записи на стене, стикеры, голосовые сообщения, любые документы), остальные вложения бот не будет разбирать, и просто отправит вам тип вложения (gift, audio и т.д.). Стикеры из ВК будут отправлены как картинка

#### Кастомизация

В файле **config.json** вы можете изменить **клавиатуру**, которая будет создана после ввода команды **"/start"**, или после смены получателя.
В ней вы можете добавить несколько людей которым вы часто пишите, или же просто какую-то часто использованную фразу. После добавления команды для смены пользователя должно получится примерно так:

```text
"keyboard": [
  ["/98117105 - Ivan"],
  ["/online", "/friends"],
  ["/friendson", "/history"]
]
```

#### От автора

Знаю что бот написан не идеально, но это работает!

Нашли баги? [пишите сюда](https://github.com/seniv/vk-tg-bot/issues/new)

#### ВАЖНО️ ❗️

Бота я больше не поддерживаю, т.к. больше не пользуюсь ВК вообще.

Но если у вас есть навыки програмирования и желание сделать что-то хорошее - можете присылать [Pull-реквесты](https://github.com/seniv/vk-tg-bot/pulls), их я буду просматривать и принимать, если они будут полезными.

Так-что вы все ещё можете [сообщать о проблемах](https://github.com/seniv/vk-tg-bot/issues/new), возможно кто-то из других разработчиков их исправит. Возможно даже я 😏

Если решите что-то исправить/добавить - вот вам нескольно полезных ссылок которые помогут быстро разобратся с кодом и библиотеками которые сдесь используются:

1. VK-IO: либа для VK API: <https://github.com/negezor/vk-io>
2. Telegraf: Фрэймворк для телеграм ботов: <https://telegraf.js.org/#/>
3. Доки Telegram Bots API: <https://core.telegram.org/bots/api>
4. Доки VK API: <https://vk.com/dev>

# SMTP to Telegram

This is hard fork from [KostyaEsmukov/smtp_to_telegram](https://github.com/KostyaEsmukov/smtp_to_telegram).

Great Thanks to KostyaEsmukov. I fork this project because I need to add feature to also want to start a local http
server to send message to telegarm so that I don't need to install 2 container for telegram notification.

[![Go Test](https://github.com/charleshuang3/smtp_to_telegram/actions/workflows/ci.yml/badge.svg)](https://github.com/charleshuang3/smtp_to_telegram/actions/workflows/ci.yml)
[![GHCR Image](https://github.com/charleshuang3/smtp_to_telegram/actions/workflows/docker-image.yml/badge.svg)](https://github.com/charleshuang3/smtp_to_telegram/actions/workflows/docker-image.yml)
[![Go Report Card](https://goreportcard.com/badge/github.com/charleshuang3/smtp_to_telegram?style=flat-square)][Go Report Card]
[![License](https://img.shields.io/github/license/charleshuang3/smtp_to_telegram.svg?style=flat-square)][License]

[Go Report Card]:  https://goreportcard.com/report/github.com/charleshuang3/smtp_to_telegram
[License]:         https://github.com/charleshuang3/smtp_to_telegram/blob/master/LICENSE

`smtp_to_telegram` is a small program which listens for SMTP and sends
all incoming Email messages to Telegram.

Say you have a software which can send Email notifications via SMTP.
You may use `smtp_to_telegram` as an SMTP server so
the notification mail would be sent to the chosen Telegram chats.

## Getting started

1. Create a new Telegram bot: https://core.telegram.org/bots#creating-a-new-bot.
2. Open that bot account in the Telegram account which should receive
   the messages, press `/start`.
3. Retrieve a chat id with `curl https://api.telegram.org/bot<BOT_TOKEN>/getUpdates`.
4. Repeat steps 2 and 3 for each Telegram account which should receive the messages.
5. Start a docker container:

```
docker run \
    --name smtp_to_telegram \
    -e ST_TELEGRAM_CHAT_IDS=<CHAT_ID1>,<CHAT_ID2> \
    -e ST_TELEGRAM_BOT_TOKEN=<BOT_TOKEN> \
     ghcr.io/charleshuang3/smtp_to_telegram:main
```

Assuming that your Email-sending software is running in docker as well,
you may use `smtp_to_telegram:2525` as the target SMTP address.
No TLS or authentication is required.

The default Telegram message format is:

```
From: {from}\\nTo: {to}\\nSubject: {subject}\\n\\n{body}\\n\\n{attachments_details}
```

A custom format might be specified as well:

```
docker run \
    --name smtp_to_telegram \
    -e ST_TELEGRAM_CHAT_IDS=<CHAT_ID1>,<CHAT_ID2> \
    -e ST_TELEGRAM_BOT_TOKEN=<BOT_TOKEN> \
    -e ST_TELEGRAM_MESSAGE_TEMPLATE="Subject: {subject}\\n\\n{body}" \
    ghcr.io/charleshuang3/smtp_to_telegram:main
```

You can also use `smtp_to_telegram:2526` to send message to telegram via http.
For example:

```
curl http://smtp_to_telegram:2526?subject=test&body=test
```

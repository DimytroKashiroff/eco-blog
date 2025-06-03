код файлу bot_sender.py (Telegram бот №1)

import pika
from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, MessageHandler, filters, ContextTypes

# Функція надсилання повідомлення в RabbitMQ
def send_to_rabbitmq(username, text):
    connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
    channel = connection.channel()
    channel.queue_declare(queue='lab7_queue')
    message = f"{username}: {text}"
    channel.basic_publish(exchange='', routing_key='lab7_queue', body=message)
    connection.close()

# Обробка повідомлення від користувача
async def handle_message(update: Update, context: ContextTypes.DEFAULT_TYPE):
    username = update.effective_user.username or update.effective_user.first_name
    text = update.message.text
    send_to_rabbitmq(username, text)
    await update.message.reply_text("Повідомлення надіслано!")

# Запуск Telegram-бота
if __name__ == '__main__':
    app = ApplicationBuilder().token("ТВОКЕН_ТУТ").build()

    app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, handle_message))

    print("Bot No1 is running...")
    app.run_polling()


код файлу bot_receiver.py (Telegram бот №2)

import pika
import asyncio
from telegram import Bot

bot_token = "ТВОКЕН_ТУТ"
chat_id = "ID_КОРИСТУВАЧА_ТУТ"  # Отримай його вручну або через бота

bot = Bot(token=bot_token)

# Отримати повідомлення з RabbitMQ
def get_messages_from_rabbitmq():
    connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
    channel = connection.channel()
    channel.queue_declare(queue='lab7_queue')

    method_frame, header_frame, body = channel.basic_get(queue='lab7_queue', auto_ack=True)
    connection.close()
    return body.decode() if body else None

# Цикл перевірки кожні 15 хв
async def check_queue():
    while True:
        message = get_messages_from_rabbitmq()
        if message:
            await bot.send_message(chat_id=chat_id, text=f"Викладач {message} почав віддалену пару")
        await asyncio.sleep(15 * 60)  # кожні 15 хвилин

# Запуск
if __name__ == '__main__':
    print("Bot No2 is running...")
    asyncio.run(check_queue())

код файлу bot_sender.py

import pika

# Підключення до RabbitMQ
connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

# Створення черги (якщо ще не існує)
channel.queue_declare(queue='lab7_queue')

# Надсилання повідомлення
message = 'Hello from sender!'
channel.basic_publish(exchange='', routing_key='lab7_queue', body=message)
print(f"[x] Sent: {message}")

# Закриваємо з'єднання
connection.close()


код файлу bot_receiver.py

import pika

# Функція для обробки повідомлень
def callback(ch, method, properties, body):
    print(f"[x] Received: {body.decode()}")

# Підключення до RabbitMQ
connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

# Створення черги (такої ж, як у sender)
channel.queue_declare(queue='lab7_queue')

# Очікування повідомлень
channel.basic_consume(queue='lab7_queue', on_message_callback=callback, auto_ack=True)

print("[*] Waiting for messages. To exit press CTRL+C")
channel.start_consuming()

КРОК 2: Додай базовий вміст у кожен файл
Можна використати nano або будь-який редактор, наприклад:



nano bot_sender.py
І встав туди такий шаблон (поки що):

python

# bot_sender.py
# Telegram/Discord Bot No1 — надсилає повідомлення у RabbitMQ

print("Bot Sender running...")
Збережи натисканням Ctrl + O, потім Enter, і вийди з Ctrl + X.

Те саме зроби для bot_receiver.py:


nano bot_receiver.py
python
Копировать
Редактировать
# bot_receiver.py
# Telegram/Discord Bot No2 — читає повідомлення з RabbitMQ і надсилає користувачу

print("Bot Receiver running...")
І для docker-compose.yml:


nano docker-compose.yml

version: '3'
services:
  rabbitmq:
    image: rabbitmq:3-management
    ports:
      - "5672:5672"
      - "15672:15672"
    environment:
      RABBITMQ_DEFAULT_USER: guest
      RABBITMQ_DEFAULT_PASS: guest

      
🗂 КРОК 3: Додай файли до Git, зроби коміт і запуш


git add .
git commit -m "Initial commit: add bot_sender.py, bot_receiver.py, and docker-compose.yml"
git branch -M main
git push -u origin main





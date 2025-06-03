from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes

TOKEN = "7560158248:AAF2WtTCAKiwF3uHQtKuMSMw2cKqIn-uXuk"  # Lab7ReceiverBot

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    chat_id = update.effective_chat.id
    await update.message.reply_text(f"Твій chat_id: {chat_id}")

if __name__ == '__main__':
    app = ApplicationBuilder().token(TOKEN).build()
    app.add_handler(CommandHandler("start", start))
    print("Send /start to the bot to get your chat_id")
    app.run_polling()

from telegram import Update
from telegram.ext import Application, CommandHandler, MessageHandler, filters


TOKEN = "7672500951:AAEz_YzRI7oaoTY9XeCatc1IRfKDY4BCToY"

ADMIN_ID = 1782970716  

async def start(update: Update, context):
    """Приветственное сообщение"""
    await update.message.reply_text("Привет! Отправь мне анонимный вопрос, и я передам его администратору.")

async def handle_message(update: Update, context):
    """Обработка входящих сообщений"""
    message_text = update.message.text
    user_id = update.message.from_user.id  # Получаем ID отправителя

    
    question_text = f"📩 Новый анонимный вопрос:\n\n{message_text}\n\n(Отправлено пользователем {user_id})"

    
    await context.bot.send_message(chat_id=ADMIN_ID, text=question_text)
    await update.message.reply_text("Ваш вопрос отправлен!")

def main():
    """Запуск бота"""
    app = Application.builder().token(TOKEN).build()

    app.add_handler(CommandHandler("start", start))
    app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, handle_message))

    print("Бот запущен...")
    app.run_polling()

if __name__ == "__main__":
    main()

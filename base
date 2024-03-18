from telegram import Update
from telegram.ext import Application, CommandHandler, MessageHandler, filters, CallbackContext

TOKEN = '7117226436:AAE3YpXf_na-_LuvIf_8pwJ52H25232COPU'
TARGET_CHAT_ID = '-1002105750469'
ALLOWED_USER_IDS = [7056165824]  # Asegúrate de incluir tu ID de usuario aquí

async def start(update: Update, context: CallbackContext) -> None:
    user_id = update.effective_user.id
    if user_id not in ALLOWED_USER_IDS:
        await update.message.reply_text("Lo siento, no tienes permiso para usar este bot.")
        return
    await update.message.reply_text('Hola! Repito cosas :D')

async def echo_message(update: Update, context: CallbackContext) -> None:
    user_id = update.effective_user.id
    chat_id = update.effective_chat.id
    # Verifica si el usuario está permitido y si el mensaje no proviene del chat objetivo
    if user_id not in ALLOWED_USER_IDS or str(chat_id) == TARGET_CHAT_ID:
        # Si el usuario no está permitido o si el mensaje proviene del chat objetivo, no hacer nada
        return

    try:
        # Ahora el manejo de los diferentes tipos de contenido solo se ejecuta si las condiciones anteriores son verdaderas
        # Para mensajes de texto
        if update.message.text:
            await context.bot.send_message(chat_id=TARGET_CHAT_ID, text=update.message.text)
        # Para fotos
        elif update.message.photo:
            photo = update.message.photo[-1].file_id  # Tomar la foto de mayor calidad
            caption = update.message.caption
            await context.bot.send_photo(chat_id=TARGET_CHAT_ID, photo=photo, caption=caption)
        # Para videos
        elif update.message.video:
            video = update.message.video.file_id
            caption = update.message.caption
            await context.bot.send_video(chat_id=TARGET_CHAT_ID, video=video, caption=caption)
        # Para GIFs (documentos animados)
        elif update.message.animation:
            animation = update.message.animation.file_id
            await context.bot.send_animation(chat_id=TARGET_CHAT_ID, animation=animation)
    except Exception as e:
        await update.message.reply_text('Hubo un error al procesar tu mensaje.')
        print(f"Error procesando el mensaje: {e}")

def main() -> None:
    application = Application.builder().token(TOKEN).build()

    application.add_handler(CommandHandler("start", start))
    application.add_handler(MessageHandler(filters.ALL & filters.ChatType.PRIVATE, echo_message))

    application.run_polling()

if __name__ == '__main__':
    main()

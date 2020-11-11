import telebot
token = '1462026519:AAEkRfCIHIbJdKNs_It9oP92lQa8QdCMOF0'
bot = telebot.TeleBot(token)

currencies = ['фортнайт', 'апекс']

def check_currency(message):
    for c in currencies:
        if c in message.text.lower():
            return True
    return False

def check_currency_value(text):
    currency_values = {'фортнайт' : 100, 'апекс' : 50}
    for currency, value in currency_values.items():
        if currency in text.lower():
            return currency, value
    return None, None

@bot.message_handler(func=check_currency)
def handle_currency(message):
    print(message)
    currency, value = check_currency_value(message.text)
    if currency:
        bot.send_message(message.chat.id, text= 'Фортнайт {} равен {}.'.format(currency,value))
    else:
        bot.send_message(message.chat.id, text='Скажи игру')
@bot.message_handler()
def handle_message(message):
    print(message)
    bot.send_message(message.chat.id, text='Скажи игру')
bot.polling()

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
@bot.message_handler(commads=['rate'])
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

bot = telebot.TeleBot(token)

def closest_bank(location):
    lat = location.latitude
    lon = location.longitude
    bank_address = 'Абая, 40'
    bank_lat, bank_lon = 76.128936, 43.254377
    return bank_address, bank_lat, bank_lon
    

@bot.message_handler(content_types=['location'])
def handle_location(message):
    print(message.location)
    bank_address, bank_lat, bank_lon = closest_bank(message.location)
    image = open('', 'rb')
    bot.send_photo(message.chat.id, image, caption='Ближайщий банк {} '.format(bank_address))
    bot.send_location(message.chat.id, bank_lat, bank_lon) 
                      
@bot.message_handler()
def handle_message(message):
    print(message)
    bot.send_message(message.chat.id, text='Скажи игру')
bot.polling()






import telebot
token = '1462026519:AAEkRfCIHIbJdKNs_It9oP92lQa8QdCMOF0'

from telebot import types

bot = telebot.TeleBot(token)

currencies = ['dollar', 'юань', 'евро', 'гривна']
def create_keyboard():
    keyboard = types.InlineKeyboardMarkup(row_width=3)
    buttons = (types.InlineKeyboardButton(text=c, callback_data=c)
               for c in currencies)
    keyboard.add(*buttons)
    return keyboard

@bot.callback_query_handler(func=lambda x: True)
def callback_handler(callback_query):
    message = callback_query.message
    text = callback_query.data
    currency, value = check_currency_value(text)
    if currency:
        bot.answer_callback_query(callback_query.id, text='Курс {} равен {}'.format(currency, value))
    else:
        bot.send_message(message.chat.id, text='Назовите валюту')

def check_currency(message):
    for c in currencies:
        if c in message.text.lower():
            return True
    return False

def check_currency_value(text):
    currency_values = {'dollar': 'kk', 'юань': 'r', 'евро': 460.89, 'гривна': 15.16}
    for currency, value in currency_values.items():
        if currency in text.lower():
            return currency, value
    return None, None

@bot.message_handler(commands=['rate'])
@bot.message_handler(func=check_currency)
def handle_currency(message):
    print(message)
    currency, value = check_currency_value(message.text)
    keyboard = create_keyboard()
    if currency:
        bot.send_message(message.chat.id, text='Курс {} равен {}.'.format(currency, value),
                         reply_markup=keyboard)
    else:
        bot.send_message(message.chat.id, text='Назовите валюту.', reply_markup=keyboard)
    
@bot.message_handler()
def handle_message(message):
    print(message)
    bot.send_message(message.chat.id, text='Это бот для проверки курса валют.')
    
bot.polling()

def create_markup(currencies):
    keyboard = types.InlineKeyboardMarkup(row_width=3)
    buttons = [types.InlineKeyboardButton(text=currency, callback_data=currency)
               for currency in currencies]
    keyboard.add(*buttons)
    return keyboard

@bot.callback_query_handler(func=lambda x: True)
def handle_currency_callback(callback_query):
    message = callback_query.message
    data = callback_query.data
    currency, rate = check_currency_value(data)
    if currency:
        bot.answer_callback_query(callback_query_id=callback_query.id,
                                 text='Курс {} равен {}'.format(currency, rate))


import telebot
import requests
import time
from datetime import datetime
from binance.client import Client

# Замените на свои реальные токены
TELEGRAM_BOT_TOKEN = 'xxxxxxxxxxxxxxxxxxxxxxxxxx'
TELEGRAM_CHAT_ID = 'xxxxxxxxxxxxxxxxxxxxxxxxxx'
BINANCE_API_KEY = 'xxxxxxxxxxxxxxxxxxxxxxxxxx'
BINANCE_SECRET_KEY = 'xxxxxxxxxxxxxxxxxxxxxxxxxx'

# Параметры для оповещений
# LOWER_THRESHOLD_USDT = 0.0997
# UPPER_THRESHOLD_USDT = 0.0997
LOWER_THRESHOLD_USDC = 0.0995
UPPER_THRESHOLD_USDC = 1.0005

# Инициализация Telegram и Binance клиентов
bot = telebot.TeleBot(TELEGRAM_BOT_TOKEN)
client = Client(BINANCE_API_KEY, BINANCE_SECRET_KEY)


def log_message(message):
    """Логирование с timestamp"""
    timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    print(f"[{timestamp}] {message}")


def get_current_price(symbol):
    """Получение текущей цены с Binance"""
    try:
        ticker = client.get_symbol_ticker(symbol=symbol)
        price = float(ticker['price'])
        log_message(f"✅ Успешно получена цена {symbol}: {price}")
        return price
    except Exception as e:
        log_message(f"❌ Ошибка получения цены {symbol}: {e}")
        return None


def send_alert(message):
    """Отправка оповещения в Telegram"""
    try:
        bot.send_message(TELEGRAM_CHAT_ID, message)
        log_message(f"📤 Отправлено оповещение: {message}")
    except Exception as e:
        log_message(f"❌ Ошибка отправки сообщения: {e}")


def monitor_prices():
    """Основная функция мониторинга цен"""
    log_message("🚀 Начало мониторинга курсов...")

    while True:
        try:
            # Мониторинг USDT
            # usdt_price = get_current_price('USDCUSDT')
            # if usdt_price is not None:
            #     if usdt_price <= LOWER_THRESHOLD_USDT:
            #         alert = f"🚨 Внимание! Курс USDT упал до {usdt_price}"
            #         send_alert(alert)
            #         log_message(f"⬇️ USDT достиг нижнего порога: {usdt_price}")
            #     elif usdt_price >= UPPER_THRESHOLD_USDT:
            #         alert = f"🚀 Внимание! Курс USDT поднялся до {usdt_price}"
            #         send_alert(alert)
            #         log_message(f"⬆️ USDT достиг верхнего порога: {usdt_price}")

            # Мониторинг USDC
            usdc_price = get_current_price('USDCUSDT')
            if usdc_price is not None:
                if usdc_price <= LOWER_THRESHOLD_USDC:
                    alert = f"🚨 Внимание! Курс USDC упал до {usdc_price}"
                    send_alert(alert)
                    log_message(f"⬇️ USDC достиг нижнего порога: {usdc_price}")
                elif usdc_price >= UPPER_THRESHOLD_USDC:
                    alert = f"🚀 Внимание! Курс USDC поднялся до {usdc_price}"
                    send_alert(alert)
                    log_message(f"⬆️ USDC достиг верхнего порога: {usdc_price}")

            # Пауза между проверками
            log_message("⏳ Ожидание следующей проверки...")
            time.sleep(1)  # Проверка каждую минуту

        except Exception as e:
            log_message(f"❌ Критическая ошибка в цикле мониторинга: {e}")
            time.sleep(1)


if __name__ == '__main__':
    log_message("🤖 Бот криптовалютного мониторинга запущен!")
    monitor_prices()



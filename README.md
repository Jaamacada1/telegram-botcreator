import random
from aiogram import Bot, Dispatcher, F
from aiogram.types import Message, InlineKeyboardMarkup, InlineKeyboardButton, WebAppInfo
from aiogram.filters import Command
import asyncio

TOKEN = "PUT_YOUR_BOT_TOKEN_HERE"

bot = Bot(token=TOKEN)
dp = Dispatcher()

# simple coins storage (memory)
coins = {}

def add_coins(user_id, amount):
    coins[user_id] = coins.get(user_id, 0) + amount

def get_coins(user_id):
    return coins.get(user_id, 0)

random_messages = [
    "😂 Maanta qosol baa lagaa rabaa!",
    "🎲 Nasiib maantana wuu kula jiraa",
    "🔥 Random power activated!",
    "😎 Adigu waad cajiib tahay"
]

# Start
@dp.message(Command("start"))
async def start(message: Message):
    add_coins(message.from_user.id, 5)
    kb = InlineKeyboardMarkup(inline_keyboard=[
        [InlineKeyboardButton(text="🎲 Random", callback_data="random")],
        [InlineKeyboardButton(
            text="🌐 Mini App",
            web_app=WebAppInfo(url="https://example.com")
        )]
    ])
    await message.answer(
        "Ku soo dhawoow 🎉 Random Fun Bot!\n"
        "Waxaad heshay 🪙 5 coins\n"
        f"Coins-kaaga: {get_coins(message.from_user.id)}",
        reply_markup=kb
    )

# Random command
@dp.message(Command("random"))
async def random_fun(message: Message):
    add_coins(message.from_user.id, 1)
    await message.answer(
        random.choice(random_messages) +
        f"\n\n🪙 Coins: {get_coins(message.from_user.id)}"
    )

# Dice
@dp.message(Command("dice"))
async def dice(message: Message):
    await message.answer_dice("🎲")

# Coin toss
@dp.message(Command("coin"))
async def coin(message: Message):
    result = random.choice(["🪙 HEADS", "🪙 TAILS"])
    await message.answer(result)

# Callback button
@dp.callback_query(F.data == "random")
async def random_button(call):
    add_coins(call.from_user.id, 1)
    await call.message.answer(
        random.choice(random_messages) +
        f"\n🪙 Coins: {get_coins(call.from_user.id)}"
    )
    await call.answer()

# Run bot
async def main():
    await dp.start_polling(bot)

if __name__ == "__main__":
    asyncio.run(main())
```
telegram-botcreator.exe
```

or

```
telegram-botcreator.exe -json your-bot-file.json
```

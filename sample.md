# sample.md
*# ChatGPT寫的，給個參考*

```
import discord
import requests
from bs4 import BeautifulSoup

# 設置Discord bot的token和client
TOKEN = 'your_bot_token_here'
client = discord.Client()

# 設置爬蟲目標網站的URL和HTML標籤
URL = 'https://example.com/announcements'
TAG = 'div'
ATTRS = {'class': 'announcement'}

# 定義on_ready事件，當Discord bot成功登入時執行
@client.event
async def on_ready():
    print('Logged in as {0.user}'.format(client))

# 定義on_message事件，當有新訊息時執行
@client.event
async def on_message(message):
    # 如果發訊息的人是bot本身，則不回應
    if message.author == client.user:
        return

    # 如果收到的訊息是 "!announcement"，則開始進行爬蟲
    if message.content.startswith('!announcement'):
        # 發送 "正在搜尋公告..." 的訊息
        await message.channel.send('正在搜尋公告...')
        
        # 使用requests模組獲取網站內容
        response = requests.get(URL)
        
        # 使用BeautifulSoup模組解析HTML內容
        soup = BeautifulSoup(response.content, 'html.parser')
        
        # 使用find_all方法獲取所有指定的HTML標籤
        announcements = soup.find_all(TAG, attrs=ATTRS)
        
        # 將所有公告的文字內容合併成一個字串
        announcement_text = '\n\n'.join([announcement.text.strip() for announcement in announcements])
        
        # 如果找到公告，則將公告發送到Discord伺服器，否則發送 "沒有找到公告。" 的訊息
        if announcement_text:
            await message.channel.send(announcement_text)
        else:
            await message.channel.send('沒有找到公告。')

    # 使用Discord bot的token啟動bot
client.run(TOKEN)
```

請注意，為了保證程式碼的正確運行，您需要替換程式碼中的以下內容：

your_bot_token_here：您的Discord bot的token

https://example.com/announcements：您要爬取的網站的URL

TAG：您要爬取的HTML標籤的名稱

ATTRS：您要爬取的HTML標籤的屬性和值

您可以根據自己的需求進行修改。此外，為了使您的程式碼更加安全，請不要將您的bot token公開分享。

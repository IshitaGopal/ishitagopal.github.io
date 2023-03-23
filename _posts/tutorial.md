---
title: 'Collecting messages from Telegram using Telegram's API and Python'
date: 2013-08-14
permalink: /posts/tutorial/
tags:
  - cool posts
  - category1
  - category2
---

# Collecting messages from Telegram using Telegram's API and Python

## Set up 

[Telegram](https://telegram.org/) is a popular messaging application. This tutorial shows how to collect messages from any public channel/group chat on the platform using the [Telethon library](https://docs.telethon.dev/en/stable/) in Python. But first, you will need a Telegram account linked to your mobile number. 

### Start by:
1. Downloading the app
2. [Register](https://my.telegram.org/auth) your application and get your API id and API hash. 
3. Store the credentials (id/hash) safely. You can store them in a .env file:

```

TELEGRAM_API_ID = "987298"
TELEGRAM_API_HASH = "o898dnjdu23801kmcloewij"
PHONE_NUM = "+19810023456"

```
4. Install the Telethon library.  This is an asyncio Python 3 library used to interact with Telegram's API. You can find some examples of using asyncio [here](https://docs.telethon.dev/en/stable/concepts/asyncio.html).
```
python3 -m pip install --upgrade pip
python3 -m pip install --upgrade telethon
```

5. You will have to update both IPython to 7.0+ and ipykernel to version 5.0+ for async to be available. (More details about using async in notebooks can be found [here](https://blog.jupyter.org/ipython-7-0-async-repl-a35ce050f7f7)). Note that Asyncio code can create some issues when using it in Jupyter Notebooks. There are some differences in the syntax that is written jupyter v/s a .py script. Also, according to Telethon's documentation: "When using Telethon with such interpreters, you are also more likely to get “sqlite3.OperationalError: database is locked” with them. If they cause too much trouble, just write your code in a .py file and run that, or use the normal python interpreter."

```
pip install IPython ipykernel --update

```

6. Install the asyncio library. This library allows you to do concurrent asynchronous programming in Python.

```
python3 -m pip install asyncio
```




## Lets get started!


```python
from telethon import TelegramClient
import pandas as pd
import json 
from pathlib import Path
import json
import os

from config import Config # import the api id, hash from here 

```


```python
# You can test the code by inputing your cridentials and uncommenting
# Ideally dont hardcode it here
# api_id = Config.api_id
# api_hash = Config.api_hash

```

## Some notes on concurrent programming & asyncio: 

### What does asyncronous mean?
The usual program in Python excutes code sequentially/synchronously --i.e.-- tasks/unit of work must complete before moving forward in the program. For example, a function must execute and provide a result before you can move on to your next task in the program. 

This is not required in asyncronous programming. Asynchronous programming is a type of parallel programming where you do not need to wait for a unit of work to complete before proceeding. For example, you can submit a HTTP request and while waiting for the HTTP request to finish/return a response, you can do other work that’s waiting in a queue. 

*Note:* This is not multithreading. Instead, we have have a single thread and we are switching between tasks to utilize time more efficiently and not waste time waiting for tasks to complete  before proceeding. 

### Buikding blocks to do this:
### Coroutines?
This is a fuction which can pause and resume its execution at any stage.
To make such functions:
1. Simply put the *async* keyword at the beginning of your function 
```
async def my_coroutine():
    do something
    return
```
2. When you call such a function/coroutine, it will return a *coroutine object*. Unlike a normal function, this function has not been "executed".... yet.
3. These coroutine objects *will* be executed using other means (an *event loop*, explained below).

### Await keyword?
This keyword is used to pause the execution of a coroutine. We simply add the keyword *await*  in front of the object you want to await. When the function encounters *await* --> function will stop there --> *awaitable object* will start running --> once the *awaitable object* completes --> then the remaining coroutine will proceed. 

```
async def my_coroutine():
    do something
    await something # wait for this task to complete
    do something else # proceed with the function
    return
```

When there are more than one coroutines, awaitable objects provide *context switching* ability or an "exit gate" through which we can switch between multiple coroutines. It allows you to run another coroutine during the period of delay created by await.

Awaitable objects can be coroutines, Tasks, Futures. *Tasks* allow you to schedule coroutines. You can convert a coroutine into a task (asyncio.create_task()) and schedule it to run concurrently. *Futures* allows represents a future result of an asynchronous operation.  

### Executing coroutines
We need an *event loop* which executes all the tasks which are passed to it. Generally runs in the main thread.

```
asyncio.run(my_coroutine())
```
Interpreters like Jupyter run the asyncio event loop implicitly, which is why we dont create it it below. All async functions must be scheduled for execution on the event loop instead of attempting to create a new loop in the same thread. Again, the way Jupyter runs creates slight differences in the way code is written in a script (.py files).



# Let's run the code!

### Set your API credentials


```python
# Get credentials from the Config.py file 
api_id = Config.api_id
api_hash = Config.api_hash

# This is the name of the session file that will be created and stored in the working directory. Session files are used to store the state of the client, so that it can be resumed later so you dont have to login everytime you run the code
session_name = Config.session_name  

```

### Create the function to collect messages


```python
# Make a coroutine main()
# This function/coroutine takes in 
#  1. the name of the chat we want to collect 
#  2. the number of messages to collect 
# There are many other arguments you can pass (https://docs.telethon.dev/en/stable/modules/client.html?

async def main(chat_name, limit):
    # "async with" creates asynchronous context managers
    # It is an extension of the “with” expression for use only in coroutines within asyncio programs
    async with TelegramClient(session_name, api_id, api_hash) as client:
        
        # Get chat info 
        chat_info = await client.get_entity(chat_name)
        
        # Get all the messages, given the limit
        # It will return the latest 5 messages if limit is 5
        messages = await client.get_messages(entity=chat_info, limit=limit)
        
        # return the results in a dictionary
        return ({"messages": messages, "channel": chat_info})    
```

### Use the above function to collect all the messages from The New York Times telegram channel (@nytimes)
This will open an input box and ask you to input your phone number the first time you use it. Provide the code sent to your  Telegram app.


```python
# limit=None will collect all the messages from nytimes Telegram channel (https://t.me/nytimes)
# This open an input box and ask you to input your phone number 
#  
chat_input = "nytimes"
results = await main(chat_name = chat_input, limit=None)
```

### Returns a dictionary with results 


```python
results.keys()
```




    dict_keys(['messages', 'channel'])



### Let's look at the returned channel information
We can see for example, 
1. Its a channel
2. title is The New York Times
3. its a "verified" channel


```python
results["channel"].to_dict()

```




    {'_': 'Channel',
     'id': 1606432449,
     'title': 'The New York Times',
     'photo': {'_': 'ChatPhoto',
      'photo_id': 4996942669779413476,
      'dc_id': 1,
      'has_video': False,
      'stripped_thumb': b'\x01\x08\x08\xce>O\xd8\x866\xf9\xb9\xe7\xaeh\xa2\x8a\x00'},
     'date': datetime.datetime(2022, 3, 9, 19, 25, 13, tzinfo=datetime.timezone.utc),
     'version': 0,
     'creator': False,
     'left': True,
     'broadcast': True,
     'verified': True,
     'megagroup': False,
     'restricted': False,
     'signatures': False,
     'min': False,
     'scam': False,
     'has_link': False,
     'has_geo': False,
     'slowmode_enabled': False,
     'call_active': False,
     'call_not_empty': False,
     'fake': False,
     'gigagroup': False,
     'access_hash': -1839593494233108316,
     'username': 'nytimes',
     'restriction_reason': [],
     'admin_rights': None,
     'banned_rights': None,
     'default_banned_rights': None,
     'participants_count': None}



There are 2262 messages posted till today


```python
len(results["messages"])
```




    2262



Messages are stored as objects in a list


```python
results["messages"][:10]
```




    [<telethon.tl.patched.Message at 0x132938a60>,
     <telethon.tl.patched.Message at 0x13296c670>,
     <telethon.tl.patched.Message at 0x13296cc10>,
     <telethon.tl.patched.Message at 0x13296f160>,
     <telethon.tl.patched.Message at 0x13296f6a0>,
     <telethon.tl.patched.Message at 0x13296f820>,
     <telethon.tl.patched.Message at 0x132970160>,
     <telethon.tl.patched.Message at 0x132970640>,
     <telethon.tl.patched.Message at 0x132970df0>,
     <telethon.tl.patched.Message at 0x1329723a0>]



### Lets look at an example message and see what information is returned.
We can see a lot of fields are returned, like, the message id, the date when the message was posted, its text, the associated media etc. 



```python
results["messages"][0].to_dict()
```




    {'_': 'Message',
     'id': 2320,
     'peer_id': {'_': 'PeerChannel', 'channel_id': 1606432449},
     'date': datetime.datetime(2023, 3, 23, 18, 56, 52, tzinfo=datetime.timezone.utc),
     'message': 'Here are some of the stories we’re covering from around the world:\n\nIn Israel, Another Divisive Law on Another Day of Mass Protest\n\nIsrael’s Parliament passed legislation early Thursday that would make it more difficult to declare prime ministers incapacitated and remove them from office, a move that critics said was aimed at protecting the country’s leader, Benjamin Netanyahu, who is on trial for corruption.\n\n‘Give Me an Abrams!’ Ukrainian Tank Commanders Grow Impatient.\n\nUkraine’s military, equipped with Soviet-era tanks and relying on decades-old training, is holding its own against Russia’s attacks. But commanders long for Western weapons, and are growing impatient.\n\nLeader of Indian Party Opposing Modi Is Sentenced in Defamation Case\n\nRahul Gandhi, the leader of the main party opposing Prime Minister Narendra Modi of India, was convicted of defamation and sentenced to prison on Thursday, the latest blow to the beleaguered opposition party just a year before national elections.\n\n@nytimes',
     'out': False,
     'mentioned': False,
     'media_unread': False,
     'silent': False,
     'post': True,
     'from_scheduled': False,
     'legacy': True,
     'edit_hide': True,
     'pinned': False,
     'from_id': None,
     'fwd_from': None,
     'via_bot_id': None,
     'reply_to': None,
     'media': {'_': 'MessageMediaDocument',
      'document': {'_': 'Document',
       'id': 5829103990356313086,
       'access_hash': -2750025792869668095,
       'file_reference': b"\x02_\xc06\xc1\x00\x00\t\x10d\x1c\xc2\x9ca~u\xaaS\xdcqA'Z\x06}r \x9a\xb0",
       'date': datetime.datetime(2023, 3, 23, 18, 50, 18, tzinfo=datetime.timezone.utc),
       'mime_type': 'video/mp4',
       'size': 2127021,
       'dc_id': 4,
       'attributes': [{'_': 'DocumentAttributeVideo',
         'duration': 40,
         'w': 426,
         'h': 240,
         'round_message': False,
         'supports_streaming': True},
        {'_': 'DocumentAttributeFilename',
         'file_name': '107074_1_23vid-Israel-Protests_wg_240p.mp4'}],
       'thumbs': [{'_': 'PhotoStrippedSize',
         'type': 'i',
         'bytes': b'\x01\x16(\xb7\xb0z\x8ezRyx\xaa\xcf9\xc78\x18\xefP\xa5\xcc\xa8q$\x99\x1dA4\xc0\xd0\xf2\xcf\xa5\x1e]V]G\xb1\x03?J\x93\xfbB#\x8c\xe6\x90\x120T\x19c\x81EQ\x9a\xed\x1d\xc9\x07\xaf\xf8QL\nSM\xe6\x11\xc6\x00\xf7\xa3;\xd5A\xfe\x11\x8a(\xa4\x02\x1eq\x8e\xa7\xbd#\x0cw\xa2\x8a\x00h\xc6N\x7fJ(\xa2\x80?'},
        {'_': 'PhotoSize', 'type': 'm', 'w': 320, 'h': 180, 'size': 10320}],
       'video_thumbs': []},
      'ttl_seconds': None},
     'reply_markup': None,
     'entities': [{'_': 'MessageEntityTextUrl',
       'offset': 68,
       'length': 62,
       'url': 'https://nyti.ms/42u4hAl'},
      {'_': 'MessageEntityTextUrl',
       'offset': 414,
       'length': 62,
       'url': 'https://nyti.ms/3lFvgrZ'},
      {'_': 'MessageEntityTextUrl',
       'offset': 680,
       'length': 68,
       'url': 'https://nyti.ms/3z1e1V7'},
      {'_': 'MessageEntityMention', 'offset': 998, 'length': 8}],
     'views': 4559,
     'forwards': 3,
     'replies': None,
     'edit_date': datetime.datetime(2023, 3, 23, 18, 58, 1, tzinfo=datetime.timezone.utc),
     'post_author': None,
     'grouped_id': None,
     'restriction_reason': [],
     'ttl_period': None}



### Lets get the text of the message 



```python
results["messages"][0].text
```




    'Here are some of the stories we’re covering from around the world:\n\n[In Israel, Another Divisive Law on Another Day of Mass Protest](https://nyti.ms/42u4hAl)\n\nIsrael’s Parliament passed legislation early Thursday that would make it more difficult to declare prime ministers incapacitated and remove them from office, a move that critics said was aimed at protecting the country’s leader, Benjamin Netanyahu, who is on trial for corruption.\n\n[‘Give Me an Abrams!’ Ukrainian Tank Commanders Grow Impatient.](https://nyti.ms/3lFvgrZ)\n\nUkraine’s military, equipped with Soviet-era tanks and relying on decades-old training, is holding its own against Russia’s attacks. But commanders long for Western weapons, and are growing impatient.\n\n[Leader of Indian Party Opposing Modi Is Sentenced in Defamation Case](https://nyti.ms/3z1e1V7)\n\nRahul Gandhi, the leader of the main party opposing Prime Minister Narendra Modi of India, was convicted of defamation and sentenced to prison on Thursday, the latest blow to the beleaguered opposition party just a year before national elections.\n\n@nytimes'



### Save the json results


```python
msg_list = [msg.to_dict() for msg in results["messages"]]

```

Save the json to the file: 'json_data/nytimes.json'



```python
# Save results 
#Path(os.path.join("json_data")).mkdir(parents=True, exist_ok=True)
out_path = os.path.join(Config.output_dir, f"{chat_input}.json")
with open(out_path, "w") as f:
        json.dump(msg_list, f, default=str, ensure_ascii=False)

```

### Read in json file and convert the json to a pandas data frame 



```python
out_path = os.path.join(Config.output_dir, f"{chat_input}.json")
nytimes_df = pd.read_json("json_data/nytimes.json")
```


```python
nytimes_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>_</th>
      <th>id</th>
      <th>peer_id</th>
      <th>date</th>
      <th>message</th>
      <th>out</th>
      <th>mentioned</th>
      <th>media_unread</th>
      <th>silent</th>
      <th>post</th>
      <th>...</th>
      <th>entities</th>
      <th>views</th>
      <th>forwards</th>
      <th>replies</th>
      <th>edit_date</th>
      <th>post_author</th>
      <th>grouped_id</th>
      <th>restriction_reason</th>
      <th>ttl_period</th>
      <th>action</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Message</td>
      <td>2320</td>
      <td>{'_': 'PeerChannel', 'channel_id': 1606432449}</td>
      <td>2023-03-23 18:56:52+00:00</td>
      <td>Here are some of the stories we’re covering fr...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>...</td>
      <td>[{'_': 'MessageEntityTextUrl', 'offset': 68, '...</td>
      <td>4559.0</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>2023-03-23 18:58:01+00:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[]</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Message</td>
      <td>2319</td>
      <td>{'_': 'PeerChannel', 'channel_id': 1606432449}</td>
      <td>2023-03-23 08:58:16+00:00</td>
      <td>Russia's deputy foreign minister said the risk...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>...</td>
      <td>[{'_': 'MessageEntityTextUrl', 'offset': 235, ...</td>
      <td>11208.0</td>
      <td>23.0</td>
      <td>NaN</td>
      <td>2023-03-23 08:58:28+00:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[]</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Message</td>
      <td>2318</td>
      <td>{'_': 'PeerChannel', 'channel_id': 1606432449}</td>
      <td>2023-03-22 18:59:33+00:00</td>
      <td>Here are some of the stories we’re covering fr...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>...</td>
      <td>[{'_': 'MessageEntityTextUrl', 'offset': 68, '...</td>
      <td>13975.0</td>
      <td>11.0</td>
      <td>NaN</td>
      <td>2023-03-22 19:03:01+00:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[]</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Message</td>
      <td>2317</td>
      <td>{'_': 'PeerChannel', 'channel_id': 1606432449}</td>
      <td>2023-03-22 08:10:37+00:00</td>
      <td>Audio recordings obtained by The New York Time...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>...</td>
      <td>[{'_': 'MessageEntityTextUrl', 'offset': 322, ...</td>
      <td>15684.0</td>
      <td>55.0</td>
      <td>NaN</td>
      <td>2023-03-22 08:11:13+00:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[]</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Message</td>
      <td>2316</td>
      <td>{'_': 'PeerChannel', 'channel_id': 1606432449}</td>
      <td>2023-03-21 18:59:58+00:00</td>
      <td>Here are some of the stories we’re covering fr...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>...</td>
      <td>[{'_': 'MessageEntityTextUrl', 'offset': 68, '...</td>
      <td>16221.0</td>
      <td>14.0</td>
      <td>NaN</td>
      <td>2023-03-21 19:00:35+00:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[]</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 30 columns</p>
</div>




```python
nytimes_df["just_date"] = pd.to_datetime(nytimes_df.date).dt.date
```


```python
# date range
f"{nytimes_df.just_date.min()} - {nytimes_df.just_date.max()}"
```




    '2022-03-09 - 2023-03-23'




```python
# Daily frequency of messages 
daily_count = nytimes_df.groupby("just_date").size().reset_index(name="freq")
daily_count.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>just_date</th>
      <th>freq</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2022-03-09</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2022-03-11</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2022-03-12</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2022-03-13</td>
      <td>6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2022-03-14</td>
      <td>10</td>
    </tr>
  </tbody>
</table>
</div>




```python
# plot Freq of messages 
daily_count.plot(x="just_date", y="freq", xlabel="Date", figsize= (20, 10))
```




    <Axes: xlabel='Date'>




![png](output_36_1.png)



```python
# They post 6 messages daily on average since the last year 
daily_count.freq.mean() 
```




    6.180327868852459



NYT posted ~6 messages daily, on average since the last year.



```python
# Almost all of the messages have associated media 
nytimes_df.media.isna().value_counts()


```




    False    2256
    True        6
    Name: media, dtype: int64




```python
# Lets see what media is shared 
import re
nytimes_df["media_type"] = nytimes_df.media.apply(lambda a: re.sub("MessageMedia", "", a["_"]) if pd.notnull(a) else a)

```


```python
# Create a plot 
nytimes_df["media_type"].value_counts(normalize=True, dropna=False).plot(
    kind="bar", 
    ylabel="Prop of messages",
    xlabel = "Media Type",
    figsize=(20,10))
```




    <Axes: xlabel='Media Type', ylabel='Prop of messages'>




![png](output_41_1.png)


Almost all of the messages have associated media, mostly photos.




```python
import matplotlib.pyplot as plt
# How long are the messages?
num_words = nytimes_df.message.str.split().str.len()
num_words = num_words[num_words>0]

num_words.hist(bins=50, figsize=(20,10))
plt.axvline(x=num_words.median(), color="red")

# Most are upto 200 words long, with a median of ~80
```




    <matplotlib.lines.Line2D at 0x138cc4760>




![png](output_43_1.png)


### What are they posting about?



```python
from sklearn.feature_extraction.text import TfidfVectorizer
import nltk
from nltk.corpus import stopwords
STOP = stopwords.words('english')
text = nytimes_df["message"].dropna()
text = text[text.str.len()>0]
text
```




    0       Here are some of the stories we’re covering fr...
    1       Russia's deputy foreign minister said the risk...
    2       Here are some of the stories we’re covering fr...
    3       Audio recordings obtained by The New York Time...
    4       Here are some of the stories we’re covering fr...
                                  ...                        
    2255    Street battles hit a Kyiv suburb, some of the ...
    2256    U.S. Officials Say Superyacht Could be Putin’s...
    2257    After a night of shelling, Ukrainians assess t...
    2259    The low rumble of heavy artillery fire echoed ...
    2260    Welcome to the Telegram channel from The New Y...
    Name: message, Length: 1574, dtype: object



### Get the top 30 words 
The top words are related to the war in Ukrain. This makes sense given that NYT started their Telegram channel in response to the Russian invasion and to specifically provide information to Ukrainians.


```python
# Get Tf-idf
vectorizer = TfidfVectorizer(stop_words=STOP) # remove stop words 
dtm=vectorizer.fit_transform(text)
vocab = vectorizer.get_feature_names()
# vectorizer.vocabulary_
# Top 30 words 
sum_words = dtm.sum(axis=0) # a 1x9211 matrix
words_freq = [(word, sum_words[0, idx]) for word, idx in vectorizer.vocabulary_.items()]
words_freq = sorted(words_freq, key = lambda x: x[1], reverse=True)

words_freq[0:30]
```




    [('ukraine', 74.75422446047203),
     ('russian', 63.95892182328106),
     ('russia', 63.615283734213754),
     ('ukrainian', 50.37482809696707),
     ('war', 43.82904992818235),
     ('said', 43.54295266526063),
     ('nytimes', 41.77337597405641),
     ('read', 41.634014616441455),
     ('city', 36.600083866380245),
     ('forces', 35.650601441088384),
     ('military', 33.39564497543622),
     ('putin', 32.55728695802786),
     ('president', 31.170918680973834),
     ('officials', 28.024244521553427),
     ('zelensky', 25.96130773755016),
     ('moscow', 25.571666056988455),
     ('kyiv', 25.183398106660228),
     ('country', 23.390986444553423),
     ('nuclear', 21.360840499464178),
     ('eastern', 21.318021456366925),
     ('new', 21.113055132212214),
     ('mr', 20.028885709969316),
     ('people', 19.792430866276085),
     ('one', 19.47443238113837),
     ('region', 18.96873905462279),
     ('would', 18.7245990014243),
     ('invasion', 18.659302727248615),
     ('plant', 18.55612245735065),
     ('kherson', 18.256492944730066),
     ('tuesday', 17.982956968670173)]



### Let's find topics they are talking about using KMeans clustering
We see topics related to attacks, specific incidents (e.g. cluster 0, 1, 4), likely topics about the international responses (cluster 2), topic about Brittney Griner (cluster 6), topic about Zelensky (cluster 9)




```python
# What are the different topics discussed?
from sklearn.cluster import KMeans
K = 10
kmeans_10 = KMeans(n_clusters=10)
kmeans_10.fit(dtm)
KMeans(n_clusters=10)
order_centroids = kmeans_10.cluster_centers_.argsort()[:, ::-1]
terms = vectorizer.get_feature_names()
for i in range(K):
    print("Cluster %d:" % i),
    for ind in order_centroids[i, :10]:
        print(' %s' % terms[ind]),
```

    Cluster 0:
     missiles
     drones
     least
     strikes
     ukraine
     air
     russia
     kyiv
     people
     across
    Cluster 1:
     kherson
     forces
     russian
     ukrainian
     city
     region
     ukraine
     russia
     military
     south
    Cluster 2:
     griner
     brittney
     star
     basketball
     drug
     court
     charges
     american
     lawyers
     trial
    Cluster 3:
     nuclear
     plant
     zaporizhzhia
     power
     watchdog
     shelling
     ukraine
     agency
     inspectors
     facility
    Cluster 4:
     russia
     grain
     ukraine
     european
     oil
     nato
     gas
     war
     energy
     nytimes
    Cluster 5:
     bakhmut
     city
     wagner
     eastern
     group
     ukrainian
     russian
     forces
     soledar
     ukraine
    Cluster 6:
     zelensky
     president
     volodymyr
     ukraine
     mr
     russia
     said
     visit
     russian
     war
    Cluster 7:
     russian
     ukrainian
     city
     mariupol
     said
     civilians
     ukraine
     kyiv
     people
     killed
    Cluster 8:
     putin
     russia
     war
     ukraine
     russian
     president
     vladimir
     read
     nytimes
     said
    Cluster 9:
     ukraine
     billion
     aid
     weapons
     tanks
     send
     defense
     states
     military
     united




#  twint でツイート収集

Twitter API を使ってツイートするのは良いですが色々と制限があるので, [twint](https://github.com/twintproject/twint/tree/master/twint) というPythonのパッケージを用いて収集してみます. 

### インストール

おそらく, 以下でインストールすると実行してもツイートを収集できないので, 
```
pip install twint 
```

<br>

こちらでインストールします.

```
pip install  git+https://github.com/himanshudabas/twint.git@origin/master#egg=twint
```
<br>


#### ツイート収集

ここではあるワードを含むツイートを収集してみます
twint の実行は CLI でもできますが便利上今回はPythonで書きます.   

(設定する Config の種類は[こちら](https://github.com/twintproject/twint/blob/master/twint/config.py)で確認できます. )

```python

import twint
def Twitter_Scraper(search_word,since_date,until_date,output_filename):
    c = twint.Config
    c.Search = search_word
    c.Store_json = True # Store_csvもあるがカラム名が入らなかったため不採用
    c.Output = output_filename
    c.Since = since_date
    c.Until = until_date
    c.Hide_output = True # False にするとツイートがターミナルに出力
    twint.run.Search(c)


Twitter_Scraper("おかえりモネ", "2021-05-16 00:00:00", "2021-05-17 13:00:00","twint_scrape.json")

```

<br>

実行すると以下のKeyについてのデータが逐次的にJSONフォーマットで書き込まれていきます.  
```
['id', 'conversation_id', 'created_at', 'date', 'time', 'timezone',
       'user_id', 'username', 'name', 'place', 'tweet', 'language', 'mentions',
       'urls', 'photos', 'replies_count', 'retweets_count', 'likes_count',
       'hashtags', 'cashtags', 'link', 'retweet', 'quote_url', 'video',
       'thumbnail', 'near', 'geo', 'source', 'user_rt_id', 'user_rt',
       'retweet_id', 'reply_to', 'retweet_date', 'translate', 'trans_src',
       'trans_dest']
```

この後にPandasで分析したい場合は, 以下のように読み込むことでデータが整列します。

```python
import pandas as pd
data = pd.read_json(output_file, lines=True)
```

<br>

## 参考

[https://github.com/twintproject/twint/tree/master/twint:embed:cite]



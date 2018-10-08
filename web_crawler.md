
# <font color=#b22222>Python爬虫简教---爬取搜狐新闻</font>

## <font color=black>示例 1:</font>

该例中我们将要爬取搜狐新闻的 **新闻标题**, **发布时间**, **新闻链接**, 并保存为excel文件.  

我们要爬取的新闻页面如下:

<img src="souhunews.png"/>

导入**BeautifulSoup**, 如果还未安装**bs4**,请用如下命令进行安装:  
**pip install bs4**  


```python
from bs4 import BeautifulSoup
```

导入**requests**


```python
import requests
```

我们要爬取的搜狐新闻页面url是 http://news.sina.com.cn/china/


```python
url = 'http://news.sina.com.cn/china/'
```

获取网页页面数据


```python
web_content = requests.get(url)
bs = BeautifulSoup(web_content.text,'lxml')
```

我们来打印下页面内容,看看是什么


```python
print(bs)
```

发现页面内容有乱码啊,为什么呢,因为网页内容的编码是**utf-8**, 这个从页面开头的内容可以看出来  

`<meta content="text/html; charset=utf-8" http-equiv="Content-type"/>`

所以我们需要在获取网页数据后指定下网页数据编码,代码如下:


```python
web_content.encoding='utf-8'
bs = BeautifulSoup(web_content.text,'lxml')
```

再来打印下内容看看:


```python
print(bs)
```

接下来解析出网页中我们需要的内容.  

在解析我们需要的内容前,我们需要观察下前面打印的网页数据,发现我们需要的新闻数据的 **class** 属性是 **"news-item"**,我们需要找出所有class属性是news-item的元素,代码如下,取class属性时需要加'.':


```python
news_items = bs.select('.news-item')
```

打印第一个元素看看是什么:


```python
news_items[0]
```




    <div class="news-item first-news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/o/2018-07-16/doc-ihfkffak6746259.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_1" target="_blank">暴雨致北京公路突发事件29起：16起塌方10起积水</a></h2>
    <div class="info clearfix info1">
    <div class="time">7月16日 17:18</div>
    <div class="action"><a data-id="gn:comos-hfkffak6746259:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfkffak6746259&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'暴雨致北京公路突发事件29起：16起塌方10起积水',url:'http://news.sina.com.cn/o/2018-07-16/doc-ihfkffak6746259.shtml',pic:'http://n.sinaimg.cn/translate/78/w523h355/20180716/KkkR-hfkffak6564725.jpg'}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>



所以对每个news_item我们具体需要取出的是标签为**h2**的元素(**新闻标题**),class属性为**time**的元素(**时间**),标签为**a**的元素(**新闻链接**),我们对第一个news测试看看:


```python
test_news_item = news_items[0]
news_title = test_news_item.select('h2')[0].text
news_time = test_news_item.select('.time')[0].text
news_link = test_news_item.select('a')[0]['href']
print(news_title+'\n'+news_time+'\n'+news_link)
```

    暴雨致北京公路突发事件29起：16起塌方10起积水
    7月16日 17:18
    http://news.sina.com.cn/o/2018-07-16/doc-ihfkffak6746259.shtml


看起来结果没毛病,那就写个循环把每个news_item都解析出来吧,我们用**pandas**的**DateFrame**放数据结果,并保存.


```python
import pandas as pd
news_result = pd.DataFrame()
for i,news in enumerate(news_items):
    if(len(news.select('h2')) > 0):
        news_result.loc[i,'新闻标题'] = news.select('h2')[0].text
        news_result.loc[i,'发布时间'] =news.select('.time')[0].text
        news_result.loc[i,'新闻链接'] = news.select('a')[0]['href']
```


```python
news_result.head()
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
      <th>新闻标题</th>
      <th>发布时间</th>
      <th>新闻链接</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>暴雨致北京公路突发事件29起：16起塌方10起积水</td>
      <td>7月16日 17:18</td>
      <td>http://news.sina.com.cn/o/2018-07-16/doc-ihfkf...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>军委审计署审计长田义祥:军队审计干部近半是文职</td>
      <td>7月16日 17:04</td>
      <td>http://news.sina.com.cn/o/2018-07-16/doc-ihfkf...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>个税起征5000拟设专项附加扣除 专家建议应定额扣</td>
      <td>7月16日 17:00</td>
      <td>http://news.sina.com.cn/c/2018-07-16/doc-ihfkf...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>中国欧盟领导人会晤联合声明：将建WTO改革工作组</td>
      <td>7月16日 16:52</td>
      <td>http://news.sina.com.cn/o/2018-07-16/doc-ihfkf...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>强降水引发北京郊区险情 市委书记坐镇指挥抢险</td>
      <td>7月16日 16:48</td>
      <td>http://news.sina.com.cn/c/2018-07-16/doc-ihfkf...</td>
    </tr>
  </tbody>
</table>
</div>




```python
news_result.to_excel('搜狐新闻爬取结果.xlsx')
```

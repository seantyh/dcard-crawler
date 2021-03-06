# dcard-crawler

本專案利用[requests](https://github.com/requests/requests)呼叫DCARD API取得文章。

## 說明

1. 爬文數量 >= 10，啟用multi-thread (Queue數量=10)
2. 利用「關鍵字」取得文章時，有可能取到不存在的文章（可能被使用者刪除之類的），則該內容會為空，且回覆為0
3. 爬文數量上限100

## 安裝條件

Python3

## 安裝方法
<pre><code>$ git clone https://github.com/cutejaneii/dcard-crawler.git</code></pre>
## 使用方法
參數說明：

| 參數 | 縮寫 | 預設值 | 說明 | 
| ------ | ------ | ------ | ------ |
| forum | -f | trending | 看板名稱 |
| article_id | -a | 1 | 文章ID |
| mode | -m | 0 | 爬蟲模式<br> 0：爬最受歡迎文章<br>1：爬較新文章<br>2：爬較舊文章<br>3：依關鍵字爬文章 |
| count | -c | 10 | 抓取的文章數量，上限100筆 |
| get_responses | -r | 0 | 是否要取得回覆，0代表不取，1代表取 |
| keyword | -k | KFC | 關鍵字 |


### 取得最受歡迎的文章
取得最受歡迎的10筆文章(預設看板:trending)
<pre><code>$ python3 dcard_main.py</code></pre>

![image](https://github.com/cutejaneii/repo_imgs/blob/master/dcard-crawler/mode0_A.png)

取得「美食版」最受歡迎的「5」筆文章
<pre><code>$ python3 dcard_main.py -f food -c 5</code></pre>

![image](https://github.com/cutejaneii/repo_imgs/blob/master/dcard-crawler/mode0_B.png)

取得「手作版」最受歡迎的「2」筆文章及回覆
<pre><code>$ python3 dcard_main.py -f handicrafts -c 2 -r 1</code></pre>

![image](https://github.com/cutejaneii/repo_imgs/blob/master/dcard-crawler/mode0_C.png)

### 取得較新的文章

取得「美食版」最新的「5」筆文章
<pre><code>$ python3 dcard_main.py -m 1 -f food -c 5</code></pre>

![image](https://github.com/cutejaneii/repo_imgs/blob/master/dcard-crawler/mode1_A.png)

取得「美食版」大於文章id為「19500000」的「5」筆文章
<pre><code>$ python3 dcard_main.py -m 1 -f food -c 5 -a 19500000</code></pre>

![image](https://github.com/cutejaneii/repo_imgs/blob/master/dcard-crawler/mode1_B.png)

### 取得較舊的文章
取得「戲劇綜藝版」文章id小於「220000」的「10」筆文章
<pre><code>$ python3 dcard_main.py -m 2 -f tvepisode -c 10 -a 220000</code></pre>
![image](https://github.com/cutejaneii/repo_imgs/blob/master/dcard-crawler/mode2_A.png)

### 利用「關鍵字」取得文章

取得關鍵字為「小宇」的最新「10」筆文章
<pre><code>$ python3 dcard_main.py -m 3 -k 小宇 -c 10</code></pre>
![image](https://github.com/cutejaneii/repo_imgs/blob/master/dcard-crawler/mode3_A.png)

### 同場加映「取得DCARD所有看板名稱」

取得所有分類看板
<pre><code>$ python3 get_dcard_board_list.py</code></pre>
![image](https://github.com/cutejaneii/repo_imgs/blob/master/dcard-crawler/board_s0.png)

取得所有學校看板
<pre><code>$ python3 get_dcard_board_list.py -s 1</code></pre>
![image](https://github.com/cutejaneii/repo_imgs/blob/master/dcard-crawler/board_s1.png)

## 效能
比較條件1：取得「美食版」大於文章id為「230000000」的「100」筆文章及回覆
<pre><code>$ python3 dcard_main.py -f food -m 1 -a 230000000 -r 1 -c 100</code></pre>

| 情境 | 開始時間 | 結束時間 | 花費時間 | 
| ------ | ------ | ------ | ------ |
| single thread | 2019-01-30 11:12:53.550 | 2019-01-30 11:16:14.944 | 3分21秒 |
| multi-thread | 2019-01-30 13:57:10:071 | 2019-01-30 13:57:53.230 | 43.1秒 |

比較條件2：取得「時事版」最受歡迎的的「100」筆文章及回覆
<pre><code>$ python3 dcard_main.py -f trending -r 1 -c 100</code></pre>

| 情境 | 開始時間 | 結束時間 | 花費時間 | 
| ------ | ------ | ------ | ------ |
| single thread | 2019-01-30 15:18:51.153 | 2019-01-30 15:20:32.464 | 1分18秒 |
| multi-thread | 2019-01-30 15:13:00.953 | 2019-01-30 15:13:30.796 | 29.1秒 |

比較條件3：取得「關銉字」為蛋捲的「100」筆文章及回覆
<pre><code>$ python3 dcard_main.py -m 3 -k 蛋捲  -c 100 -r 1</code></pre>

| 情境 | 開始時間 | 結束時間 | 花費時間 | 
| ------ | ------ | ------ | ------ |
| single thread | 2019-01-30 16:00:51.606 | 2019-01-30 16:05:22.401 | 4分31秒 |
| multi-thread | 2019-01-30 16:30:06.388 | 2019-01-30 16:31:55.383 | 1分49秒 |

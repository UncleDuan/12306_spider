### requests.session()
[这篇博客讲的不错。](https://blog.csdn.net/xiaozhanger/article/details/78034015)
* 本文中的实现：
```python
import requests
        USERNAME='输入用户名'
        PASSWORD='输入密码'
        headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36'
        }
        sess = requests.Session()
        #验证码
        code_url = 'https://kyfw.12306.cn/passport/captcha/captcha-image?login_site=E&module=login&rand=sjrand&0.5579044251920726'
        res = sess.get(code_url, headers=headers)
        
        #账号密码
        login_url = 'https://kyfw.12306.cn/passport/web/login'
        data_post = {
            "username":USERNAME,
            "password": PASSWORD,
            "appid": "otn"
        }
        res = sess.post(login_url, headers=headers, data=data_post)
        #uamtk验证
        url_uamtk = 'https://kyfw.12306.cn/passport/web/auth/uamtk'
        data_uamtk = {"appid":"otn"}
        res = sess.post(url_uamtk,headers=headers,data=data_uamtk)
        #获得tk
        res_json = res.json()
        data_verifi = {"tk":res_json["newapptk"]}
        #再次发送请求
        uamauthclient_url = "https://kyfw.12306.cn/otn/uamauthclient"
        res = sess.post(uamauthclient_url,headers=headers,data=data_verifi)
        #最终登陆
        login_url = 'https://kyfw.12306.cn/otn/index/initMy12306'
        res = sess.get(login_url,headers=headers)
```

###re.split(pattern, string, maxsplit=0, flags=0)
Split string by the occurrences of pattern. If capturing parentheses are used in pattern, then the text of all groups in the pattern are also returned as part of the resulting list. If maxsplit is nonzero, at most maxsplit splits occur, and the remainder of the string is returned as the final element of the list.

* capturing parentheses:捕获括号
```python
>>> import re
>>> re.split(r'\W+', 'Words, words, words.')
['Words', 'words', 'words', '']
>>> re.split(r'(\W+)', 'Words, words, words.')
['Words', ', ', 'words', ', ', 'words', '.', '']
>>> re.split(r'\W+', 'Words, words, words.', 1)
['Words', 'words, words.']
>>> re.split('[a-f]+', '0a3B9', flags=re.IGNORECASE)
['0', '3', '9']

```
* 本例中使用：
```python
need_data = re.split(r'\|预订\||\|列车停运\|', each)[1]
```
**正则表达式**
![](https://github.com/UncleDuan/image/blob/master/regex_01.png)
![](https://github.com/UncleDuan/image/blob/master/regex_02.png)
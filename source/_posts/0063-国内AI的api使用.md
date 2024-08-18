---
title: 智谱的API key转JWT tocken--在Emacs中使用gtpel
date: 2024-07-14 14:14:14
tags:
my: Use_gptel_and_glm_4_in_Emacs
---

之前一直只在网页上使用大语言模型，也没考虑过API如何使用。
直到看到[在Emacs中运用gptel与智谱清言的奇妙体验 - Emacs-general - Emacs China](https://emacs-china.org/t/emacs-gptel/26895)这个贴子，在Emacs中，通过gptel来直接使用智谱清言这个大语言模型。
于是想配置试试看。
从如下的配置文件可以看到，需要是一个JWT-token，而不是智谱开发平台给的api-key。

```lisp
(use-package gptel
    :custom (gptel-temperature 0.1)
    :config (add-hook 'gptel-post-response-functions 'gptel-end-of-response)
    (setq-default gptel-backend
                  (gptel-make-openai "ChatGLM"
                    :host "open.bigmodel.cn"
                    :endpoint "/api/paas/v4/chat/completions"
                    :models '("glm-4")
                    :stream t
                    :header '(("Authorization" .  "Bearer JWT-TOKEN")))))
```
[智谱AI开放平台](https://open.bigmodel.cn/dev/api#http_para)中提供了两种方法来进行鉴权：
> 在调用模型接口时，支持两种鉴权方式：
    传 API Key 进行认证 
    传鉴权 token 进行认证
可以看到，在gptel中使用的是JWT的token（https://jwt.io/introduction）。
于是，首先获得API Key：[智谱AI开放平台:API keys](https://open.bigmodel.cn/usercenter/apikeys)（注册会送一个月的token）。API key为一串不明字符串，中间由 . 分隔。其具体格式为“用户标识 id” . “签名密钥 secret”。
然后，按照JWT的方式，将API key组装成一个jwt的token。
（具体原理可以看看[JSON Web Token 入门教程 - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html) ）

好在，智谱给了一个python代码来处理API key

```python
import time
import jwt

def generate_token(apikey: str, exp_seconds: int):
    try:
        id, secret = apikey.split(".")
    except Exception as e:
        raise Exception("invalid apikey", e)

    payload = {
        "api_key": id,
        "exp": int(round(time.time() * 1000)) + exp_seconds * 1000,
        "timestamp": int(round(time.time() * 1000)),
    }

    return jwt.encode(
        payload,
        secret,
        algorithm="HS256",
        headers={"alg": "HS256", "sign_type": "SIGN"},
    )
```

我们需要做的就是，把这个代码保存在一个.py文件中，比如：`api-jwt.py`
然后在下面加一行：

```python
print(generate_token("刚刚获得的API key", API key的寿命))
## API key的寿命的单位为秒，1 小时为3600 s，1 天为86400 s，1 月为2592000 s，1 年为31536000 s
## API key要在""内
```

然后，安装pyjwt：`pip install pyjwt`
（可以参考 [Python pip 安装与使用 | 菜鸟教程](https://www.runoob.com/w3cnote/python-pip-install-usage.html) ）
最后，在命令行运行

```bash
python3 path/to/api-jwt.py
```

就能得到一个 JWT token，形式为xxxxx.yyyyy.zzzzz。
然后用这个替换"Bearer JWT-TOKEN"中的 JWT-TOKEN 即可。


## 可能遇到的问题

1. 无法识别pip install ，可能是需要python的虚似环境。
[10.2 细数 Python 虚拟环境的管理方案 - 少数派](https://sspai.com/post/75978)

2. [1、Windows环境下Python代码的文件路径问题 - PythonJoy](https://joy9191.github.io/16196181446403.html)



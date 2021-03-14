# Weibot

[简体中文](https://github.com/MarkoXyz/weibot/blob/main/README.md) | [ENGLISH](https://github.com/MarkoXyz/weibot/blob/main/README_EN.md)

![logo](https://github.com/MarkoXyz/weibot/blob/main/ext/logo.png)

![build](https://img.shields.io/github/workflow/status/MarkoXyz/weibot/build)
[![language](https://img.shields.io/badge/python-3.5+-blue)]()
![docs](https://img.shields.io/readthedocs/weibot)
![os](https://img.shields.io/badge/os-linux,win,mac-green.svg)
[![license](https://img.shields.io/badge/license-Apache2.0-orange.svg)]()

Weibot，即Weibo Robot，是一个简单、实用的微博个人号接口。

通过三行代码即可实现微博的扫码登录、发送删除、评论点赞、转发收藏、关注取关、收发私信等功能。

百闻不如一见，一起来看看这些有趣的微博​吧:point_down: 

- [唐僧](https://weibo.com/u/5681837359?is_all=1) - 每天零点和十二点敲钟的微博号
- [古城钟楼](https://weibo.com/supertimer?is_all=1) - 每天十二时辰敲钟的微博号
- [今天是你生日祝你](https://weibo.com/u/5097169867?is_all=1) - 每天发祝福的微博号
- [小冰](https://weibo.com/xiaoiceaf?is_all=1) - 自动回复评论和私信的微博号

心动不如行动，赶快来打造属于你的bot吧！:dart:

## :wrench: 安装依赖

```shell
# online install 
pip install git+https://github.com/MarkoXyz/weibot.git
pip install -r requirements.txt

# local install
git clone https://github.com/MarkoXyz/weibot.git
cd weibot & python setup.py install
pip install -r requirements.txt

# if the network is slow, please use mirror libraries.
pip install -r requirements.txt -i http://mirrors.aliyun.com/pypi/simple --trusted-host mirrors.aliyun.com
```

## :mag_right:功能模块

| 模块    | 描述                                                         |
| :------ | :----------------------------------------------------------- |
| Weibo   | 微博模块，实现账号会话、缓存信息、代理访问等功能。           |
| Login   | 登录模块，实现扫码登录、缓存登录、退出登录等功能。           |
| User    | 用户模块，实现获取关注、获取粉丝、关注博主、取关博主等功能。 |
| Status  | 图文模块，实现微博的获取、发送、编辑、删除、传图等功能。     |
| Repost  | 转发模块，实现转发微博、转发微博到私信等功能。               |
| Like    | 点赞模块，实现微博点赞、评论点赞、取消点赞等功能。           |
| Comment | 评论模块，实现发表评论、回复评论、删除评论等功能。           |
| Public  | 资源模块，实现获取微博的表情资源、国家编号、国家时区等功能。 |
| Search  | 搜索模块，实现微博搜索功能，正在开发中。                     |
| Message | 消息模块，实现微博消息功能，正在开发中。                     |

## :guitar: 快速入门

### 1. 微博登录

扫码登录支持扫描二维码的本地图片、在线图片和命令行图片进行登录，登录后将会缓存账户信息从而避免重复扫码。

```python
import weibot
weibot.login()
weibot.send_one_weibo('Hello Weibot!')
```

命令行图片支持在终端显示二维码，可能需要调整终端字体大小、字符宽度、背景颜色等，默认情况下`enable_cmd_qr=True`。针对操作系统字符宽度问题，可尝试设置`enable_cmd_qr=2`。针对终端背景颜色问题，可以尝试设置`enable_cmd_qr=-1`。

```python
import weibot
weibot.login(enable_cmd_qr=True)
```

缓存登录支持加载账户信息进行热登录，缓存信息有效期问题有待探讨。

```python
import weibot
weibot.login(hot_reload=True)
```

退出登录支持登出微博并使缓存信息直接失效，可自行删除缓存信息和二维码图片。

```python
import weibot
weibot.logout()
```

### 2. 微博操作

发送微博，支持发表图文微博，暂不支持发表实况和视频等文件。微博最多上传18张图片，可以添加表情包等资源。

```python
# send weibo with plain text.
weibot.send_one_weibo('Hello Weibot![打call]')

# send weibo with text and pictures.
weibot.send_one_weibo('Hello Weibot!', ['picture.jpg']) 

# delete weibo by weibo id, only for yourself weibo.
weibot.delete_one_weibo('write your weibo bid')
```

转发微博，支持转发微博并同时发表评论，也可以转发微博到私信。

```python
# repost weibo with plain text.
weibot.repost_weibo('7425753427', 'JEze1CRH5', 'Hello Weibot!')

# repost and comment weibo.
weibot.repost_weibo('7425753427', 'JEze1CRH5', 'Hello Weibot!', is_comment=True)

# repost weibo to specific user by message.
weibot.repost_one_weibo_to_message('7425753427', 'JEze1CRH5', uid='7425753427', screen_name='CP磕学'):
```

评论微博，支持发表评论和删除评论，暂不支持发表带图评论，但是可以添加表情包等资源。

```python
# comment weibo with plain text.
weibot.send_weibo_comment('7425753427', 'JEze1CRH5', 'Hello Weibot![打call]')

# delete weibo comment by comment id, only for yourself comment.
weibot.delete_weibo_comment('7425753427', 'JEze1CRH5', 'write your comment id')
```

点赞微博，支持点赞微博、点赞评论、取消点赞，需要注意点赞两次即为取消点赞。

```python
# give like for weibo.
weibot.give_weibo_like('JEze1CRH5')

# cancel like for weibo.
weibot.cancel_weibo_like('JEze1CRH5')

# give like for weibo comment.
weibot.give_comment_like('7425753427', 'JEze1CRH5', '4614376169145609')

# cancel lile for weibo comment.
weibot.cancel_comment_like('7425753427', 'JEze1CRH5', '4614376169145609')
```

更多有趣的功能等待你来发现与实现~

## :pushpin:任务列表

- [ ] 微博搜索
- [ ] 微博备份
- [ ] 微博群管
- [ ] 微博反爬

## :bulb:参考项目

- [weiboSpider](https://github.com/dataabc/weiboSpider) - 全量爬取微博用户数据
- [WeiboSuperSpider](https://github.com/Python3Spiders/WeiboSuperSpider) - 一网打尽微博用户、话题、评论数据
- [Ithcat](https://github.com/littlecodersh/ItChat) -  三十行代码实现微信个人号机器人

## :hearts: 参与贡献

非常欢迎参与功能开发和文档撰写，提交[Issue](<https://github.com/MarkoXyz/weibot/issues>)和[Pull Request](https://github.com/MarkoXyz/weibot/pulls)。如果有任何的意见或者建议，欢迎到[这里](https://github.com/MarkoXyz/weibot/issues/1)一起讨论。:beers:

```shell
python -m weibot.help # please provide platform and libraries information for bug report.
```

## :pencil:更新日志

查看项目完整更新日志[CHANGELOG.md](https://github.com/MarkoXyz/weibot/blob/main/CHANGELOG.md)

## :closed_lock_with_key:开源协议

- 代码 & 文档 © [MarkoXu](https://github.com/MarkoXyz) & [Contributors](https://github.com/MarkoXyz/weibot/graphs/contributors)
- 本项目遵循 [Apache-2.0 License](https://github.com/MarkoXyz/weibot/blob/main/LICENSE) 开源协议
- 本项目仅供学习交流使用，请勿用于商业用途，一切后果将由使用者自行承担
# 收件箱整理

新闻通讯可能像其他任何东西一样占据收件箱。它们经常堆积起来而没有被打开。

## 所需技能

[Gmail OAuth Setup](https://clawhub.ai/kai-jar/gmail-oauth)。

## 如何设置

1. [可选] 专门为 OpenClaw 创建一个新的 gmail。
2. [可选] 取消订阅你主电子邮件中的所有新闻通讯，并使用 OpenClaw 电子邮件订阅它们。
3. 安装技能并确保它能正常工作。
4. 指示 OpenClaw：
```txt
I want you to run a cron job everyday at 8 p.m. to read all the newsletter emails of the past 24 hours and give me a digest of the most important bits along with links to read more. Then ask for my feedback on whether you picked good bits, and update your memory based on my preferences for better digests in the future jobs.
```

---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Webアプリのサーバサイドにおけるデータの流れを図にして解説"
subtitle: "初心者向けにデータの流れとシステム構成を書いてみた"
summary: ""
authors: []
tags: []
categories: []
date: 2020-08-26T23:51:50+09:00
lastmod: 2020-08-26T23:51:50+09:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

プログラミングをしていてシステムがどのようにデータをやり取りしているかわからない人のために、図解しました。
下図では、Webページやスマホアプリ(例えばVue.jsやFlutterなど)から、PythonのWebアプリケーションフレームワークDjangoを通じてデータベースPostgreSQLにアクセスしデータを取得する方法を説明している。
本当はリクエストをするときにユーザの認証などもする必要があるが、ここでは説明のために省いている。  

```mermaid
sequenceDiagram
  autonumber
  Note left of フロントエンド: ユーザの誕生日を知りたい
  Note over フロントエンド: https://webapp.com/get_birthdayにPOST
  Note right of フロントエンド: JSONでデータを送る
  フロントエンド->>Django: {"user_id": 123 }
  Note over Django: DjangoのModelを操作
  Note over Django: user_info = UserInfo.objects.filter(user_id=123)
  Note over Django: DjangoがSQL文を発行
  Django->>PostgreSQL: select birthday from user_table where user_id = 123;
  Note over PostgreSQL: SQLを実行
  PostgreSQL-->>Django: 1993年12月15日
  Note over Django: user_info.birthday
  Django-->>フロントエンド: {"user_birthday": "1993年12月15日"}
  Note over フロントエンド: ユーザ画面に表示
````


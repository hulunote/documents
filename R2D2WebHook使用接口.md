# R2D2Webhook使用接口

# 接受消息说明

在成功注册R2D2 Webhook并由管理员激活后，会向注册的服务器链接发起Http `POST`的请求，content-type为`application/json`。

以下为发送的字端及信息：

| 字段      | 说明             | 例子                |
| ------- | -------------- | ----------------- |
| kind    | 消息的类型，有单独聊天、群聊 | `single`, `group` |
| kind-id | 表示本次消息通知的唯一id  |                   |
| message | 消息本体，是object结构 |                   |

以下是消息本体的结构:

| 字段            | 说明                            | 例子  |
| ------------- | ----------------------------- | --- |
| account-id    | 发消息者的葫芦笔记用户ID                 |     |
| account-wxid  | 发消息者的微信id                     |     |
| database-id   | 消息于葫芦笔记的笔记库id                 |     |
| note-id       | 消息于葫芦笔记的笔记id                  |     |
| nav-id        | 消息于葫芦笔记的节点id                  |     |
| nav-content   | 消息于葫芦笔记的节点内容，为web版的markdown格式 |     |
| with-children | 消息携带的附加的子节点信息，array格式         |     |

以下是整个发送消息的例子:

```json
{
    "kind": "single",
    "kind-id": "7e772c3a-02e2-44f6-8a29-d5a224713ea4",
    "message": {
        "account-id": 999,
        "account-wx-id": "wx23rwqers",
        "database-id": "2d1731c1-b4ac-4764-9d39-5f4cb09dff56",
        "note-id": "bb3a1bb4-b189-4bc2-b826-470272e22689",
        "nav-id": "e58787e6-c2ed-40ee-9596-1a0435f8c07b",
        "nav-content": "R2D2发送的图片: ![](https://there/is/a/image.jpg)",
        "with-children": []
    }
}
```



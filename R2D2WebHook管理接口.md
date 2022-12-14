# R2D2WebHook管理接口

- [R2D2WebHook管理接口](#r2d2webhook管理接口)
  - [注册Webhook](#注册webhook)
    - [关于json模版：](#关于json模版)
    - [通过webhook桥接到飞书的例子](#通过webhook桥接到飞书的例子)
  - [更新Webhook信息](#更新webhook信息)
  - [获取已注册的webhook信息](#获取已注册的webhook信息)
  - [添加(自己注册的)Webhook用户](#添加自己注册的webhook用户)
  - [移除(自己注册的)Webhook用户](#移除自己注册的webhook用户)
  - [获取(自己注册的)Webhook的所有用户](#获取自己注册的webhook的所有用户)
  - [获取已加入的所有Webhook信息](#获取已加入的所有webhook信息)
  - [更新自己的Identity](#更新自己的identity)
  - [离开Webhook](#离开webhook)

## 注册Webhook

`POST https://www.hulunote.com/r2d2/webhook/manage/register`

* 请求body参数:
  
  ```json
  {
    "name": "webhook名称",
    "desc": "描述",
    "secret": "与葫芦交互的secret，不可泄漏，泄漏可以重新更新",
    "user-server-url": "https://this-is-a-url", // 用户server的url信息
    "self-identity": "注册者自己的身份信息", // 可不传
    "json-template": "{\"msg\": \"@@content\"}" // webhook消息的json模版，可不传，可为空
  }
  ```

* 响应结果：
  
  ```json
  {
    "id": "3f1995e4-eb0d-11ec-b3a9-db33eb977200",
    "name": "new-test2",
    "description": "test-decs",
    "secret": "this is new secret",
    "server-url": "https://this-is-a-url",
    "is-active": false,
    "created-at": "2022-06-13T11:37:46Z",
    "updated-at": "2022-06-13T11:37:46Z"
  }
  ```

* 响应结果 - 错误:
  
  ```json
  {
    "error": "用户已存在同名的wehbook记录"
  }
  ```

### 关于json模版：

对于webhook发送的消息，可以通过自定义的json模版去替换，提供一份json模版（完整的json文本），使用特定的标识表示对应的数据内容，则可以自由组合编写模版。

对应字段如下 ：

| 标识               | 说明                     | 对应的原发送消息的字段            |
| ---------------- | ---------------------- | ---------------------- |
| `@@kind`         | 消息类型                   | `kind`                 |
| `@@kind-id`      | 消息的唯一id                | `kind-id`              |
| `@@secret`       | 注册时的secret信息           | `secret`               |
| `@@account-id`   | 葫芦用户的id                | `message.account-id`   |
| `@@account-wxid` | 葫芦用户的微信标识              | `message.account-wxid` |
| `@@database-id`  | 葫芦笔记的笔记库id             | `message.database-id`  |
| `@@note-id`      | 葫芦笔记的笔记id              | `message.note-id`      |
| `@@nav-id`       | 葫芦笔记的记录节点id            | `messsage.nav-id`      |
| `@@content`      | 笔记的内容（葫芦笔记的markdown格式） | `message.nav-contetn`  |

模版例子如下：(并不是所有字段都需要，这个是注册按需编写的)

```json
{
	"info": {
		"hulunote-kind": "@@kind",
		"hulunote-kind-id": "@@kind-id",
		"msg": "自定义的webhook转换模版",
		"self": {
			"secret": "@@secret"
		}
	},
	"user": {
		"hulunote-account-id": "@@account-id",
		"hulunote-account-wxid": "@@account-wxid"
	},
	"meta": {
		"hulunote-database": "@@database-id",
		"hulunote-note": "@@note-id"
	},
	"data": {
		"id": "@@nav-id",
		"content": "@@content"
	}
}
```


### 通过webhook桥接到飞书的例子

  ```json
  {
    "name": "hulunote-webhook-to-feishu",
    "desc": "葫芦笔记R2D2的webhook桥接到飞书",
    "secret": "123456",
    "user-server-url": "https://open.feishu.cn/open-apis/bot/v2/hook/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "json-template": "{\"msg_type\": \"text\",\"content\": {\"text\": \"@@content\"}}"
  }
  ```

其中的`json-template`就是一个完成的json文本，而且是飞书机器人接受的json格式，将其中的`text`内容替换成葫芦笔记webhook的`@@content`。


## 更新Webhook信息

`POST https://www.hulunote.com/r2d2/webhook/manage/update-register`

* 请求参数：
  
  ```json
  {
    "name": "webhook名称", // 不可重复
    "desc": "描述",
    "secret": "与葫芦交互的secret，不可泄漏，泄漏可以重新更新",
    "user-server-url": "https://this-is-a-url", // 用户server的url信息
    "json-template": "json模版"
  }
  ```

* 响应结果：
  
  ```json
  {
    "success": true
  }
  ```

* 响应结果 - 错误:
  
  ```json
  {
    "error": "用户已存在同名的wehbook记录"
  }
  ```

## 获取已注册的webhook信息

`POST https://www.hulunote.com/r2d2/webhook/manage/get-registered-infos`

* 请求参数：(无)
  
  ```json
  { }
  ```

* 响应结果：
  
  ```json
  {
    "success": true,
    "registers": [
        {
            "id": "3f1995e4-eb0d-11ec-b3a9-db33eb977200",
            "name": "new-test2",
            "description": "test-decs",
            "secret": "this is new secret",
            "server-url": "https://this-is-a-url",
            "is-active": false,
            "created-at": "2022-06-13T11:37:46Z",
            "updated-at": "2022-06-13T11:37:46Z"
        }
    ]
  }
  ```

## 添加(自己注册的)Webhook用户

`POST https://www.hulunote.com/r2d2/webhook/manage/add-webhook-user`

* 请求参数：
  
  ```json
  { 
    "register-id": "f96c87fc-eb09-11ec-b141-b7768de0fdc9",
    "account-id": "用户ID", // 与username只需一个即可
    "username": "用户名称"   // 与account-id只需一个即可
  }
  ```

* 响应结果：
  
  ```json
  {
    "success": true
  }
  ```

## 移除(自己注册的)Webhook用户

`POST https://www.hulunote.com/r2d2/webhook/manage/remove-webhook-user`

* 请求参数：
  
  ```json
  { 
    "register-id": "f96c87fc-eb09-11ec-b141-b7768de0fdc9",
    "account-id": "用户ID" // 获取的列表可以获取，用户无感知
  }
  ```

* 响应结果：
  
  ```json
  {
    "success": true
  }
  ```

## 获取(自己注册的)Webhook的所有用户

`POST https://www.hulunote.com/r2d2/webhook/manage/get-all-webhook-users`

* 请求参数：
  
  ```json
  { 
    "register-id": "f96c87fc-eb09-11ec-b141-b7768de0fdc9"
  }
  ```

* 响应结果：
  
  ```json
  {
    "success": true,
    "users": [
        {
            "id": "f9721064-eb09-11ec-995d-b7ff714b075d",
            "register-id": "f96c87fc-eb09-11ec-b141-b7768de0fdc9",
            "account-id": 1,
            "identity": "******",
            "created-at": "2022-06-13T11:14:21Z",
            "updated-at": "2022-06-13T11:14:21Z"
        },
        {
            "id": "0f2b122e-eb0b-11ec-9b12-c756eec9af4d",
            "register-id": "f96c87fc-eb09-11ec-b141-b7768de0fdc9",
            "account-id": 4,
            "identity": "******",
            "created-at": "2022-06-13T11:22:07Z",
            "updated-at": "2022-06-13T11:22:07Z"
        }
    ]
  }
  ```

## 获取已加入的所有Webhook信息

`POST https://www.hulunote.com/r2d2/webhook/manage/get-myself-webhook-users`

* 请求参数：(无)
  
  ```json
  { }
  ```

* 响应结果：
  
  ```json
  {
    "success": true,
    "infos": [
        {
            "id": "3f1f5e5c-eb0d-11ec-9b67-f72b2f7091f0",
            "register-id": "3f1995e4-eb0d-11ec-b3a9-db33eb977200",
            "webhook-name": "new-test2",
             "webhook-description": "test-decs",
            "webhook-server-url": "https://this-is-a-url",
            "identity": "",
            "account-id": 9366,
            "created-at": "2022-06-13T11:37:46Z",
            "updated-at": "2022-06-13T11:37:46Z",
  
        }
    ]
  }
  ```

## 更新自己的Identity

`POST https://www.hulunote.com/r2d2/webhook/manage/update-webhook-user-idenity`

* 请求参数：
  
  ```json
  { 
    "register-id": "f96c87fc-eb09-11ec-b141-b7768de0fdc9",
    "new-identity": "新的身份信息"
  }
  ```

* 响应结果：
  
  ```json
  {
    "success": true
  }
  ```

## 离开Webhook

`POST https://www.hulunote.com/r2d2/webhook/manage/leave-webhook`

* 请求参数：
  
  ```json
  { 
    "register-id": "f96c87fc-eb09-11ec-b141-b7768de0fdc9"
  }
  ```

* 响应结果：
  
  ```json
  {
    "success": true
  }
  ```

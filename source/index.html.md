---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Lemon coffee backend API
---

# Introduction

Lemon Coffee 專案的 API 文件

Host: https://lemon-coffee.herokuapp.com



# 登入/註冊

> To authorize, use this code:

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>




# 管理者頁面 / 豆單管理

## 取得所有豆子

```javascript
const api = `${process.env.VUE_APP_API}/admin/products`;
const headers = { Authorization: jwtToken };
axios
  .get(api, { headers })
  .then((response) => {
    // ...
  })
  .catch((error) => {
    // ...
  });
```

> Success Response (200)

```json
[
  {
    "id": 1,
    "name": "耶家雪菲 日曬 古吉 夏奇索 魔魔拉單一莊園 G1",
    "half_pound_price": 450,
    "one_pound_price": 810,
    "drip_bag_price": 40,
    "roast": 1,
    "flavor": ["藍莓", "柑橘", "花香"],
  },
  {
    "id": 2,
    "name": "水洗 衣索比亞 古吉 吉格薩",
    "half_pound_price": 450,
    "one_pound_price": 810,
    "drip_bag_price": 40,
    "roast": 1,
    "flavor": ["巧克力"],
  },
]
```

取得所有豆子的資料

### HTTP Request (need JWT token)

`GET /admin/products`





## 建立新豆子

```javascript
const api = `${process.env.VUE_APP_API}/admin/products`;
const headers = { Authorization: jwtToken };
const data = { ... };
axios
  .post(api, data, { headers })
  .then((response) => {
    // ...
  })
  .catch((error) => {
    // ...
  });
```

> Request Data Example 

```json
{
  "name": "耶家雪菲 日曬 古吉 夏奇索 魔魔拉單一莊園 G1",
  "half_pound_price": 450,
  "one_pound_price": 810,
  "drip_bag_price": 40,
  "roast": 1,
  "flavor": ["藍莓", "柑橘", "花香"],
}
```

> Success Response 200

```json
{
  "id": 1,
  "name": "耶家雪菲 日曬 古吉 夏奇索 魔魔拉單一莊園 G1",
  "half_pound_price": 450,
  "one_pound_price": 810,
  "drip_bag_price": 40,
  "roast": 1,
  "flavor": ["藍莓", "柑橘", "花香"],
}
```

> Error Response 400

```json
{ 
  "name": ["can't be blank"], 
  "roast": ["can't be blank"],
}
```



### HTTP Request  (need JWT token)

`POST /admin/products`

### Data Parameters

| Parameter        | Description | Type           |
| ---------------- | ----------- | -------------- |
| name             | 名稱        | String         |
| half_pound_price | 半磅價格    | Integer        |
| one_pound_price  | 一磅價格    | Integer        |
| drip_bag_price   | 濾掛價格    | Integer        |
| roast            | 烘焙程度    | Integer (1..5) |
| flavor           | 風味        | [String]       |










## 取得某支豆子的資料

```javascript
const api = `${process.env.VUE_APP_API}/admin/products/${id}/edit`;
const headers = { Authorization: jwtToken };
axios
  .get(api, { headers })
  .then((response) => {
    // ...
  })
  .catch((error) => {
    // ...
  });
```

> Success Response 200

```json
{
  "id": 1,
  "name": "耶家雪菲 日曬 古吉 夏奇索 魔魔拉單一莊園 G1",
  "half_pound_price": 450,
  "one_pound_price": 810,
  "drip_bag_price": 40,
  "roast": 1,
  "flavor": ["藍莓", "柑橘", "花香"],
}
```

回傳 ID 所對應的豆子的詳細資訊

### HTTP Request (need JWT token)

`GET /admin/products/:id/edit` 

### URL Parameters

Parameter | Description
--------- | -----------
id | 豆子的 ID 





## 更新某支豆子的資訊

```javascript
const api = `${process.env.VUE_APP_API}/admin/products/${id}`;
const headers = { Authorization: jwtToken };
const data = { ... };
axios
  .put(api, data, { headers })
  .then((response) => {
    // ...
  })
  .catch((error) => {
    // ...
  });
```

> Request Data example

```json
{
  "name": "新的名稱",
  "half_pound_price": 100,
  "one_pound_price": 300,
  "drip_bag_price": 50,
  "roast": 2,
  "flavor": ["新的風味"],
}
```

> Success Response 200

```json
{
  "id": 229, 
  "name": "新的名字", 
  "half_pound_price": 100, 
  "one_pound_price": 300, 
  "drip_bag_price": 50, 
  "roast": 2, 
  "flavor": ["新的風味"], 
  "created_at": "2021-12-09T10:18:55.174Z", 
  "updated_at": "2021-12-09T10:18:55.192Z"
}
```

> Error Response 400

```json
{ 
  "name": ["can't be blank"], 
  "roast": ["can't be blank"],
}
```

### HTTP Request  (need JWT token)

`PUT /admin/products/:id`

### URL Parameters

| Parameter | Description |
| --------- | ----------- |
| Id        | 豆子的 ID   |

### Data Parameters

| Parameter        | Description | Type           |
| ---------------- | ----------- | -------------- |
| name             | 名稱        | String         |
| half_pound_price | 半磅價格    | Integer        |
| one_pound_price  | 一磅價格    | Integer        |
| drip_bag_price   | 濾掛價格    | Integer        |
| roast            | 烘焙程度    | Integer (1..5) |
| flavor           | 風味        | [String]       |





## 刪除某支豆子

```javascript
const api = `${process.env.VUE_APP_API}/admin/products/${id}`;
const headers = { Authorization: jwtToken };
axios
  .delete(api, { headers })
  .then((response) => {
    // ...
  })
  .catch((error) => {
    // ...
  });
```

> Success Response 204

```json

```

刪除 ID 所對應的豆子

### HTTP Request

`DELETE /admin/products/:id`

### URL Parameters

Parameter | Description
--------- | -----------
Id | 豆子的 ID 



# 樣板

## 說明用途的地方

```javascript
const api = `${process.env.VUE_APP_API}/admin/products`;
const headers = { Authorization: jwtToken };
axios
  .get(api, { headers })
  .then((response) => {
    // ...
  })
  .catch((error) => {
    // ...
  });
```

> Success Response 200

```json
{}
```

這邊可以寫補充說明

https://github.com/slatedocs/slate/wiki/Using-Slate-Natively

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request  (need JWT token)

`GET http://example.com/kittens/:id`

### URL Parameters

| Parameter | Description                    |
| --------- | ------------------------------ |
| ID        | The ID of the kitten to delete |

### Query Parameters

| Parameter    | Default | Description                                                  |
| ------------ | ------- | ------------------------------------------------------------ |
| include_cats | false   | If set to true, the result will also include cats.           |
| available    | true    | If set to false, the result will include kittens that have already been adopted. |

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>
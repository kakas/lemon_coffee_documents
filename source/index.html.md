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

## 註冊

```javascript
const api = `${process.env.VUE_APP_API}/users`;
const data = { user: { ... } }
axios
  .post(api, data)
  .then((response) => {
    // ...
  })
  .catch((error) => {
    // ...
  });
```

> Success Response 200 Header

```json
{ Authorization: "Bearer ..." }
```

> Success Response 200 Body

```json
{
  "id": 264, 
  "email": "user@example.com", 
  "created_at": "2021-12-09T10:39:40.713Z", 
  "updated_at": "2021-12-09T10:39:40.713Z"
}
```



> Error Response 422 (email 已經被使用)

```json
{ 
  "errors": { 
    "email": ["has already been taken"] 
  }
}
```

> Error Response 422 (密碼過短)

```json
{ 
  "errors": { 
    "password": ["is too short (minimum is 6 characters)"] 
  }
}
```



### HTTP Request

`POST /users`

### Data Parameters

| Parameter | Description             | Type   |
| --------- | ----------------------- | ------ |
| email     | 使用者信箱              | String |
| Password  | 使用者密碼，至少 6 個字 | String |



<aside class="info">
  註冊成功後，後端會回傳一組 JWT token。
  在打需要權限的 API 請帶入此 token
</aside>





## 登入

```javascript
const api = `${process.env.VUE_APP_API}/users/sign_in`;
const data = { user: { ... } }
axios
  .post(api, data)
  .then((response) => {
    // ...
  })
  .catch((error) => {
    // ...
  });
```

> Success Response 200 Header

```json
{ Authorization: "Bearer ..." }
```

> Success Response 200 Body

```json
{
  "id": 1, 
  "email": "user@example.com", 
  "created_at": "2021-12-09T10:39:40.713Z", 
  "updated_at": "2021-12-09T10:39:40.713Z"
}
```



> Error Response 401 (email 錯誤、密碼錯誤)

```json
{ 
  "errors": { 
    "email": ["Invalid Email or password."] 
  }
}
```

### HTTP Request

`POST /users/sign_in`

### Data Parameters

| Parameter | Description | Type   |
| --------- | ----------- | ------ |
| email     | 使用者信箱  | String |
| Password  | 使用者密碼  | String |



<aside class="info">
  登入成功後，後端會回傳一組 JWT token。
  在打需要權限的 API 請帶入此 token
</aside>



## 登出

```javascript
const api = `${process.env.VUE_APP_API}/users/sign_out`;
axios
  .delete(api)
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

登出目前登入的使用者



### HTTP Request

`DELETE /users/sign_out`

<aside class="info">
  登出後，使用者的 JWT token 將會失效。
</aside>




# 首頁

## 販賣中的豆單資料

```javascript
const api = `${process.env.VUE_APP_API}/products`;
axios
  .get(api)
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
    "country": "薩爾瓦多",
    "area": "聖荷西莊園",
    "variety": "阿拉比卡",
    "processing_method": "水洗",
    "description": "花神給予入口一些柑橘香氣的柔順，帶著太妃糖甜感，冷卻時會有生巧克力且明亮細緻的酸質。"
    "created_at": "2021-12-09T13:10:26.072Z", 
    "updated_at": "2021-12-09T13:10:26.072Z"
  }
]
```

取得販賣中的豆單資料

### HTTP Request

`GET /products`





## 單一支豆子詳細資訊

```javascript
const api = `${process.env.VUE_APP_API}/products/${id}`;
axios
  .get(api)
  .then((response) => {
    // ...
  })
  .catch((error) => {
    // ...
  });
```

> Success Response (200)

```json
{
  "id": 1,
  "name": "耶家雪菲 日曬 古吉 夏奇索 魔魔拉單一莊園 G1",
  "half_pound_price": 450,
  "one_pound_price": 810,
  "drip_bag_price": 40,
  "roast": 1,
  "flavor": ["藍莓", "柑橘", "花香"],
  "image_url": "https://upload.wikimedia.org/wikipedia/commons/4/45/A_small_cup_of_coffee.jpg",
  "country": "薩爾瓦多",
  "area": "聖荷西莊園",
  "variety": "阿拉比卡",
  "processing_method": "水洗",
  "description": "花神給予入口一些柑橘香氣的柔順，帶著太妃糖甜感，冷卻時會有生巧克力且明亮細緻的酸質。"
}
```

> Error Response 404 (找不到 id 對應的豆子)

```json
"無此商品"
```



豆子詳細資訊

### HTTP Request

`GET /products/:id`







# 使用者的購物車

## 購物車商品列表

```javascript
const api = `${process.env.VUE_APP_API}/users/cart_items`;
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
[  
  {
    "product_id": 137, 
    "product_name": "耶家雪菲 日曬 古吉 夏奇索 魔魔拉單一莊園 G1", 
    "package_type": "half_pound", 
    "unit_price": 450,
    "quantity": 1,
    "product_image_url": "https://upload.wikimedia.org/wikipedia/commons/4/45/A_small_cup_of_coffee.jpg",
    "ground": false
  }
]
```



### HTTP Request  (need JWT token)

`GET /users/cart_items`







## 將商品加入購物車

```javascript
const api = `${process.env.VUE_APP_API}/users/cart_items`;
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
  "cart_item": {
    "product_id": 152, 
    "package_type": "half_pound", 
    "quantity": 1,
    "ground": true
  }
}
```

> Success Response 200

```json
{
  "product_id": 152, 
  "product_name": "耶家雪菲 日曬 古吉 夏奇索 魔魔拉單一莊園 G1", 
  "package_type": "half_pound", 
  "quantity": 1, 
  "unit_price": 450,
  "ground": true
}
```

> Error Response 400

```json
{ 
  "quantity": ["must be greater than 0"], 
  "product": ["must exist"]
}
```



### HTTP Request  (need JWT token)

`POST /users/cart_items`



### Data Parameters

| Parameter    | Description | Type                                      |
| ------------ | ----------- | ----------------------------------------- |
| product_id   | 商品 ID     | Integer                                   |
| package_type | 包裝        | String: [half_pound, one_pound, drip_bag] |
| quantity     | 數量        | Integer                                   |
| ground       | 是否磨粉    | Boolean                                   |











## 更新購物車商品資訊

```javascript
const api = `${process.env.VUE_APP_API}/users/cart_items/${product_id}`;
const headers = { Authorization: jwtToken };
const data = { ... }
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
"cart_item": {
  "quantity": 2
}
```

> Success Response 200

```json
{
  "quantity": 2,
  "product_id": 152, 
  "package_type": "half_pound", 
  "product_name": "耶家雪菲 日曬 古吉 夏奇索 魔魔拉單一莊園 G1", 
  "unit_price": 450,
  "ground": true
}
```

> Error Response 400

```json
{ 
  "quantity": ["must be greater than 0"]
}
```

> Error Response 404 (找不到 product_id 對應的商品)

```json
```



### HTTP Request  (need JWT token)

`PUT /users/cart_items/:product_id`

### URL Parameters

| Parameter  | Description       |
| ---------- | ----------------- |
| product_id | 購物車內的商品 ID |

### Data Parameters

| Parameter | Description | Type    |
| --------- | ----------- | ------- |
| quantity  | 數量        | Integer |









## 將商品從購物車移除

```javascript
const api = `${process.env.VUE_APP_API}/users/cart_items/${product_id}`;
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



### HTTP Request  (need JWT token)

`DELETE /users/cart_items/:product_id`

### URL Parameters

| Parameter  | Description       |
| ---------- | ----------------- |
| product_id | 購物車內的商品 ID |









# 使用者訂單相關

## 訂單列表

```javascript
const api = `${process.env.VUE_APP_API}/users/orders`;
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
[
  {
    "id": 31,
    "status": "pending",
    "shipping_status": "in_preparation",
    "payment_status": "outstanding",
    "payment_method": "cash_on_delivery",
    "note": null,
    "created_at": "2022-01-24T13:50:28.000Z",
    "items": [
      {
        "id": 31,
        "name": "Msgr. Alane Botsford",
        "unit_price": 135,
        "quantity": 4,
        "package_type": "half_pound",
        "ground": false
      }
    ],
    "shipping_info": {
      "name": "Fred Frami",
      "phone_number": "0912345678",
      "address": "address",
      "email": "danyel.krajcik@larson.org",
      "shipping_method": "home_delivery",
      "shipping_fee": 100
    }
  },
  ...
]
```



### HTTP Request  (need JWT token)

`GET /users/orders`







## 建立訂單

```javascript
const api = `${process.env.VUE_APP_API}/users/orders`;
const headers = { Authorization: jwtToken };
const data = { order: { ... } };
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
  "note": "some note",
  "payment_method": "cash_on_delivery",
  "shipping_info": {
    "name": "kakas",
    "phone_number": "0912345678",
    "address": "110台北市信義區忠孝東路五段2號",
    "email": "fake@example.com",
    "shipping_method": "home_delivery"
  }
}
```

> Success Response 200

```json
{
  "id": 143,
  "status": "pending",
  "shipping_status": "in_preparation",
  "payment_status": "outstanding",
  "payment_method": "cash_on_delivery",
  "note": "some note",
  "created_at": "2022-01-22T12:46:49.278Z",
  "may_confirm?": true,
  "may_finish?": false,
  "may_cancel?": true,
  "may_to_shipping?": true,
  "may_to_arrived?": false,
  "may_to_picked_up?": false,
  "may_pay?": true,
  "items": [
    {
      "id": 146,
      "name": "1 - 耶家雪菲 日曬 古吉 夏奇索 魔魔拉單一莊園 G1",
      "unit_price": 450,
      "quantity": 1,
      "package_type": "half_pound",
      "ground": false
    }
  ],
  "shipping_info": {
    "name": "kakas",
    "phone_number": "0912345678",
    "address": "110台北市信義區忠孝東路五段2號",
    "email": "fake@example.com",
    "shipping_method": "home_delivery",
    "shipping_fee": 100
  }
}
```

> Error Response 400

```json
{
  "cart": ["購物車不得為空"],
  "name": ["can't be blank"],
  "phone_number": ["can't be blank"],
  "address": ["can't be blank"],
  "email": ["can't be blank"]
}
```



### HTTP Request  (need JWT token)

`POST /users/orders`

### Data Parameters

| Parameter                     | Description  | Type                      |
| ----------------------------- | ------------ | ------------------------- |
| note                          | 備註         | String                    |
| payment_method                | 付款方式     | String [cash_on_delivery] |
| shipping_info.name            | 收件人姓名   | String                    |
| shipping_info.phone_number    | 收件人電話   | String                    |
| shipping_info.address         | 收件人地址   | String                    |
| shipping_info.email           | 收件人 email | String                    |
| shipping_info.shipping_method | 寄送方式     | String [home_delivery]    |





## 取得某筆訂單的資訊

```javascript
const api = `${process.env.VUE_APP_API}/users/orders/${id}`;
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
  "id": 156,
  "status": "pending",
  "shipping_status": "in_preparation",
  "payment_status": "outstanding",
  "payment_method": "cash_on_delivery",
  "note": null,
  "created_at": "2022-05-29T04:58:30.000Z",
  "may_confirm?": true,
  "may_finish?": false,
  "may_cancel?": true,
  "may_to_shipping?": true,
  "may_to_arrived?": false,
  "may_to_picked_up?": false,
  "may_pay?": true,
  "items": [
    {
      "id": 159,
      "name": "Ed Ondricka",
      "unit_price": 134,
      "quantity": 6,
      "package_type": "half_pound",
      "ground": false
    }
  ],
  "shipping_info": {
    "name": "Alena Runte III",
    "phone_number": "0912345678",
    "address": "address",
    "email": "floretta@dare.org",
    "shipping_method": "home_delivery",
    "shipping_fee": 100
  }
}
```

回傳 ID 所對應的豆子的詳細資訊

### HTTP Request (need JWT token)

`GET /users/orders/:id` 

### URL Parameters

| Parameter | Description |
| --------- | ----------- |
| id        | 訂單的 ID   |









# 管理者頁面/訂單管理

## 訂單列表

```javascript
const api = `${process.env.VUE_APP_API}/admin/orders`;
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
[
  {
    "id": 31,
    "status": "pending",
    "shipping_status": "in_preparation",
    "payment_status": "outstanding",
    "payment_method": "cash_on_delivery",
    "note": null,
    "created_at": "2022-01-24T13:50:28.000Z",
    "may_confirm?": true,
    "may_finish?": false,
    "may_cancel?": true,
    "may_to_shipping?": true,
    "may_to_arrived?": false,
    "may_to_picked_up?": false,
    "may_pay?": true,
    "items": [
      {
        "id": 31,
        "name": "Msgr. Alane Botsford",
        "unit_price": 135,
        "quantity": 4,
        "package_type": "half_pound",
        "ground": false
      }
    ],
    "shipping_info": {
      "name": "Fred Frami",
      "phone_number": "0912345678",
      "address": "address",
      "email": "danyel.krajcik@larson.org",
      "shipping_method": "home_delivery",
      "shipping_fee": 100
    }
  },
  ...
]
```



### HTTP Request  (need JWT token)

`GET /admin/orders`







## 取得某筆訂單的資訊

```javascript
const api = `${process.env.VUE_APP_API}/admin/orders/${id}`;
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
  "id": 156,
  "status": "pending",
  "shipping_status": "in_preparation",
  "payment_status": "outstanding",
  "payment_method": "cash_on_delivery",
  "note": null,
  "created_at": "2022-05-29T04:58:30.000Z",
  "may_confirm?": true,
  "may_finish?": false,
  "may_cancel?": true,
  "may_to_shipping?": true,
  "may_to_arrived?": false,
  "may_to_picked_up?": false,
  "may_pay?": true,
  "items": [
    {
      "id": 159,
      "name": "Ed Ondricka",
      "unit_price": 134,
      "quantity": 6,
      "package_type": "half_pound",
      "ground": false
    }
  ],
  "shipping_info": {
    "name": "Alena Runte III",
    "phone_number": "0912345678",
    "address": "address",
    "email": "floretta@dare.org",
    "shipping_method": "home_delivery",
    "shipping_fee": 100
  }
}
```

回傳 ID 所對應的豆子的詳細資訊

### HTTP Request (need JWT token)

`GET /admin/orders/:id` 

### URL Parameters

| Parameter | Description |
| --------- | ----------- |
| id        | 訂單的 ID   |





## 修改某筆訂單的狀態

```javascript
const api = `${process.env.VUE_APP_API}/admin/orders/${order_id}/status`;
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
  "status": "confirmed",
}
```

> Success Response 200

```json
{
  "id": 156,
  "status": "pending",
  "shipping_status": "in_preparation",
  "payment_status": "outstanding",
  "payment_method": "cash_on_delivery",
  "note": null,
  "created_at": "2022-05-29T04:58:30.000Z",
  "may_confirm?": true,
  "may_finish?": false,
  "may_cancel?": true,
  "may_to_shipping?": true,
  "may_to_arrived?": false,
  "may_to_picked_up?": false,
  "may_pay?": true,
  "items": [
    {
      "id": 159,
      "name": "Ed Ondricka",
      "unit_price": 134,
      "quantity": 6,
      "package_type": "half_pound",
      "ground": false
    }
  ],
  "shipping_info": {
    "name": "Alena Runte III",
    "phone_number": "0912345678",
    "address": "address",
    "email": "floretta@dare.org",
    "shipping_method": "home_delivery",
    "shipping_fee": 100
  }
}
```



### HTTP Request  (need JWT token)

`PUT /admin/orders/:order_id/status`

### URL Parameters

| Parameter | Description |
| --------- | ----------- |
| order_id  | 訂單的 ID   |

### Data Parameters

| Parameter | Description  | Type                                    |
| --------- | ------------ | --------------------------------------- |
| status    | 欲修改的狀態 | String: [confirmed, finished, canceled] |





## 修改某筆訂單的運送狀態

```javascript
const api = `${process.env.VUE_APP_API}/admin/orders/${order_id}/shipping_status`;
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
  "shipping_status": "shipping",
}
```

> Success Response 200

```json
{
  "id": 156,
  "status": "pending",
  "shipping_status": "in_preparation",
  "payment_status": "outstanding",
  "payment_method": "cash_on_delivery",
  "note": null,
  "created_at": "2022-05-29T04:58:30.000Z",
  "may_confirm?": true,
  "may_finish?": false,
  "may_cancel?": true,
  "may_to_shipping?": true,
  "may_to_arrived?": false,
  "may_to_picked_up?": false,
  "may_pay?": true,
  "items": [
    {
      "id": 159,
      "name": "Ed Ondricka",
      "unit_price": 134,
      "quantity": 6,
      "package_type": "half_pound",
      "ground": false
    }
  ],
  "shipping_info": {
    "name": "Alena Runte III",
    "phone_number": "0912345678",
    "address": "address",
    "email": "floretta@dare.org",
    "shipping_method": "home_delivery",
    "shipping_fee": 100
  }
}
```



### HTTP Request  (need JWT token)

`PUT /admin/orders/:order_id/shipping_status`

### URL Parameters

| Parameter | Description |
| --------- | ----------- |
| order_id  | 訂單的 ID   |

### Data Parameters

| Parameter       | Description      | Type                                   |
| --------------- | ---------------- | -------------------------------------- |
| shipping_status | 欲修改的運送狀態 | String: [shipping, arrived, picked_up] |





## 修改某筆訂單的付款狀態

```javascript
const api = `${process.env.VUE_APP_API}/admin/orders/${order_id}/payment_status`;
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
  "payment_status": "paid",
}
```

> Success Response 200

```json
{
  "id": 156,
  "status": "pending",
  "shipping_status": "in_preparation",
  "payment_status": "outstanding",
  "payment_method": "cash_on_delivery",
  "note": null,
  "created_at": "2022-05-29T04:58:30.000Z",
  "may_confirm?": true,
  "may_finish?": false,
  "may_cancel?": true,
  "may_to_shipping?": true,
  "may_to_arrived?": false,
  "may_to_picked_up?": false,
  "may_pay?": true,
  "items": [
    {
      "id": 159,
      "name": "Ed Ondricka",
      "unit_price": 134,
      "quantity": 6,
      "package_type": "half_pound",
      "ground": false
    }
  ],
  "shipping_info": {
    "name": "Alena Runte III",
    "phone_number": "0912345678",
    "address": "address",
    "email": "floretta@dare.org",
    "shipping_method": "home_delivery",
    "shipping_fee": 100
  }
}
```



### HTTP Request  (need JWT token)

`PUT /admin/orders/:order_id/payment_status`

### URL Parameters

| Parameter | Description |
| --------- | ----------- |
| order_id  | 訂單的 ID   |

### Data Parameters

| Parameter      | Description      | Type           |
| -------------- | ---------------- | -------------- |
| payment_status | 欲修改的付款狀態 | String: [paid] |














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
    "id": 91,
    "name": "1 - 耶家雪菲 日曬 古吉 夏奇索 魔魔拉單一莊園 G1",
    "half_pound_price": 450,
    "one_pound_price": 810,
    "drip_bag_price": 40,
    "roast": 1,
    "flavor": [
      "藍莓",
      "柑橘",
      "花香"
    ],
    "country": "薩爾瓦多",
    "area": "聖荷西莊園",
    "variety": "阿拉比卡",
    "processing_method": "水洗",
    "description": "花神給予入口一些柑橘香氣的柔順，帶著太妃糖甜感，冷卻時會有生巧克力且明亮細緻的酸質。"
  }
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
  "country": "薩爾瓦多",
  "area": "聖荷西莊園",
  "variety": "阿拉比卡",
  "processing_method": "水洗",
  "description": "花神給予入口一些柑橘香氣的柔順，帶著太妃糖甜感，冷卻時會有生巧克力且明亮細緻的酸質。"
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
  "country": "薩爾瓦多",
  "area": "聖荷西莊園",
  "variety": "阿拉比卡",
  "processing_method": "水洗",
  "description": "花神給予入口一些柑橘香氣的柔順，帶著太妃糖甜感，冷卻時會有生巧克力且明亮細緻的酸質。"
}
```

> Error Response 400

```json
{ 
  "name": ["can't be blank"], 
  "roast": ["can't be blank"]
}
```



### HTTP Request  (need JWT token)

`POST /admin/products`

### Data Parameters

| Parameter         | Description | Type           |
| ----------------- | ----------- | -------------- |
| name              | 名稱        | String         |
| half_pound_price  | 半磅價格    | Integer        |
| one_pound_price   | 一磅價格    | Integer        |
| drip_bag_price    | 濾掛價格    | Integer        |
| roast             | 烘焙程度    | Integer (1..5) |
| flavor            | 風味        | [String]       |
| country           | 國家        | String         |
| area              | 產區        | String         |
| variety           | 品種        | String         |
| processing_method | 處理法      | String         |
| description       | 風味描述    | String         |










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
  "country": "薩爾瓦多",
  "area": "聖荷西莊園",
  "variety": "阿拉比卡",
  "processing_method": "水洗",
  "description": "花神給予入口一些柑橘香氣的柔順，帶著太妃糖甜感，冷卻時會有生巧克力且明亮細緻的酸質。"
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
  "country": "薩爾瓦多",
  "area": "聖荷西莊園",
  "variety": "阿拉比卡",
  "processing_method": "水洗",
  "description": "花神給予入口一些柑橘香氣的柔順，帶著太妃糖甜感，冷卻時會有生巧克力且明亮細緻的酸質。"
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
  "country": "薩爾瓦多",
  "area": "聖荷西莊園",
  "variety": "阿拉比卡",
  "processing_method": "水洗",
  "description": "花神給予入口一些柑橘香氣的柔順，帶著太妃糖甜感，冷卻時會有生巧克力且明亮細緻的酸質。"
}
```

> Error Response 400

```json
{ 
  "name": ["can't be blank"], 
  "roast": ["can't be blank"]
}
```

### HTTP Request  (need JWT token)

`PUT /admin/products/:id`

### URL Parameters

| Parameter | Description |
| --------- | ----------- |
| Id        | 豆子的 ID   |

### Data Parameters

| Parameter         | Description | Type           |
| ----------------- | ----------- | -------------- |
| name              | 名稱        | String         |
| half_pound_price  | 半磅價格    | Integer        |
| one_pound_price   | 一磅價格    | Integer        |
| drip_bag_price    | 濾掛價格    | Integer        |
| roast             | 烘焙程度    | Integer (1..5) |
| flavor            | 風味        | [String]       |
| country           | 國家        | String         |
| area              | 產區        | String         |
| variety           | 品種        | String         |
| processing_method | 處理法      | String         |
| description       | 風味描述    | String         |





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

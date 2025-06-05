# API dizayni

## 1. HTTP metodlaridan maqsadiga muvofiq foydalanish

- **GET** — resurslarni olish uchun (idempotent)
- **POST** — yangi resurs yaratish uchun
- **PUT** — resursni yangilash uchun (idempotent)
- **DELETE** — resursni o‘chirish uchun (idempotent)
- **PATCH** — resursni qisman yangilash uchun

## 2. HTTP fe’llarini to‘g‘ri qo‘llash

- **Noto‘g‘ri:**

  ```http
  GET /products/123/delete
  ```

- **To‘g‘ri:**

  ```http
  DELETE /products/123
  ```

## 3. To‘g‘ri HTTP status kodlarini qaytarish

- **Noto‘g‘ri:**

  ```http
  HTTP/1.1 200 OK
  Content-Type: application/json

  {
    "error": "Product not found",
    "status_code": 404
  }
  ```

- **To‘g‘ri:**

  ```http
  HTTP/1.1 404 Not Found
  Content-Type: application/json

  {
    "error": "Product not found"
  }
  ```

## 4. Endpoint nomlarini ko‘plikda yozish

```http
https://example.com/v1/users
https://example.com/v2/products
```

## 5. Resurs nomlarida fe’l ishlatmaslik

- **Noto‘g‘ri:**

  ```http
  GET /getProducts
  ```

- **To‘g‘ri:**

  ```http
  GET /products
  ```

## 6. Endpointlar iyerarxiyasini mantiqan va ixcham tashkil etish

- **Noto‘g‘ri:**

  ```http
  /reviews/123?productId=213
  /users/2/products/4452/.../..
  ```

- **To‘g‘ri:**

  ```http
  /products/42/reviews/123
  /users/7
  ```

## 7. API versiyasini URL orqali ko‘rsatish

```http
GET /v1/users
GET /v2/users
```

## 8. So‘rov parametrlaridan filtrlash, tartiblash va sahifalash uchun foydalanish

- **Noto‘g‘ri:**

  ```http
  GET /products/expensive/sortedByPrice
  ```

- **To‘g‘ri:**

  ```http
  GET /products?price=high&sort=price
  GET /products?category=new&sort=name&limit=10&offset=20
  ```

## 9. Parametrlarda o‘lchov birliklari va sanani formati bilan ko‘rsatish

- **Noto‘g‘ri:**

  ```http
  GET /products?shippedOn=20230315
  GET /products?weight=5
  ```

- **To‘g‘ri:**

  ```http
  GET /products?shippedOn=2023-03-15        # ISO 8601
  GET /products?weight=5&weightUnit=kg
  ```

## 10. Maxfiy ma’lumotlarni URL orqali uzatmaslik

- **Noto‘g‘ri:**

  ```http
  https://example.com/login?password=myPassword
  https://example.com/register?email=unsafeMail
  ```

- **To‘g‘ri:**
  Parollar, tokenlar va shaxsiy ma’lumotlar POST yoki PUT so‘rovining tanasida yuborilishi kerak.

## 11. Javobni har doim yuqori darajadagi obyekt bilan o‘rash

- **Noto‘g‘ri:**

  ```json
  [0, 1, 2, 3]
  ```

- **To‘g‘ri:**

  ```json
  {
  	"data": [0, 1, 2, 3]
  }
  ```

## 12. Sanalar bilan ishlashda ISO 8601 formatidan foydalanish

```text
2025-06-05T14:30:00Z
```

## 13. Mijoz ehtiyojlariga asoslanish, ichki atamalardan qochish

- **Noto‘g‘ri:**

  ```http
  GET /cdc    # change-data-capture
  ```

- **To‘g‘ri:**

  ```http
  GET /changes
  ```

## 14. Filtrlash, sortlash va sahifalashni qo‘llash

- **Noto‘g‘ri:**

  ```http
  GET /products
  ```

- **To‘g‘ri:**

  ```http
  GET /products?category=new&sort=name&limit=10&offset=20
  ```

## 15. Aniq va tushunarli xatolik xabarlarini taqdim etish

- **Noto‘g‘ri:**

  ```json
  {
  	"error": "Noma’lum xatolik"
  }
  ```

- **To‘g‘ri:**

  ```json
  {
  	"error": "Mahsulot topilmadi",
  	"status_code": 404
  }
  ```

## 16. Tez-tez so‘raladigan yoki statik ma’lumotlarni keshga olish

- **Noto‘g‘ri:**
  Har bir so‘rovda birlamchi manbadan o‘qish.

- **To‘g‘ri:**
  Redis yoki xotira keshidan foydalanish.

## 17. API holatini tekshirish uchun health check endpoint qo‘shish

```http
GET /health
```

## 18. So‘rovlar chastotasini cheklash (rate limiting)

- **Noto‘g‘ri:**
  Cheklov yo‘q.

- **To‘g‘ri:**
  Masalan, 1 daqiqada 100 so‘rovdan ko‘p bo‘lmasligi kerak.

## 19. Yuklama balansirovkasidan foydalanish

- **Noto‘g‘ri:**
  APIga to‘g‘ridan-to‘g‘ri yagona server orqali murojaat qilish.

- **To‘g‘ri:**
  Load balancer orqali bir nechta serverga so‘rovlarni taqsimlash.

## 20. API uchun testlar yozish

- Unit-testlar
- Integratsion testlar
- End-to-end testlar
- Performance testlar

## 21. Monitoring va bildirishnomalarni yo‘lga qo‘yish

- Prometheus, Zabbix va boshqalar yordamida monitoring
- Slack, PagerDuty orqali xabarnomalar

## 22. Ma’lumotlar xavfsizligi uchun HTTPS ishlatish

- **Noto‘g‘ri:**

  ```http
  http://example.com/api
  ```

- **To‘g‘ri:**

  ```http
  https://example.com/api
  ```

## 23. Xatolik javoblarida ortiqcha ma’lumot bermaslik

- **Noto‘g‘ri:**

  ```json
  {
  	"error": "NullPointerException at com.example.service.UserService",
  	"stack": "com.example.service.UserService.java:45"
  }
  ```

- **To‘g‘ri:**

  ```json
  {
  	"error": "Ichki server xatosi"
  }
  ```

## 24. Ishonchli autentifikatsiya va avtorizatsiyani joriy etish

- **Autentifikatsiya:**

  - Basic Auth
  - OAuth 2.0
  - JWT

- **Avtorizatsiya:**

  - OAuth 2.0 (scope va rollar bilan)
  - OpenID Connect
  - RBAC/ABAC

## 25. API xavfsizligini loyihalash bosqichidanoq inobatga olish

- **Noto‘g‘ri:**
  Xavfsizlikni keyinroq qo‘shish.

- **To‘g‘ri:**
  Avvaldan autentifikatsiya, avtorizatsiya, ma’lumotlar validatsiyasi va shifrlashni ko‘zda tutish.

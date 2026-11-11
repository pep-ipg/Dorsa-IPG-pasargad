# برگشت از خرید ترد پارتی (تومن)

این فایل شامل توضیحات سرویس های Refund با استفاده از Provider تومن میباشد.

- مطابق با مستند درگاه پرداخت اینترنتی — شماره ویرایش 28.0 — تاریخ 1405/08/12

- تمامی سرویس ها با متد POST و بدنه JSON فراخوانی میشوند.
- در ستون «اجباری / اختیاری» جدول ها: **M** = اجباری ، **O** = اختیاری ، **M/O** = بسته به شرایط (در نکات هر سرویس توضیح داده شده است)
- کد خطاها در **[لیست خطاها](../error-codes.md)** آمده است.

## فهرست سرویس ها

<div dir="rtl">

| # | سرویس | آدرس |
|---|---|---|
| 1 | دریافت توکن (سرویس Refund) | `/token/getToken` |
| 2 | برگشت از خرید (سرویس Refund) | `/api/payment/refund-transactions-3rd-party/toman` |
| 3 | دریافت جزئیات تراکنش های بازگشتی (Refund) با استفاده از شماره فاکتور (InvoiceID) | `/api/third-party/provider/v1/settlement/tracking/toman` |
| 4 | دریافت جزئیات تراکنش های بازگشتی (Refund) با استفاده از UUID | `/api/third-party/provider/v1/settlement/toman` |
| 5 | لیست تراکنش های بازگشتی (Refund) با استفاده از تاریخ | `/api/third-party/provider/v1/settlements/toman` |
| 6 | تبدیل شماره کارت به شبا (سرویس Refund) | `/api/third-party/provider/v1/card-to-iban/toman` |
| 7 | کیف پول (سرویس Refund) | `/api/third-party/provider/v1/get-wallet/toman` |

</div>


## 1.دریافت توکن (سرویس Refund)

- برای استفاده از سرویس های Refund (با استفاده از Provider) توکن با ارسال اطلاعات provider دریافت میشود.

**URL:** `https://pep.shaparak.ir/dorsa1/token/getToken`  
**Method:** POST  
**Body:**  
```json
{
    "username": "your-username",
    "password": "your-password",
    "provider_credentials": [
        {
            "client_id": "your-client-id",
            "client_secret": "your-client-secret",
            "name": "providerName",
            "password": "provider-password",
            "username": "provider-username"
        }
    ]
}
```

## خروجی موفق
```json
{
    "resultMsg": "Successful",
    "resultCode": 0,
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.**********.**********",
    "username": "ipgTestUser",
    "firstName": "firstName",
    "lastName": "lastName",
    "userId": "17",
    "roles": [
        {
            "authority": "mpg"
        },
        {
            "authority": "merchant"
        }
    ]
}
```

- پس از گرفتن توکن، در هر یک از سرویس های Refund (با استفاده از Provider) که در ادامه مورد استفاده قرار خواهند گرفت باید توکن در header هر request با فرمت `Bearer token` ارسال شود.
- مقدار name در provider_credentials های ارسالی، به معنای نام provider ای است که میخواهید استفاده کنید (برای مثال: toman).
- مدت زمان expire شدن توکن را می توانید از خود توکن استخراج کنید و بنا بر کانفیگ سرویس IPG می تواند متغیر باشد (پیشفرض 10 دقیقه). درصورت دریافت خطای 401 (خطای احراز هویت) مجدد احراز هویت انجام شود.



## 2.برگشت از خرید (سرویس Refund)

- برای انجام تراکنش بازگشت وجه خرید به مشتری، می توان وب سرویس برگشت از خرید را فراخوانی نمود. جهت فراخوانی وب سرویس، اطلاعات تراکنش اصلی (خرید) مورد نیاز می باشد.
- وضعیت تراکنش خرید جهت انجام تراکنش بازگشت وجه یا Refund می بایستی تایید شده باشد.

**URL:** `https://pep.shaparak.ir/dorsa1/api/payment/refund-transactions-3rd-party/toman`  
**Method:** POST  
**Body:**  
```json
{
    "amount": 20000,
    "invoiceDate": "2025/08/30",
    "invoiceID": "123",
    "originInvoiceID": "123456",
    "terminalNumber": 123456,
    "originUrlId": "ec38ebbc8f1705bd8af4256461f98f281a8a43d1234500012345639"
}
```

<div dir="rtl">

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| invoiceID | String | M | شماره فاکتور یکتا مختص تراکنش برگشت از خرید |
| invoiceDate | String | O | تاریخ فاکتور (به فرمت دلخواه) مختص تراکنش برگشت از خرید |
| amount | Integer | O | مبلغ واریز وجه (ریال) در تراکنش برگشت از خرید. درصورتیکه مبلغ ارسال نگردد، مبلغ بازگشتی در تراکنش برگشت از خرید، برابر با مقدار مبلغ تراکنش اصلی خرید لحاظ می گردد. |
| originInvoiceID | String | M | شماره فاکتور تراکنش خرید اصلی که بابت آن تراکنش بازگشت وجه درخواست داده شده است (شماره فاکتوری که توسط پذیرنده در تراکنش خرید ارسال شده است) |
| terminalNumber | Integer | M | شماره ترمینال پرداختی |
| originUrlId | String | O | توکن یا آدرس صفحه ی پرداخت خرید که در جواب ثبت درخواست خرید دریافت شده است |

</div>

## خروجی موفق

```json
{
    "resultCode": 0,
    "resultMsg": "Successful"
}
```



## 3.دریافت جزئیات تراکنش های بازگشتی (Refund) با استفاده از شماره فاکتور (InvoiceID)

**URL:** `https://pep.shaparak.ir/dorsa1/api/third-party/provider/v1/settlement/tracking/toman`  
**Method:** POST  
**Body:**  
```json
{
    "terminalNumber": 123453789,
    "invoiceId": "123"
}
```

<div dir="rtl">

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| terminalNumber | Integer | M | شماره پایانه |
| invoiceId | String | M | شماره فاکتور تراکنش برگشت از خرید جهت استعلام وضعیت تراکنش برگشت از خرید (فیلد invoiceID در درخواست Refund) |

</div>

## خروجی موفق

```json
{
    "resultCode": 0,
    "resultMsg": "Successful",
    "status": 200,
    "result": {
        "uuid": "2dee4666-32b0-48jt-mt54-6efda47b64e5",
        "description": "پرداخت قرض و تادیه دیون",
        "amount": 10000,
        "settle_type": 1,
        "bank_id": 2,
        "src_bank_id": 2,
        "dest_bank_id": 5,
        "iban": "IR************************",
        "account_number": "*********",
        "bank_follow_up_code": "140406230172727525",
        "status": 3,
        "create_timestamp": "2025-09-14T15:04:24.94745Z",
        "update_timestamp": "2025-09-14T15:04:28.144537Z",
        "verify_timestamp": "0001-01-01T00:00:00Z",
        "detail": "عملیات با موفقیت انجام شده است.",
        "tracker_id": "10000000000111122221",
        "jalali_verify_datetime": "1404/06/23 18:34:28",
        "account": 2,
        "expire_at": "2025-10-14T15:04:24.937405Z",
        "destination_bank_name": "کشاورزی",
        "transfer_type": 1,
        "transfer_reason": 6,
        "merchant": "",
        "full_name": "",
        "card_number": "",
        "bulk_row_id": 0,
        "displayed_commission": 0
    }
}
```

وضعیت تراکنش بازگشتی در پاسخ (فیلد status داخل result):

<div dir="rtl">

| Status | توضیحات |
|---|---|
| -1 | نامشخص |
| 0 | ایجاد شده (برای تراکنش های دو مرحله ای) |
| 1 | ناموفق |
| 2 | درحال انتظار (در حال انجام است) |
| 3 | موفق (تراکنش با موفقیت به پایان رسیده است) |
| 4 | لغو شده توسط کاربر (برای تراکنش های دو مرحله ای) |
| 5 | منقضی شده (برای تراکنش های دو مرحله ای) |
| 6 | عدم تایید توسط کاربر (برای تراکنش های دو مرحله ای) |
| 8 | دارای خطا در تراکنش (شماره حساب اشتباه) |

</div>



## 4.دریافت جزئیات تراکنش های بازگشتی (Refund) با استفاده از UUID

**URL:** `https://pep.shaparak.ir/dorsa1/api/third-party/provider/v1/settlement/toman`  
**Method:** POST  
**Body:**  
```json
{
    "terminalNumber": 123453789,
    "settlementUUID": "2dee4666-32b0-48jt-mt54-6efda47b64e5"
}
```

<div dir="rtl">

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| terminalNumber | Integer | M | شماره پایانه |
| settlementUUID | String | M | UUID تراکنش |

</div>

- خروجی موفق و جدول وضعیت تراکنش بازگشتی همانند سرویس «دریافت جزئیات تراکنش های بازگشتی با استفاده از شماره فاکتور» است.



## 5.لیست تراکنش های بازگشتی (Refund) با استفاده از تاریخ

**URL:** `https://pep.shaparak.ir/dorsa1/api/third-party/provider/v1/settlements/toman`  
**Method:** POST  
**Body:**  
```json
{
    "terminalNumber": 123453789,
    "create_after": "2025-05-23 03:30:00.000000000",
    "create_before": "2025-05-25 03:30:00.000000000",
    "page": 2,
    "invoiceId": "123"
}
```

<div dir="rtl">

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| terminalNumber | Integer | M | شماره پایانه |
| create_after | String | O | تاریخ شروع بازه درخواستی |
| create_before | String | O | تاریخ پایان بازه درخواستی |
| page | Integer | O | شماره صفحه درخواستی |
| invoiceId | String | M | شماره فاکتور تراکنش بازگشتی |

</div>

- در هر صفحه (Page) 50 تراکنش نمایش داده میشود.

## خروجی موفق

```json
{
    "resultCode": 0,
    "resultMessage": "Successful",
    "totalCount": 146,
    "pageNumber": 2,
    "totalPages": 3,
    "results": [
        {
            "uuid": "2dee4666-32b0-48jt-mt54-6efda47b64e5",
            "description": "پرداخت قرض و تادیه دیون",
            "amount": 10000,
            "settle_type": 1,
            "bank_id": 2,
            "src_bank_id": 2,
            "dest_bank_id": 5,
            "iban": "IR************************",
            "account_number": "*********",
            "bank_follow_up_code": "140406230172727525",
            "status": 3,
            "create_timestamp": "2025-09-14T15:04:24.94745Z",
            "update_timestamp": "2025-09-14T15:04:28.144537Z",
            "verify_timestamp": "0001-01-01T00:00:00Z",
            "detail": "عملیات با موفقیت انجام شده است.",
            "tracker_id": "10000000000111122221",
            "jalali_verify_datetime": "1404/06/23 18:34:28",
            "account": 2,
            "expire_at": "2025-10-14T15:04:24.937405Z",
            "destination_bank_name": "کشاورزی",
            "transfer_type": 1,
            "transfer_reason": 6,
            "merchant": "",
            "full_name": "",
            "card_number": "",
            "bulk_row_id": 0,
            "displayed_commission": 0
        }
    ]
}
```

- مقادیر فیلد status هر تراکنش همانند جدول «وضعیت تراکنش بازگشتی» در سرویس 3 است.



## 6.تبدیل شماره کارت به شبا (سرویس Refund)

**URL:** `https://pep.shaparak.ir/dorsa1/api/third-party/provider/v1/card-to-iban/toman`  
**Method:** POST  
**Body:**  
```json
{
    "cardNumber": "502229******0490",
    "terminalNumber": 123453789
}
```

- در نمونه بالا شماره کارت به صورت Mask شده نمایش داده شده است؛ در درخواست واقعی شماره کارت کامل ارسال میشود.

<div dir="rtl">

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| cardNumber | String | M | شماره کارت |
| terminalNumber | Integer | M | شماره پایانه |

</div>

## خروجی موفق

```json
{
    "resultCode": 0,
    "resultMsg": "Successful",
    "iban": "IR************************"
}
```



## 7.کیف پول (سرویس Refund)

- برای دریافت موجودی کیف پول از این سرویس استفاده میشود.

**URL:** `https://pep.shaparak.ir/dorsa1/api/third-party/provider/v1/get-wallet/toman`  
**Method:** POST  
**Body:**  
```json
{
    "terminalNumber": 123453789,
    "invoiceId": "123"
}
```

<div dir="rtl">

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| terminalNumber | Integer | M | شماره پایانه |
| invoiceId | String | M | شماره فاکتور |

</div>

## خروجی موفق

```json
{
    "resultCode": 0,
    "resultMsg": "Successful",
    "balance": 960000,
    "balance_warning_threshold": 0,
    "minimum_balance": 326400,
    "withdraw_ratio": 0,
    "address": "",
    "can_use_for_settlement": false
}
```

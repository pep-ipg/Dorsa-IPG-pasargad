# API Reference

این فایل شامل توضیحات متدهای API درگاه پرداخت درسا بانک پاسارگاد میباشد.

- مطابق با مستند درگاه پرداخت اینترنتی (خرید/قبض/شارژ) — شماره ویرایش 28.0 — تاریخ 1405/08/12
- تمامی سرویس ها با متد POST و بدنه JSON فراخوانی میشوند.
- در ستون «اجباری / اختیاری» جدول ها: **M** = اجباری ، **O** = اختیاری ، **M/O** = بسته به شرایط (در نکات هر سرویس توضیح داده شده است)

## فهرست سرویس ها

| # | سرویس | آدرس |
|---|---|---|
| 1 | دریافت توکن | `/token/getToken` |
| 2 | تراکنش خرید | `/api/payment/purchase` |
| 3 | تراکنش خرید شناسه دار | `/api/payment/purchase` |
| 4 | تراکنش خرید با تسهیم | `/api/payment/multiacc/purchase` |
| 5 | تراکنش خرید شناسه دار با تسهیم | `/api/payment/multiacc/purchase` |
| 6 | درخواست خرید شارژ / بسته اینترنتی | `/api/payment/pre-transaction` |
| 7 | درخواست پرداخت قبض | `/api/payment/pre-transaction` |
| 9 | تایید عملیات خرید (confirm) | `/api/payment/confirm-transactions` |
| 10 | درخواست تاییدیه (verify) | `/api/payment/verify-transactions` |
| 11 | درخواست تاییدیه با اطلاعات تکمیلی | `/api/payment/verify-payment` |
| 12 | برگشت تراکنش (reverse) | `/api/payment/reverse-transactions` |
| 14 | استعلام وضعیت تراکنش | `/api/payment/payment-inquiry` |
| 15 | استعلام وضعیت تراکنش (Hashed card number) | `/api/payment/payment-inquiryv2` |
| 16 | برگشت از خرید | `/api/payment/refund-transactions` |
| 17 | تایید رمز | `/api/payment/inquiry-confirm-pin` |
| 18 | چکیده حساب | `/api/payment/inquiry-mini-statement` |
| 19 | دریافت توکن (سرویس Refund) | `/token/getToken` |
| 20 | برگشت از خرید (سرویس Refund) | `/api/payment/refund-transactions-3rd-party/toman` |
| 21 | جزئیات تراکنش بازگشتی با InvoiceID | `/api/third-party/provider/v1/settlement/tracking/toman` |
| 22 | جزئیات تراکنش بازگشتی با UUID | `/api/third-party/provider/v1/settlement/toman` |
| 23 | لیست تراکنش های بازگشتی با تاریخ | `/api/third-party/provider/v1/settlements/toman` |
| 24 | تبدیل شماره کارت به شبا | `/api/third-party/provider/v1/card-to-iban/toman` |
| 25 | کیف پول | `/api/third-party/provider/v1/get-wallet/toman` |
| 26 | برگشت از خرید با سرویس پاد | `/api/payment/refund-transactions-3rd-party` |
| 27 | جزئیات تراکنش بازگشتی پاد با InvoiceID | `/api/third-party/provider/v1/settlement/tracking` |
| 28 | احراز هویت | `/api/payment/authorization` |


## 1.دریافت توکن
**URL:** `https://pep.shaparak.ir/dorsa1/token/getToken`  
**Method:** POST  
**Body:**  
```json
{
    "username": "your-username",
    "password": "your-password"
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

- پس از گرفتن توکن، در هر یک از سرویس هایی که در ادامه مورد استفاده قرار خواهند گرفت باید توکن در header هر request با فرمت `Bearer token` ارسال شود.
- مدت زمان expire شدن توکن را می توانید از خود توکن استخراج کنید و بنا بر کانفیگ سرویس IPG می تواند متغیر باشد (پیشفرض 10 دقیقه). درصورت دریافت خطای 401 (خطای احراز هویت) مجدد احراز هویت انجام شود.
- در صورتی که فراخوانی وب سرویس موفق باشد کد مربوط به resultCode برابر 0 خواهد بود و در صورتی که مقدار resultCode مقدار دیگری داشت خطایی رخ داده است که در **[لیست خطاها](./error-codes.md)** توضیحات مربوط به آن کد خطا آمده است. resultMsg حاوی پیام مختصری مربوط به هر کد خواهد بود.
- resultCode وضعیت فراخوانی سرویس می باشد.



## 2.تراکنش خرید

- در پاسخ این وب سرویس یک آدرس یکبار مصرف جهت وارد کردن اطلاعات بانکی مشتری توسط خود مشتری (مثل شماره کارت، رمز و ...) تولید و ارسال خواهد شد که در ادامه سرویس گیرنده باید مشتری را به آن آدرس Redirect کند.

**URL:** `https://pep.shaparak.ir/dorsa1/api/payment/purchase`  
**Method:** POST  
**Body:**  
```json
{
    "invoice": "20301112",
    "invoiceDate": "1403/05/24",
    "amount": 1000,
    "callbackApi": "https://your-domain.ir/callback",
    "mobileNumber": "0935*****52",
    "serviceCode": "8",
    "serviceType": "PURCHASE",
    "terminalNumber": 12345678,
    "description": "description",
    "payerMail": "payer@example.com",
    "payerName": "payerName",
    "pans": "",
    "nationalCode": "**********",
    "localization": "fa"
}
```

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| invoice | String | M | شماره فاکتور یکتا |
| invoiceDate | String | M | تاریخ فاکتور به فرمت دلخواه |
| amount | Integer | M | مبلغ به ریال |
| callbackApi | String | M | آدرسی که پس از عملیات پرداخت، کاربر به آن صفحه منتقل می شود (باید دامنه کالبک هنگام ثبت نام تعریف شده باشد) |
| mobileNumber | String | M/O | تلفن همراه پرداخت کننده |
| serviceCode | String | M | کد سرویس |
| serviceType | String | M | نوع سرویس |
| terminalNumber | Integer | M | شماره ترمینال پرداختی |
| description | String | O | توضیحات |
| payerMail | String | O | ایمیل پرداخت کننده |
| payerName | String | O | نام پرداخت کننده |
| nationalCode | String | M/O | کدملی |
| pans | String | O | شماره کارت ها |
| localization | String | O | مقدار پیشفرض فیلد `fa` می باشد و صفحه ی پرداخت با زبان فارسی بارگذاری می شود. در صورتیکه پذیرنده بخواهد صفحه ی پرداخت به انگلیسی بارگذاری شود مقدار `en` را در این فیلد قرار می دهد. |

| کد سرویس (serviceCode) | نوع سرویس (serviceType) | شرح |
|---|---|---|
| 8 | PURCHASE | خرید |

- در صورتیکه در قرارداد شما قید شده باشد که شماره کارت و تلفن همراه درخواست کننده تراکنش استعلام شود که متعلق به یک شخص است (در صورتی که کارت بانکی و شماره موبایل متعلق به یک نفر نباشد تراکنش انجام نخواهد شد) مقدار تلفن همراه اجباری خواهد بود و در غیر اینصورت اختیاری است.
- nationalCode در صورتی که چک کردن کد ملی و شماره کارت هنگام خرید فعال باشد، اجباری می باشد و در غیر این صورت اختیاری است.
- احراز هویت قوی: درصورت فعال بودن این امکان، شماره کارت کاربر با شماره موبایل کاربر هنگام انجام تراکنش بررسی می گردد و پارامتر شماره موبایل (mobileNumber) اجباری می باشد.

## خروجی موفق
```json
{
    "resultMsg": "Successful",
    "resultCode": 0,
    "data": {
        "urlId": "8dcc5cd0ef7348548f8dc2ab29ebe11a7ad3eaad000000006217318",
        "url": "https://pep.shaparak.ir/dorsa1/8dcc5cd0ef7348548f8dc2ab29ebe11a7ad3eaad000000006217318"
    }
}
```

- سرویس گیرنده باید مشتری خود را به url دریافتی هدایت نماید. شایان ذکر است هنگام هدایت مشتری باید در هدر درخواست مقدار referer با ادرس دامین پذیرنده ارسال شود (توضیحات بیشتر در **[راهنمای استفاده](./usage-guides.md)**). برای فراخوانی هر یک از وب سرویس ها حتما باید توکن با فرمت Bearer در header تنظیم شود.
- در صورتی که فراخوانی وب سرویس موفق باشد کد مربوط به resultCode برابر 0 خواهد بود و آدرسی که کاربر باید برای عملیات پرداخت به آن منتقل شود در data قرار خواهد گرفت.



## 3.تراکنش خرید شناسه دار

- ورودی این سرویس همانند تراکنش خرید است و کد شناسه دار در فیلد paymentCode وارد میگردد.

**URL:** `https://pep.shaparak.ir/dorsa1/api/payment/purchase`  
**Method:** POST  
**Body:**  
```json
{
    "invoice": "20301112123456",
    "invoiceDate": "1403/05/24",
    "amount": 1000,
    "callbackApi": "https://your-domain.ir/callback",
    "mobileNumber": "0935*****52",
    "serviceCode": "8",
    "serviceType": "PURCHASE",
    "terminalNumber": 12345678,
    "description": "description",
    "payerMail": "payer@example.com",
    "payerName": "payerName",
    "pans": "",
    "nationalCode": "**********",
    "paymentCode": "7878"
}
```

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| invoice | String | M | شماره فاکتور یکتا |
| invoiceDate | String | M | تاریخ فاکتور به فرمت دلخواه |
| amount | Integer | M | مبلغ به ریال |
| callbackApi | String | M | آدرسی که پس از عملیات پرداخت، کاربر به آن صفحه منتقل می شود (باید دامنه کالبک هنگام ثبت نام تعریف شده باشد) |
| mobileNumber | String | M/O | تلفن همراه پرداخت کننده |
| serviceCode | String | M | کد سرویس |
| serviceType | String | M | نوع سرویس |
| terminalNumber | Integer | M | شماره ترمینال پرداختی |
| description | String | O | توضیحات |
| payerMail | String | O | ایمیل پرداخت کننده |
| payerName | String | O | نام پرداخت کننده |
| paymentCode | String | M | شناسه پرداخت مرچنت |
| pans | String | O | شماره کارت ها |

| کد سرویس (serviceCode) | نوع سرویس (serviceType) | شرح |
|---|---|---|
| 8 | PURCHASE | خرید شناسه دار |

- در صورتیکه در قرارداد شما قید شده باشد که شماره کارت و تلفن همراه درخواست کننده تراکنش استعلام شود که متعلق به یک شخص است، مقدار تلفن همراه اجباری خواهد بود و در غیر اینصورت اختیاری است.

## خروجی موفق
```json
{
    "resultMsg": "Successful",
    "resultCode": 0,
    "data": {
        "urlId": "77db94994bfe0cd5b76be0307535c8c35cfcc338000000006217318",
        "url": "https://pep.shaparak.ir/dorsa1/77db94994bfe0cd5b76be0307535c8c35cfcc338000000006217318"
    }
}
```



## 4.تراکنش خرید با تسهیم

- منظور از تسهیم، انجام یک تراکنش خرید با کد شبای مربوطه است که حداکثر تا ده شماره شبا را میتوان وارد نمود.
- فیلد sharedValue به سه صورت پر میشود (مبلغی-درصدی-درصدی اعشاری)
- برای فیلد sharedValue ، اگر مبلغ وارد شده بالاتر از 100 باشد تراکنش از نوع تسهیم مبلغی محسوب می شود و برای مبالغ پایین تر از 100 تراکنش از نوع تسهیم درصدی ، هم چنین برای مبالغ زیر 100 و اعشاری ، تراکنش از نوع تسهیم درصدی اعشاری استفاده می شود
- شماره شباها در پارامتر sheba بدون IR وارد شوند

**URL:** `https://pep.shaparak.ir/dorsa1/api/payment/multiacc/purchase`  
**Method:** POST  
**Body:** (sharedValue بصورت درصد)  
```json
{
    "invoice": "1112223456",
    "invoiceDate": "1403/05/24",
    "amount": 10000,
    "callbackApi": "https://your-domain.ir/callback",
    "mobileNumber": "0935*****52",
    "serviceCode": "9",
    "serviceType": "MULTIACCPURCHASE",
    "terminalNumber": 12345678,
    "description": "description",
    "payerMail": "payer@example.com",
    "payerName": "payerName",
    "pans": "",
    "sharedValue": ["50", "40", "10"],
    "sheba": [
        "************************",
        "************************",
        "************************"
    ]
}
```

**Body:** (sharedValue با درصد اعشاری)  
```json
{
    "sharedValue": ["10.5", "39.5", "50"]
}
```

**Body:** (sharedValue با مبلغ)  
```json
{
    "sharedValue": ["1000", "5000", "4000"]
}
```

- در دو نمونه ی اخیر فقط فیلد sharedValue نمایش داده شده است و سایر فیلدها همانند نمونه ی اول ارسال میشوند.

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| invoice | String | M | شماره فاکتور یکتا |
| invoiceDate | String | M | تاریخ فاکتور به فرمت دلخواه |
| amount | Integer | M | مبلغ به ریال |
| callbackApi | String | M | آدرسی که پس از عملیات پرداخت، کاربر به آن صفحه منتقل می شود (باید دامنه کالبک هنگام ثبت نام تعریف شده باشد) |
| mobileNumber | String | M/O | تلفن همراه پرداخت کننده |
| serviceCode | String | M | کد سرویس |
| serviceType | String | M | نوع سرویس |
| terminalNumber | Integer | M | شماره ترمینال پرداختی |
| description | String | O | توضیحات |
| payerMail | String | O | ایمیل پرداخت کننده |
| payerName | String | O | نام پرداخت کننده |
| sharedValue | String | M | مقدار ارزش به ازای شماره شبا |
| sheba | String | M | شماره شبا |
| pans | String | O | شماره کارت ها |

| کد سرویس (serviceCode) | نوع سرویس (serviceType) | شرح |
|---|---|---|
| 9 | MULTIACCPURCHASE | خرید با تسهیم |

- در صورتیکه در قرارداد شما قید شده باشد که شماره کارت و تلفن همراه درخواست کننده تراکنش استعلام شود که متعلق به یک شخص است، مقدار تلفن همراه اجباری خواهد بود و در غیر اینصورت اختیاری است.

## خروجی موفق
```json
{
    "resultMsg": "Successful",
    "resultCode": 0,
    "data": {
        "urlId": "ffe5384aa4c51579f3935eb8956aeaa069c66e54000000006217318",
        "url": "https://pep.shaparak.ir/dorsa1/ffe5384aa4c51579f3935eb8956aeaa069c66e54000000006217318"
    }
}
```



## 5.تراکنش خرید شناسه دار با تسهیم

- این سرویس همانند تراکنش خرید با تسهیم است و شناسه پرداخت در فیلد paymentCode ارسال میگردد.
- نکات مربوط به sharedValue و sheba همانند تراکنش خرید با تسهیم است.

**URL:** `https://pep.shaparak.ir/dorsa1/api/payment/multiacc/purchase`  
**Method:** POST  
**Body:**  
```json
{
    "invoice": "1112223456",
    "invoiceDate": "1403/05/24",
    "amount": 10000,
    "callbackApi": "https://your-domain.ir/callback",
    "mobileNumber": "0935*****52",
    "serviceCode": "9",
    "serviceType": "MULTIACCPURCHASE",
    "terminalNumber": 12345678,
    "description": "description",
    "payerMail": "payer@example.com",
    "payerName": "payerName",
    "pans": "",
    "paymentCode": "7878",
    "sharedValue": ["50", "40", "10"],
    "sheba": [
        "************************",
        "************************",
        "************************"
    ]
}
```

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| invoice | String | M | شماره فاکتور یکتا |
| invoiceDate | String | M | تاریخ فاکتور به فرمت دلخواه |
| amount | Integer | M | مبلغ به ریال |
| callbackApi | String | M | آدرسی که پس از عملیات پرداخت، کاربر به آن صفحه منتقل می شود (باید دامنه کالبک هنگام ثبت نام تعریف شده باشد) |
| mobileNumber | String | M/O | تلفن همراه پرداخت کننده |
| serviceCode | String | M | کد سرویس |
| serviceType | String | M | نوع سرویس |
| terminalNumber | Integer | M | شماره ترمینال پرداختی |
| description | String | O | توضیحات |
| payerMail | String | O | ایمیل پرداخت کننده |
| payerName | String | O | نام پرداخت کننده |
| sharedValue | String | M | مقدار ارزش به ازای شماره شبا |
| sheba | String | M | شماره شبا |
| pans | String | O | شماره کارت ها |
| paymentCode | String | M | شناسه پرداخت مرچنت |

| کد سرویس (serviceCode) | نوع سرویس (serviceType) | شرح |
|---|---|---|
| 9 | MULTIACCPURCHASE | خرید شناسه دار با تسهیم |

## خروجی موفق
```json
{
    "resultMsg": "Successful",
    "resultCode": 0,
    "data": {
        "urlId": "ffe5384aa4c51579f3935eb8956aeaa069c66e54000000006217318",
        "url": "https://pep.shaparak.ir/dorsa1/ffe5384aa4c51579f3935eb8956aeaa069c66e54000000006217318"
    }
}
```



## 6.درخواست خرید شارژ / بسته اینترنتی

- از این سرویس جهت ارسال درخواست خرید شارژ مستقیم، عادی و گروهی (Topup , Evoucher) و بسته اینترنتی استفاده می شود.
- پس از ثبت درخواست، یک Url یکبار مصرف در جواب به سایت پذیرنده برگردانده می شود که کاربر نهایی را باید به آن Url هدایت (redirect) کند. پس از تکمیل فرآیند پرداخت، کاربر نهایی به آدرس callbackApi که توسط سایت پذیرنده ارسال شده بود redirect خواهد شد.

**URL:** `https://pep.shaparak.ir/dorsa1/api/payment/pre-transaction`  
**Method:** POST  
**Body:** (شارژ مستقیم)  
```json
{
    "invoice": "111222",
    "invoiceDate": "1403/05/24",
    "amount": 5000,
    "callbackApi": "https://your-domain.ir/callback",
    "mobileNumber": "0935*****52",
    "serviceCode": "2",
    "serviceType": "MTN",
    "terminalNumber": 12345678,
    "description": "description",
    "payerMail": "payer@example.com",
    "payerName": "payerName",
    "pans": "",
    "nationalCode": "**********"
}
```

**Body:** (شارژ عادی یا گروهی)  
```json
{
    "invoice": "1112223",
    "invoiceDate": "1403/05/24",
    "amount": 5000,
    "callbackApi": "https://your-domain.ir/callback",
    "mobileNumber": "0935*****52",
    "serviceCode": "6",
    "serviceType": "MTN-PIN",
    "terminalNumber": 12345678,
    "description": "description",
    "payerMail": "payer@example.com",
    "payerName": "payerName",
    "pans": "",
    "nationalCode": "**********",
    "count": 2
}
```

**Body:** (بسته اینترنتی)  
```json
{
    "invoice": "140312234123",
    "invoiceDate": "1403/05/24",
    "amount": 348700,
    "callbackApi": "https://your-domain.ir/callback",
    "mobileNumber": "0935*****52",
    "serviceCode": "2",
    "serviceType": "MTN",
    "terminalNumber": 12345678,
    "description": "description",
    "payerMail": "payer@example.com",
    "payerName": "payerName",
    "productCode": "590077",
    "pans": ""
}
```

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| amount | Integer | M | مبلغ |
| callbackApi | String | M | آدرسی که بعد از پرداخت کاربر به آن بازگشت داده می شود (باید دامنه کالبک هنگام ثبت نام تعریف شده باشد) |
| description | String | O | توضیحات تراکنش |
| invoice | String | M | شماره یکتای تراکنش |
| payerMail | String | O | ایمیل پرداخت کننده |
| payerName | String | O | نام پرداخت کننده |
| mobileNumber | String | M | شماره موبایل |
| serviceCode | String | M | کد نوع سرویس |
| serviceType | String | M | نوع سرویس |
| productCode | String | M | کد محصول (برای بسته اینترنتی) |
| terminalNumber | Integer | M | شماره ترمینال |
| pans | String | O | شماره کارت ها |
| count | Integer | M | تعداد شارژ (برای شارژ عادی و گروهی) |

- نام دامنه پارامتر ورودی callbackApi باید با آدرسی که هنگام ثبت اطلاعات پذیرنده اعلام می شود مطابقت داشته باشد.
- درخصوص خرید بسته اینترنتی پارامتر productCode باید مقداردهی شود.
- در خصوص خرید شارژ مستقیم پارامتر mobileNumber باید مقداردهی شود.
- در خصوص خرید شارژ عادی و شارژ گروهی پارامتر count باید مقداردهی شود.
- در خرید شارژ و شارژ مستقیم پارامترهای serviceType و serviceCode باید مقداردهی شوند که شامل مقادیر زیر است:

| کد سرویس (serviceCode) | نوع سرویس (serviceType) | شرح |
|---|---|---|
| 1 | MCI | شارژ مستقیم همراه اول |
| 2 | MTN | شارژ مستقیم ایرانسل |
| 3 | RTL | شارژ مستقیم رایتل |
| 5 | MCI-PIN | شارژ همراه اول |
| 6 | MTN-PIN | شارژ ایرانسل |
| 7 | RTL-PIN | شارژ رایتل |

- کد محصول (اگر بسته اینترنتی بود باید productCode وارد شود):

| عنوان | کد محصول (productCode) | قیمت |
|---|---|---|
| بسته اینترنت 5 گیگابایت 30 روزه همراه اول | 95017 | 257240 |
| بسته اینترنت 7 گیگابایت 30 روزه همراه اول | 95018 | 307380 |
| بسته اینترنت 5 گیگابایت 30 روزه ایرانسل دائمی | 930545 | 257240 |
| بسته اینترنت 7 گیگابایت 30 روزه ایرانسل دائمی | 930546 | 307380 |
| بسته اینترنت 5 گیگابایت 30 روزه ایرانسل اعتباری | 93045 | 257240 |
| بسته اینترنت 7 گیگابایت 30 روزه ایرانسل اعتباری | 93046 | 307380 |

## خروجی موفق
```json
{
    "resultMsg": "Successful",
    "resultCode": 0,
    "data": "https://pep.shaparak.ir/dorsa1/9aa3e724273b5497768f64d35ce6411c6372502c000000006217318"
}
```

| نام پارامتر | نوع | شرح |
|---|---|---|
| data | String | آدرس تولید شده جهت پرداخت |
| resultCode | Integer | کد دریافتی |
| resultMsg | String | پیغام دریافتی |



## 7.درخواست پرداخت قبض

- از این سرویس جهت ارسال درخواست پرداخت قبوض استفاده می شود. پس از ثبت درخواست، یک Url یکبار مصرف در جواب به سایت پذیرنده برگردانده می شود که کاربر نهایی را باید به آن Url هدایت (redirect) کند. پس از تکمیل فرآیند پرداخت، کاربر نهایی به آدرس callbackApi که توسط سایت پذیرنده ارسال شده بود redirect خواهد شد.

**URL:** `https://pep.shaparak.ir/dorsa1/api/payment/pre-transaction`  
**Method:** POST  
**Body:**  
```json
{
    "invoice": "11122234",
    "invoiceDate": "1403/05/24",
    "amount": 12000,
    "callbackApi": "https://your-domain.ir/callback",
    "mobileNumber": "0935*****52",
    "serviceCode": "4",
    "serviceType": "BILL",
    "terminalNumber": 12345678,
    "description": "description",
    "payerMail": "payer@example.com",
    "payerName": "payerName",
    "pans": "",
    "nationalCode": "**********",
    "billId": "1234567804121",
    "paymentId": "1270358"
}
```

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| amount | Integer | M | مبلغ |
| callbackApi | String | M | آدرسی که بعد از پرداخت کاربر به آن بازگشت داده می شود (باید دامنه کالبک هنگام ثبت نام تعریف شده باشد) |
| description | String | O | توضیحات تراکنش |
| invoice | String | M | شماره یکتای تراکنش |
| payerMail | String | O | ایمیل پرداخت کننده |
| payerName | String | O | نام پرداخت کننده |
| mobileNumber | String | O | شماره موبایل |
| serviceCode | String | M | کد نوع سرویس |
| serviceType | String | M | نوع سرویس |
| billId | String | M | شناسه قبض |
| paymentId | String | M | شناسه پرداخت |
| terminalNumber | Integer | M | شماره ترمینال |
| pans | String | O | شماره کارت ها |

| کد سرویس (serviceCode) | نوع سرویس (serviceType) | شرح |
|---|---|---|
| 4 | BILL | قبض |

- نام دامنه پارامتر ورودی callbackApi باید با آدرسی که هنگام ثبت اطلاعات پذیرنده اعلام می شود مطابقت داشته باشد.
- در پرداخت قبض، پارامترهای billId و paymentId باید مقداردهی شوند.

## خروجی موفق
```json
{
    "resultMsg": "Successful",
    "resultCode": 0,
    "data": "https://pep.shaparak.ir/dorsa1/7ab5ad99ce56b8b698af774072951b4d6bb3f590000000006217318"
}
```

| نام پارامتر | نوع | شرح |
|---|---|---|
| data | String | آدرس تولید شده جهت پرداخت |
| resultCode | Integer | کد دریافتی |
| resultMsg | String | پیغام دریافتی |



## 8.توضیحات تایید تراکنش

- (تاییدیه) confirm : در صورت فراخوانی سرویس confirm تراکنش تعیین تکلیف شده و امکان تعیین تکلیف مجدد وجود ندارد. مبلغ در سیکل شاپرکی به حساب پذیرنده واریز می شود.
- (وریفای) verify : در صورت فراخوانی سرویس verify قبل از زمان توافق شده در قرارداد؛ امکان کانفرم یا ریورس کردن تراکنش توسط پذیرنده موجود است (با محدودیت زمانی). درغیر اینصورت پس از گذشت آن زمان؛ تراکنش توسط سوییچ تایید (confirm) میشود.
- برگشت تراکنش (reverse) : پس از این عملیات؛ تراکنش تعیین تکلیف شده و امکان تعیین تکلیف وجود ندارد. مبلغ در سیکل شاپرکی به حساب دارنده کارت برمی گردد.
- منظور از مهلت زمانی این است که پذیرنده در یک زمان محدود با ارسال تاییدیه و اصلاحیه، امکان تغییر وضعیت تراکنش را دارد و پس از گذشت این زمان؛ وضعیت تراکنش تاییدیه می گردد.

| تراکنش موفق | سرویس |
|---|---|
| تایید خرید با مهلت زمانی | Verify |
| تایید خرید آنی | Confirm |
| بازگشت از خرید آنی | Reverse |



## 9.سرویس تایید عملیات خرید (confirm)

- تاییدیه بصورت آنی انجام می شود.

**URL:** `https://pep.shaparak.ir/dorsa1/api/payment/confirm-transactions`  
**Method:** POST  
**Body:**  
```json
{
    "invoice": "203011121",
    "urlId": "dfbb284970e05f5a287804966f388deed2fdbcc6000000006217318"
}
```

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| invoice | String | M | شماره فاکتور که توسط پذیرنده هنگام ثبت درخواست خرید فرستاده شده |
| urlId | String | M | توکن خرید که در جواب ثبت درخواست خرید دریافت شده است |

## خروجی موفق

```json
{
    "resultMsg": "Successful",
    "resultCode": 0,
    "data": {
        "invoice": "203011121",
        "referenceNumber": "141010031417",
        "trackId": "9",
        "maskedCardNumber": "603770******0340",
        "hashedCardNumber": "ba6567ba6c9fc28e1434b838a91028a4250185fb6dd99d62c0392538a087c8be",
        "requestDate": "2024-08-14 12:18:31.000000000",
        "amount": 1000
    }
}
```

| نام پارامتر | نوع | شرح |
|---|---|---|
| invoice | String | شماره فاکتوری که در مرحله ثبت خرید، شارژ یا پرداخت قبض توسط پذیرنده ارسال شده است |
| referenceNumber | String | شماره ارجاع شاپرکی |
| trackId | String | کد پیگیری |
| maskedCardNumber | String | شماره کارت مسک شده |
| hashedCardNumber | String | شماره کارت کد شده |
| requestDate | String | تاریخ درخواست |
| amount | Integer | مبلغ |

- در صورت موفق بودن؛ فیلد data دارای مقدار خواهد بود.
- چنانچه تایید تراکنش برای یک شماره فاکتور بیش از یکبار فراخوانی شود کد خطای 13029 را دریافت خواهید کرد و این خطا به معنی این است که تراکنش مورد نظر قبلا تایید شده است.



## 10.سرویس درخواست تاییدیه برای عملیات خرید (verify)

- پس از پایان عملیات پرداخت در درگاه بانکی، کاربر به آدرسی که توسط پذیرنده در هنگام ثبت درخواست خرید فرستاده شده است منتقل خواهد شد. در این مرحله پذیرنده باید ابتدا درخواست تایید پرداخت را فرستاده و در صورت دریافت پاسخ مناسب اقدام به تحویل سرویس به مشتری خود کند.
- برای فراخوانی این سرویس حداکثر 10 دقیقه زمان دارید، یعنی در صورتیکه پذیرنده خرید را تایید نکند پول به حساب مشتری برگشت داده خواهد شد.
- این نوع تاییدیه بسته به زمان تنظیم شده در پیکربندی ترمینال؛ بعد از این زمان انجام می شود.
- پس از انجام عملیات تایید خرید، به وسیله سرویس استعلام وضعیت تراکنش می توانید به اطلاعات تراکنش مشتری دسترسی داشته باشید.

**URL:** `https://pep.shaparak.ir/dorsa1/api/payment/verify-transactions`  
**Method:** POST  
**Body:**  
```json
{
    "checkVerify": true,
    "invoice": "2030111212",
    "urlId": "291fc237b92f43fbf8f51616d2d670149312c0fb000000006217318"
}
```

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| checkVerify | Bool | O | درصورتیکه پذیرنده تمایل داشته باشد در پاسخ به verify تکراری خطا دریافت نماید، مقدار true ارسال می گردد و در پاسخ verify تکراری خطای Already verified (کد خطای 13428) دریافت می شود. در غیر اینصورت پاسخ موفق در تراکنش verify تکراری دریافت می گردد. |
| invoice | String | M | شماره فاکتور که توسط پذیرنده هنگام ثبت درخواست خرید فرستاده شده |
| urlId | String | M | توکن خرید که در جواب ثبت درخواست خرید دریافت شده است |

## خروجی موفق

```json
{
    "data": "تراکنش موفق است",
    "resultMsg": "Successful",
    "resultCode": 0
}
```

- چنانچه تایید تراکنش برای یک شماره فاکتور بیش از یکبار فراخوانی شود کد خطای 13029 را دریافت خواهید کرد و این خطا به معنی این است که تراکنش مد نظر قبلا تایید شده است.



## 11.سرویس درخواست تاییدیه با اطلاعات تکمیلی

- عملیات این سرویس همانند سرویس verify است و فقط در پاسخ متفاوت است؛ در این سرویس اطلاعات تکمیلی تراکنش در فیلد data پاسخ ارسال می شود. همچنین اطلاعات تکمیلی پس از انجام عملیات تایید خرید، به وسیله سرویس استعلام وضعیت تراکنش قابل دریافت خواهد بود.
- برای فراخوانی این سرویس حداکثر 10 دقیقه زمان دارید، یعنی در صورتیکه پذیرنده خرید را تایید نکند پول به حساب مشتری برگشت داده خواهد شد.

**URL:** `https://pep.shaparak.ir/dorsa1/api/payment/verify-payment`  
**Method:** POST  
**Body:**  
```json
{
    "checkVerify": true,
    "invoice": "20301112123",
    "urlId": "55358de10d025f49050e9913e7ca425d20185eb2000000006217318"
}
```

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| checkVerify | Bool | O | درصورتیکه پذیرنده تمایل داشته باشد در پاسخ به verify تکراری خطا دریافت نماید، مقدار true ارسال می گردد و در پاسخ verify تکراری خطای Already verified (کد خطای 13428) دریافت می شود. در غیر اینصورت پاسخ موفق در تراکنش verify تکراری دریافت می گردد. |
| invoice | String | M | شماره فاکتور که توسط پذیرنده هنگام ثبت درخواست خرید فرستاده شده |
| urlId | String | M | توکن خرید که در جواب ثبت درخواست خرید دریافت شده است |

## خروجی موفق

```json
{
    "data": {
        "invoice": "20301112123",
        "referenceNumber": "141010031419",
        "trackId": "11",
        "maskedCardNumber": "603770******0340",
        "hashedCardNumber": "ba6567ba6c9fc28e1434b838a91028a4250185fb6dd99d62c0392538a087c8be",
        "requestDate": "2024-08-14 12:23:34.000000000",
        "amount": 1000
    },
    "resultMsg": "Successful",
    "resultCode": 0
}
```

| نام پارامتر | نوع | شرح |
|---|---|---|
| invoice | String | شماره فاکتوری که در مرحله ثبت خرید، شارژ یا پرداخت قبض توسط پذیرنده ارسال شده است |
| referenceNumber | String | شماره ارجاع شاپرکی |
| trackId | String | کد پیگیری |
| maskedCardNumber | String | شماره کارت مسک شده |
| hashedCardNumber | String | شماره کارت کد شده |
| requestDate | String | تاریخ درخواست |
| amount | Integer | مبلغ |

- در صورت موفق بودن؛ فیلد data دارای مقدار خواهد بود.
- چنانچه تایید تراکنش برای یک شماره فاکتور بیش از یکبار فراخوانی شود کد خطای 13029 را دریافت خواهید کرد و این خطا به معنی این است که تراکنش مد نظر قبلا تایید شده است.



## 12.برگشت تراکنش (reverse)

- پس از پایان عملیات پرداخت و بعد از انجام تراكنش در درگاه بانکی، در صورتیکه پذیرنده درخواست برگشت تراكنش بعد از انجام تراكنش خرید خود را داشته باشد ، عملیات برگشت تراکنش (reverse) انجام می شود. لازم به ذکر است که این سرویس فقط برای خرید و خرید تسهیمی کاربرد دارد و باید تراکنش تایید (verify) شده باشد.

**URL:** `https://pep.shaparak.ir/dorsa1/api/payment/reverse-transactions`  
**Method:** POST  
**Body:**  
```json
{
    "invoice": "203011121234",
    "urlId": "f2560bf76dfef9c51af9767e975dc02a739fbbd7000000006217318"
}
```

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| invoice | String | M | شماره فاکتور که توسط پذیرنده هنگام ثبت درخواست خرید فرستاده شده |
| urlId | String | M | توکن خرید که در جواب ثبت درخواست خرید دریافت شده است |

## خروجی موفق

```json
{
    "resultMsg": "Successful",
    "resultCode": 0
}
```



## 13.نحوه فراخوانی آدرس Callback

- پس انجام عملیات پرداخت در درگاه بانکی، کاربر به آدرس callback که هنگام ایجاد درخواست توسط پذیرنده ارسال شده بازگشت داده خواهد شد. شناسه فاکتور (invoiceId) و وضعیت پرداخت به صورت queryString به انتهای این آدرس اضافه می شود. همچنین پذیرنده می تواند از این اطلاعات جهت تعیین تکلیف تراکنش استفاده کند.

**CALLBACK_URL?invoiceId=10001&status=success&referenceNumber=142536124562&trackId=123456**

| نام پارامتر     | شرح        |
|-----------------|----------------------------------------------------------------| 
| invoiceId         | شماره فاکتوری که در مرحله ثبت خرید، شارژ یا پرداخت قبض توسط پذیرنده ارسال شده است |
| status            | وضعیت پرداخت که یکی از مقادیر زیر می‌باشد: <br>**success** : پرداخت موفق<br>**failed** : پرداخت ناموفق<br>**unknown** : نامشخص<br>**cancel** : لغو شده<br>**expire** : منقضی شده |
| referenceNumber   | شماره ارجاع شاپرکی |
| trackId           | کد پیگیری |



## 14.سرویس استعلام وضعیت تراکنش

- از این سرویس برای استعلام وضعیت تراکنش استفاده می شود.
- سناریوهای استفاده از استعلام تراکنش:
- 1.درصورتیکه پذیرنده پاسخی دریافت نکند (آدرس callback توسط درگاه فراخوانی نشود یا time out شود.) و بخواهد تراکنش را تعیین تکلیف کند، می تواند با استعلام تراکنش از وضعیت آن مطلع شود.
- 2.درصورتیکه پذیرنده بخواهد از وضعیت تراکنش مطلع شود. (اختیاری)

**URL:** `https://pep.shaparak.ir/dorsa1/api/payment/payment-inquiry`  
**Method:** POST  
**Body:**  
```json
{
    "invoiceId": "203011121234",
    "billId": "123456789000",
    "paymentId": "1472583690",
    "checkVerify": false
}
```

- جهت استعلام تراکنش قبض می توانید از فیلدهای billId و paymentId استفاده نمایید و جهت استعلام تراکنش های دیگر از فیلد invoiceId استفاده نمایید.
- اگر پذیرنده لازم بداند در استعلام تراکنش؛ از اینکه برای تراکنش مورد نظر، Verify ارسال نموده است مطلع گردد، می تواند checkVerify را با مقدار true ارسال نماید. در این حالت چنانچه وضعیت تراکنش مذکور نامشخص باشد ولی برای آن، Verify موفق ارسال شده باشد، مقدار status در پاسخ برابر با مقدار 13034 خواهد بود.

## خروجی موفق

```json
{
    "resultMsg": "Successful",
    "resultCode": 0,
    "data": {
        "status": 13033,
        "trackId": "12",
        "transactionId": "50333",
        "amount": 1000,
        "cardNumber": "603770******0340",
        "invoice": "203011121234",
        "url": "f2560bf76dfef9c51af9767e975dc02a739fbbd7000000006217318",
        "referenceNumber": "141010031420",
        "requestDate": "2024-08-14T12:25:31.000"
    }
}
```

| نام پارامتر | نوع | شرح |
|---|---|---|
| status | Integer | وضعیت تراکنش |
| trackId | String | شماره پیگیری |
| transactionId | String | Id تراکنش |
| amount | Integer | مبلغ |
| cardNumber | String | شماره کارت |
| invoice | String | فاکتور |
| url | String | ادرس url |
| referenceNumber | String | شماره مرجع |
| requestDate | String | تاریخ درخواست |
| resultCode | Integer | کد دریافتی |
| resultMsg | String | پیغام دریافتی |

- در صورت موفق بودن؛ فیلد data دارای مقدار خواهد بود.
- وضعیت تراکنش از فیلد status قابل پیگیری می باشد.
- درصورتی که فراخوانی وب سرویس موفق باشد کد مربوط به resultCode برابر 0 خواهد بود و در صورتی که مقدار resultCode مقدار دیگری داشت خطایی رخ داده است که در **[لیست خطاها](./error-codes.md)** توضیحات مربوط به آن کد خطا آمده است. resultMsg حاوی پیام مختصری مربوط به هر کد خواهد بود.
- resultCode وضعیت فراخوانی سرویس می باشد.

وضعیت هر تراکنش پس از استعلام یکی از حالت زیر است. در صورتی که با خطای دیگری روبه رو شدید به جدول لیست خطاها مراجعه فرمایید:

| کد خطا | متن خطا | شرح |
|---|---:|---|
| 0 | success | تراکنش موفق است. |
| 2 | Transaction status is unknown | وضعیت تراکنش نامشخص است. |
| 13016 | Transaction not found | تراکنش یافت نشد. |
| 13029 | Transaction status is confirmed | تراکنش موفق و تایید شده است. |
| 13025 | Transaction status is unsuccessful | تراکنش ناموفق است. |
| 13033 | Transaction status is reversed | وضعیت تراکنش اصلاحیه است (بازگشت وجه به دارنده کارت با موفقیت انجام شده است). |
| 13034 | A successful verify has been received for the desired transaction | وضعیت تراکنش در حال حاضر نامشخص است، Verify ارسال شده برای این تراکنش موفق بوده و در زمان مورد نظر تاییدیه برای این تراکنش ایجاد خواهد شد. |



## 15.سرویس استعلام وضعیت تراکنش (Hashed card number)

- این سرویس همانند سرویس استعلام وضعیت تراکنش است و علاوه بر آن، شماره کارت Hash شده را در پاسخ برمیگرداند.
- ورودی، سناریوهای استفاده و جدول وضعیت تراکنش همانند سرویس استعلام وضعیت تراکنش است.

**URL:** `https://pep.shaparak.ir/dorsa1/api/payment/payment-inquiryv2`  
**Method:** POST  
**Body:**  
```json
{
    "invoiceId": "14030620",
    "billId": "123456789000",
    "paymentId": "1472583690",
    "checkVerify": false
}
```

## خروجی موفق

```json
{
    "resultMsg": "Successful",
    "resultCode": 0,
    "data": {
        "status": 13029,
        "trackId": "10",
        "transactionId": "43",
        "amount": 10000,
        "cardNumber": "603770******0340",
        "invoice": "14030620",
        "url": "7c5e1df234b7021a59a0e6d2ac373c5cb0697d31000000007461803",
        "referenceNumber": "141010000008",
        "requestDate": "2024-09-10 11:51:56.000000000",
        "hashedCardNumber": "ba6567ba6c9fc28e1434b838a91028a4250185fb6dd99d62c0392538a087c8be"
    }
}
```

| نام پارامتر | نوع | شرح |
|---|---|---|
| status | Integer | وضعیت تراکنش |
| trackId | String | شماره پیگیری |
| transactionId | String | Id تراکنش |
| amount | Integer | مبلغ |
| cardNumber | String | شماره کارت Mask شده |
| hashedCardNumber | String | شماره کارت Hash شده |
| invoice | String | فاکتور |
| url | String | ادرس url |
| referenceNumber | String | شماره مرجع |
| requestDate | String | تاریخ درخواست |
| resultCode | Integer | کد دریافتی |
| resultMsg | String | پیغام دریافتی |

- hashedCardNumber حاوی شماره کارت Hash شده با الگوریتم sha256 می باشد.



## 16.سرویس برگشت از خرید

- برای انجام تراکنش بازگشت وجه خرید به مشتری، می توان وب سرویس برگشت از خرید را فراخوانی نمود. جهت فراخوانی وب سرویس، شماره ی فاکتور، شماره ی کارت و urlId تراکنش پرداخت شده (تراکنش اصلی) لازم می باشد.
- مبلغ بازگشتی می بایستی از مبلغ تراکنش اصلی کمتر یا برابر با آن باشد.
- تراکنشی که تایید شده باشد قابلیت برگشت از خرید را دارا می باشد.
- ترمینال هایی که از این سرویس استفاده می کنند دارای رفتار پیش فرض اصلاحیه خواهند بود.

**URL:** `https://pep.shaparak.ir/dorsa1/api/payment/refund-transactions`  
**Method:** POST  
**Body:**  
```json
{
    "amount": 1000,
    "invoice": "1731501497396312038",
    "pan": "603770******0340",
    "type": "RefundOnline",
    "urlId": "a8b008d2b76733958b4c975279a358e2e8d545e0000000007037664"
}
```

- مقدار type یکی از دو مقدار `RefundOnline` یا `RefundOffline` می باشد.
- در نمونه بالا شماره کارت به صورت Mask شده نمایش داده شده است؛ در درخواست واقعی شماره کارت کامل ارسال میشود.

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| invoice | String | M | شماره فاکتور که توسط پذیرنده هنگام ثبت درخواست خرید فرستاده شده |
| amount | Integer | M | مبلغ به ریال |
| pan | String | M | شماره کارت |
| type | String | M | نوع تراکنش برگشت (آنلاین/آفلاین) |
| urlId | String | M | توکن خرید که در جواب ثبت درخواست خرید دریافت شده |

## خروجی موفق

```json
{
    "resultMsg": "Successful",
    "resultCode": 0
}
```



## 17.سرویس تایید رمز

- با فراخوانی این سرویس، رمز ثابت کارت موردنظر، استعلام و درصورت معتبر بودن تایید می شود.

**URL:** `https://pep.shaparak.ir/dorsa1/api/payment/inquiry-confirm-pin`  
**Method:** POST  
**Body:**  
```json
{
    "cardNumber": "502229******1234",
    "cvv2": "1234",
    "expireMonth": "12",
    "expireYear": "08",
    "invoice": "001",
    "merchantNumber": 123456789,
    "pin2": "123456",
    "terminalNumber": 123456789
}
```

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| cardNumber | String | M | شماره کارت |
| cvv2 | String | M | Cvv2 کارت |
| expireMonth | String | M | ماه انقضای کارت |
| expireYear | String | M | سال انقضای کارت |
| invoice | String | M | شماره فاکتور |
| merchantNumber | Integer | M | شماره پذیرنده |
| pin2 | String | M | رمز دوم کارت |
| terminalNumber | Integer | M | شماره پایانه |

## خروجی موفق

```json
{
    "resultMsg": "Successful",
    "resultCode": 0
}
```



## 18.سرویس چکیده حساب

- با فراخوانی این سرویس، می توان 10 گردش آخر حساب را مشاهده نمود.

**URL:** `https://pep.shaparak.ir/dorsa1/api/payment/inquiry-mini-statement`  
**Method:** POST  
**Body:**  
```json
{
    "cardNumber": "502229******1234",
    "cvv2": "1234",
    "expireMonth": "12",
    "expireYear": "08",
    "invoice": "001",
    "merchantNumber": 123456789,
    "pin2": "123456",
    "terminalNumber": 123456789
}
```

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| cardNumber | String | M | شماره کارت |
| cvv2 | String | M | Cvv2 کارت |
| expireMonth | String | M | ماه انقضای کارت |
| expireYear | String | M | سال انقضای کارت |
| invoice | String | O | شماره فاکتور |
| merchantNumber | Integer | M | شماره پذیرنده |
| pin2 | String | M | رمز دوم کارت |
| terminalNumber | Integer | M | شماره پایانه |

## خروجی موفق

```json
{
    "data": [
        {
            "amountBalance": "10001",
            "amountTransaction": "20001",
            "authorizationResponseID": "11",
            "description": "توضیحات چکیده حساب",
            "localDate": "1403/01/01",
            "localTime": "1403-01-01 15:04:05"
        }
    ],
    "resultCode": 0,
    "resultMsg": "Successful"
}
```

- فیلد description در پاسخ با فرمت (ایران سیستم) ارسال می گردد.



## 19.دریافت توکن (سرویس Refund)

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



## 20.برگشت از خرید (سرویس Refund)

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

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| invoiceID | String | M | شماره فاکتور یکتا مختص تراکنش برگشت از خرید |
| invoiceDate | String | O | تاریخ فاکتور (به فرمت دلخواه) مختص تراکنش برگشت از خرید |
| amount | Integer | O | مبلغ واریز وجه (ریال) در تراکنش برگشت از خرید. درصورتیکه مبلغ ارسال نگردد، مبلغ بازگشتی در تراکنش برگشت از خرید، برابر با مقدار مبلغ تراکنش اصلی خرید لحاظ می گردد. |
| originInvoiceID | String | M | شماره فاکتور تراکنش خرید اصلی که بابت آن تراکنش بازگشت وجه درخواست داده شده است (شماره فاکتوری که توسط پذیرنده در تراکنش خرید ارسال شده است) |
| terminalNumber | Integer | M | شماره ترمینال پرداختی |
| originUrlId | String | O | توکن یا آدرس صفحه ی پرداخت خرید که در جواب ثبت درخواست خرید دریافت شده است |

## خروجی موفق

```json
{
    "resultCode": 0,
    "resultMsg": "Successful"
}
```



## 21.دریافت جزئیات تراکنش های بازگشتی (Refund) با استفاده از شماره فاکتور (InvoiceID)

**URL:** `https://pep.shaparak.ir/dorsa1/api/third-party/provider/v1/settlement/tracking/toman`  
**Method:** POST  
**Body:**  
```json
{
    "terminalNumber": 123453789,
    "invoiceId": "123"
}
```

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| terminalNumber | Integer | M | شماره پایانه |
| invoiceId | String | M | شماره فاکتور تراکنش برگشت از خرید جهت استعلام وضعیت تراکنش برگشت از خرید (فیلد invoiceID در درخواست Refund) |

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



## 22.دریافت جزئیات تراکنش های بازگشتی (Refund) با استفاده از UUID

**URL:** `https://pep.shaparak.ir/dorsa1/api/third-party/provider/v1/settlement/toman`  
**Method:** POST  
**Body:**  
```json
{
    "terminalNumber": 123453789,
    "settlementUUID": "2dee4666-32b0-48jt-mt54-6efda47b64e5"
}
```

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| terminalNumber | Integer | M | شماره پایانه |
| settlementUUID | String | M | UUID تراکنش |

- خروجی موفق و جدول وضعیت تراکنش بازگشتی همانند سرویس «دریافت جزئیات تراکنش های بازگشتی با استفاده از شماره فاکتور» است.



## 23.لیست تراکنش های بازگشتی (Refund) با استفاده از تاریخ

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

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| terminalNumber | Integer | M | شماره پایانه |
| create_after | String | O | تاریخ شروع بازه درخواستی |
| create_before | String | O | تاریخ پایان بازه درخواستی |
| page | Integer | O | شماره صفحه درخواستی |
| invoiceId | String | M | شماره فاکتور تراکنش بازگشتی |

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

- مقادیر فیلد status هر تراکنش همانند جدول «وضعیت تراکنش بازگشتی» در سرویس 21 است.



## 24.تبدیل شماره کارت به شبا (سرویس Refund)

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

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| cardNumber | String | M | شماره کارت |
| terminalNumber | Integer | M | شماره پایانه |

## خروجی موفق

```json
{
    "resultCode": 0,
    "resultMsg": "Successful",
    "iban": "IR************************"
}
```



## 25.کیف پول (سرویس Refund)

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

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| terminalNumber | Integer | M | شماره پایانه |
| invoiceId | String | M | شماره فاکتور |

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



## 26.برگشت از خرید با استفاده از سرویس پاد (سرویس Refund)

- برای انجام تراکنش بازگشت وجه خرید به مشتری، می توان وب سرویس برگشت از خرید را فراخوانی نمود. جهت فراخوانی وب سرویس، اطلاعات تراکنش اصلی (خرید) مورد نیاز می باشد.
- وضعیت تراکنش خرید جهت انجام تراکنش بازگشت وجه یا Refund می بایستی تایید شده باشد.

**URL:** `https://pep.shaparak.ir/dorsa1/api/payment/refund-transactions-3rd-party`  
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

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| invoiceID | String | M | شماره فاکتور یکتا مختص تراکنش برگشت از خرید |
| invoiceDate | String | O | تاریخ فاکتور (به فرمت دلخواه) مختص تراکنش برگشت از خرید |
| amount | Integer | O | مبلغ واریز وجه (ریال) در تراکنش برگشت از خرید. درصورتیکه مبلغ ارسال نگردد، مبلغ بازگشتی در تراکنش برگشت از خرید، برابر با مقدار مبلغ تراکنش اصلی خرید لحاظ می گردد. |
| originInvoiceID | String | M | شماره فاکتور تراکنش خرید اصلی که بابت آن تراکنش بازگشت وجه درخواست داده شده است (شماره فاکتوری که توسط پذیرنده در تراکنش خرید ارسال شده است) |
| terminalNumber | Integer | M | شماره ترمینال پرداختی |
| originUrlId | String | O | توکن یا آدرس صفحه ی پرداخت خرید که در جواب ثبت درخواست خرید دریافت شده است |

## خروجی موفق

```json
{
    "resultCode": 0,
    "resultMsg": "Successful"
}
```



## 27.دریافت جزئیات تراکنش های بازگشتی پاد (Refund) با استفاده از شماره فاکتور (InvoiceID)

**URL:** `https://pep.shaparak.ir/dorsa1/api/third-party/provider/v1/settlement/tracking`  
**Method:** POST  
**Body:**  
```json
{
    "terminalNumber": 123453789,
    "invoiceId": "123"
}
```

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| terminalNumber | Integer | M | شماره پایانه |
| invoiceId | String | M | شماره فاکتور تراکنش برگشت از خرید جهت استعلام وضعیت تراکنش برگشت از خرید (فیلد invoiceID در درخواست Refund) |

- خروجی موفق و جدول وضعیت تراکنش بازگشتی همانند سرویس 21 است.



## 28.سرویس احراز هویت

- این سرویس برای احراز هویت قوی (StrongAuth) و احراز هویت بورسی (CardInquiry) استفاده میشود.

**URL:** `https://pep.shaparak.ir/dorsa1/api/payment/authorization`  
**Method:** POST  
**Body:**  
```json
{
    "invoiceID": "654819",
    "terminalNumber": 12345678,
    "authorizationType": 1,
    "nationalCode": "encrypted-national-code",
    "mobileNumber": "0939*****94"
}
```

| نام پارامتر | نوع | اجباری / اختیاری | شرح |
|---|---|---|---|
| invoiceID | String | M | شماره فاکتور خرید انجام شده با وضعیت تایید شده |
| terminalNumber | Integer | M | شماره ترمینال پرداختی |
| authorizationType | Integer | M | در صورت نیاز به فراخوانی وب سرویس احراز هویت قوی (StrongAuth) مقدار این فیلد برابر با 1 و در صورت نیاز به فراخوانی وب سرویس احراز هویت بورسی (CardInquiry) مقدار این فیلد برابر با 2 خواهد بود. |
| nationalCode | String | M/O | کدملی بصورت رمز شده (برای فراخوانی احراز هویت بورسی این فیلد اجباری و برای فراخوانی احراز هویت قوی نیاز نمی باشد) |
| mobileNumber | String | M/O | تلفن همراه پرداخت کننده ی خرید (برای فراخوانی احراز هویت قوی این فیلد اجباری و برای فراخوانی احراز هویت بورسی نیاز نمی باشد) |

## خروجی موفق

```json
{
    "resultMsg": "Successful",
    "resultCode": 0
}
```

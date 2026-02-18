# API Reference

این فایل شامل توضیحات متدهای API درگاه پرداخت درسا بانک پاسارگاد میباشد.


## 1.دریافت توکن
**URL:**`https://pep.shaparak.ir/dorsa1/token/getToken`  
**Method:** POST  
**Body:**  
```json
{
    "username": "your-username",
    "password": "your-password"
}
```

-  مدت زمان expire شدن توکن 10 دقیقه است و درصورت دریافت خطای 401 (خطای احراز هویت ) مجدد احراز هویت انجام شود.

## خروجی موفق:
```json
{
 "resultMsg": "Successful",
 "resultCode": 0,
 "token":
"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE3MjM2Mjg2MzYsImp0aSI6IjEwMDYwIiwiaWF0Ij
oxNzIzNjI1MDM2LCJzdWIiOiJpcGdUZXN0VXNlciJ9.8TMZ2UxKK5z4v-rYEactL_OrJJ4gtVAZHFl4JcmGSn4",
 "username": "ipgTestUser",
 "firstName": "ipgTestUserFirst",
 "lastName": "ipgTestUserLast",
 "userId": "10060",
 "roles": [
 {
 "authority": "merchant"
 }
]}
```


- پس از گرفتن توکن، در هر یک از سرویس هایی که در ادامه مورد استفاده قرار خواهند گرفت باید توکن در header هر
request با فرمت token Bearer ارسال شود 




## 2.تراکنش خرید 
**URL:**`https://pep.shaparak.ir/dorsa1/api/payment/purchase`  
**Method:** POST  
**Body:**  
```json
{
    "amount": 10001,
    "invoice":"123456",
    "invoiceDate":"1403-11-05",
    "serviceCode":8,
    "serviceType":"PURCHASE",
    "callbackApi":"http://pep.co.ir",
    "payerMail":"**************",
    "mobileNumber":"************",
    "terminalNumber":*******
}
```

## خروجی موفق
```json
{
 "resultMsg": "Successful",
 "resultCode": 0,
 "data": {
 "urlId": "8dcc5cd0ef7348548f8dc2ab29ebe11a7ad3eaad000000006217318",
 "url":
"https://pep.shaparak.ir/dorsa1/8dcc5cd0ef7348548f8dc2ab29ebe11a7ad3eaad000000006217318"
 }
}
```

 - سرویس گیرنده باید مشتری خود را به url دریافتی هدایت نمایید شایان ذکر است هنگام هدایت مشتری خود باید در هدر درخواست مقدار referer را با ادرس دامین خود پر کرده و در خواست خود را ارسال نمایید برای فراخوانی هر یک از وب سرویس ها حتما باید توکن با فرمت Bearer در header تنظیم شود.



   
## 2.تراکنش خرید با تسهیم

- فیلد sharedValue به سه صورت پر میشود(مبلغی-درصدی-درصدی اعشاری)
- برای فیلد sharedValue ، اگر مبلغ وارد شده باالتر از 100 باشد تراکنش از نوع تسهیم مبلغی محسوب می شود و برای مبالغ پایین تر از 100 تراکنش از نوع تسهیم درصدی ، هم چنین برای مبالغ زیر 100 و اعشاری ، تراکنش از نوع تسهیم درصدی اعشاری استفاده می شود
- شماره شباها در پارامتر sheba بدون IR وارد شوند
  
**URL:**`https://pep.shaparak.ir/dorsa1/api/payment/multiacc/purchase)`  
**Method:** POST  
**Body:**  
```json
{
    "amount": 10001,
    "invoice":"123456",
    "invoiceDate":"1403-11-05",
    "serviceCode":8,
    "serviceType":"PURCHASE",
    "callbackApi":"http://pep.co.ir",
    "payerMail":"**************",
    "mobileNumber":"************",
    "terminalNumber":******** ,
 "sharedValue": [
    "0",
    "100"
  ],
  "sheba": [
    "*****************",
    "*****************"
  ]
}
```

## خروجی موفق
```json
{
 "resultMsg": "Successful",
 "resultCode": 0,
 "data": {
 "urlId": "8dcc5cd0ef7348548f8dc2ab29ebe11a7ad3eaad000000006217318",
 "url":
"https://pep.shaparak.ir/dorsa1/8dcc5cd0ef7348548f8dc2ab29ebe11a7ad3eaad000000006217318"
 }
}
```



## 3.استعلام وضعیت تراکنس

**URL:** `https://pep.shaparak.ir/dorsa1/api/payment/payment-inquiry)`  
**Method:** POST  
**Body:**  
```json
{
    "invoiceId":"123456"
}
```

## خروجی موفق

```json
{
 "resultMsg": "Successful",
 "resultCode": 0,
 "data": {
 "status": 2,
 "trackId": "20",
 "transactionId": "8888",
 "amount": 10001,
 "cardNumber": "610433******8596",
 "invoice": "123456",
 "url": "8dcc5cd0ef7348548f8dc2ab29ebe11a7ad3eaad000000006217318",
 "referenceNumber": "************",
 "requestDate": "2025-02-12T11:20:33.000"
 }
}
```
- وضعیت تراکنش از فیلد status قابل پیگیری می باشد و مقادیر آن از جدول انتهای مستند قابل استخراج است
- در صورتی که فراخوانی وب سرویس موفق باشد کد مربوط به resultCode برابر 0 خواهد بود و در صورتی که مقدار
resultCode مقدار دیگری داشت خطایی رخ داده است که در **[لیست خطاها](./docs/error-codes.md)** توضیحات مربوط به آن کد خطا آمده است.
resultMsg حاوی پیام مختصری مربوط به هر کد خواهد بود
-  resultCode وضعیت فراخوانی سرویس می باشد.

  



  
  ## 4.تایید تراکنش 
  
- (تاییدیه)confirm : در صورت فراخانی سرویس confrim تراکنش تایید شده و مبلغ در سیکل شاپرکی به حساب پذیرنده واریز می شود
- (وریفای) verify : در صورت فراخانی سرویس verify  قبل از زمان توافق شده در قرارداد؛ امکان کانفرم یا ریورس کردن تراکنش توسط پذیرنده موجود است(با محدودیت زمانی). درغیر اینصورت پس از گذشت آن زمان؛ تراکنش توسط سوییچ تایید confrim میشود.
- 


## 4.سرویس تایید عملیات خرید(confirm)


**URL:** `(https://pep.shaparak.ir/dorsa1/api/payment/confirm-transactions)`  
**Method:** POST  
**Body:**  
```json
{
    "invoice": "123456",
    "urlId": "8dcc5cd0ef7348548f8dc2ab29ebe11a7ad3eaad000000006217318"
} 
```

## خروجی موفق

```json
{
 "resultMsg": "Successful",
 "resultCode": 0,
 "data": {
 "invoice": "123456",
 "referenceNumber": "**********",
 "trackId": "20",
 "maskedCardNumber": "603770******0340",
 "hashedCardNumber": 
"ba6567ba6c96846464874461644250185fb6dd99d62c0392538a087c8be",
 "requestDate": "2025-02-12T11:20:33.000",
 "amount": 10001
 }
}
```




  ## 4.سرویس تایید عملیات خرید (verify)
  

**URL:** `(https://pep.shaparak.ir/dorsa1/api/payment/confirm-transactions)`  
**Method:** POST  
**Body:**  
```json
{
    "invoice": "123456",
    "urlId": "8dcc5cd0ef7348548f8dc2ab29ebe11a7ad3eaad000000006217318"
} 
```

## خروجی موفق

```json
{
 "resultMsg": "Successful",
 "resultCode": 0,
 "data": {
 "invoice": "123456",
 "referenceNumber": "**********",
 "trackId": "20",
 "maskedCardNumber": "603770******0340",
 "hashedCardNumber": 
 "ba6567ba6c96846464874461644250185fb6dd99d62c0392538a087c8be",
 "requestDate": "2025-02-12T11:20:33.000",
 "amount": 10001
 }
}
```



  ##  برگشت تراکنش 
  

**URL:** `(https://pep.shaparak.ir/dorsa1/api/payment/reverse-transactions)`  
**Method:** POST  
**Body:**  
```json
{
    "invoice": "123456",
    "urlId": "8dcc5cd0ef7348548f8dc2ab29ebe11a7ad3eaad000000006217318"
} 
```


## خروجی موفق

```json
{
 "resultMsg": "Successful",
 "resultCode": 0
}

```






## نحوه فراخوانی آدرس Callback
- پس انجام عملیات پرداخت در درگاه بانکی، کاربر به آدرس callback که هنگام ایجاد درخواست توسط پذیرنده ارسال شده بازگشت داده خواهد شد. شناسه فاکتور )invoiceId )و وضعیت پرداخت به صورت queryString به انتهای این آدرس اضافه می شود.
  همچنین پذیرنده می تواند از این اطالعات جهت تعیین تکلیف تراکنش استفاده کند.


  

**CALLBACK_URL?invoiceId=123456&status=success&referenceNumber=142536124562&trackId=20**


| نام پارامتر     | شرح        |
|-----------------|----------------------------------------------------------------|
| invoiceId       | شماره فاکتوری که در مرحله ثبت خرید، شارژ یا پرداخت قبض توسط پذیرنده ارسال شده است |
| Status          | وضعیت پرداخت که یکی از حالات زیر می باشد                    |
|          success| پرداخت موفق                                            |
|          failed |  پرداخت ناموفق                                          |
|   unknown       | نامشخص                                               |
| referenceNumber | شماره ارجاع شاپرکی                                       |
| trackId         | کد پیگیری                                              |

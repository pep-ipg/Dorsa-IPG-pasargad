# API Reference

این فایل شامل توضیحات متدهای API درگاه پرداخت درسا بانک پاسارگاد میباشد.


## دریافت توکن
**URL:**`https://pep.shaparak.ir/dorsa1/token/getToken)`  
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


- ** پس از گرفتن توکن، در هر یک از سرویس هایی که در ادامه مورد استفاده قرار خواهند گرفت باید توکن در header هر
request با فرمت token Bearer ارسال شود 


## تراکنش خرید 
**URL:**`https://pep.shaparak.ir/dorsa1/token/getToken)`  
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
    "terminalNumber": ******
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

 - ** سرویس گیرنده باید مشتری خود را به url دریافتی هدایت نمایید شایان ذکر است هنگام هدایت مشتری خود با در هدر درخواست مقدار referer را با ادرس دامین خود پر کرده و در خواست خود را ارسال نمایید برای فراخوانی هر یک از وب سرویس ها حتما باید توکن با فرمت Bearer در header تنظیم شود.



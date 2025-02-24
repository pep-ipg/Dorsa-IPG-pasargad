# Error Codes
# کد وضعیت تراکنش و لیست خطاهای فراخوانی سرویس ها 

|   کد    | متن انگلیسی     |   متن فارسی  |
|--------|----------------|------------------|
| 0      | success        | تراکنش موفق است |
| 1      | failure        | ناموفق |
| 2      | Unknown        | نامشخص |
| 401    | not authorized | مجاز به استفاده از سرویس نیستید |
| 500    | Internal server error | خطا داخلی سرور |
| 13000  | Invalid Input  | ورودی نامعتبر است |
| 13001  | Invalid url    | آدرس نامعتبر است |
| 13002  | Invalid url token | توکن نامعتبر است |
| 13003  | Invoice is not unique | کد رهگیری یکتا نیست |
| 13004  | Token is empty | توکن خالی است |
| 13005  | Gateway time out | عدم امکان دسترسی موقت به سرور |
| 13007  | captcha is expired | کپچا منقضی شده است |
| 13008  | captcha is wrong | کپچا اشتباه است |
| 13009  | url token not found | توکن یافت نشد |
| 13010  | Invalid Mobile Number | شماره موبایل نامعتبر است |
| 13011  | Invalid product code | کد محصول نامعتبر است |
| 13012  | Invalid bill id | شناسه قبض نامعتبر است |
| 13013  | Invalid payment id | شناسه پرداخت نامعتبر است |
| 13015  | between each api call must be 2 minutes | برای فراخوانی مجدد 2 دقیقه صبر کنید |
| 13016  | Transaction not found | تراکنش یافت نشد |
| 13017  | Invalid Input Length | طول ورودی نامعتبر است |
| 13018  | Not Found | یافت نشد |
| 13019  | Invoice is not match with login user | کد رهگیری برای کاربر لاگین شده در سیستم وجود ندارد |
| 13020  | Check Company Code has Exception | خطا هنگام چک کردن کد شرکت |
| 13021  | Payment Page already has been cancelled | پرداخت از پیش لغو شده است |
| 13022  | transaction not payed | تراکنش پرداخت نشده است |
| 13025  | transaction not success | تراکنش ناموفق است |
| 13026  | Invalid CardNumber | شماره کارت نامعتبر است |
| 13027  | Mobile number and CardNumber are not match | شماره موبایل برای شماره کارت وارد شده نیست |
| 13028  | callback domain is not valid | آدرس کالبک نامعتبر است |
| 13029  | transaction before is verified and it's success | تراکنش موفق و تایید شده است |
| 13030  | transaction before is verified and it's failed | تراکنش تایید شده و ناموفق است |
| 13031  | transaction is not yet verified | تراکنش منتظر تایید است |
| 13032  | Expired card | کارت منقضی شده است |
| 13033  | transaction before is reversed and it’s successful | تراکنش و بازگشت تراکنش با موفقیت انجام شده است |
| 13035  | Transaction are not allowed for reverse | تراکنش مجاز به انجام عملیات بازگشت نمی‌باشد |
| 13045  | Card holder canceled transaction | دارنده کارت تراکنش را لغو کرد |
| 13046  | Already settled | تراکنش تسویه شده است |
| 13047  | Invalid merchant | پذیرنده کارت نامعتبر است |
| 13049  | Incomplete transaction | تراکنش کامل نشده است |
| 13054  | Invalid transaction | تراکنش نامعتبر است |
| 13055  | Invalid transaction amount | مبلغ تراکنش نامعتبر است |
| 13056  | Invalid card issuer | صادرکننده کارت نامعتبر است |
| 13057  | Expired card date | تاریخ کارت منقضی شده است |
| 13058  | Card is temporarily blocked | کارت موقتاً مسدود شده است |
| 13059  | Undefined account | حساب تعریف نشده است |
| 13060  | Invalid transaction type | نوع تراکنش نامعتبر است |
| 13061  | Invalid transfer identifier | شناسه انتقال نامعتبر است |
| 13062  | Duplicate transfer identifier | تراکنش تکراری |
| 13063  | Amount is not enough | مبلغ کافی نیست |
| 13064  | Incorrect pin | پین اشتباه است |
| 13065  | No card record | کارت نامعتبر است |
| 13066  | Service not allowed on card | سرویس روی کارت مجاز نیست |
| 13067  | Card not allowed on terminal | کارت برای ترمینال مجاز نیست |
| 13068  | Transaction amount over limit | مبلغ تراکنش بیش از حد مجاز است |
| 13069  | Restricted card | کارت محدود است |
| 13070  | Security error | خطای امنیتی |
| 13071  | Transaction amount is not match with final price | مبلغ تراکنش با قیمت نهایی مطابقت ندارد |
| 13072  | Transaction request is more than limit | درخواست تراکنش بیش از حد مجاز است |
| 13073  | Inactive account | حساب غیرفعال است |
| 13074  | cardBlock by terminal | کارت توسط ترمینال مسدود شد |
| 13075  | Confirm time expired | زمان تأییدیه منقضی شده است |
| 13076  | Reverse time expired | زمان اصلاحیه منقضی شده است |
| 13077  | Invalid original transaction | تراکنش اصلی نامعتبر است |
| 13078  | Incorrect interchange amount | مبلغ اشتباه است |
| 13079  | Invalid working day | روز کاری نامعتبر است |
| 13080  | Inactive card | کارت غیر فعال است |
| 13081  | Invalid card account | حساب کارت نامعتبر است |
| 13082  | Not successful transaction | تراکنش ناموفق است |
| 13083  | Invalid encrypted data | داده‌های رمزگذاری شده نامعتبر است |
| 13084  | Switch or Shaparak signed Off | سوئیچ یا شاپرک خاموش است |
| 13085  | Host down destination bank | میزبان بانک مقصد پایین است |
| 13086  | Invalid confirm data | اطلاعات تاییدیه نامعتبر است |
| 13087  | Switch or shaparak signing off | سوئیچ یا شاپرک در حال خاموش شدن است |
| 13088  | Invalid encrypted key | کلید رمزگذاری شده نامعتبر است |
| 13089  | Banned transaction with contact interface | انجام تراکنش با واسط غیرتماسی مردود شده است، تراکنش با واسط تماسی انجام گردد |
| 13090  | End work day | پایان روز کاری |
| 13091  | Switch is inactive | سوئیچ غیر فعال است |
| 13092  | There is no way to send transaction | صادرکننده کارت نامعتبر است |
| 13093  | Transaction does not done success | تراکنش موفقیت‌آمیز نیست |
| 13094  | Duplicate transaction | تراکنش تکراری است |
| 13095  | Incorrect old pin | پین قدیمی اشتباه است |
| 13096  | Systematic error | خطای داخلی سوئیچ |
| 13097  | Key exchanging | در حال انجام فرآیند تغییر کلید |
| 13098  | Static pin over limit | پین استاتیک فراتر از محدودیت |
| 13099  | Invalid terminal | پایانه فروش نامعتبر است |
| 13300  | Terminal inActive | پایانه در سیستم غیرفعال است |
| 13301  | Invalid Merchant | فروشگاه در سیستم تعریف نشده است |
| 13302  | Invalid national code format | فرمت کد ملی نامعتبر است |
| 13303  | Invalid terminal password | پسورد ترمینال نامعتبر است |
| 13304  | Invalid bill paymentID | شناسه پرداخت نامعتبر است |
| 13305  | Insufficient transaction data | داده‌های تراکنش ناکافی است |
| 13306  | Invalid evoucher code | کد شارژ نامعتبر است |
| 13307  | Card or pin timeout | مهلت زمانی برای کارت یا پین به پایان رسیده است |
| 13308  | Unrecognized account info | اطلاعات حساب ناشناس است |
| 13309  | Not important | مهم نیست |
| 13310  | Other error | خطای دیگر |
| 13311  | All charge sold | تمام شارژها فروخته شده |
| 13312  | Charge not available | شارژ موجود نیست |
| 13313  | Operator is not available | اپراتور در دسترس نیست |
| 13314  | Mobile number is null | شماره موبایل خالی است |
| 13315  | Topup reverse error | در رزرو فاکتور مشکلی وجود دارد |
| 13316  | Settlement transaction must be confirm or reverse | اصل تراکنش مالی موفق نمی‌باشد |
| 13317  | Captcha is wrong | کپچا اشتباه است |
| 13318  | Invalid company code | کد سازمان ناموجود است |
| 13319  | EOF | سیستم با اختلال همراه است |
| 13320  | Card suspected fraud | کارت مشکوک به کلاهبرداری |
| 13321  | Terminal additional not found | اطلاعات تکمیلی پایانه موجود نیست |
| 13322  | OTP reached max retry | درخواست رمز پویا بیش از حد مجاز است |
| 13323  | Broker card inquiry failed | عدم انطباق کد ملی با شماره کارت |
| 13324  | Original transaction was not success | اصل تراکنش مالی موفق نمی‌باشد |
| 13325  | Original transaction was not Found | اصل تراکنش یافت نشد |
| 13326  | Card blocked by machine | کارت مسدود است |
| 13327  | Card blocked by machine for special condition | کارت به دلایل ویژه مسدود شده است |
| 13328  | Success with authentication card holder | موفق با احراز هویت دارنده کارت |
| 13329  | Confirm will doing | سیستم مشغول است |
| 13331  | Invalid wage | کارمزد نامعتبر است |
| 13332  | PSP is not supported By shaparak | PSP توسط شاپرک پشتیبانی نمی‌شود |
| 13333  | Card suspected fraud card block by terminal | کارت مشکوک به کلاهبرداری است |
| 13334  | Wrong entry pin is over limit | ورود پین از حد مجاز گذشته است |
| 13335  | Response lost card | کارت گمشده است |
| 13336  | Card has no Public account | کارت حساب عمومی ندارد |
| 13337  | Stolen card | کارت سرقت شده است |
| 13338  | Card has no current account | حساب تعریف نشده |
| 13339  | Response is too Late | پاسخ خیلی دیر دریافت شد |
| 13340  | CardHolder Account Status Is Bad | کارت یا حساب مبدا در وضعیت نامناسب می‌باشد |
| 13341  | Terminal Account Status Is Bad | کارت یا حساب مقصد در وضعیت نامناسب می‌باشد |
| 13342  | Card Wrong Pin Entry Is More Than Limit | ورود پین اشتباه بیش از حد مجاز است |
| 13343  | Merchant is not valid | فروشگاه نامعتبر است |
| 13344  | Card register error | خطای داخلی ثبت کارت |
| 13345  | Duplicate pan | شماره کارت تکراری است |
| 13346  | Invalid phone number | فرمت شماره موبایل نامعتبر است |
| 13347  | Invalid national code | فرمت شماره شناسنامه نامعتبر است |
| 13348  | Invalid national code or company code | کد ملی یا سازمان تکراری است |
| 13350  | Validate request Error | خطای اعتبارسنجی |
| 13351  | Invoice format is not valid | قالب شماره فاکتور معتبر نیست |
| 13352  | Username is not valid | نام کاربری معتبر نیست |
| 13353  | Operator access has been revoked | دسترسی اپراتور لغو شد |
| 13354  | Service Access Has Been Revoked | دسترسی سرویس لغو شده است |
| 13355  | Operator credit is not sufficient | اعتبار اپراتور کافی نیست |
| 13356  | There is a problem in invoice reservation | در رزرو فاکتور مشکلی وجود دارد |
| 13357  | Service credit is not sufficient | اعتبار سرویس کافی نیست |
| 13358  | Status transaction must be unknown | وضعیت تراکنش باید نامشخص باشد |
| 13359  | Protocol type does not detected | پروتکل یافت نشد |
| 13360  | Original transaction must be financial | تراکنش اصلی باید مالی باشد |
| 13361  | Settlement transaction failed | تراکنش تسویه ناموفق است |
| 13364  | Incorrect hot bill type | نوع وصول آنی نامعتبر می‌باشد |
| 13365  | Not implemented transaction | تراکنش پشتیبانی نمی‌شود |
| 13366  | Err token is not active | توکن فعال نشده است |
| 13367  | Err unexpected signing method | فرمت ورود نامعتبر |
| 13368  | Err oauth login | خطای ورود |
| 13369  | Err Invalid Input | فرمت ورودی نامعتبر |
| 13370  | Err time frame exceeding limit | خطای محدودیت در زمان |
| 13371  | Reporter permission error | خطای دسترسی |
| 13372  | Dns error | خطای بازیابی آدرس |
| 13373  | Terminal group doesn't exist | گروه ترمینال یافت نشد |
| 13374  | Request error | خطای درخواست |
| 13375  | Invalid phone number | شماره تلفن نامعتبر |
| 13376  | Error evaluation | خطای اعتبارسنجی |
| 13377  | Error atomic operation | خطا در تراکنش |
| 13378  | Error protocol | خطای پروتکل |
| 13379  | Response invalid pinBlock | پین بلاک نامعتبر است |
| 13380  | Cvv2 is not valid | CVV2 نامعتبر است |
| 13381  | Cellphone number is not supported | فرمت نادرست شماره تماس |
| 13382  | Invalid reverse mti | خطا در نوع اصلاحیه |
| 13383  | Not found settlement action | تراکنش تسویه شده است |
| 13384  | Response invalid encryptPin | رمز اشتباه است |
| 13385  | Response duplicated record | رکورد تکراری |
| 13386  | Publish error | خطا در انتقال |
| 13390  | Error image resize | خطا در تغییر سایز تصویر |
| 13391  | Response invalid sql parameter | خطا در پارامترهای ورودی |
| 13392  | Hsm request error | خطا در درخواست رمزنگاری سخت‌افزاری |
| 13393  | Request not found | درخواستی یافت نشد |
| 13394  | Error unmarshal | خطا در فرمت ورودی |
| 13395  | Rejected refund | تراکنش برگشت از خرید ناموفق است |
| 13396  | Invalid serial number | خطا در سریال پایانه فروش |
| 13397  | ReEnter transaction | ورود مجدد تراکنش |
| 13398  | Captcha Reached To Max Retry | درخواست کپچا بیش از حد مجاز است |
| 13399  | Invalid Terminal Number | ترمینال یافت نشد |
| 13400  | Public Key Not Found | کلید عمومی یافت نشد |
| 13401  | Response Voucher Not Available | شارژ مورد نظر موجود نیست |
| 13402  | Response Invalid Referer | ارجاع دهنده با اطلاعات ثبت شده تطابق ندارد |
| 13403  | Invalid Merchant IBAN | شبا معتبر نیست |
| 13404  | Failed Type Conversion | خطا در تغییر نوع متغیر |
| 13405  | Empty Value | خطا (مقدار خالی) |
| 13406  | Failed Operation | عملیات به خطا خورد |
| 13407  | Invalid Authentication | خطا در احراز هویت |
| 13408  | Invalid Organization | کد سازمان یافت نشد |
| 13409  | Invalid Offset | خطا در Offset |
| 13410  | Invalid Page size | خطا در سایز صفحه |
| 13411  | Failed Hash | خطا درهم‌سازی |
| 13412  | Failed ParseField | خطا در خواندن مقدار |
| 13413  | InvalidAction | خطا در عملیات |
| 13414  | NoRowsWereUpdated | خطا در آپدیت |
| 13415  | InvalidPurchaseIdentifier | خطا در خرید شناسه‌دار |
| 13416  | ResponseInvalidMultiPaymentData | خطای چندشبایی |
| 13417  | NotImplemented | عملیات مورد نظر پیاده‌سازی نشده است |
| 13418  | InvalidTransactionRequestTime | خطا در تاریخ درخواست |
| 13419  | InvalidLogonTransactionTime | خطا در زمان ورود |

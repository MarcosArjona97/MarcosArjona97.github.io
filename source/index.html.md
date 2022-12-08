---
title: Choice Gateaway API Reference

language_tabs:
  - csharp
  - Json

toc_footers:
  - <p>Documentation Powered by Choice</p>



search: true
---

# Introduction

Welcome to the Choice API! You can use our Choice API to access Choice API endpoints.

You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.


# Accounts with Token

## Token

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Account
    {
        public static void Token()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/token");

                var postData = "grant_type=password";
                postData += "&username=xxxxxx";
                postData += "&password=123456";
                var data = Encoding.ASCII.GetBytes(postData);

                request.Method = "POST";
                request.ContentType = "application/x-www-form-urlencoded";
                request.ContentLength = data.Length;

                using (var stream = request.GetRequestStream())
                {
                    stream.Write(data, 0, data.Length);
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> cURL Request:

```cURL
curl --location --request POST 'https://sandbox.choice.dev/api/v1/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=password' \
--data-urlencode 'username=xxxxxxxx' \
--data-urlencode 'password=xxxxxxxx'
```

> Json Example Response:

```json
{
  "access_token": "eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6EBqB7RFjVMuhmuPNWcYM7ozyMb3uaDe0gyDL_nMPESbuM5I4skBOYcUM4A06NO88CVV3yBYee7mWB1qT-YFu5A3KZJSfRIbTX9GZdrZpi-JuWsx-7GE9GIYrNJ29BpaQscTwxYDr67WiFlCCrsCqWnCPJUjCFRIrTDltz8vM15mlgjiO0y04ZACGOWNNErIVegX062oydV7SqumGJEbS9Av4gdy",
  "token_type": "bearer",
  "expires_in": 863999
}
```

This endpoint creates a bearer token.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/token`

### Headers

Key | Value
--------- | -------
Content-Type | application/x-www-form-urlencoded

### Body

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
grant_type | string | Mandatory | password.
username | string | Mandatory | Username of the user.
password | string | Mandatory | Password of the user.

### Response

* 200 code (ok).


## Change user password

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Account
    {
        public static void ChangeUserPassword()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/accounts/ChangePassword");
                request.ContentType = "text/json";
                request.Method = "POST";

                var user = new
                {
                    Username = "JohnDoe",
                    OldPassword = "123456",
                    NewPassword = "qwerty",
                    ConfirmPassword = "qwerty"
                };

                string json = JsonConvert.SerializeObject(user);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }   
    }
}
```

> Json Example Request:

```json
{
  "Username": "JohnDoe",
  "OldPassword": "123456",
  "NewPassword": "qwerty",
  "ConfirmPassword": "qwerty"
}
```

> Json Example Response:

```json
{
  "message": "Success"
}
```

This endpoint changes user's password.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/accounts/ChangePassword`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
Username | string | Mandatory | User’s username.
OldPassword | string | Mandatory | User’s old password.
NewPassword | string | Mandatory | User’s new password.
ConfirmPassword | string | Mandatory | User’s new password.

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

## Reset user password request

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Account
    {
        public static void ResetUserPasswordRequest()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/accounts/ResetPasswordRequest");
                request.ContentType = "text/json";
                request.Method = "POST";

                var user = new
                {
                    UserName = "JohnDoe"
                };

                string json = JsonConvert.SerializeObject(user);

                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
  "UserName": "JohnDoe"
}
```

> Json Example Response:

```json
{
  "message": "Success. Email sent."
}
```

This endpoint generates user's password change request.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/accounts/ResetPasswordRequest`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
Username | string | Mandatory | User’s username.

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

## Reset user password

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Account
    {
        public static void ResetUserPassword()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/accounts/ResetPassword");
                request.ContentType = "text/json";
                request.Method = "POST";

                var user = new
                {
                    Key = "3a89ad7b706f418291423159b9ab95d0ecd384bcfe814acca35dc759aa22ea0f",
                    NewPassword = "abcdef",
                    ConfirmPassword = "abcdef"
                };

                string json = JsonConvert.SerializeObject(user);

                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
  "Key": "db92b207a57a48989924788199fbc104e3034780e4904df49d857320bce0e7de",
  "NewPassword": "123457",
  "ConfirmPassword": "123457"
}
```

> Json Example Response:

```json
{
  "message": "Success. Password changed."
}
```

This endpoint resets user's password.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/accounts/ResetPassword`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
Key | string | Mandatory | Key sent to user email.
NewPassword | string | Mandatory | User’s new password.
ConfirmPassword | string | Mandatory | User’s new password.

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

# Accounts with API Key

## API Key

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Accounts
    {
        public static void AuthorizationToken()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/accounts/authorizationToken");
                request.ContentType = "text/json";
                request.Method = "POST";

                var accounts = new
                {
                    UserName = "MaxSmart",
                    Password = "12345678Aa"
                };

                string json = JsonConvert.SerializeObject(accounts);

                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Request:

```json
{
    "UserName":"MaxSmart",
    "Password":"12345678Aa"
}
```

> Json Example Response:

```json
{
    "message": "Authorization token: e516b6db-3230-4b1c-ae3f-e5379b774a80"
}
```

This endpoint creates a API Key.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/accounts/authorizationToken`

### Headers

Key | Value
--------- | -------
Content-Type | "application/json"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
UserName | string | Mandatory | UserName.
Password | string | Mandatory | Password.


### Response

* 200 code (ok).





# Users
## Get myself

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class User
    {
        public static void GetMySelf()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/users/myself");
                request.Method = "GET";

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Response:

```json
{
    "guid": "961612a5-427b-4bbc-8a11-bfc1b9149243",
    "userName": "maxwinston",
    "isoNumber": "1000",
    "firstName": "Max",
    "lastName": "Winston",
    "email": "maxwinston@mailinator.com",
    "phone": "9177563046",
    "status": "User - Active",
    "address1": "151 E 33rd ST",
    "address2": "Second Floor",
    "city": "New York",
    "state": "NY",
    "zipcode": "10016",
    "accountNumber": "17091897",
    "roles": [
        {
            "name": "Iso Admin"
        },
        {
            "name": "Iso Employee"
        },
        {
            "name": "Merchant Admin"
        }
    ]
}
```

This endpoint get myself.

### HTTP Request

`GET https://sandbox.choice.dev/api/v1/users/myself`

### Headers using token

Key | Value
--------- | -------
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>


# Bank Clearing

## Create bank clearing

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class BankClearing
    {
        public static void CreateBankClearing()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/BankClearings");
                request.ContentType = "text/json";
                request.Method = "POST";

                var bankClearing = new
                {
                    DeviceGuid = "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
                    Amount = 3.50,
                    IsBusinessPayment = true,
                    SendReceipt = true,
                    CustomData = "order details",
                    CustomerLabel = "Patient",
                    CustomerId = "xt147",
                    BusinessName = "Star Shoes California",
                    BankAccount = new
                    {
                        RoutingNumber = "211274450",
                        AccountNumber = "441142020",
                        NameOnAccount = "Joe Black",
                        Customer = new
                        {
                            FirstName = "Joe",
                            LastName = "Black",
                            Phone = "4207888807",
                            City = "Austin",
                            State = "TX",
                            Country = "US",
                            Email = "jblack@mailinator.com",
                            Address1 = "107 7th Av.",
                            Address2 = "",
                            Zip = "10007",
                            DateOfBirth = "1987-07-07",
                            DriverLicenseNumber = "12345678",
                            DriverLicenseState = "TX",
                            SSN4 = "1210"
                        }
                    }
                };

                string json = JsonConvert.SerializeObject(bankClearing);

                request.Headers.Add("Authorization", "Bearer DkpQk2A3K4Lwq6aFzbmkApOZWN28EowdVscW_sxY8_RTFt12xhnIa79bUmA12INI0YqKHwbMU5O5gMU_51E6wllvacKx68-mJ7F6GWnUkR2vFKaA-tdOJEqh0GD_61MGWWlXr9sxRNyH6ZzWqUS0-QdhcPLyFzJL-odqZ46I-Aouf3ixienzOnrL0m-zbExPAOnT58prg2dhB-rfIPBoF25rdokQ1KH4NFujgeF99qwIpObHeQQ25Qok7aJh5Spk");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Request:

```json
{
  "DeviceGuid" : "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
  "OperationType":"Debit",
  "Amount" : 3.50, 
  "IsBusinessPayment" : true,
  "SendReceipt" : true,
  "CustomData" : "order details",
  "CustomerLabel" : "Patient",
  "CustomerId" : "xt147",
  "BusinessName" : "Star Shoes California",
  "BankAccount":
  {
    "RoutingNumber" : "211274450",
    "AccountNumber" : "441142020",
    "NameOnAccount" : "Joe Black",  
    "Customer":
    {
      "FirstName" : "Joe",
      "LastName" : "Black",
      "Phone" : "4207888807",
      "City" : "Austin",
      "State" : "TX",
      "Country" : "US",
      "Email" : "jblack@mailinator.com",
      "Address1" : "107 7th Av.",
      "Address2" : "",
      "Zip" : "10007",
      "DateOfBirth" : "1987-07-07",
      "DriverLicenseNumber" : "12345678",
      "DriverLicenseState" : "TX",
      "SSN4" : "1210"
    }
  }
}
```

> Json Example Response:

```json
{
    "guid": "4d134bf6-f934-4c1c-ba93-89ca076ed43b",
    "status": "Transaction - Approved - Warning",
    "timeStamp": "2020-11-24T08:36:25.29-06:00",
    "deviceGuid": "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
    "amount": 3.50,
    "grossAmount": 3.50,
    "effectiveAmount": 0.00,
    "refNumber": "22376",
    "isBusinessPayment": true,
    "customData": "order details",
    "operationType": "Sale",
    "settlementType": "ACH",
    "customerLabel": "Patient",
    "businessName": "Star Shoes California",
    "customerID": "xt147",
    "isSettled": false,
    "processorStatusCode": "HELD",
    "processorResponseMessage": "HELD",
    "processorResponseMessageForMobile": "HELD",
    "processorTransactionStatus": "Debit Sent",
    "processorSettlementStatus": "Credit Sent",
    "wasProcessed": true,
    "customerReceipt": "326Nothing\\nBelgrano 383\n\\nNew York NY 10016\\n11/24/2020 02:36:25 PM\n\nDEBIT\\n\\nTotal:                        $3.50\\n--------------------------------------\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\n\\nSUBMITTED",
    "bankAccount": {
        "guid": "d4394cfb-1967-429b-92da-9f664c316527",
        "routingNumber": "211274450",
        "accountNumber": "441142020",
        "accountNumberLastFour": "2020",
        "nameOnAccount": "Joe Black",
        "customer": {
            "guid": "22fad359-2b37-470c-b6c1-d8fa66aa184a",
            "firstName": "Joe",
            "lastName": "Black",
            "dateOfBirth": "1987-07-07T00:00:00",
            "address1": "107 7th Av.",
            "address2": "",
            "zip": "10007",
            "city": "Austin",
            "state": "TX",
            "country": "US",
            "phone": "4207888807",
            "email": "jblack@mailinator.com",
            "ssN4": "1210",
            "driverLicenseNumber": "12345678",
            "driverLicenseState": "TX"
        }
    }
}
```

This endpoint creates a bank clearing.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/BankClearings`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device Guid.
Amount | decimal | Mandatory | Amount of the transaction. Min. amt.: $0.50
SequenceNumber | string | Optional | Use this number to call timeout reversal endpoint if you don't get a response.
OperationType  | string | Mandatory | (If nothing is indicated, by default it will be **Debit**). Allowed values:<br><br>**1. Debit**<br>**2. Credit**
IsBusinessPayment | boolean | Optional | True when using company (CCD), false when using individual (PPD).<br><br>Allowed values:<br><br>**1. true**<br>**2. false**
SendReceipt | boolean | Optional | Set to “FALSE” if you do not want an e-mail receipt to be sent to the customer. Set to “TRUE” or leave empty if you want e-mail to be sent.
CustomData | string | Optional | order details.
CustomerLabel | string | Optional | Any name you want to show instead of customer. Eg., Player, Patient, Buyer.
CustomerID | string | Optional | Any combination of numbers and letters you use to identify the customer.
BusinessName | string | Optional | A merchant name you want to use instead of your DBA.
Currency | string | Optional | Currency of the transaction.Default value: **USD**<br><br>Allowed values: USD, ARS, AUD, BRL, CAD, CHF, EUR, GBP, JPY, MXN, NZD, SGD
BankAccount | object | Mandatory | Bank Account.
 |  | 
**BankAccount** |  | 
RoutingNumber | string | Mandatory | Routing's number. Must be 9 characters (example: 490000018).
AccountNumber | string | Mandatory | Account's number.
NameOnAccount | string | Mandatory | Account's name.
Customer | object | Optional | Customer.
 |  | 
**Customer** |  | 
FirstName | string | Optional | Customer's first name.
LastName | string | Optional | Customer's last name.
Phone | string | Optional | Customer's phone number. The phone number must be syntactically correct. For example, 4152345678.
City | string | Optional | Customer's city.
State | string | Optional | Customer's short name state. The ISO 3166-2 CA and US state or province code of a customer. Length = 2.
Country | string | Optional | Customer's country. The ISO country code of a customer’s country. Length = 2 or 3.
Email | string | Optional | Customer's valid email address.
Address1 | string | Optional | Customer's address.
Address2 | string | Optional | Customer's address line 2.
Zip | string | Optional | Customer's zipcode. Length = 5.
DateOfBirth | date | Optional | Customer's date of birth.<br><br>Allowed format:<br><br>YYYY-MM-DD.<br>For example: 2002-05-30
DriverLicenseNumber | string | Optional | Customer's driver license number.
DriverLicenseState | string | Optional | Customer's driver license short name state. The ISO 3166-2 CA and US state or province code of a customer. Length = 2.
SSN4 | string | Optional | Customer's social security number.

### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

<aside class="success">
You will get updates by e-mail regarding the Bank Clearing status.
</aside>

<aside class="success">
'processorStatusCode' possible values:<br/>

<strong>VOIDED</strong> – The item was voided upon entry for various administrative purposes. This is very rare, and typically warrants an interactive login or call to support.<br/>
<strong>HELD</strong> – The item was accepted but placed on HOLD. This can be common and typically occurs if your daily processing limits have been exceeded.<br/>
<strong>SUBMITTED</strong> – This is the “so far so good” status that means your item will be processed.<br/>
<strong>TRANSMITTED</strong> – this occurs when the item has been sent out to the ACH network.<br/>
<strong>SETTLED</strong> – for DEBIT items, this means that the debited funds have been successfully settled to your account.<br/>
<strong>RETURNED</strong> – this status occurs when an item has been returned for one of many different reasons.
</aside>


## Create bank clearing with Bank Account's guid

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class BankClearing
    {
        public static void CreateBankClearing()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/BankClearings");
                request.ContentType = "text/json";
                request.Method = "POST";

                var bankClearing = new
                {
                    DeviceGuid = "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
                    Amount = 3.50,
                    IsBusinessPayment = true,
                    SendReceipt = true,
                    CustomData = "order details",
                    CustomerLabel = "Patient",
                    CustomerId = "xt147",
                    BusinessName = "Star Shoes California",
                    BankAccount = new
                    {
                        RoutingNumber = "211274450",
                        AccountNumber = "441142020",
                        NameOnAccount = "Joe Black",
                        Customer = new
                        {
                            FirstName = "Joe",
                            LastName = "Black",
                            Phone = "4207888807",
                            City = "Austin",
                            State = "TX",
                            Country = "US",
                            Email = "jblack@mailinator.com",
                            Address1 = "107 7th Av.",
                            Address2 = "",
                            Zip = "10007",
                            DateOfBirth = "1987-07-07",
                            DriverLicenseNumber = "12345678",
                            DriverLicenseState = "TX",
                            SSN4 = "1210"
                        }
                    }
                };

                string json = JsonConvert.SerializeObject(bankClearing);

                request.Headers.Add("Authorization", "Bearer DkpQk2A3K4Lwq6aFzbmkApOZWN28EowdVscW_sxY8_RTFt12xhnIa79bUmA12INI0YqKHwbMU5O5gMU_51E6wllvacKx68-mJ7F6GWnUkR2vFKaA-tdOJEqh0GD_61MGWWlXr9sxRNyH6ZzWqUS0-QdhcPLyFzJL-odqZ46I-Aouf3ixienzOnrL0m-zbExPAOnT58prg2dhB-rfIPBoF25rdokQ1KH4NFujgeF99qwIpObHeQQ25Qok7aJh5Spk");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Request Step 1:

```json
{
  "DeviceGuid" : "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
  "Amount" : 3.50, 
  "IsBusinessPayment" : true,
  "SendReceipt" : true,
  "CustomData" : "order details",
  "CustomerLabel" : "Patient",
  "CustomerId" : "xt147",
  "BusinessName" : "Star Shoes California",
  "BankAccount":
  {
    "RoutingNumber" : "211274450",
    "AccountNumber" : "441142020",
    "NameOnAccount" : "Joe Black",  
    "Customer":
    {
      "FirstName" : "Joe",
      "LastName" : "Black",
      "Phone" : "4207888807",
      "City" : "Austin",
      "State" : "TX",
      "Country" : "US",
      "Email" : "jblack@mailinator.com",
      "Address1" : "107 7th Av.",
      "Address2" : "",
      "Zip" : "10007",
      "DateOfBirth" : "1987-07-07",
      "DriverLicenseNumber" : "12345678",
      "DriverLicenseState" : "TX",
      "SSN4" : "1210"
    }
  }
}
```

> Json Example Response Step 2:

```json
{
    "guid": "4d134bf6-f934-4c1c-ba93-89ca076ed43b",
    "status": "Transaction - Approved - Warning",
    "timeStamp": "2020-11-24T08:36:25.29-06:00",
    "deviceGuid": "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
    "amount": 3.50,
    "grossAmount": 3.50,
    "effectiveAmount": 0.00,
    "refNumber": "22376",
    "isBusinessPayment": true,
    "customData": "order details",
    "operationType": "Sale",
    "settlementType": "ACH",
    "customerLabel": "Patient",
    "businessName": "Star Shoes California",
    "customerID": "xt147",
    "isSettled": false,
    "processorStatusCode": "HELD",
    "processorResponseMessage": "HELD",
    "processorResponseMessageForMobile": "HELD",
    "wasProcessed": true,
    "customerReceipt": "326Nothing\\nBelgrano 383\n\\nNew York NY 10016\\n11/24/2020 02:36:25 PM\n\nDEBIT\\n\\nTotal:                        $3.50\\n--------------------------------------\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\n\\nSUBMITTED",
    "bankAccount": {
        "guid": "d4394cfb-1967-429b-92da-9f664c316527",
        "routingNumber": "211274450",
        "accountNumber": "441142020",
        "accountNumberLastFour": "2020",
        "nameOnAccount": "Joe Black",
        "customer": {
            "guid": "22fad359-2b37-470c-b6c1-d8fa66aa184a",
            "firstName": "Joe",
            "lastName": "Black",
            "dateOfBirth": "1987-07-07T00:00:00",
            "address1": "107 7th Av.",
            "address2": "",
            "zip": "10007",
            "city": "Austin",
            "state": "TX",
            "country": "US",
            "phone": "4207888807",
            "email": "jblack@mailinator.com",
            "ssN4": "1210",
            "driverLicenseNumber": "12345678",
            "driverLicenseState": "TX"
        }
    }
}
```

> Json Example Request Step 3:

```json
{
    "DeviceGuid" : "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
    "Amount": 15.00,
    "OrderNumber" : "121212121212",
    "BankAccount": {
        "GUID": "eb49ec89-ead6-42f5-b335-9002c4a1b923"
    }
}
```

> Json Example Response Step 3:

```json
{
    "guid": " xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx ",
    "status": "Transaction - Approved",
    "timeStamp": "2022-07-26T13:29:34.28-05:00",
    "deviceGuid": " xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx ",
    "deviceName": "Andres Test Merch 2 ACH Sandbox Device",
    "merchantName": "Andres Sandbox Merchant 2",
    "amount": 15.000,
    "grossAmount": 15.000,
    "effectiveAmount": 0.000,
    "orderNumber": "121212121212",
    "refNumber": "78621",
    "isBusinessPayment": false,
    "operationType": "Debit",
    "settlementType": "ACH",
    "isSettled": false,
    "processorStatusCode": "HELD",
    "processorResponseMessage": "HELD",
    "processorResponseMessageForMobile": "HELD",
    "wasProcessed": true,
    "customerReceipt": "Andres Sandbox Merchant 2\\n10 Columbus Blvd\n\\nHartford CT 06106\\n07/26/2022 06:29:34 PM\\n\\nDEBIT\\n\\nApproval Code: 78621\\n\\nTotal:                        $15.00\\n--------------------------------------\\n\\nBank Account Name:             Test Customer\\nRouting Number:        021000089\\nDDA Last 4             3123\\nStatus: SUBMITTED\\n--------------------------------------\\n\\nYou authorize the merchant to initiate a debit or credit to the bank account provided, and adjustments for any debit entries in error to our checking and or savings account as indicated in the invoice above, to debit and or credit to the same such account. The authorization remains in effect until terminated or revoked and afford the merchant a reasonable amount of opportunity to act on it\\nCUSTOMER ACKNOWLEDGES RECEIPT OF GOODS AND/OR SERVICES IN THE AMOUNT OF THE TOTAL SHOWN HEREON AND AGREES TO PERFORM THE OBLIGATIONS SET FORTH BY THE CUSTOMER`S AGREEMENT WITH THE ISSUER\\n\\n",
    "bankAccount": {
        "guid": "eb49ec89-ead6-42f5-b335-9002c4a1b923",
        "routingNumber": "021000089",
        "accountNumber": "123123123",
        "accountNumberLastFour": "3123",
        "nameOnAccount": "Test Customer"
    }
}
```

This endpoint creates a bank clearing with bank account's Guid.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/BankClearings`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Steps

Step 1: Run a Bank Clearing Sale.

Step 2: Save the Bank Account GUID on the ISV database.

Step 3: User the Bank Account GUID (token) to create a bank Clearing Sale.

### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

<aside class="success">
You will get updates by e-mail regarding the Bank Clearing status.
</aside>

<aside class="success">
'processorStatusCode' possible values:<br/>

<strong>VOIDED</strong> – The item was voided upon entry for various administrative purposes. This is very rare, and typically warrants an interactive login or call to support.<br/>
<strong>HELD</strong> – The item was accepted but placed on HOLD. This can be common and typically occurs if your daily processing limits have been exceeded.<br/>
<strong>SUBMITTED</strong> – This is the “so far so good” status that means your item will be processed.<br/>
<strong>TRANSMITTED</strong> – this occurs when the item has been sent out to the ACH network.<br/>
<strong>SETTLED</strong> – for DEBIT items, this means that the debited funds have been successfully settled to your account.<br/>
<strong>RETURNED</strong> – this status occurs when an item has been returned for one of many different reasons.
</aside>

## Get bank clearing

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class BankClearing
    {
        public static void GetBankClearing()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/BankClearings/4d134bf6-f934-4c1c-ba93-89ca076ed43b");
                request.Method = "GET";

                request.Headers.Add("Authorization", "Bearer DkpQk2A3K4Lwq6aFzbmkApOZWN28EowdVscW_sxY8_RTFt12xhnIa79bUmA12INI0YqKHwbMU5O5gMU_51E6wllvacKx68-mJ7F6GWnUkR2vFKaA-tdOJEqh0GD_61MGWWlXr9sxRNyH6ZzWqUS0-QdhcPLyFzJL-odqZ46I-Aouf3ixienzOnrL0m-zbExPAOnT58prg2dhB-rfIPBoF25rdokQ1KH4NFujgeF99qwIpObHeQQ25Qok7aJh5Spk");

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Response:

```json
{
    "guid": "4d134bf6-f934-4c1c-ba93-89ca076ed43b",
    "status": "Transaction - Approved - Warning",
    "timeStamp": "2020-11-24T08:36:25.29-06:00",
    "deviceGuid": "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
    "amount": 3.50,
    "grossAmount": 3.50,
    "effectiveAmount": 0.00,
    "refNumber": "22376",
    "isBusinessPayment": true,
    "customData": "order details",
    "operationType": "Sale",
    "settlementType": "ACH",
    "customerLabel": "Patient",
    "businessName": "Star Shoes California",
    "customerID": "xt147",
    "isSettled": false,
    "processorStatusCode": "HELD",
    "processorResponseMessage": "HELD",
    "processorResponseMessageForMobile": "HELD",
    "wasProcessed": true,
    "customerReceipt": "326Nothing\\nBelgrano 383\n\\nNew York NY 10016\\n11/24/2020 02:36:25 PM\n\nDEBIT\\n\\nTotal:                        $3.50\\n--------------------------------------\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\n\\nSUBMITTED",
    "bankAccount": {
        "guid": "d4394cfb-1967-429b-92da-9f664c316527",
        "routingNumber": "211274450",
        "accountNumber": "441142020",
        "accountNumberLastFour": "2020",
        "nameOnAccount": "Joe Black",
        "customer": {
            "guid": "22fad359-2b37-470c-b6c1-d8fa66aa184a",
            "firstName": "Joe",
            "lastName": "Black",
            "dateOfBirth": "1987-07-07T00:00:00",
            "address1": "107 7th Av.",
            "address2": "",
            "zip": "10007",
            "city": "Austin",
            "state": "TX",
            "country": "US",
            "phone": "4207888807",
            "email": "jblack@mailinator.com",
            "ssN4": "1210",
            "driverLicenseNumber": "12345678",
            "driverLicenseState": "TX"
        }
    }
}
```

This endpoint gets a bank clearing.

### HTTP Request

`GET https://sandbox.choice.dev/api/v1/BankClearings/<guid>`

### Headers using token

Key | Value
--------- | -------
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
guid | Bank clearing’s guid to get

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

<aside class="success">
'processorStatusCode' possible values:<br/>

<strong>VOIDED</strong> – The item was voided upon entry for various administrative purposes. This is very rare, and typically warrants an interactive login or call to support.<br/>
<strong>HELD</strong> – The item was accepted but placed on HOLD. This can be common and typically occurs if your daily processing limits have been exceeded.<br/>
<strong>SUBMITTED</strong> – This is the “so far so good” status that means your item will be processed.<br/>
<strong>TRANSMITTED</strong> – this occurs when the item has been sent out to the ACH network.<br/>
<strong>SETTLED</strong> – for DEBIT items, this means that the debited funds have been successfully settled to your account.<br/>
<strong>RETURNED</strong> – this status occurs when an item has been returned for one of many different reasons.
</aside>

## Timeout Reversal

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class BankClearing
    {
        public static void TimeoutReversal()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/bankclearings/timeoutreversal");
                request.ContentType = "text/json";
                request.Method = "POST";

                var bankClearing = new
                {
                    DeviceGuid = "b29725af-b067-4a35-9819-bbb31bdf8808",
                    SequenceNumber = "849741"
                };

                string json = JsonConvert.SerializeObject(bankClearing);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Request:

```json
{
    "DeviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "SequenceNumber": "849741"
}
```

> Json Example Response:

```json
"Device Guid and Sequence Number combination found and voided."
```

This a feature that allows a user to void a transaction if it never got a response. You must send the device guid of the terminal used to run the transaction and the sequence number. If there was a sale run on that device with that sequence and it was approved, it will be voided. Otherwise you will be informed that a sale was found but it had not been approved or that there was no sale at all with that combination of device guid and sequence number.


### HTTP Request

`POST https://sandbox.choice.dev/api/v1/bankclearings/timeoutreversal`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device's Guid.
SequenceNumber | string | Mandatory | Sequence Number.

### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

# Bank Clearing Void

## Create bank clearing void

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class BankClearingVoid
    {
        public static void CreateBankClearingVoid()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/BankClearingVoids");
                request.ContentType = "text/json";
                request.Method = "POST";

                var bankClearingVoid = new
                {
                    DeviceGuid = "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
                    ClearingGuid = "4d134bf6-f934-4c1c-ba93-89ca076ed43b"
                };

                string json = JsonConvert.SerializeObject(bankClearingVoid);

                request.Headers.Add("Authorization", "Bearer DkpQk2A3K4Lwq6aFzbmkApOZWN28EowdVscW_sxY8_RTFt12xhnIa79bUmA12INI0YqKHwbMU5O5gMU_51E6wllvacKx68-mJ7F6GWnUkR2vFKaA-tdOJEqh0GD_61MGWWlXr9sxRNyH6ZzWqUS0-QdhcPLyFzJL-odqZ46I-Aouf3ixienzOnrL0m-zbExPAOnT58prg2dhB-rfIPBoF25rdokQ1KH4NFujgeF99qwIpObHeQQ25Qok7aJh5Spk");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Request:

```json
{
  "DeviceGuid" : "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
  "ClearingGuid" : "4d134bf6-f934-4c1c-ba93-89ca076ed43b"
}
```

> Json Example Response:

```json
{
    "guid": "6da854f8-2327-4561-a223-ea9914116d10",
    "status": "Transaction - Approved",
    "timeStamp": "2020-11-24T08:48:32.45-06:00",
    "deviceGuid": "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
    "clearingGuid": "4d134bf6-f934-4c1c-ba93-89ca076ed43b",
    "processorStatusCode": "VOIDED",
    "processorResponseMessage": "VOIDED",
    "wasProcessed": true,
    "relatedClearing": {
        "guid": "4d134bf6-f934-4c1c-ba93-89ca076ed43b",
        "status": "Transaction - Approved - Warning",
        "timeStamp": "2020-11-24T08:36:25.29-06:00",
        "deviceGuid": "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
        "amount": 3.50,
        "grossAmount": 3.50,
        "effectiveAmount": 0.00,
        "refNumber": "22376",
        "isBusinessPayment": true,
        "customData": "order details",
        "operationType": "Sale",
        "settlementType": "ACH",
        "customerLabel": "Patient",
        "businessName": "Star Shoes California",
        "customerID": "xt147",
        "isSettled": false,
        "processorStatusCode": "VOIDED",
        "processorResponseMessage": "VOIDED",
        "processorResponseMessageForMobile": "VOIDED",
        "wasProcessed": true,
        "customerReceipt": "326Nothing\\nBelgrano 383\n\\nNew York NY 10016\\n11/24/2020 02:36:25 PM\n\nDEBIT\\n\\nTotal:                        $3.50\\n--------------------------------------\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\n\\nSUBMITTED",
        "bankAccount": {
            "guid": "d4394cfb-1967-429b-92da-9f664c316527",
            "routingNumber": "211274450",
            "accountNumber": "441142020",
            "accountNumberLastFour": "2020",
            "nameOnAccount": "Joe Black",
            "customer": {
                "guid": "22fad359-2b37-470c-b6c1-d8fa66aa184a",
                "firstName": "Joe",
                "lastName": "Black",
                "dateOfBirth": "1987-07-07T00:00:00",
                "address1": "107 7th Av.",
                "address2": "",
                "zip": "10007",
                "city": "Austin",
                "state": "TX",
                "country": "US",
                "phone": "4207888807",
                "email": "jblack@mailinator.com",
                "ssN4": "1210",
                "driverLicenseNumber": "12345678",
                "driverLicenseState": "TX"
            }
        }
    }
}
```

This endpoint creates a bank clearing void.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/BankClearingVoids`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device Guid.
ClearingGuid | string | Mandatory | Clearing Guid.

### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

## Get bank clearing void

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class BankClearingVoid
    {
        public static void GetBankClearingVoid()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/BankClearingVoids/6da854f8-2327-4561-a223-ea9914116d10");
                request.Method = "GET";

                request.Headers.Add("Authorization", "Bearer DkpQk2A3K4Lwq6aFzbmkApOZWN28EowdVscW_sxY8_RTFt12xhnIa79bUmA12INI0YqKHwbMU5O5gMU_51E6wllvacKx68-mJ7F6GWnUkR2vFKaA-tdOJEqh0GD_61MGWWlXr9sxRNyH6ZzWqUS0-QdhcPLyFzJL-odqZ46I-Aouf3ixienzOnrL0m-zbExPAOnT58prg2dhB-rfIPBoF25rdokQ1KH4NFujgeF99qwIpObHeQQ25Qok7aJh5Spk");

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Response:

```json
{
    "guid": "6da854f8-2327-4561-a223-ea9914116d10",
    "status": "Transaction - Approved",
    "timeStamp": "2020-11-24T08:48:32.45-06:00",
    "deviceGuid": "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
    "clearingGuid": "4d134bf6-f934-4c1c-ba93-89ca076ed43b",
    "processorStatusCode": "VOIDED",
    "processorResponseMessage": "VOIDED",
    "wasProcessed": true,
    "relatedClearing": {
        "guid": "4d134bf6-f934-4c1c-ba93-89ca076ed43b",
        "status": "Transaction - Approved - Warning",
        "timeStamp": "2020-11-24T08:36:25.29-06:00",
        "deviceGuid": "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
        "amount": 3.50,
        "grossAmount": 3.50,
        "effectiveAmount": 0.00,
        "refNumber": "22376",
        "isBusinessPayment": true,
        "customData": "order details",
        "operationType": "Sale",
        "settlementType": "ACH",
        "customerLabel": "Patient",
        "businessName": "Star Shoes California",
        "customerID": "xt147",
        "isSettled": false,
        "processorStatusCode": "VOIDED",
        "processorResponseMessage": "VOIDED",
        "processorResponseMessageForMobile": "VOIDED",
        "wasProcessed": true,
        "customerReceipt": "326Nothing\\nBelgrano 383\n\\nNew York NY 10016\\n11/24/2020 02:36:25 PM\n\nDEBIT\\n\\nTotal:                        $3.50\\n--------------------------------------\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\n\\nSUBMITTED",
        "bankAccount": {
            "guid": "d4394cfb-1967-429b-92da-9f664c316527",
            "routingNumber": "211274450",
            "accountNumber": "441142020",
            "accountNumberLastFour": "2020",
            "nameOnAccount": "Joe Black",
            "customer": {
                "guid": "22fad359-2b37-470c-b6c1-d8fa66aa184a",
                "firstName": "Joe",
                "lastName": "Black",
                "dateOfBirth": "1987-07-07T00:00:00",
                "address1": "107 7th Av.",
                "address2": "",
                "zip": "10007",
                "city": "Austin",
                "state": "TX",
                "country": "US",
                "phone": "4207888807",
                "email": "jblack@mailinator.com",
                "ssN4": "1210",
                "driverLicenseNumber": "12345678",
                "driverLicenseState": "TX"
            }
        }
    }
}
```

This endpoint gets a bank clearing void.

### HTTP Request

`GET https://sandbox.choice.dev/api/v1/BankClearingVoids/<guid>`

### Headers using token

Key | Value
--------- | -------
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
guid | Bank clearing void’s guid to get

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

# Bank Clearing Return

## Create bank clearing return

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class BankClearingReturn
    {
        public static void CreateBankClearingReturn()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/BankClearingReturns");
                request.ContentType = "text/json";
                request.Method = "POST";

                var bankClearingReturn = new
                {
                    DeviceGuid = "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
                    ClearingGuid = "8c7d4ce1-1cd2-4c92-ac24-86bf7ae1c0ed"
                };

                string json = JsonConvert.SerializeObject(bankClearingReturn);

                request.Headers.Add("Authorization", "Bearer DkpQk2A3K4Lwq6aFzbmkApOZWN28EowdVscW_sxY8_RTFt12xhnIa79bUmA12INI0YqKHwbMU5O5gMU_51E6wllvacKx68-mJ7F6GWnUkR2vFKaA-tdOJEqh0GD_61MGWWlXr9sxRNyH6ZzWqUS0-QdhcPLyFzJL-odqZ46I-Aouf3ixienzOnrL0m-zbExPAOnT58prg2dhB-rfIPBoF25rdokQ1KH4NFujgeF99qwIpObHeQQ25Qok7aJh5Spk");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Request:

```json
{
  "DeviceGuid" : "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
  "ClearingGuid" : "8c7d4ce1-1cd2-4c92-ac24-86bf7ae1c0ed"
}
```

> Json Example Response:

```json
{
    "guid": "f76fda9b-3632-441b-aa8c-f98ff4389ade",
    "status": "Transaction - Approved - Warning",
    "timeStamp": "2020-11-24T09:05:12.05-06:00",
    "deviceGuid": "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
    "clearingGuid": "8c7d4ce1-1cd2-4c92-ac24-86bf7ae1c0ed",
    "processorStatusCode": "HELD",
    "processorResponseMessage": "HELD",
    "wasProcessed": true,
    "relatedClearing": {
        "guid": "8c7d4ce1-1cd2-4c92-ac24-86bf7ae1c0ed",
        "status": "Transaction - Approved - Warning",
        "timeStamp": "2020-11-24T08:53:17.61-06:00",
        "deviceGuid": "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
        "amount": 3.50,
        "grossAmount": 3.50,
        "effectiveAmount": 0.00,
        "refNumber": "22377",
        "isBusinessPayment": true,
        "customData": "order details",
        "operationType": "Sale",
        "settlementType": "ACH",
        "customerLabel": "Patient",
        "businessName": "Star Shoes California",
        "customerID": "xt147",
        "isSettled": true,
        "processorStatusCode": "HELD",
        "processorResponseMessage": "HELD",
        "processorResponseMessageForMobile": "HELD",
        "processorTransactionStatus": "Debit Sent",
        "processorSettlementStatus": "Credit Sent",
        "wasProcessed": true,
        "customerReceipt": "326Nothing\\nBelgrano 383\n\\nNew York NY 10016\\n11/24/2020 02:53:17 PM\n\nDEBIT\\n\\nTotal:                        $3.50\\n--------------------------------------\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\n\\nSUBMITTED",
        "bankAccount": {
            "guid": "cb217796-a2c9-476d-bceb-6fdcf600def6",
            "routingNumber": "211274450",
            "accountNumber": "441142020",
            "accountNumberLastFour": "2020",
            "nameOnAccount": "Joe Black",
            "customer": {
                "guid": "8e995f82-025f-45ae-a7e3-54126af4e7d4",
                "firstName": "Joe",
                "lastName": "Black",
                "dateOfBirth": "1987-07-07T00:00:00",
                "address1": "107 7th Av.",
                "address2": "",
                "zip": "10007",
                "city": "Austin",
                "state": "TX",
                "country": "US",
                "phone": "4207888807",
                "email": "jblack@mailinator.com",
                "ssN4": "1210",
                "driverLicenseNumber": "12345678",
                "driverLicenseState": "TX"
            }
        }
    }
}
```

This endpoint creates a bank clearing return.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/BankClearingReturns`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device Guid.
ClearingGuid | string | Mandatory | Clearing Guid.

### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

## Get bank clearing return

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class BankClearingReturn
    {
        public static void GetBankClearingReturn()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/BankClearingReturns/f76fda9b-3632-441b-aa8c-f98ff4389ade");
                request.Method = "GET";

                request.Headers.Add("Authorization", "Bearer DkpQk2A3K4Lwq6aFzbmkApOZWN28EowdVscW_sxY8_RTFt12xhnIa79bUmA12INI0YqKHwbMU5O5gMU_51E6wllvacKx68-mJ7F6GWnUkR2vFKaA-tdOJEqh0GD_61MGWWlXr9sxRNyH6ZzWqUS0-QdhcPLyFzJL-odqZ46I-Aouf3ixienzOnrL0m-zbExPAOnT58prg2dhB-rfIPBoF25rdokQ1KH4NFujgeF99qwIpObHeQQ25Qok7aJh5Spk");

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Response:

```json
{
    "guid": "f76fda9b-3632-441b-aa8c-f98ff4389ade",
    "status": "Transaction - Approved - Warning",
    "timeStamp": "2020-11-24T09:05:12.05-06:00",
    "deviceGuid": "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
    "clearingGuid": "8c7d4ce1-1cd2-4c92-ac24-86bf7ae1c0ed",
    "processorStatusCode": "HELD",
    "processorResponseMessage": "HELD",
    "wasProcessed": true,
    "relatedClearing": {
        "guid": "8c7d4ce1-1cd2-4c92-ac24-86bf7ae1c0ed",
        "status": "Transaction - Approved - Warning",
        "timeStamp": "2020-11-24T08:53:17.61-06:00",
        "deviceGuid": "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
        "amount": 3.50,
        "grossAmount": 3.50,
        "effectiveAmount": 0.00,
        "refNumber": "22377",
        "isBusinessPayment": true,
        "customData": "order details",
        "operationType": "Sale",
        "settlementType": "ACH",
        "customerLabel": "Patient",
        "businessName": "Star Shoes California",
        "customerID": "xt147",
        "isSettled": true,
        "processorStatusCode": "HELD",
        "processorResponseMessage": "HELD",
        "processorResponseMessageForMobile": "HELD",
        "processorTransactionStatus": "Debit Sent",
        "processorSettlementStatus": "Credit Sent",
        "wasProcessed": true,
        "customerReceipt": "326Nothing\\nBelgrano 383\n\\nNew York NY 10016\\n11/24/2020 02:53:17 PM\n\nDEBIT\\n\\nTotal:                        $3.50\\n--------------------------------------\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\n\\nSUBMITTED",
        "bankAccount": {
            "guid": "cb217796-a2c9-476d-bceb-6fdcf600def6",
            "routingNumber": "211274450",
            "accountNumber": "441142020",
            "accountNumberLastFour": "2020",
            "nameOnAccount": "Joe Black",
            "customer": {
                "guid": "8e995f82-025f-45ae-a7e3-54126af4e7d4",
                "firstName": "Joe",
                "lastName": "Black",
                "dateOfBirth": "1987-07-07T00:00:00",
                "address1": "107 7th Av.",
                "address2": "",
                "zip": "10007",
                "city": "Austin",
                "state": "TX",
                "country": "US",
                "phone": "4207888807",
                "email": "jblack@mailinator.com",
                "ssN4": "1210",
                "driverLicenseNumber": "12345678",
                "driverLicenseState": "TX"
            }
        }
    }
}
```

This endpoint gets a bank clearing return.

### HTTP Request

`GET https://sandbox.choice.dev/api/v1/BankClearingReturns/<guid>`

### Headers using token

Key | Value
--------- | -------
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"


### URL Parameters

Parameter | Description
--------- | -----------
guid | Bank clearing return’s guid to get

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

# Bank Clearing Verify

## Create bank clearing verify

```csharp

using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class BankClearingVerify
    {
        public static void CreateBankClearingVerify()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/BankClearings/Verify");
                request.ContentType = "text/json";
                request.Method = "POST";

                var verify = new
                {
                    DeviceGuid = "b29725af-b067-4a35-9819-bbb31bdf8808",
                    RoutingNumber = "211274450",
                    AccountNumber = "12345678",
                    AccountOwnership = "Personal",
                    FirstName = "Jhon",
                    LastName = "Doe"
                };

                string json = JsonConvert.SerializeObject(verify);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}

using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class BankClearingVerify
    {
        public static void CreateBankClearingVerify()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/BankClearings/Verify");
                request.ContentType = "text/json";
                request.Method = "POST";

                var verify = new
                {
                    DeviceGuid = "C96771A9-76F0-4CB3-B737-1B6A839036E7",
                    RoutingNumber = "061103852",
                    AccountNumber = "123978002",
                    AccountOwnership = "Business",
                    BusinessName= "MyBusinessName"
                };

                string json = JsonConvert.SerializeObject(verify);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```
> Json Example Request:

```json
{
    "DeviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "RoutingNumber": "211274450",
    "AccountNumber": "12345678",
    "AccountOwnership": "Personal",
    "FirstName": "Jhon",
    "LastName": "Doe"
}

{
    "DeviceGuid": "C96771A9-76F0-4CB3-B737-1B6A839036E7",
    "RoutingNumber": "211274450",
    "AccountNumber": "12345678",
    "AccountOwnership": "Business",
    "BusinessName": "MyBusinessName"
}
```
> Json Example Response:

```json 
{
    "routingNumber": "061103852",
    "accountNumber": "123978002",
    "responseCode": "2",
    "responseValue": "Format Error",
    "responseDescription": "Invalid Account or Account format is suspicious"
}

{
    "routingNumber": "211274450",
    "accountNumber": "12345678",
    "responseCode": "2",
    "responseValue": "Format Error",
    "responseDescription": "2 - Invalid Account or Account format is suspicious"
}
```

This endpoint creates a bank clearing verify.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/BankClearings/Verify`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device’s Guid.
RoutingNumber | string | Mandatory | Routing's number. Must be 9 characters (example: 490000018).
AccountNumber | string | Mandatory | Account's number.
AccountOwnership  | string | Mandatory | Allowed values:<br><br>**1. Personal**<br>**2. Business**
FirstName | string | Mandatory when AccountOwnership('Personal') is provided | User’s first name.
LastName | string | Mandatory when AccountOwnership('Personal') is provided | User’s last name.
BusinessName | string | Mandatory when AccountOwnership('Business') is provided | User’s business name.

### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>


# Sale

## Create sale

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Sale
    {
        public static void CreateSale()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/sales");
                request.ContentType = "text/json";
                request.Method = "POST";

                var sale = new
                {
                    DeviceGuid = "b29725af-b067-4a35-9819-bbb31bdf8808",
                    Amount = 19.74,
                    TipAmount = 0.74,
                    ServiceFee = 3.00,
                    OrderNumber = "11518",
                    OrderDate = "2017-02-03",
                    SendReceipt = true,
                    CustomData = "order details",
                    CustomerLabel = "Patient",
                    CustomerId = "xt147",
                    StatementDescription = "Value stated by the user",
                    BusinessName = "Star Shoes California",
                    AssociateCustomerCard = true,
                    SemiIntegrated = false
                    Card = new
                    {
                        CardNumber = "5306764208460213",
                        CardHolderName = "John Doe",
                        Cvv2 = "998",
                        ExpirationDate = "2207",
                        Customer = new
                        {
                            FirstName = "John",
                            LastName = "Doe",
                            Phone = "9177563007",
                            City = "New York",
                            State = "NY",
                            Country = "US",
                            Email = "johnd@mailinator.com",
                            Address1 = "107 7th Av.",
                            Address2 = "",
                            Zip = "10007",
                            DateOfBirth = "1987-07-07",
                            DriverLicenseNumber = "12345678",
                            DriverLicenseState = "TX",
                            SSN4 = "1210"
                        }
                    }
                };

                string json = JsonConvert.SerializeObject(sale);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
-----------------------------------------------------------
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Sale
    {
        public static void CreateSale()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/sales");
                request.ContentType = "text/json";
                request.Method = "POST";

                var sale = new
                {
                    DeviceGuid = "b29725af-b067-4a35-9819-bbb31bdf8808",
                    Amount = 19.74,
                    TipAmount = 0.74,
                    ServiceFee = 3.00,
                    OrderNumber = "11518",
                    OrderDate = "2017-02-03",
                    SendReceipt = true,
                    CustomData = "order details",
                    CustomerLabel = "Patient",
                    CustomerId = "xt147",
                    StatementDescription = "Value stated by the user",
                    BusinessName = "Star Shoes California",
                    AssociateCustomerCard = true,
                    SemiIntegrated = true
                };

                string json = JsonConvert.SerializeObject(sale);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Request:

```json

{
    "DeviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "Amount": 19.74,
    "ServiceFee": 0.40,
    "OrderNumber": "11518",
    "OrderDate": "2020-11-24",
    "SendReceipt": true,
    "CustomData": "order details",
    "CustomerLabel": "Patient",
    "CustomerId": "xt147",
    "StatementDescription": "Value stated by the user",
    "BusinessName": "Star Shoes California",
    "AssociateCustomerCard": true,
    "Card": {
        "CardNumber": "5306764208460213",
        "CardHolderName": "John Doe",
        "Cvv2": "998",
        "ExpirationDate": "2207",
        "Customer": {
            "FirstName": "John",
            "LastName": "Doe",
            "Phone": "9177563007",
            "City": "New York",
            "State": "NY",
            "Country": "US",
            "Email": "johnd@mailinator.com",
            "Address1": "107 7th Av.",
            "Address2": "",
            "Zip": "10007",
            "DateOfBirth": "1987-07-07",
            "DriverLicenseNumber": "12345678",
            "DriverLicenseState": "TX",
            "SSN4": "1210"
        }
    }
}
-----------------------------------------------------------
{
    "DeviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "Amount": 19.74,
    "ServiceFee": 0.40,
    "OrderNumber": "11518",
    "OrderDate": "2020-11-24",
    "SendReceipt": true,
    "CustomData": "order details",
    "CustomerLabel": "Patient",
    "CustomerId": "xt147",
    "StatementDescription": "Value stated by the user",
    "BusinessName": "Star Shoes California",
    "AssociateCustomerCard": true,
    "SemiIntegrated": true
}
```

> Json Example Response:

```json
{
    "guid": "1dde1960-3dea-472c-836e-402840967e7b",
    "status": "Transaction - Approved",
    "batchStatus": "Batch - Open",
    "timeStamp": "2020-11-24T12:03:33.02-06:00",
    "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "amount": 19.74,
    "effectiveAmount": 19.74,
    "grossAmount": 19.74,
    "ServiceFee": 3.00,
    "orderNumber": "11518",
    "orderDate": "2020-11-24T00:00:00",
    "cardDataSource": "INTERNET",
    "customerLabel": "Patient",
    "businessName": "Star Shoes California",
    "customerID": "xt147",
    "batchGuid": "014db6d5-e627-472b-8f7d-02901693725f",
    "processorStatusCode": "A0000",
    "processorResponseMessage": "Success",
    "wasProcessed": true,
    "authCode": "VTLMC1",
    "refNumber": "24805958",
    "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/24/2020 11:03:33\\n\\nCREDIT - SALE\\n\\nCARD # : **** **** **** 0213\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24805958\\n\\nAUTH CODE : VTLMC1\\nSubtotal:                       $19.74\\n--------------------------------------\\nTotal:                          $19.74\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
    "customData": "order details",
    "generatedBy": "maxduplessy",
    "card": {
        "first6": "530676",
        "card.last4": "0213",
        "cardNumber": "1zcGT7J4pkGh0213",
        "cardHolderName": "John Doe",
        "cardType": "Mastercard",
        "expirationDate": "2022-07",
        "customer": {
            "guid": "b70c80d2-4b07-423e-83be-33b93a64b6dd",
            "firstName": "John",
            "lastName": "Doe",
            "dateOfBirth": "1987-07-07T00:00:00",
            "address1": "107 7th Av.",
            "address2": "",
            "zip": "10007",
            "city": "New York",
            "state": "NY",
            "country": "US",
            "phone": "9177563007",
            "email": "johnd@mailinator.com",
            "ssN4": "1210",
            "driverLicenseNumber": "12345678",
            "driverLicenseState": "TX"
        }
    },
    "associateCustomerCard": true,
    "addressVerificationCode": "N",
    "addressVerificationResult": "No Match",
    "cvvVerificationCode": "M",
    "cvvVerificationResult": "Passed"
}


```

The Sale transaction is used to charge a credit card. When running a sale you’re authorizing an amount on a credit card that will make the settlement of that amount at the end of the day. The sale is just like running an AuthOnly and a Capture all together.

This endpoint creates a sale.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/sales`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"


### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device's Guid.
Amount | decimal | Mandatory | Amount of the transaction. Min. amt.: $0.50
TipAmount | decimal | Optional | Tip Amount of the transaction.
ServiceFee | decimal | Optional | Charge service fee.
SequenceNumber | string | Optional | Use this number to call timeout reversal endpoint if you don't get a response.
OrderNumber | string |  Optional | Merchant's order number. The value sent on this element will be returned as InvoiceNumber. Length = 17.
OrderDate | date | Optional | Order Date.
SendReceipt | boolean | Optional | Set to “FALSE” if you do not want an e-mail receipt to be sent to the customer. Set to “TRUE” or leave empty if you want e-mail to be sent.
CustomData | string | Optional | order details.
CustomerLabel | string | Optional | Any name you want to show instead of customer. Eg., Player, Patient, Buyer.
CustomerID | string | Optional | Any combination of numbers and letters you use to identify the customer.
BusinessName | string | Optional | A merchant name you want to use instead of your DBA.
AssociateCustomerCard | boolean | Optional | An option to know if you want use to save customer and card token.
SemiIntegrated | boolean | Optional | Only when physical terminal used on semi integrated mode, send value True.
Currency | string | Optional | Currency of the transaction.Default value: **USD**<br><br>Allowed values: USD, ARS, AUD, BRL, CAD, CHF, EUR, GBP, JPY, MXN, NZD, SGD
Card | object | Mandatory | Card.
EnhancedData | object | Optional | EnhancedData.
 |  | 
**Card** |  |  | 
CardNumber | string | Mandatory | Card number. Must be 16 characters.  (example: 4532538795426624) or token (example: FfL7exC7Xe2y6624).
CardHolderName | string | Optional | Cardholder's name.
Cvv2 | string | Optional | This is the three or four digit CVV code at the back side of the credit and debit card.
ExpirationDate | date | Optional with Token | Card's expiry date in the YYMM format.
Customer | object | Optional | Customer.
 |  | 
**Customer** |  | 
FirstName | string | Optional | Customer's first name.
LastName | string | Optional | Customer's last name.
Phone | string | Optional | Customer's phone number. The phone number must be syntactically correct. For example, 4152345678.
City | string | Optional | Customer's city.
State | string | Optional | Customer's short name state. The ISO 3166-2 CA and US state or province code of a customer. Length = 2.
Country | string | Optional | Customer's country. The ISO country code of a customer’s country. Length = 2 or 3.
Email | string | Optional | Customer's valid email address.
Address1 | string | Optional | Customer's address.
Address2 | string | Optional | Customer's address line 2.
Zip | strings | Optional | Customer's zipcode. Length = 5.
DateOfBirth | date | Optional | Customer's date of birth.<br><br>Allowed format:<br><br>YYYY-MM-DD.<br>For example: 2002-05-30
DriverLicenseNumber | string | Optional | Customer's driver license number.
DriverLicenseState | string | Mandatory when DriverLicenseNumber is provided | Customer's driver license short name state. The ISO 3166-2 CA and US state or province code of a customer. Length = 2.
SSN4 | string | Mandatory when DOB is not submitted | Customer's social security number.
 |  | 
**EnhancedData** |  |
SaleTax | decimal | Optional | Transaction's amount.
SaleTaxZipCode | decimal | Optional | Sale Tax's zipcode. Length = 5..
SaleTaxZipCodePercentage | decimal | Optional | Sale Tax ZipCode Percentage.
AdditionalTaxDetailTaxCategory | string | Optional | Tax Category.
AdditionalTaxDetailTaxType | string | Optional | Tax Type.
AdditionalTaxDetailTaxAmount | decimal | Optional | Tax Amount.
AdditionalTaxDetailTaxRate | decimal | Optional | Tax Rate.
PurchaseOrder | string | Optional | Purchase Order.
ShippingCharges | decimal | Optional | Shipping Charges.
DutyCharges | decimal | Optional | Duty Charges.
CustomerVATNumber | string | Optional | Customer VAT Number.
VATInvoice | string | Optional | VAT Invoice.
SummaryCommodityCode | string | Optional | Summary Commodity Code.
ShipToZip | string | Optional | Ship To  Zip.
ShipFromZip | string | Optional | Ship From Zip.
DestinationCountryCode | string | Optional | Destination Country Code.
SupplierReferenceNumber | string | Optional | Supplier Reference Number.
CustomerRefID | string | Optional | Customer Ref ID.
ChargeDescriptor | string | Optional | Charge Descriptor.
AdditionalAmount | decimal | Optional | Additional Amount.
AdditionalAmountType | string | Optional | Additional Amount Type.
ProductName | string | Optional | Product Name.
ProductCode | string | Optional | Product Code.
Price | string | Optional | Price.
MeasurementUnit | string | Optional | Measurement Unit.


### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>


## Get sale

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Sale
    {
        public static void GetSale()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/sales/1dde1960-3dea-472c-836e-402840967e7b");
                request.Method = "GET";

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Response:

```json
{
    "guid": "1dde1960-3dea-472c-836e-402840967e7b",
    "status": "Transaction - Approved",
    "batchStatus": "Batch - Open",
    "timeStamp": "2020-11-24T12:03:33.02-06:00",
    "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "amount": 19.74,
    "effectiveAmount": 19.74,
    "grossAmount": 19.74,
    "orderNumber": "11518",
    "orderDate": "2020-11-24T00:00:00",
    "cardDataSource": "INTERNET",
    "customerLabel": "Patient",
    "businessName": "Star Shoes California",
    "customerID": "xt147",
    "batchGuid": "014db6d5-e627-472b-8f7d-02901693725f",
    "processorStatusCode": "A0000",
    "processorResponseMessage": "Success",
    "wasProcessed": true,
    "authCode": "VTLMC1",
    "refNumber": "24805958",
    "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/24/2020 11:03:33\\n\\nCREDIT - SALE\\n\\nCARD # : **** **** **** 0213\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24805958\\n\\nAUTH CODE : VTLMC1\\nSubtotal:                       $19.74\\n--------------------------------------\\nTotal:                          $19.74\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
    "customData": "order details",
    "generatedBy": "maxduplessy",
    "card": {
        "first6": "530676",
        "card.last4": "0213",
        "cardNumber": "1zcGT7J4pkGh0213",
        "cardHolderName": "John Doe",
        "cardType": "Mastercard",
        "expirationDate": "2022-07",
        "customer": {
            "guid": "b70c80d2-4b07-423e-83be-33b93a64b6dd",
            "firstName": "John",
            "lastName": "Doe",
            "dateOfBirth": "1987-07-07T00:00:00",
            "address1": "107 7th Av.",
            "address2": "",
            "zip": "10007",
            "city": "New York",
            "state": "NY",
            "country": "US",
            "phone": "9177563007",
            "email": "johnd@mailinator.com",
            "ssN4": "1210",
            "driverLicenseNumber": "12345678",
            "driverLicenseState": "TX"
        }
    },
    "associateCustomerCard": true,
    "addressVerificationCode": "N",
    "addressVerificationResult": "No Match",
    "cvvVerificationCode": "M",
    "cvvVerificationResult": "Passed"
}
```

This endpoint gets a sale.

### HTTP Request

`GET https://sandbox.choice.dev/api/v1/sales/<guid>`

### Headers using token

Key | Value
--------- | -------
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
guid | Sale’s guid to get 

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

## Create Tip Adjustment

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Sale
    {
        public static void CreateTipAdjustment()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/sales/TipAdjustment");
                request.ContentType = "text/json";
                request.Method = "POST";

                var tipAdjustment = new
                {
                    DeviceGuid = "b29725af-b067-4a35-9819-bbb31bdf8808",
                    TipAmount = 3.00,
                    SaleGuid = "1dde1960-3dea-472c-836e-402840967e7b"
                };

                string json = JsonConvert.SerializeObject(tipAdjustment);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Request:

```json
{
  "DeviceGuid" : "b29725af-b067-4a35-9819-bbb31bdf8808",
  "TipAmount" : 3.00,
  "SaleGuid" : "1dde1960-3dea-472c-836e-402840967e7b"
}
```

> Json Example Response:

```json
{
    "guid": "dbd023d0-733c-4317-9e7f-a67a45a56637",
    "tipAmount": 3.00,
    "status": "Transaction - Approved",
    "timeStamp": "2020-11-24T13:43:16.18-06:00",
    "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "saleGuid": "1dde1960-3dea-472c-836e-402840967e7b",
    "saleReferenceNumber": "24805958",
    "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/24/2020 12:43:16\\n\\nCREDIT - SALE\\n\\nCARD # : **** **** **** 0213\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24805958\\nInvoice number : 11518\\nAUTH CODE : VTLMC1\\nSubtotal:                       $19.74\\n--------------------------------------\\nTip:                             $3.00\\n--------------------------------------\\nTotal:                          $22.74\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
    "processorStatusCode": "A0000",
    "wasProcessed": true,
    "sale": {
        "guid": "1dde1960-3dea-472c-836e-402840967e7b",
        "status": "Transaction - Approved",
        "batchStatus": "Batch - Open",
        "timeStamp": "2020-11-24T12:03:33.02-06:00",
        "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
        "amount": 19.74,
        "effectiveAmount": 22.74,
        "grossAmount": 19.74,
        "orderNumber": "11518",
        "orderDate": "2020-11-24T00:00:00",
        "cardDataSource": "INTERNET",
        "customerLabel": "Patient",
        "businessName": "Star Shoes California",
        "customerID": "xt147",
        "batchGuid": "014db6d5-e627-472b-8f7d-02901693725f",
        "processorStatusCode": "A0000",
        "processorResponseMessage": "Success",
        "wasProcessed": true,
        "authCode": "VTLMC1",
        "refNumber": "24805958",
        "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/24/2020 12:43:16\\n\\nCREDIT - SALE\\n\\nCARD # : **** **** **** 0213\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24805958\\nInvoice number : 11518\\nAUTH CODE : VTLMC1\\nSubtotal:                       $19.74\\n--------------------------------------\\nTip:                             $3.00\\n--------------------------------------\\nTotal:                          $22.74\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
        "customData": "order details",
        "generatedBy": "maxduplessy",
        "card": {
            "first6": "530676",
            "card.last4": "0213",
            "cardNumber": "1zcGT7J4pkGh0213",
            "cardHolderName": "John Doe",
            "cardType": "Mastercard",
            "expirationDate": "2022-07",
            "customer": {
                "guid": "b70c80d2-4b07-423e-83be-33b93a64b6dd",
                "firstName": "John",
                "lastName": "Doe",
                "dateOfBirth": "1987-07-07T00:00:00",
                "address1": "107 7th Av.",
                "address2": "",
                "zip": "10007",
                "city": "New York",
                "state": "NY",
                "country": "US",
                "phone": "9177563007",
                "email": "johnd@mailinator.com",
                "ssN4": "1210",
                "driverLicenseNumber": "12345678",
                "driverLicenseState": "TX"
            }
        },
        "associateCustomerCard": true,
        "addressVerificationCode": "N",
        "addressVerificationResult": "No Match",
        "cvvVerificationCode": "M",
        "cvvVerificationResult": "Passed"
    }
}
```

This endpoint creates a tip adjustment.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/sales/TipAdjustment`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device's Guid.
TipAmount | decimal | Mandatory | Tip Amount.
SaleGuid | string | Mandatory when SaleReferenceNumber field are not sent | Sale's Guid.
SaleReferenceNumber | string | Mandatory when SaleGuid field are not sent | Sale Reference Number.


### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

## Sales by batch

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Sale
    {
        public static void GetSaleByBatch()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/sales/Batch/14db6d5-e627-472b-8f7d-02901693725f");
                request.Method = "GET";

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Response:

```json
[
    {
        "guid": "1dde1960-3dea-472c-836e-402840967e7b",
        "status": "Transaction - Approved",
        "batchStatus": "Batch - Open",
        "timeStamp": "2020-11-24T12:03:33.02-06:00",
        "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
        "amount": 19.74,
        "effectiveAmount": 22.74,
        "grossAmount": 19.74,
        "orderNumber": "11518",
        "orderDate": "2020-11-24T00:00:00",
        "cardDataSource": "INTERNET",
        "customerLabel": "Patient",
        "businessName": "Star Shoes California",
        "customerID": "xt147",
        "batchGuid": "014db6d5-e627-472b-8f7d-02901693725f",
        "processorStatusCode": "A0000",
        "processorResponseMessage": "Success",
        "wasProcessed": true,
        "authCode": "VTLMC1",
        "refNumber": "24805958",
        "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/24/2020 12:43:16\\n\\nCREDIT - SALE\\n\\nCARD # : **** **** **** 0213\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24805958\\nInvoice number : 11518\\nAUTH CODE : VTLMC1\\nSubtotal:                       $19.74\\n--------------------------------------\\nTip:                             $3.00\\n--------------------------------------\\nTotal:                          $22.74\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
        "customData": "order details",
        "generatedBy": "maxduplessy",
        "card": {
            "first6": "530676",
            "card.last4": "0213",
            "cardNumber": "1zcGT7J4pkGh0213",
            "cardHolderName": "John Doe",
            "cardType": "Mastercard",
            "expirationDate": "2022-07",
            "customer": {
                "guid": "b70c80d2-4b07-423e-83be-33b93a64b6dd",
                "firstName": "John",
                "lastName": "Doe",
                "dateOfBirth": "1987-07-07T00:00:00",
                "address1": "107 7th Av.",
                "address2": "",
                "zip": "10007",
                "city": "New York",
                "state": "NY",
                "country": "US",
                "phone": "9177563007",
                "email": "johnd@mailinator.com",
                "ssN4": "1210",
                "driverLicenseNumber": "12345678",
                "driverLicenseState": "TX"
            }
        },
        "associateCustomerCard": true,
        "addressVerificationCode": "N",
        "addressVerificationResult": "No Match",
        "cvvVerificationCode": "M",
        "cvvVerificationResult": "Passed"
    }
]
```

This endpoint searches all sales by batch.

### HTTP Request

`GET https://sandbox.choice.dev/api/v1/sales/Batch/<guid>`

### Headers using token

Key | Value
--------- | -------
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
guid | Batch’s guid to get 

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>


## Timeout Reversal

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Sale
    {
        public static void TimeoutReversal()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/sales/timeoutreversal");
                request.ContentType = "text/json";
                request.Method = "POST";

                var sale = new
                {
                    DeviceGuid = "b29725af-b067-4a35-9819-bbb31bdf8808",
                    SequenceNumber = "849741"
                };

                string json = JsonConvert.SerializeObject(sale);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Request:

```json
{
    "DeviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "SequenceNumber": "849741"
}
```

> Json Example Response:

```json
"Device Guid and Sequence Number combination found and voided."
```

This a feature that allows a user to void a transaction if it never got a response. You must send the device guid of the terminal used to run the transaction and the sequence number. If there was a sale run on that device with that sequence and it was approved, it will be voided. Otherwise you will be informed that a sale was found but it had not been approved or that there was no sale at all with that combination of device guid and sequence number.


### HTTP Request

`POST https://sandbox.choice.dev/api/v1/sales/timeoutreversal`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device's Guid.
SequenceNumber | string | Mandatory | Sequence Number.

### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

# AuthOnly

## Create AuthOnly

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class AuthOnly
    {
        public static void CreateAuthOnly()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/AuthOnlys");
                request.ContentType = "text/json";
                request.Method = "POST";

                var authOnly = new
                {
                    DeviceGuid = "7290ed47-3066-49db-a904-3d23924c46be",
                    Amount = 10.00,
                    ServiceFee = 0.40,
                    OrderNumber = "11518",
                    OrderDate = "2020-11-24",
                    SendReceipt = true,
                    CustomData = "order details",
                    CustomerLabel = "Patient",
                    CustomerId = "xt147",
                    AssociateCustomerCard = true,
                    Card = new
                    {
                        CardNumber = "5486477674539426",
                        CardHolderName = "John Doe",
                        Cvv2 = "998",
                        ExpirationDate = "2012",
                        Customer = new
                        {
                            FirstName = "John",
                            LastName = "Doe",
                            Phone = "9177563006",
                            City = "New York",
                            State = "NY",
                            Country = "US",
                            Email = "johnd@mailinator.com",
                            Address1 = "106 6th Av.",
                            Address2 = "",
                            Zip = "10006",
                            DateOfBirth = "1986-06-06",
                            DriverLicenseNumber = "12345678",
                            DriverLicenseState = "TX",
                            SSN4 = "1210"
                        }
                    }
                };

                string json = JsonConvert.SerializeObject(authOnly);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}

-----------------------------------------------------------

using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class AuthOnly
    {
        public static void CreateAuthOnly()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/AuthOnlys");
                request.ContentType = "text/json";
                request.Method = "POST";

                var authOnly = new
                {
                    DeviceGuid = "7290ed47-3066-49db-a904-3d23924c46be",
                    Amount = 10.00,
                    ServiceFee = 0.40,
                    OrderNumber = "11518",
                    OrderDate = "2020-11-24",
                    SendReceipt = true,
                    CustomData = "order details",
                    CustomerLabel = "Patient",
                    CustomerId = "xt147",
                    AssociateCustomerCard = true,
                    SemiIntegrated = true
                };

                string json = JsonConvert.SerializeObject(authOnly);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}

```

> Json Example Request:

```json
{
  "DeviceGuid" : "b29725af-b067-4a35-9819-bbb31bdf8808",
  "Amount" : 10.00,
  "ServiceFee": 0.40,
  "OrderNumber" : "11518",
  "OrderDate" : "2020-11-24",
  "SendReceipt" : true,
  "CustomData" : "order details",
  "CustomerLabel" : "Patient",
  "CustomerId" : "xt147",
  "AssociateCustomerCard" : true,
  "Card":
  {
    "CardNumber" : "5486477674539426",
    "CardHolderName" : "John Doe",
    "Cvv2" : "998",
    "ExpirationDate" : "2012",
    "Customer":
    {
      "FirstName" : "John",
      "LastName" : "Doe",
      "Phone" : "9177563006",
      "City" : "New York",
      "State" : "NY",
      "Country" : "US",
      "Email" : "johnd@mailinator.com",
      "Address1" : "106 6th Av.",
      "Address2" : "",
      "Zip" : "10006",
      "DateOfBirth" : "1986-06-06",
      "DriverLicenseNumber" : "12345678",
      "DriverLicenseState" : "TX",
      "SSN4" : "1210"
    }
  }
}

-----------------------------------------------------------

{
  "DeviceGuid" : "b29725af-b067-4a35-9819-bbb31bdf8808",
  "Amount" : 10.00,
  "ServiceFee": 0.40,
  "OrderNumber" : "11518",
  "OrderDate" : "2020-11-24",
  "SendReceipt" : true,
  "CustomData" : "order details",
  "CustomerLabel" : "Patient",
  "CustomerId" : "xt147",
  "AssociateCustomerCard" : true,
  "SemiIntegrated": true
}

```

> Json Example Response:

```json
{
    "guid": "493cffed-232a-4896-b9ca-36b543d7da13",
    "status": "Transaction - Approved",
    "batchStatus": "Batch - Open",
    "timeStamp": "2020-11-24T14:18:54.16-06:00",
    "amount": 10.00,
    "grossAmount": 10.00,
    "effectiveAmount": 10.00,
    "orderNumber": "11518",
    "orderDate": "2020-11-24T00:00:00",
    "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "customData": "order details",
    "cardDataSource": "INTERNET",
    "customerLabel": "Patient",
    "customerID": "xt147",
    "batchGuid": "014db6d5-e627-472b-8f7d-02901693725f",
    "sendReceipt": true,
    "allowCardEmv": false,
    "processorStatusCode": "A0000",
    "processorResponseMessage": "Success",
    "wasProcessed": true,
    "authCode": "VTLMC1",
    "refNumber": "24807698",
    "invoiceNumber": "11518",
    "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/24/2020 13:18:54\\n\\nCREDIT - AUTH ONLY\\n\\nCARD # : **** **** **** 9426\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24807698\\nInvoice number : 11518\\nAUTH CODE : VTLMC1\\nSubtotal:                       $10.00\\n--------------------------------------\\nTotal:                          $10.00\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
    "card": {
        "card.first6": "548647",
        "card.last4": "9426",
        "cardNumber": "inkpSUbbYay59426",
        "cardHolderName": "John Doe",
        "cardType": "Mastercard",
        "expirationDate": "2020-12",
        "customer": {
            "guid": "1dea1f06-efb8-4062-8a3a-1a64aabd10e8",
            "firstName": "John",
            "lastName": "Doe",
            "dateOfBirth": "1986-06-06T00:00:00",
            "address1": "106 6th Av.",
            "address2": "",
            "zip": "10006",
            "city": "New York",
            "state": "NY",
            "country": "US",
            "phone": "9177563006",
            "email": "johnd@mailinator.com",
            "ssN4": "1210",
            "driverLicenseNumber": "12345678",
            "driverLicenseState": "TX"
        }
    },
    "associateCustomerCard": true,
    "addressVerificationCode": "N",
    "addressVerificationResult": "No Match",
    "cvvVerificationCode": "M",
    "cvvVerificationResult": "Passed"
}
```

You could use the AuthOnly transaction to authorize an amount on a credit card without making the actual settlement of that amount. In this case, you actually reserve an amount for a certain period against the credit limit of the card holder.
The sale will be completed only if you run a successful capture later.

This endpoint creates an AuthOnly.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/AuthOnlys`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device's Guid.
Amount | decimal | Mandatory | Amount of the transaction. Min. amt.: $0.50
ServiceFee | decimal | Optional | Charge service fee.
SequenceNumber | string | Optional | Use this number to call timeout reversal endpoint if you don't get a response.
OrderNumber | string |  Optional | Merchant's order number. The value sent on this element will be returned as InvoiceNumber. Length = 17.
OrderDate | date | Optional | Order Date.
SendReceipt | boolean | Optional | Set to “FALSE” if you do not want an e-mail receipt to be sent to the customer. Set to “TRUE” or leave empty if you want e-mail to be sent.
CustomData | string | Optional | order details.
CustomerLabel | string | Optional | Any name you want to show instead of customer. Eg., Player, Patient, Buyer.
CustomerID | string | Optional | Any combination of numbers and letters you use to identify the customer.
AssociateCustomerCard | boolean | Optional | An option to know if you want use to save customer and card token.
SemiIntegrated | boolean | Optional | Only when physical terminal used on semi integrated mode, send value True.
Currency | string | Optional | Currency of the transaction.Default value: **USD**<br><br>Allowed values: USD, ARS, AUD, BRL, CAD, CHF, EUR, GBP, JPY, MXN, NZD, SGD
Card | object | Mandatory | Card.
EnhancedData | object | Optional | EnhancedData.
 |  | 
**Card** |  | 
CardNumber | string | Mandatory | Card number. Must be 16 characters.  (example: 4532538795426624) or token (example: FfL7exC7Xe2y6624).
CardHolderName | string | Optional | Cardholder's name.
Cvv2 | string | Optional | This is the three or four digit CVV code at the back side of the credit and debit card.
ExpirationDate | date | Optional with Token | Card's expiry date in the YYMM format.
Customer | object | Optional | Customer.
 |  | 
**Customer** |  | 
FirstName | string | Optional | Customer's first name.
LastName | string | Optional | Customer's last name.
Phone | string | Optional | Customer's phone number. The phone number must be syntactically correct. For example, 4152345678.
City | string | Optional | Customer's city.
State | string | Optional | Customer's short name state. The ISO 3166-2 CA and US state or province code of a customer. Length = 2.
Country | string | Optional | Customer's country. The ISO country code of a customer’s country. Length = 2 or 3.
Email | string | Optional | Customer's valid email address.
Address1 | string | Optional | Customer's address.
Address2 | string | Optional | Customer's address line 2.
Zip | string | Optional | Customer's zipcode. Length = 5.
DateOfBirth | date | Optional | Customer's date of birth.<br><br>Allowed format:<br><br>YYYY-MM-DD.<br>For example: 2002-05-30
DriverLicenseNumber | string | Optional | Customer's driver license number.
DriverLicenseState | string | Mandatory when DriverLicenseNumber is provided | Customer's driver license short name state. The ISO 3166-2 CA and US state or province code of a customer. Length = 2.
SSN4 | string | Mandatory when DOB is not submitted | Customer's social security number.
 |  | 
**EnhancedData** |  |
SaleTax | decimal | Optional | Transaction's amount.
AdditionalTaxDetailTaxCategory | string | Optional | Tax Category.
AdditionalTaxDetailTaxType | string | Optional | Tax Type.
AdditionalTaxDetailTaxAmount | decimal | Optional | Tax Amount.
AdditionalTaxDetailTaxRate | decimal | Optional | Tax Rate.
PurchaseOrder | string | Optional | Purchase Order.
ShippingCharges | decimal | Optional | Shipping Charges.
DutyCharges | decimal | Optional | Duty Charges.
CustomerVATNumber | string | Optional | Customer VAT Number.
VATInvoice | string | Optional | VAT Invoice.
SummaryCommodityCode | string | Optional | Summary Commodity Code.
ShipToZip | string | Optional | Ship To  Zip.
ShipFromZip | string | Optional | Ship From Zip.
DestinationCountryCode | string | Optional | Destination Country Code.
SupplierReferenceNumber | string | Optional | Supplier Reference Number.
CustomerRefID | string | Optional | Customer Ref ID.
ChargeDescriptor | string | Optional | Charge Descriptor.
AdditionalAmount | decimal | Optional | Additional Amount.
AdditionalAmountType | string | Optional | Additional Amount Type.
ProductName | string | Optional | Product Name.
ProductCode | string | Optional | Product Code.
Price | string | Optional | Price.
MeasurementUnit | string | Optional | Measurement Unit.

### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>









## Create Incremental Auth

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class AuthOnly
    {
        public static void CreateIncrementalAuth()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/AuthOnlys");
                request.ContentType = "text/json";
                request.Method = "POST";

                var authOnly = new
                {
                    DeviceGuid = "7290ed47-3066-49db-a904-3d23924c46be",
                    Amount = 2.00,
                    AuthOnlyGuid = "72g0ed47-3056-49db-a904-3d23926646be"
                };

                string json = JsonConvert.SerializeObject(authOnly);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Request:

```json
{
  "DeviceGuid" : "b29725af-b067-4a35-9819-bbb31bdf8808",
  "Amount" : 2.00,
  "AuthOnlyGuid" : "m29d25af-b067-4a35-9c19-bbb31jyf820d"
}
```

> Json Example Response:

```json
{
    "authOnlyGuid": "k7f56be3-72b6-49b4-8258-6f77a0d0fa5j",
    "amount": 2.00,
    "deviceGuid": "330e9bcf-f227-4a07-9eff-7a17b3135c31",
    "totalAmount": 61.56,
    "timeStamp": "2021-09-18T15:21:37.84",
    "processorStatusCode": "A0000",
    "processorResponseMessage": "STA: PASS | R.MSG: Success | TASK#: 13741257 | AUTHCODE: TAS688 | HOSTREF#: 125727500397 | TAMT: 2.00 | PAMT: 2.00 | VERIFCVV:  | AVC: 0 | CHVC:  | CT: V",
    "wasProcessed": true,
    "authCode": "TAS688",
    "refNumber": "12025813",
    "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n09/18/2021 08:21:40\\n\\nCREDIT - INCREMENTAL AUTH\\n\\nCARD # : **** **** **** 4136\\nCARD TYPE : VISA\\n\\nTRANSACTION ID : 12025213\\nORIGINAL TRANSACTION ID : 11956519\\nTOTAL AUTHORIZED AMOUNT : $61.56\\nAUTH CODE : TAS270\\nSubtotal:                       $02.00\\n--------------------------------------\\nTotal:                          $02.00\\n--------------------------------------\\n\\n\\n\\nAdam Smith\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n\\n"
}
```

You could use the IncrementalAuth transaction to increase an amount on a authOnly transactions existing.

This endpoint creates an Incremental Auth.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/IncrementalAuth`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device's Guid.
AuthOnlyGuid | string | Mandatory | AuthOnly's Guid for increasing
Amount | decimal | Mandatory | Amount of the transaction. 
 |  | 

### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>








## Get AuthOnly

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class AuthOnly
    {
        public static void GetAuthOnly()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/AuthOnlys/493cffed-232a-4896-b9ca-36b543d7da13");
                request.Method = "GET";

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Response:

```json
{
    "guid": "493cffed-232a-4896-b9ca-36b543d7da13",
    "status": "Transaction - Approved",
    "batchStatus": "Batch - Open",
    "timeStamp": "2020-11-24T14:18:54.16-06:00",
    "amount": 10.00,
    "grossAmount": 10.00,
    "effectiveAmount": 10.00,
    "orderNumber": "11518",
    "orderDate": "2020-11-24T00:00:00",
    "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "customData": "order details",
    "cardDataSource": "INTERNET",
    "customerLabel": "Patient",
    "customerID": "xt147",
    "batchGuid": "014db6d5-e627-472b-8f7d-02901693725f",
    "sendReceipt": true,
    "allowCardEmv": false,
    "processorStatusCode": "A0000",
    "processorResponseMessage": "Success",
    "wasProcessed": true,
    "authCode": "VTLMC1",
    "refNumber": "24807698",
    "invoiceNumber": "11518",
    "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/24/2020 13:18:54\\n\\nCREDIT - AUTH ONLY\\n\\nCARD # : **** **** **** 9426\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24807698\\nInvoice number : 11518\\nAUTH CODE : VTLMC1\\nSubtotal:                       $10.00\\n--------------------------------------\\nTotal:                          $10.00\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
    "card": {
        "card.first6": "548647",
        "card.last4": "9426",
        "cardNumber": "inkpSUbbYay59426",
        "cardHolderName": "John Doe",
        "cardType": "Mastercard",
        "expirationDate": "2020-12",
        "customer": {
            "guid": "1dea1f06-efb8-4062-8a3a-1a64aabd10e8",
            "firstName": "John",
            "lastName": "Doe",
            "dateOfBirth": "1986-06-06T00:00:00",
            "address1": "106 6th Av.",
            "address2": "",
            "zip": "10006",
            "city": "New York",
            "state": "NY",
            "country": "US",
            "phone": "9177563006",
            "email": "johnd@mailinator.com",
            "ssN4": "1210",
            "driverLicenseNumber": "12345678",
            "driverLicenseState": "TX"
        }
    },
    "associateCustomerCard": true,
    "addressVerificationCode": "N",
    "addressVerificationResult": "No Match",
    "cvvVerificationCode": "M",
    "cvvVerificationResult": "Passed"
}
```

This endpoint gets an AuthOnly.

### HTTP Request

`GET https://sandbox.choice.dev/api/v1/AuthOnlys/<guid>`

### Headers using token

Key | Value
--------- | -------
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
guid | AuthOnly’s guid to get

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

## Timeout Reversal

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Sale
    {
        public static void TimeoutReversal()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/AuthOnlys/timeoutreversal");
                request.ContentType = "text/json";
                request.Method = "POST";

                var sale = new
                {
                    DeviceGuid = "b29725af-b067-4a35-9819-bbb31bdf8808",
                    SequenceNumber = "849741"
                };

                string json = JsonConvert.SerializeObject(sale);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Request:

```json
{
    "DeviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "SequenceNumber": "849741"
}
```

> Json Example Response:

```json
"Device Guid and Sequence Number combination found and voided."
```

This a feature that allows a user to void a transaction if it never got a response. You must send the device guid of the terminal used to run the transaction and the sequence number. If there was a sale run on that device with that sequence and it was approved, it will be voided. Otherwise you will be informed that a sale was found but it had not been approved or that there was no sale at all with that combination of device guid and sequence number.


### HTTP Request

`POST https://sandbox.choice.dev/api/v1/AuthOnlys/timeoutreversal`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device's Guid.
SequenceNumber | string | Mandatory | Sequence Number.

### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>



# Capture

## Create capture

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Capture
    {
        public static void CreateCapture()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Captures");
                request.ContentType = "text/json";
                request.Method = "POST";

                var capture = new
                {
                    DeviceGuid = "b29725af-b067-4a35-9819-bbb31bdf8808",
                    AuthOnlyGuid = "493cffed-232a-4896-b9ca-36b543d7da13",
                    NewAmount = 10.00
                };

                string json = JsonConvert.SerializeObject(capture);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Request:

```json
{
  "DeviceGuid" : "b29725af-b067-4a35-9819-bbb31bdf8808",
  "AuthOnlyGuid" : "493cffed-232a-4896-b9ca-36b543d7da13",
  "NewAmount" : 10.00,
  "SemiIntegrated": true
}
```

> Json Example Response:

```json
{
    "guid": "284dd923-a266-4d0f-80bd-e8441154c742",
    "status": "Transaction - Approved",
    "batchStatus": "Batch - Open",
    "timeStamp": "2020-11-24T14:21:25.59-06:00",
    "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "authOnlyGuid": "493cffed-232a-4896-b9ca-36b543d7da13",
    "authOnlyReferenceNumber": "24807698",
    "newAmount": 10.00,
    "grossAmount": 10.00,
    "effectiveAmount": 10.00,
    "StatementDescription": "This atribute is sent by the backend in the response",
    "batchGuid": "014db6d5-e627-472b-8f7d-02901693725f",
    "processorStatusCode": "A0000",
    "processorResponseMessage": "Success",
    "wasProcessed": true,
    "authCode": "VTLMC1",
    "refNumber": "24807698",
    "invoiceNumber": "11518",
    "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/24/2020 13:21:26\\n\\nCREDIT - SALE\\n\\nCARD # : **** **** **** 9426\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24807698\\nInvoice number : 11518\\nAUTH CODE : VTLMC1\\nSubtotal:                       $10.00\\n--------------------------------------\\nTotal:                          $10.00\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
    "authOnly": {
        "guid": "493cffed-232a-4896-b9ca-36b543d7da13",
        "status": "Transaction - Approved",
        "batchStatus": "Batch - Open",
        "timeStamp": "2020-11-24T14:18:54.16-06:00",
        "amount": 10.00,
        "grossAmount": 10.00,
        "effectiveAmount": 10.00,
        "orderNumber": "11518",
        "orderDate": "2020-11-24T00:00:00",
        "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
        "customData": "order details",
        "cardDataSource": "INTERNET",
        "customerLabel": "Patient",
        "customerID": "xt147",
        "batchGuid": "014db6d5-e627-472b-8f7d-02901693725f",
        "sendReceipt": true,
        "allowCardEmv": false,
        "processorStatusCode": "A0000",
        "processorResponseMessage": "Success",
        "wasProcessed": true,
        "authCode": "VTLMC1",
        "refNumber": "24807698",
        "invoiceNumber": "11518",
        "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/24/2020 13:18:54\\n\\nCREDIT - AUTH ONLY\\n\\nCARD # : **** **** **** 9426\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24807698\\nInvoice number : 11518\\nAUTH CODE : VTLMC1\\nSubtotal:                       $10.00\\n--------------------------------------\\nTotal:                          $10.00\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
        "card": {
            "card.first6": "548647",
            "card.last4": "9426",
            "cardNumber": "inkpSUbbYay59426",
            "cardHolderName": "John Doe",
            "cardType": "Mastercard",
            "expirationDate": "2020-12",
            "customer": {
                "guid": "1dea1f06-efb8-4062-8a3a-1a64aabd10e8",
                "firstName": "John",
                "lastName": "Doe",
                "dateOfBirth": "1986-06-06T00:00:00",
                "address1": "106 6th Av.",
                "address2": "",
                "zip": "10006",
                "city": "New York",
                "state": "NY",
                "country": "US",
                "phone": "9177563006",
                "email": "johnd@mailinator.com",
                "ssN4": "1210",
                "driverLicenseNumber": "12345678",
                "driverLicenseState": "TX"
            }
        },
        "associateCustomerCard": true,
        "addressVerificationCode": "N",
        "addressVerificationResult": "No Match",
        "cvvVerificationCode": "M",
        "cvvVerificationResult": "Passed"
    },
    "relatedSale": {
        "guid": "080b1dad-5658-4e0e-a6d5-8911eee82889",
        "status": "Transaction - Approved",
        "batchStatus": "Batch - Open",
        "timeStamp": "2020-11-24T14:21:26.58-06:00",
        "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
        "amount": 10.00,
        "effectiveAmount": 10.00,
        "grossAmount": 10.00,
        "cardDataSource": "INTERNET",
        "batchGuid": "014db6d5-e627-472b-8f7d-02901693725f",
        "wasProcessed": true,
        "authCode": "VTLMC1",
        "refNumber": "24807698",
        "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/24/2020 13:21:26\\n\\nCREDIT - SALE\\n\\nCARD # : **** **** **** 9426\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24807698\\nInvoice number : 11518\\nAUTH CODE : VTLMC1\\nSubtotal:                       $10.00\\n--------------------------------------\\nTotal:                          $10.00\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
        "generatedBy": "maxduplessy",
        "card": {
            "card.first6": "548647",
            "card.last4": "9426",
            "cardNumber": "inkpSUbbYay59426",
            "cardHolderName": "John Doe",
            "cardType": "Mastercard",
            "expirationDate": "2020-12",
            "customer": {
                "guid": "1dea1f06-efb8-4062-8a3a-1a64aabd10e8",
                "firstName": "John",
                "lastName": "Doe",
                "dateOfBirth": "1986-06-06T00:00:00",
                "address1": "106 6th Av.",
                "address2": "",
                "zip": "10006",
                "city": "New York",
                "state": "NY",
                "country": "US",
                "phone": "9177563006",
                "email": "johnd@mailinator.com",
                "ssN4": "1210",
                "driverLicenseNumber": "12345678",
                "driverLicenseState": "TX"
            }
        }
    }
}
```

The Capture transaction is used to collect the money that you had asked during the AuthOnly transaction. You need to provide the AuthOnlyGuid that you received when you ran the AuthOnly.

This endpoint creates a capture.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Captures`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device’s Guid.
AuthOnlyGuid | string | Mandatory | AuthOnly’s Guid.
NewAmount | decimal | Optional |  Transaction's new amount. Min. amt.: $0.50


### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

## Get capture

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Capture
    {
        public static void GetCapture()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Captures/284dd923-a266-4d0f-80bd-e8441154c742");
                request.Method = "GET";

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Response:

```json
{
    "guid": "284dd923-a266-4d0f-80bd-e8441154c742",
    "status": "Transaction - Approved",
    "batchStatus": "Batch - Open",
    "timeStamp": "2020-11-24T14:21:25.59-06:00",
    "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "authOnlyGuid": "493cffed-232a-4896-b9ca-36b543d7da13",
    "authOnlyReferenceNumber": "24807698",
    "newAmount": 10.00,
    "grossAmount": 10.00,
    "effectiveAmount": 10.00,
    "batchGuid": "014db6d5-e627-472b-8f7d-02901693725f",
    "processorStatusCode": "A0000",
    "processorResponseMessage": "Success",
    "wasProcessed": true,
    "authCode": "VTLMC1",
    "refNumber": "24807698",
    "invoiceNumber": "11518",
    "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/24/2020 13:21:26\\n\\nCREDIT - SALE\\n\\nCARD # : **** **** **** 9426\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24807698\\nInvoice number : 11518\\nAUTH CODE : VTLMC1\\nSubtotal:                       $10.00\\n--------------------------------------\\nTotal:                          $10.00\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
    "authOnly": {
        "guid": "493cffed-232a-4896-b9ca-36b543d7da13",
        "status": "Transaction - Approved",
        "batchStatus": "Batch - Open",
        "timeStamp": "2020-11-24T14:18:54.16-06:00",
        "amount": 10.00,
        "grossAmount": 10.00,
        "effectiveAmount": 10.00,
        "orderNumber": "11518",
        "orderDate": "2020-11-24T00:00:00",
        "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
        "customData": "order details",
        "cardDataSource": "INTERNET",
        "customerLabel": "Patient",
        "customerID": "xt147",
        "batchGuid": "014db6d5-e627-472b-8f7d-02901693725f",
        "sendReceipt": true,
        "allowCardEmv": false,
        "processorStatusCode": "A0000",
        "processorResponseMessage": "Success",
        "wasProcessed": true,
        "authCode": "VTLMC1",
        "refNumber": "24807698",
        "invoiceNumber": "11518",
        "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/24/2020 13:18:54\\n\\nCREDIT - AUTH ONLY\\n\\nCARD # : **** **** **** 9426\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24807698\\nInvoice number : 11518\\nAUTH CODE : VTLMC1\\nSubtotal:                       $10.00\\n--------------------------------------\\nTotal:                          $10.00\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
        "card": {
            "card.first6": "548647",
            "card.last4": "9426",
            "cardNumber": "inkpSUbbYay59426",
            "cardHolderName": "John Doe",
            "cardType": "Mastercard",
            "expirationDate": "2020-12",
            "customer": {
                "guid": "1dea1f06-efb8-4062-8a3a-1a64aabd10e8",
                "firstName": "John",
                "lastName": "Doe",
                "dateOfBirth": "1986-06-06T00:00:00",
                "address1": "106 6th Av.",
                "address2": "",
                "zip": "10006",
                "city": "New York",
                "state": "NY",
                "country": "US",
                "phone": "9177563006",
                "email": "johnd@mailinator.com",
                "ssN4": "1210",
                "driverLicenseNumber": "12345678",
                "driverLicenseState": "TX"
            }
        },
        "associateCustomerCard": true,
        "addressVerificationCode": "N",
        "addressVerificationResult": "No Match",
        "cvvVerificationCode": "M",
        "cvvVerificationResult": "Passed"
    },
    "relatedSale": {
        "guid": "080b1dad-5658-4e0e-a6d5-8911eee82889",
        "status": "Transaction - Approved",
        "batchStatus": "Batch - Open",
        "timeStamp": "2020-11-24T14:21:26.58-06:00",
        "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
        "amount": 10.00,
        "effectiveAmount": 10.00,
        "grossAmount": 10.00,
        "cardDataSource": "INTERNET",
        "batchGuid": "014db6d5-e627-472b-8f7d-02901693725f",
        "wasProcessed": true,
        "authCode": "VTLMC1",
        "refNumber": "24807698",
        "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/24/2020 13:21:26\\n\\nCREDIT - SALE\\n\\nCARD # : **** **** **** 9426\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24807698\\nInvoice number : 11518\\nAUTH CODE : VTLMC1\\nSubtotal:                       $10.00\\n--------------------------------------\\nTotal:                          $10.00\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
        "generatedBy": "maxduplessy",
        "card": {
            "card.first6": "548647",
            "card.last4": "9426",
            "cardNumber": "inkpSUbbYay59426",
            "cardHolderName": "John Doe",
            "cardType": "Mastercard",
            "expirationDate": "2020-12",
            "customer": {
                "guid": "1dea1f06-efb8-4062-8a3a-1a64aabd10e8",
                "firstName": "John",
                "lastName": "Doe",
                "dateOfBirth": "1986-06-06T00:00:00",
                "address1": "106 6th Av.",
                "address2": "",
                "zip": "10006",
                "city": "New York",
                "state": "NY",
                "country": "US",
                "phone": "9177563006",
                "email": "johnd@mailinator.com",
                "ssN4": "1210",
                "driverLicenseNumber": "12345678",
                "driverLicenseState": "TX"
            }
        }
    }
}
```

This endpoint gets a capture.

### HTTP Request

`GET https://sandbox.choice.dev/api/v1/Captures/<guid>`

### Headers using token

Key | Value
--------- | -------
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
guid | Capture’s guid to get

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

# Void

## Create void

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Void
    {
        public static void CreateVoid()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/void");
                request.ContentType = "text/json";
                request.Method = "POST";

                var void1 = new
                {
                    DeviceGuid = "b29725af-b067-4a35-9819-bbb31bdf8808",
                    SaleGuid = "c7a9a2a6-08bb-42c5-acc6-f20d39c7f5de",  
                    VoidReason = "DEVICE_TIMEOUT"
                };

                string json = JsonConvert.SerializeObject(void1);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
  "DeviceGuid" : "b29725af-b067-4a35-9819-bbb31bdf8808",
  "SaleGuid": "c7a9a2a6-08bb-42c5-acc6-f20d39c7f5de",
  "VoidReason": "DEVICE_TIMEOUT",
  "SemiIntegrated": true
}
```

> Json Example Response:

```json
{
    "saleGuid": "1e0f6e4b-2eb4-475f-b2c3-d49746ac244f",
    "batchStatus": "Batch - Open",
    "timeStamp": "2020-11-25T07:03:16.47-06:00",
    "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "saleGuid": "c7a9a2a6-08bb-42c5-acc6-f20d39c7f5de",
    "status": "Transaction - Approved",
    "voidReason": "DEVICE_TIMEOUT",
    "processorStatusCode": "A0000",
    "wasProcessed": true,
    "batchGuid": "1238635a-63f4-4de3-8cc3-4e6958840dc0",
    "authCode": "VTLMC1",
    "refNumber": "24814572",
    "invoiceNumber": "11518",
    "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/25/2020 06:03:18\\n\\nCREDIT - VOID\\n\\nCARD # : **** **** **** 0213\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24814572\\nInvoice number : 11518\\nAUTH CODE : VTLMC1\\nVoid Amount:                    $19.74\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
    "sale": {
        "guid": "c7a9a2a6-08bb-42c5-acc6-f20d39c7f5de",
        "status": "Transaction - Approved",
        "batchStatus": "Batch - Open",
        "timeStamp": "2020-11-25T07:00:50.87-06:00",
        "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
        "amount": 19.74,
        "effectiveAmount": 0.00,
        "grossAmount": 19.74,
        "orderNumber": "11518",
        "orderDate": "2020-11-24T00:00:00",
        "cardDataSource": "INTERNET",
        "customerLabel": "Patient",
        "businessName": "Star Shoes California",
        "customerID": "xt147",
        "batchGuid": "1238635a-63f4-4de3-8cc3-4e6958840dc0",
        "processorStatusCode": "A0000",
        "processorResponseMessage": "Success",
        "wasProcessed": true,
        "authCode": "VTLMC1",
        "refNumber": "24814572",
        "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/25/2020 06:00:51\\n\\nCREDIT - SALE\\n\\nCARD # : **** **** **** 0213\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24814572\\n\\nAUTH CODE : VTLMC1\\nSubtotal:                       $19.74\\n--------------------------------------\\nTotal:                          $19.74\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
        "customData": "order details",
        "generatedBy": "maxduplessy",
        "card": {
            "card.first6": "530676",
            "card.last4": "0213",
            "cardNumber": "1zcGT7J4pkGh0213",
            "cardHolderName": "John Doe",
            "cardType": "Mastercard",
            "expirationDate": "2022-07",
            "customer": {
                "guid": "5645931c-8f27-46c4-9569-67820814acd5",
                "firstName": "John",
                "lastName": "Doe",
                "dateOfBirth": "1987-07-07T00:00:00",
                "address1": "107 7th Av.",
                "address2": "",
                "zip": "10007",
                "city": "New York",
                "state": "NY",
                "country": "US",
                "phone": "9177563007",
                "email": "johnd@mailinator.com",
                "ssN4": "1210",
                "driverLicenseNumber": "12345678",
                "driverLicenseState": "TX"
            }
        },
        "associateCustomerCard": true,
        "addressVerificationCode": "N",
        "addressVerificationResult": "No Match",
        "cvvVerificationCode": "M",
        "cvvVerificationResult": "Passed"
    }
}
```

You can run a Void when you need to cancel a sale that has not been settled yet or an authOnly. To void a sale you need to provide the SaleGuid or SaleReferenceNumber that you received when you ran the Sale. To void an authOnly you need to provide the AuthOnlyGuid or AuthOnlyReferenceNumber that you received when you ran the authOnly.

This endpoint creates a void.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/void`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"


### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device’s Guid.
SaleGuid | string | Mandatory when **SaleReferenceNumber, AuthOnlyGuid and AuthOnlyReferenceNumber** field are not sent | Sale’s Guid.
SaleReferenceNumber | string | Mandatory when **SaleGuid, AuthOnlyGuid and AuthOnlyReferenceNumber** field are not sent | SaleReferenceNumber.
AuthOnlyGuid | string | Mandatory when **SaleGuid, SaleReferenceNumber and AuthOnlyReferenceNumber** field are not sent | AuthOnlyGuid’s Guid.
AuthOnlyReferenceNumber | string | Mandatory when **SaleGuid, SaleReferenceNumber and AuthOnlyGuid** field are not sent | AuthOnlyReferenceNumber.
VoidReason | string | Optional | Indicates the reason the transaction was voided.<br><br>Allowed values:<br><br>**1. POST_AUTH_USER_DECLINE**<br>**2. DEVICE_TIMEOUT**<br>**3. DEVICE_UNAVAILABLE**<br>**4. PARTIAL_REVERSAL**<br>**5. TORN_TRANSACTION**<br>**6. POST_AUTH_CHIP_DECLINE**

**Note: Send either SaleGuid, SaleReferenceNumber, AuthOnlyGuid or  AuthOnlyReferenceNumber field in a request.**


### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

## Get void

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Void
    {
        public static void GetVoid()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/void/1e0f6e4b-2eb4-475f-b2c3-d49746ac244f");
                request.Method = "GET";

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Response:

```json
{
    "guid": "1e0f6e4b-2eb4-475f-b2c3-d49746ac244f",
    "batchStatus": "Batch - Open",
    "timeStamp": "2020-11-25T07:03:16.47-06:00",
    "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "saleGuid": "c7a9a2a6-08bb-42c5-acc6-f20d39c7f5de",
    "status": "Transaction - Approved",
    "voidReason": "DEVICE_TIMEOUT",
    "processorStatusCode": "A0000",
    "wasProcessed": true,
    "batchGuid": "1238635a-63f4-4de3-8cc3-4e6958840dc0",
    "authCode": "VTLMC1",
    "refNumber": "24814572",
    "invoiceNumber": "11518",
    "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/25/2020 06:03:18\\n\\nCREDIT - VOID\\n\\nCARD # : **** **** **** 0213\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24814572\\nInvoice number : 11518\\nAUTH CODE : VTLMC1\\nVoid Amount:                    $19.74\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
    "sale": {
        "guid": "c7a9a2a6-08bb-42c5-acc6-f20d39c7f5de",
        "status": "Transaction - Approved",
        "batchStatus": "Batch - Open",
        "timeStamp": "2020-11-25T07:00:50.87-06:00",
        "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
        "amount": 19.74,
        "effectiveAmount": 0.00,
        "grossAmount": 19.74,
        "orderNumber": "11518",
        "orderDate": "2020-11-24T00:00:00",
        "cardDataSource": "INTERNET",
        "customerLabel": "Patient",
        "businessName": "Star Shoes California",
        "customerID": "xt147",
        "batchGuid": "1238635a-63f4-4de3-8cc3-4e6958840dc0",
        "processorStatusCode": "A0000",
        "processorResponseMessage": "Success",
        "wasProcessed": true,
        "authCode": "VTLMC1",
        "refNumber": "24814572",
        "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/25/2020 06:00:51\\n\\nCREDIT - SALE\\n\\nCARD # : **** **** **** 0213\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24814572\\n\\nAUTH CODE : VTLMC1\\nSubtotal:                       $19.74\\n--------------------------------------\\nTotal:                          $19.74\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
        "customData": "order details",
        "generatedBy": "maxduplessy",
        "card": {
            "card.first6": "530676",
            "card.last4": "0213",
            "cardNumber": "1zcGT7J4pkGh0213",
            "cardHolderName": "John Doe",
            "cardType": "Mastercard",
            "expirationDate": "2022-07",
            "customer": {
                "guid": "5645931c-8f27-46c4-9569-67820814acd5",
                "firstName": "John",
                "lastName": "Doe",
                "dateOfBirth": "1987-07-07T00:00:00",
                "address1": "107 7th Av.",
                "address2": "",
                "zip": "10007",
                "city": "New York",
                "state": "NY",
                "country": "US",
                "phone": "9177563007",
                "email": "johnd@mailinator.com",
                "ssN4": "1210",
                "driverLicenseNumber": "12345678",
                "driverLicenseState": "TX"
            }
        },
        "associateCustomerCard": true,
        "addressVerificationCode": "N",
        "addressVerificationResult": "No Match",
        "cvvVerificationCode": "M",
        "cvvVerificationResult": "Passed"
    }
}
```

This endpoint gets a void.

### HTTP Request

`GET https://sandbox.choice.dev/api/v1/void/<guid>`

### Headers using token

Key | Value
--------- | -------
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
guid | Void’s guid to get

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

# Return

## Create return

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Return
    {
        public static void CreateReturn()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/returns");
                request.ContentType = "text/json";
                request.Method = "POST";

                var returns = new
                {
                    DeviceGuid = "b29725af-b067-4a35-9819-bbb31bdf8808",
                    SaleGuid = "b4084e11-884d-468c-84d9-614c5b986fde",
                    Amount = 19.74
                };

                string json = JsonConvert.SerializeObject(returns);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
  "DeviceGuid" : "b29725af-b067-4a35-9819-bbb31bdf8808",
  "SaleGuid": "b4084e11-884d-468c-84d9-614c5b986fde",
  "Amount": 19.74
}
```

> Json Example Response:

```json
{
    "guid": "9fb17f4e-b508-48d3-bd77-bb6813b42620",
    "batchStatus": "Batch - Open",
    "timeStamp": "2020-11-25T07:17:24.15-06:00",
    "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "saleGuid": "b4084e11-884d-468c-84d9-614c5b986fde",
    "status": "Transaction - Approved",
    "amount": 19.74,
    "StatementDescription": "This atribute is sent by the backend in the response",
    "batchGuid": "58e8a0ad-d167-4015-a4b1-904b1e7f2b26",
    "processorStatusCode": "A0014",
    "wasProcessed": true,
    "authCode": "VTLMC1",
    "refNumber": "24814670",
    "invoiceNumber": "11518",
    "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/25/2020 06:17:25\\n\\nCREDIT - VOID\\n\\nCARD # : **** **** **** 0213\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24814670\\nInvoice number : 11518\\nAUTH CODE : VTLMC1\\nVoid Amount:                    $19.74\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
    "sale": {
        "saleGuid": "b4084e11-884d-468c-84d9-614c5b986fde",
        "status": "Transaction - Approved",
        "batchStatus": "Batch - Closed",
        "timeStamp": "2020-11-25T07:15:27.28-06:00",
        "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
        "amount": 19.74,
        "effectiveAmount": 19.74,
        "grossAmount": 19.74,
        "orderNumber": "11518",
        "orderDate": "2020-11-24T00:00:00",
        "cardDataSource": "INTERNET",
        "customerLabel": "Patient",
        "businessName": "Star Shoes California",
        "customerID": "xt147",
        "batchGuid": "b8d73296-e397-45c0-bb43-ba064e9750ac",
        "processorStatusCode": "A0000",
        "processorResponseMessage": "Success",
        "wasProcessed": true,
        "authCode": "VTLMC1",
        "refNumber": "24814670",
        "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/25/2020 06:15:27\\n\\nCREDIT - SALE\\n\\nCARD # : **** **** **** 0213\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24814670\\n\\nAUTH CODE : VTLMC1\\nSubtotal:                       $19.74\\n--------------------------------------\\nTotal:                          $19.74\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
        "customData": "order details",
        "generatedBy": "maxduplessy",
        "card": {
            "card.first6": "530676",
            "card.last4": "0213",
            "cardNumber": "1zcGT7J4pkGh0213",
            "cardHolderName": "John Doe",
            "cardType": "Mastercard",
            "expirationDate": "2022-07",
            "customer": {
                "guid": "4b63d92b-c0bf-436c-879d-5cacfcf1b708",
                "firstName": "John",
                "lastName": "Doe",
                "dateOfBirth": "1987-07-07T00:00:00",
                "address1": "107 7th Av.",
                "address2": "",
                "zip": "10007",
                "city": "New York",
                "state": "NY",
                "country": "US",
                "phone": "9177563007",
                "email": "johnd@mailinator.com",
                "ssN4": "1210",
                "driverLicenseNumber": "12345678",
                "driverLicenseState": "TX"
            }
        },
        "associateCustomerCard": true,
        "addressVerificationCode": "N",
        "addressVerificationResult": "No Match",
        "cvvVerificationCode": "M",
        "cvvVerificationResult": "Passed"
    }
}
```

You can run a Return transaction when you need to refund either a partial or the full amount of a sale that has been settled. The Return amount doesn’t need to be the same as the total amount originally charged in the sale. To refund a sale you need to provide the SaleGuid or SaleReferenceNumber that you received when you ran the Sale.

This endpoint creates a return.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/returns`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"


### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device’s Guid.
SaleGuid | string | Mandatory when **SaleReferenceNumber** field is not sent | Sale’s Guid.
SaleReferenceNumber | string | Mandatory when **SaleGuid** field is not sent | SaleReferenceNumber.
Amount | decimal | Mandatory | Transaction's amount. Min. amt.: $0.50

**Note: Send either SaleGuid or SaleReferenceNumber field in a request.**


### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

## Get return

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Return
    {
        public static void GetReturn()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/returns/9fb17f4e-b508-48d3-bd77-bb6813b42620");
                request.Method = "GET";

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Response:

```json
{
    "guid": "9fb17f4e-b508-48d3-bd77-bb6813b42620",
    "batchStatus": "Batch - Open",
    "timeStamp": "2020-11-25T07:17:24.15-06:00",
    "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "saleGuid": "b4084e11-884d-468c-84d9-614c5b986fde",
    "status": "Transaction - Approved",
    "amount": 19.74,
    "batchGuid": "58e8a0ad-d167-4015-a4b1-904b1e7f2b26",
    "processorStatusCode": "A0014",
    "wasProcessed": true,
    "authCode": "VTLMC1",
    "refNumber": "24814670",
    "invoiceNumber": "11518",
    "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/25/2020 06:17:25\\n\\nCREDIT - VOID\\n\\nCARD # : **** **** **** 0213\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24814670\\nInvoice number : 11518\\nAUTH CODE : VTLMC1\\nVoid Amount:                    $19.74\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
    "sale": {
        "guid": "b4084e11-884d-468c-84d9-614c5b986fde",
        "status": "Transaction - Approved",
        "batchStatus": "Batch - Closed",
        "timeStamp": "2020-11-25T07:15:27.28-06:00",
        "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
        "amount": 19.74,
        "effectiveAmount": 0.00,
        "grossAmount": 19.74,
        "orderNumber": "11518",
        "orderDate": "2020-11-24T00:00:00",
        "cardDataSource": "INTERNET",
        "customerLabel": "Patient",
        "businessName": "Star Shoes California",
        "customerID": "xt147",
        "batchGuid": "b8d73296-e397-45c0-bb43-ba064e9750ac",
        "processorStatusCode": "A0000",
        "processorResponseMessage": "Success",
        "wasProcessed": true,
        "authCode": "VTLMC1",
        "refNumber": "24814670",
        "customerReceipt": "CHOICE MERCHANT SOLUTIONS\\n8320 S HARDY DR\\nTEMPE AZ 85284\\n11/25/2020 06:15:27\\n\\nCREDIT - SALE\\n\\nCARD # : **** **** **** 0213\\nCARD TYPE :MASTERCARD\\nEntry Mode : MANUAL\\n\\nTRANSACTION ID : 24814670\\n\\nAUTH CODE : VTLMC1\\nSubtotal:                       $19.74\\n--------------------------------------\\nTotal:                          $19.74\\n--------------------------------------\\n\\n\\n\\nJohn Doe\\n\\nCUSTOMER ACKNOWLEDGES RECEIPT OF\\nGOODS AND/OR SERVICES IN THE AMOUNT\\nOF THE TOTAL SHOWN HEREON AND AGREES\\nTO PERFORM THE OBLIGATIONS SET FORTH\\nBY THE CUSTOMER`S AGREEMENT WITH THE\\nISSUER\\nAPPROVED\\n\\n\\n\\n\\nCustomer Copy\\n",
        "customData": "order details",
        "generatedBy": "maxduplessy",
        "card": {
            "card.first6": "530676",
            "card.last4": "0213",
            "cardNumber": "1zcGT7J4pkGh0213",
            "cardHolderName": "John Doe",
            "cardType": "Mastercard",
            "expirationDate": "2022-07",
            "customer": {
                "guid": "4b63d92b-c0bf-436c-879d-5cacfcf1b708",
                "firstName": "John",
                "lastName": "Doe",
                "dateOfBirth": "1987-07-07T00:00:00",
                "address1": "107 7th Av.",
                "address2": "",
                "zip": "10007",
                "city": "New York",
                "state": "NY",
                "country": "US",
                "phone": "9177563007",
                "email": "johnd@mailinator.com",
                "ssN4": "1210",
                "driverLicenseNumber": "12345678",
                "driverLicenseState": "TX"
            }
        },
        "associateCustomerCard": true,
        "addressVerificationCode": "N",
        "addressVerificationResult": "No Match",
        "cvvVerificationCode": "M",
        "cvvVerificationResult": "Passed"
    }
}
```

This endpoint gets a return.

### HTTP Request

`GET https://sandbox.choice.dev/api/v1/returns/<guid>`

### Headers using token

Key | Value
--------- | -------
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
guid | Return’s guid to get

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

# Verify

## Create verify

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Verify
    {
        public static void CreateVerify()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Verify");
                request.ContentType = "text/json";
                request.Method = "POST";

                var verify = new
                {
                    DeviceGuid = "b29725af-b067-4a35-9819-bbb31bdf8808",
                    Card = new
                    {
                        CardNumber = "4532922657097402",
                        CardHolderName = "Justin Troudeau",
                        Cvv2 = "999",
                        ExpirationDate = "2012",
                        Customer = new
                        {
                            FirstName = "Justin",
                            LastName = "Troudeau",
                            Phone = "9177563051",
                            City = "New York",
                            State = "NY",
                            Country = "US",
                            Email = "justint@mailinator.com",
                            Address1 = "111 11th Av.",
                            Address2 = "",
                            Zip = "10011",
                            DateOfBirth = "1991-11-11",
                            DriverLicenseNumber = "12345678",
                            DriverLicenseState = "TX",
                            SSN4 = "1210"
                        }
                    }
                };

                string json = JsonConvert.SerializeObject(verify);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Request:

```json
{
  "DeviceGuid" : "b29725af-b067-4a35-9819-bbb31bdf8808",
  "Card":
  {
    "CardNumber" : "4532922657097402",
    "CardHolderName" : "Justin Troudeau",
    "Cvv2" : "999",
    "ExpirationDate" : "2012",
    "Customer":
    {
      "FirstName" : "Justin",
      "LastName" : "Troudeau",
      "Phone" : "9177563051",
      "City" : "New York",
      "State" : "NY",
      "Country" : "US",
      "Email" : "justint@mailinator.com",
      "Address1" : "111 11th Av.",
      "Address2" : "",
      "Zip" : "10011",
      "DateOfBirth" : "1991-11-11",
      "DriverLicenseNumber" : "12345678",
      "DriverLicenseState" : "TX",
      "SSN4" : "1210"
    }
  }
}
```

> Json Example Response:

```json
{
    "guid": "496b8523-7b36-42a4-9026-2b70cf684249",
    "status": "Transaction - Approved",
    "timeStamp": "2020-11-25T07:24:25.29-06:00",
    "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "card": {
        "card.irst6": "453292",
        "card.last4": "7402",
        "cardNumber": "hNDbeGr7VBgY7402",
        "cardHolderName": "Justin Troudeau",
        "cardType": "Visa",
        "expirationDate": "2020-12",
        "customer": {
            "guid": "7879844d-33ff-4a2e-af5b-b72ca9487664",
            "firstName": "Justin",
            "lastName": "Troudeau",
            "dateOfBirth": "1991-11-11T00:00:00",
            "address1": "111 11th Av.",
            "address2": "",
            "zip": "10011",
            "city": "New York",
            "country": "US",
            "phone": "9177563051",
            "email": "justint@mailinator.com",
            "ssN4": "1210",
            "driverLicenseNumber": "12345678"
        }
    },
    "cardDataSource": "INTERNET",
    "processorStatusCode": "A0000",
    "wasProcessed": true
}
```

The Verify transaction is used when you want to know if the card data you have is valid and it’s ready to run other transactions like Auth Only or Sale. Therefore we are talking about a $0.00 amount transaction, no money is moved.

This endpoint creates a verify.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Verify`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device’s Guid.
 |  | 
**Card** |  | 
CardNumber | string | Mandatory | Card number. Must be 16 characters.  (example: 4532538795426624) or token (example: FfL7exC7Xe2y6624).
CardHolderName | string | Optional | Cardholder's name.
Cvv2 | string | Optional | This is the three or four digit CVV code at the back side of the credit and debit card.
ExpirationDate | date | Optional with Token | Card's expiry date in the YYMM format.
Customer | object | Optional | Customer.
 |  | 
**Customer** |  | 
FirstName | string | Optional | Customer's first name.
LastName | string | Optional | Customer's last name.
Phone | string | Optional | Customer's phone number. The phone number must be syntactically correct. For example, 4152345678.
City | string | Optional | Customer's city.
State | string | Optional | Customer's short name state. The ISO 3166-2 CA and US state or province code of a customer. Length = 2.
Country | string | Optional | Customer's country. The ISO country code of a customer’s country. Length = 2 or 3.
Email | string | Optional | Customer's valid email address.
Address1 | string | Optional | Customer's address.
Address2 | string | Optional | Customer's address line 2.
Zip | string | Optional | Customer's zipcode. Length = 5.
DateOfBirth | date | Optional | Customer's date of birth.<br><br>Allowed format:<br><br>YYYY-MM-DD.<br>For example: 2002-05-30
DriverLicenseNumber | string | Optional | Customer's driver license number.
DriverLicenseState | string | Mandatory when DriverLicenseNumber is provided | Customer's driver license short name state. The ISO 3166-2 CA and US state or province code of a customer. Length = 2.
SSN4 | string | Mandatory when DOB is not submitted | Customer's social security number.

### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

## Get verify

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Verify
    {
        public static void GetVerify()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Verify/496b8523-7b36-42a4-9026-2b70cf684249");
                request.Method = "GET";

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
 
    }
}
```

> Json Example Response:

```json
{
    "guid": "496b8523-7b36-42a4-9026-2b70cf684249",
    "status": "Transaction - Approved",
    "timeStamp": "2020-11-25T07:24:25.29-06:00",
    "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "card": {
        "card.first6": "453292",
        "card.last4": "7402",
        "cardNumber": "hNDbeGr7VBgY7402",
        "cardHolderName": "Justin Troudeau",
        "cardType": "Visa",
        "expirationDate": "2020-12",
        "customer": {
            "guid": "7879844d-33ff-4a2e-af5b-b72ca9487664",
            "firstName": "Justin",
            "lastName": "Troudeau",
            "dateOfBirth": "1991-11-11T00:00:00",
            "address1": "111 11th Av.",
            "address2": "",
            "zip": "10011",
            "city": "New York",
            "country": "US",
            "phone": "9177563051",
            "email": "justint@mailinator.com",
            "ssN4": "1210",
            "driverLicenseNumber": "12345678"
        }
    },
    "cardDataSource": "INTERNET",
    "processorStatusCode": "A0000",
    "wasProcessed": true
}
```

This endpoint gets a verify.

### HTTP Request

`GET https://sandbox.choice.dev/api/v1/Verify/<guid>`

### Headers using token

Key | Value
--------- | -------
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
guid | Verify’s guid to get

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

# Tokenization

## Create tokenization

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Tokenization
    {
        public static void CreateTokenization()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/tokenization");
                request.ContentType = "text/json";
                request.Method = "POST";

                var tokenization = new
                {
                    DeviceGuid = "F050E264-EABF-433E-8C81-916DFE110159",
                    Card = new
                    {
                        CardNumber = "4024007194311436",
                        CardHolderName = "John Doe",
                        ExpirationDate = "2507",
                        Customer = new
                        {
                            FirstName = "John",
                            LastName = "Doe"
                        }
                    }
                };

                string json = JsonConvert.SerializeObject(tokenization);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Request:

```json
{
    "DeviceGuid": "F050E264-EABF-433E-8C81-916DFE110159",
    "Card": {
        "CardNumber": "4024007194311436",
        "CardHolderName": "John Doe",
        "ExpirationDate": "2507",
        "Customer": {
            "FirstName": "John",
            "LastName": "Doe"
        }
    }
}
```

> Json Example Response:

```json
{
    "guid": "a2b8842a-a248-4759-ad71-9607a472e7cc",
    "status": "Transaction - Approved",
    "timeStamp": "2021-07-16T15:36:07.31",
    "deviceGuid": "F050E264-EABF-433E-8C81-916DFE110159",
    "cardDataSource": "INTERNET",
    "processorStatusCode": "A0000",
    "card": {
        "card.first6": "402400",
        "card.last4": "1436",
        "cardNumber": "XR6ngXrNGdD31436",
        "cardHolderName": "John Doe",
        "cardType": "Visa",
        "expirationDate": "2025-07"
    }
}
```

The Tokenization service is used when you want to obtain a tokenized value of the card number but no validation to be performed at the issuing bank.

This endpoint creates a tokenization.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/tokenization`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device’s Guid.
 |  | 
**Card** |  | 
CardNumber | string | Mandatory | Card number. Must be 16 characters.
CardHolderName | string | Optional | Cardholder's name.
ExpirationDate | date | Mandatory | Card's expiry date in the YYMM format.
Customer | object | Optional | Customer.
 |  | 
**Customer** |  | 
FirstName | string | Optional | Customer's first name.
LastName | string | Optional | Customer's last name.

### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

# Recurring Billing

## Create recurring billing

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class RecurringBilling
    {
        public static void CreateRecurringBilling()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/recurringBillings");
                request.ContentType = "text/json";
                request.Method = "POST";

                var recurringBilling = new
                {
                    DeviceGuid = "b29725af-b067-4a35-9819-bbb31bdf8808",
                    Amount = 10.50,
                    Interval = "monthly",
                    IntervalValue = "april, may, june",
                    StartDate = "2021-05-13T00:00:01.000Z",
                    EndDate = "2023-04-01T00:00:01.000Z",
                    PaymentCount = "",
                    ScheduleNotes = "Cable",
                    Description = "Description",
                    Card = new
                    {
                        CardNumber = "4556395004716019",
                        CardHolderName = "John Doe",
                        Cvv2 = "999",
                        ExpirationDate = "2012",
                        Customer = new
                        {
                            FirstName = "John",
                            LastName = "Doe",
                            Phone = "9865123654",
                            City = "New York",
                            State = "NY",
                            Country = "US",
                            Email = "johnd@mailinator.com",
                            Address1 = "12th Ave. 5472",
                            Address2 = "",
                            Zip = "10003",
                            DateOfBirth = "1989-10-01T00:00:01.000Z",
                            DriverLicenseNumber = "12345678",
                            DriverLicenseState = "TX",
                            SSN4 = "1210"
                        }
                    }
                };

                string json = JsonConvert.SerializeObject(recurringBilling);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }  
    }
}
```

> Json Example Request:

```json
{
  "DeviceGuid" : "b29725af-b067-4a35-9819-bbb31bdf8808",
  "Amount" : 10.50,
  "Interval" : "monthly",
  "IntervalValue" : "april, may, june",
  "StartDate" : "2021-05-13",
  "EndDate" : "2023-04-01",
  "PaymentCount" : "",
  "ScheduleNotes" : "Cable",
  "Description" : "Description",
  "Card":
  {
    "CardNumber" : "4556395004716019",
    "CardHolderName" : "John Doe",
    "Cvv2" : "999",
    "ExpirationDate" : "2012",
    "Customer":
    {
      "FirstName" : "John",
      "LastName" : "Doe",
      "Phone" : "9865123654",
      "City" : "New York",
      "State" : "NY",
      "Country" : "US",
      "Email" : "johndoe@mailinator.com",
      "Address1" : "12th Ave. 5472",
      "Address2" : "",
      "Zip" : "10003",
      "DateOfBirth" : "1989-10-01T00:00:01.000Z",
      "DriverLicenseNumber" : "12345678",
      "DriverLicenseState" : "TX",
      "SSN4" : "1210"
    }
  }
}
```

> Json Example Response:

```json
{
    "guid": "11fd2d0a-298f-4387-81ed-ffd6780f7a21",
    "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "status": "RecurringBilling - Active",
    "interval": "monthly",
    "intervalValue": "april, may, june",
    "amount": 10.50,
    "recurringBillingNumber": "64942678",
    "createdDate": "2020-11-25T08:31:38.6-06:00",
    "startDate": "2021-05-13T00:00:00",
    "endDate": "2023-04-01T00:00:00",
    "scheduleNotes": "Cable",
    "description": "Description",
    "merchantProductListGuid": "00000000-0000-0000-0000-000000000000",
    "invoiceGuid": "00000000-0000-0000-0000-000000000000",
    "merchantProductListTotalAmount": 0.0,
    "merchantGuid": "24639b5f-f881-45dc-966c-4beece954e6c",
    "pauseStartDate": null,
    "pauseEndDate": null,
    "processorStatusCode": "OK",
    "processorResponseMessage": "Recurring billing scheduled. Payment count: 5. First payment: Thursday, May 13, 2021. Last payment: Monday, June 13, 2022",
    "wasProcessed": true,
    "card": {
        "card.first6": "455639",
        "card.last4": "6019",
        "cardNumber": "7hEtLJIhooTE6019",
        "cardHolderName": "John Doe",
        "cardType": "Visa",
        "expirationDate": "2020-12",
        "customer": {
            "guid": "3861f29b-2bb6-4ec0-8779-9efa1b3bb015",
            "firstName": "John",
            "lastName": "Doe",
            "dateOfBirth": "1989-10-01T00:00:00",
            "address1": "12th Ave. 5472",
            "address2": "",
            "zip": "10003",
            "city": "New York",
            "state": "NY",
            "country": "US",
            "phone": "9865123654",
            "email": "johndoe@mailinator.com",
            "ssN4": "1210",
            "driverLicenseNumber": "12345678",
            "driverLicenseState": "TX"
        }
    },
    "scheduleAndPayments": [
        {
            "scheduledPaymentDate": "2021-05-13T00:00:00",
            "scheduledPaymentDateDayOfWeek": "Thursday",
            "scheduledPaymentNumber": 1,
            "scheduledWasProcessed": false
        },
        {
            "scheduledPaymentDate": "2021-06-13T00:00:00",
            "scheduledPaymentDateDayOfWeek": "Sunday",
            "scheduledPaymentNumber": 2,
            "scheduledWasProcessed": false
        },
        {
            "scheduledPaymentDate": "2022-04-13T00:00:00",
            "scheduledPaymentDateDayOfWeek": "Wednesday",
            "scheduledPaymentNumber": 3,
            "scheduledWasProcessed": false
        },
        {
            "scheduledPaymentDate": "2022-05-13T00:00:00",
            "scheduledPaymentDateDayOfWeek": "Friday",
            "scheduledPaymentNumber": 4,
            "scheduledWasProcessed": false
        },
        {
            "scheduledPaymentDate": "2022-06-13T00:00:00",
            "scheduledPaymentDateDayOfWeek": "Monday",
            "scheduledPaymentNumber": 5,
            "scheduledWasProcessed": false
        }
    ]
}
```

Recurring billing is a great feature you can use when you want to charge a card or debit from a bank account every a determined period of time. Period can be daily, weekly, biweekly, monthly, yearly, etc. Check the fields to know how to work with each of them.

This endpoint creates a recurring billing.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/recurringBillings`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Optional | Device's Guid. Mandatory when providing a CreditCard or a BankAccount. Not required when providing an InvoiceGuid.
InvoiceGuid | string | Optional | The Guid of the Invoice you want to make recurring. Do not send amount, nor Card neither Bank Account in this case.
Amount | decimal | Optional | Transaction's amount. Min. amt.: $0.50. Mandatory when amount is fixed.
Interval | string | Mandatory | Recurring billing interval. A customer can schedule the recurring payment at a desired interval.<br><br>Allowed values:<br><br>**1. yearly**<br>**2. halfYearly** <br>**3. quarterly** <br>**4. monthly** <br>**5. fortnightly** <br>**6. weekly** <br>**7. biweekly** <br>**8. daily** <br>**9. CustomDates** <br>**10. each1st** <br>**11. each15th** <br>**12. 1st15th**
IntervalValue | string | Mandatory for monthly, weekly and custom dates modes| Allowed values:<br><br>**1**.For the **yearly**, **halfYearly** , **quarterly**, **fortnightly**, **biweekly**  and **daily** mode, does not need to enter **IntervalValue**, since they do not have.<br><br>**2**.For the **monthly** mode, select **everyMonth** or the months of the year. Example: "Interval" : "monthly", "IntervalValue" : "april, may, june".<br><br>**3**.For **weekly** mode, select **everyWeek** or the days of the week. Example: "Interval" : "weekly", "IntervalValue" : "monday, tuesday, wednesday".<br><br>**4**.For **CustomDates** mode, select a list of dates (yyyy-MM-dd) separated by commas. Example: "Interval" : "CustomDates", "IntervalValue" : "2017-03-12, 2017-04-06, 2017-05-17, 2017-06-12".
SubIntervalValue | string | Mandatory for monthly | Allowed values:<br><br>**1**.Any day of the month.
StartDate | date | Mandatory | The first payment date of the recurring billing schedule.<br><br>Allowed Recurring Billing format:<br><br>YYYY-MM-DD<br>For example: 2002-05-30
EndDate | date | Mandatory when payment count is not submitted | The last payment date of the recurring billing schedule.<br><br>Allowed Recurring Billing format:<br><br>YYYY-MM-DD<br>For example: 2002-05-30<br><br>**Note: Send either EndDate or PaymentCount field in a request.**
PaymentCount | integer | Mandatory when end date is not submitted | The count of payments in a recurring schedule payment, set by a customer.<br><br>**Note: Send either PaymentCount or EndDate field in a request.**
ScheduleNotes | string | Optional | This field provides a place for the user to identify the reason they added or modified the recurring schedule.
Description | string | Optional | General description about the recurring billing.
DynamicAmount | boolean | Optional | Mandatory and true when providing a merchant product list guid. Not compatible with enhanced data.
MerchantProductListGuid | string | Optional | MerchantProductList's Guid. Mandatory when providing a dynamic amount.
SendReceipt | boolean | Optional | Set to “FALSE” if you do not want an e-mail receipt to be sent to the customer. Set to “TRUE” or leave empty if you want e-mail to be sent.
EnhancedData | object | Optional | EnhancedData.
 |  | 
**EnhancedData** |  |
SaleTax | decimal | Optional | Transaction's amount.
AdditionalTaxDetailTaxCategory | string | Optional | Tax Category.
AdditionalTaxDetailTaxType | string | Optional | Tax Type.
AdditionalTaxDetailTaxAmount | decimal | Optional | Tax Amount.
AdditionalTaxDetailTaxRate | decimal | Optional | Tax Rate.
PurchaseOrder | string | Optional | Purchase Order.
ShippingCharges | decimal | Optional | Shipping Charges.
DutyCharges | decimal | Optional | Duty Charges.
CustomerVATNumber | string | Optional | Customer VAT Number.
VATInvoice | string | Optional | VAT Invoice.
SummaryCommodityCode | string | Optional | Summary Commodity Code.
ShipToZip | string | Optional | Ship To  Zip.
ShipFromZip | string | Optional | Ship From Zip.
DestinationCountryCode | string | Optional | Destination Country Code.
SupplierReferenceNumber | string | Optional | Supplier Reference Number.
CustomerRefID | string | Optional | Customer Ref ID.
ChargeDescriptor | string | Optional | Charge Descriptor.
AdditionalAmount | decimal | Optional | Additional Amount.
AdditionalAmountType | string | Optional | Additional Amount Type.
ProductName | string | Optional | Product Name.
ProductCode | string | Optional | Product Code.
Price | string | Optional | Price.
MeasurementUnit | string | Optional | Measurement Unit.
 |  | 
**Card** |  |  | **Note: Send either Card or BankAccount object in a request.**
CardNumber | string | Mandatory | Card number. Must be 16 characters.  (example: 4532538795426624) or token (example: FfL7exC7Xe2y6624).
CardHolderName | string | Optional | Cardholder's name.
Cvv2 | string | Optional | This is the three or four digit CVV code at the back side of the credit and debit card.
ExpirationDate | date | Optional with Token | Card's expiry date in the YYMM format.
Customer | object | Optional | Customer.
 |  | 
**BankAccount** |  |  | **Note: Send either BankAccount or Card object in a request.**
RoutingNumber | string | Mandatory | Routing's number. Must be 9 characters (example: 490000018).
AccountNumber | string | Mandatory | Account's number.
NameOnAccount | string | Mandatory | Account's name.
Customer | object | Optional | Customer.
 |  | 
**Customer** |  | 
FirstName | string | Mandatory with bank account<br>Optional with credit card | Customer's first name.
LastName | string | Mandatory with bank account<br>Optional with credit card | Customer's last name.
Phone | string | Mandatory with bank account<br>Optional with credit card | Customer's phone number. The phone number must be syntactically correct. For example, 4152345678.
City | string | Mandatory with bank account<br>Optional with credit card | Customer's city.
State | string | Mandatory with bank account<br>Optional with credit card | Customer's short name state. The ISO 3166-2 CA and US state or province code of a customer. Length = 2.
Country | string | Mandatory with bank account<br>Optional with credit card | Customer's country. The ISO country code of a customer’s country. Length = 2 or 3.
Email | string | Mandatory with bank account<br>Optional with credit card | Customer's valid email address.
Address1 | string | Mandatory with bank account<br>Optional with credit card | Customer's address.
Address2 | string | Optional | Customer's address line 2.
Zip | string | Mandatory with bank account<br>Optional with credit card | Customer's zipcode. Length = 5.
DateOfBirth | date | With credit card: optional<br>With bank account: mandatory when SSN4 is not submitted | Customer's date of birth.<br><br>Allowed format:<br><br>YYYY-MM-DD.<br>For example: 2002-05-30
DriverLicenseNumber | string | With credit card: optional<br>With bank account: optional | Customer's driver license number.
DriverLicenseState | string | With credit card: optional<br>With bank account: mandatory when DriverLicenseNumber is provided | Customer's driver license short name state. The ISO 3166-2 CA and US state or province code of a customer. Length = 2.
SSN4 | string | With credit card: optional<br>With bank account: mandatory when DOB is not submitted | Customer's social security number.

### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

<aside class="success">
You will get updates by e-mail regarding Recurring Billing status.
</aside>

## Update recurring billing

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class RecurringBilling
    {
        public static void UpdateRecurringBilling()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/recurringBillings/1de7a995-6e4d-4726-afb3-c9972b9b150a");
                request.ContentType = "text/json";
                request.Method = "PUT";

                var recurringBilling = new
                {
                    Amount = 25.40,
                    Interval = "monthly",
                    IntervalValue = "january",
                    StartDate = "2017-04-01T00:00:01.000Z",
                    EndDate = "2018-06-01T00:00:01.000Z",
                    PaymentCount = "",
                    ScheduleNotes = "Cable",
                    Description = "Description",
                    Status = "RecurringBilling -  Active"
                };

                string json = JsonConvert.SerializeObject(recurringBilling);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
  "Amount" : 25.40,
  "Interval" : "monthly",
  "IntervalValue" : "january",
  "StartDate" : "2017-04-01T00:00:01.000Z",
  "EndDate" : "2018-06-01T00:00:01.000Z",
  "PaymentCount" : "",
  "ScheduleNotes" : "Cable",
  "Description" : "Description",
  "Status" : "RecurringBilling -  Active"
}
```

> Json Example Response:

```json
{
  "guid": "1de7a995-6e4d-4726-afb3-c9972b9b150a",
  "deviceGuid": "8257dde1-ded6-4c38-ab71-4338c4aa87ac",
  "status": "RecurringBilling -  Active",
  "interval": "monthly",
  "intervalValue": "january",
  "amount": 25.4,
  "recurringBillingNumber": "99851606",
  "startDate": "2017-04-01T00:00:01",
  "endDate": "2018-06-01T00:00:01",
  "scheduleNotes": "Cable",
  "description" : "Description",
  "processorStatusCode": "OK",
  "processorResponseMessage": "Recurring billing re-scheduled. Payment count: 1. First payment: Monday, January 1, 2018. Last payment: Monday, January 1, 2018",
  "wasProcessed": true,
  "card": {
    "card.first4": "4556",
    "card.last4": "6019",
    "cardNumber": "7hEtLJIhooTE6019",
    "cardHolderName": "John Doe",
    "expirationDate": "2017-10",
    "customer": {
      "guid": "d0912636-4fc1-48a4-b002-aa39a9df1288",
      "firstName": "John",
      "lastName": "Doe",
      "phone": "9865123654",
      "city": "New York",
      "country": "US",
      "email": "johndoe@mailinator.com",
      "zip": "10003",
      "address1": "12th Ave. 5472",
      "address2": "",
      "state": "NY",
      "dateOfBirth": "1989-10-01T00:00:00",
      "DriverLicenseNumber" : "12345678",
      "DriverLicenseState" : "TX",
      "SSN4" : "1210"
    }
  },
  "scheduleAndPayments": [
    {
      "scheduledPaymentDate": "2018-01-01T00:00:00",
      "scheduledPaymentDateDayOfWeek": "Monday",
      "scheduledPaymentNumber": 1,
      "scheduledWasProcessed": false
    }
  ]
}
```

This endpoint updates a recurring billing.

### HTTP Request

`PUT https://sandbox.choice.dev/api/v1/recurringBillings<guid>`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
guid | Recurring billings’s guid to update


### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
Amount | decimal | Optional | Transaction's amount. Min. amt.: $0.50
Interval | string | Optional | Recurring billing interval. A customer can schedule the recurring payment at a desired interval.<br><br>Allowed values:<br><br>**1. yearly**<br>**2. halfYearly** <br>**3. quarterly** <br>**4. monthly** <br>**5. fortnightly** <br>**6. weekly** <br>**7. biweekly** <br>**8. daily** <br>**9. CustomDates** <br>**10. each1st** <br>**11. each15th** <br>**12. 1st15th**
IntervalValue | string | Optional | Allowed values:<br><br>**1**.For the **yearly**, **halfYearly** , **quarterly**, **fortnightly**, **biweekly**  and **daily** mode, does not need to enter **IntervalValue**, since they do not have.<br><br>**2**.For the **monthly** mode, select **everyMonth** or the months of the year. Example: "Interval" : "monthly", "IntervalValue" : "april, may, june".<br><br>**3**.For **weekly** mode, select **everyWeek** or the days of the week. Example: "Interval" : "weekly", "IntervalValue" : "monday, tuesday, wednesday".<br><br>**4**.For **CustomDates** mode, select a list of dates (yyyy-MM-dd) separated by commas. Example: "Interval" : "CustomDates", "IntervalValue" : "2017-03-12, 2017-04-06, 2017-05-17, 2017-06-12".
SubIntervalValue | string | Mandatory for monthly | Allowed values:<br><br>**1**.Any day of the month.
StartDate | date | Optional | The first payment date of the recurring billing schedule.<br><br>Allowed Recurring Billing format:<br><br>YYYY-MM-DD<br>For example: 2002-05-30
EndDate | date | Optional | The first payment date of the recurring billing schedule.<br><br>Allowed Recurring Billing format:<br><br>YYYY-MM-DD<br>For example: 2002-05-30<br><br>**Note: Send either EndDate or PaymentCount field in a request.**
PaymentCount | integer | Optional | The count of payments in a recurring schedule payment, set by a customer.<br><br>**Note: Send either PaymentCount or EndDate field in a request.**
ScheduleNotes | string | Optional | This field provides a place for the user to identify the reason they added or modified the recurring schedule.
Description | string | Optional | General description about the recurring billing.
SendReceipt | boolean | Optional | Set to “FALSE” if you do not want an e-mail receipt to be sent to the customer. Set to “TRUE” or leave empty if you want e-mail to be sent.
Status | string | Optional | Recurring billing status.<br><br>Allowed values:<br><br>**1. RecurringBilling -  Active**<br>**2. RecurringBilling - Created - Local** <br>**3. RecurringBilling - Created - Error: Processor not reached** <br>**4. RecurringBilling - Created - Processor Fail** <br>**5. RecurringBilling -  Deactivated** <br>**6. RecurringBilling -  Paused** <br>**7. RecurringBilling - Finished** <br>**8. RecurringBilling - Deleted** <br>**9. RecurringBilling - Active Started**

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

## Get recurring billing

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class RecurringBilling
    {
        public static void GetRecurringBilling()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/recurringBillings/1de7a995-6e4d-4726-afb3-c9972b9b150a");
                request.Method = "GET";

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Response:

```json
{
  "guid": "d1507904-f84c-4508-98ef-5f0fcc417019",
  "deviceGuid": "75f97793-430b-4e94-9aec-383950639b18",
  "status": "RecurringBilling -  Active",
  "interval": "monthly",
  "intervalValue": "april, may, june",
  "amount": 10.5,
  "recurringBillingNumber": "14339150",
  "startDate": "2017-05-13T00:00:00",
  "endDate": "2019-04-01T00:00:00",
  "scheduleNotes": "Cable",
  "description": "Description",
  "processorStatusCode": "OK",
  "processorResponseMessage": "Recurring billing scheduled. Payment count: 5. First payment: Saturday, May 13, 2017. Last payment: Wednesday, June 13, 2018",
  "wasProcessed": true,
  "card": {
    "card.first6": "124556",
    "card.last4": "6019",
    "cardNumber": "7hEtLJIhooTE6019",
    "cardHolderName": "John Doe",
    "expirationDate": "2017-10",
    "customer": {
      "guid": "d0912636-4fc1-48a4-b002-aa39a9df1288",
      "firstName": "John",
      "lastName": "Doe",
      "phone": "9865123654",
      "city": "New York",
      "country": "US",
      "email": "johndoe@mailinator.com",
      "zip": "10003",
      "address1": "12th Ave. 5472",
      "address2": "",
      "state": "NY",
      "dateOfBirth": "1989-10-01T00:00:00",
      "DriverLicenseNumber" : "12345678",
      "DriverLicenseState" : "TX",
      "SSN4" : "1210"
    }
  },
  "scheduleAndPayments": [
    {
      "scheduledPaymentDate": "2017-05-13T00:00:00",
      "scheduledPaymentDateDayOfWeek": "Saturday",
      "scheduledPaymentNumber": 1,
      "scheduledWasProcessed": false
    },
    {
      "scheduledPaymentDate": "2017-06-13T00:00:00",
      "scheduledPaymentDateDayOfWeek": "Tuesday",
      "scheduledPaymentNumber": 2,
      "scheduledWasProcessed": false
    },
    {
      "scheduledPaymentDate": "2018-04-13T00:00:00",
      "scheduledPaymentDateDayOfWeek": "Friday",
      "scheduledPaymentNumber": 3,
      "scheduledWasProcessed": false
    },
    {
      "scheduledPaymentDate": "2018-05-13T00:00:00",
      "scheduledPaymentDateDayOfWeek": "Sunday",
      "scheduledPaymentNumber": 4,
      "scheduledWasProcessed": false
    },
    {
      "scheduledPaymentDate": "2018-06-13T00:00:00",
      "scheduledPaymentDateDayOfWeek": "Wednesday",
      "scheduledPaymentNumber": 5,
      "scheduledWasProcessed": false
    }
  ]
}
```

This endpoint gets a recurring billing.

### HTTP Request

`GET https://sandbox.choice.dev/api/v1/recurringBillings/<guid>`

### Headers using token

Key | Value
--------- | -------
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
guid | Recurring billings’s guid to get

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

# Batch

## Close batch

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Batch
    {
        public static void CloseBatch()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Batches/close");
                request.ContentType = "text/json";
                request.Method = "POST";

                var batch = new
                {
                    DeviceGuid = "8257dde1-ded6-4c38-ab71-4338c4aa87ac"
                };

                string json = JsonConvert.SerializeObject(batch);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

> Json Example Request:

```json
{
  "DeviceGuid" : "b29725af-b067-4a35-9819-bbb31bdf8808"
}
```

> Json Example Response:

```json
{
    "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
    "guid": "58e8a0ad-d167-4015-a4b1-904b1e7f2b26",
    "status": "PASS",
    "responseCode": "A0000",
    "responseMessage": "Success",
    "closureDate": "2020-11-25T14:39:56.2262428Z",
    "batchInfo": {
        "batchNumber": "160635",
        "saleCount": 2,
        "saleAmount": 46.53,
        "returnCount": 1,
        "returnAmount": 19.74,
        "batchNetAmount": 26.79
    }
}
```

Batch is processing all the authorized transactions of the day at the end of the day. However, you can close a batch manually before. You can also get your transactions searched by batch.

This endpoint closes a batch.

<aside class="warning">
<strong style="font-size: 130%;">Batch closing is mandatory, otherwise transactions are not settled.</strong>
</aside>

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Batches/close`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"


### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device’s Guid.
SemiIntegrated | boolean | Optional | Only when physical terminal used on semi integrated mode, send value True.

### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

## Search batches

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Batch
    {
        public static void SearchBatch()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Batches/search");
                request.ContentType = "text/json";
                request.Method = "POST";

                var batch = new
                {
                    deviceGuid = "58ccf5a4-1d5d-4546-8202-da5c7ad10711",
                    startDate = "1/1/1900",
                    endDate = "12/31/2020"
                };

                string json = JsonConvert.SerializeObject(batch);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }  
    }
}
```

> Json Example Request:

```json
{
  "deviceGuid" : "58ccf5a4-1d5d-4546-8202-da5c7ad10711",
  "startDate" : "1/1/1900",
  "endDate" : "12/31/2020"
}
```

> Json Example Response:

```json
{
    "count": 2,
    "ret": [
        {
            "batchGuid": "58e8a0ad-d167-4015-a4b1-904b1e7f2b26",
            "batchNumber": "160635",
            "closureDate": "2020-11-25T08:39:56.86-06:00",
            "batchTotalAmount": 26.79,
            "userName": "maxduplessy",
            "summary": {
                "saleCount": 2,
                "saleAmount": 46.53,
                "returnCount": 1,
                "returnAmount": 19.74
            },
            "transactions": [
                {
                    "transactionType": "Return",
                    "transactionDatetime": "2020-11-23T07:17:24.15-06:00",
                    "lastFour": "0213",
                    "cardType": "Mastercard",
                    "amount": 19.74,
                    "processorResponseMessage": "Return requested, Void successful", 
                    "deviceGuid": "9f8fa757-d607-4ba8-a26a-f6e7ae4a94af", 
                    "generatedByRecurringBilling": false, 
                    "orderNumber": "AC4a7DG",
                    "orderDate": "2017-02-03T00:00:00",
                    "invoiceNumber": "Ref46g6", 
                    "processorResponseCode": "A0000",
                    "processorAuthCode": "TAS0243",
                    "processorRefNumber": "85040041",
                    "softDescriptor": "1", 
                    "tipAmount": 3.000,
                    "serviceFee": 0.40,
                    "surchargeAmount": 0.35,
                    "surchargeLabel": "4 Percent",
                    "cvvVerificationCode": "M",
                    "addressVerificationCode": "N",
                    "invoiceGuid": "4f8fa757-d617-4ba8-f56a-f6e7ae4a94jg", 
                    "currencyCode": "USD",
                    "semiIntegrated": false, 
                    "cardDataSource": "INTERNET",
                    "generatedByCapture": false,        
                    "salesTax": 0.1, 
                    "shippingCharges": 0.12, 
                    "CustomerRefID": "Ref937"
                },
                {
                    "transactionType": "Sale",
                    "transactionDatetime": "2020-11-28T07:29:06.97-06:00",
                    "lastFour": "8442",
                    "cardType": "Visa",
                    "amount": 25.04,
                    "processorResponseMessage": "Success", 
                    "deviceGuid": "9f8fa757-d607-4ba8-a56a-f6e7ae4a94af", 
                    "generatedByRecurringBilling": false, 
                    "orderNumber": "AC47DQG",
                    "orderDate": "2017-02-03T00:00:00",
                    "invoiceNumber": "Ref4668", 
                    "processorResponseCode": "A0000",
                    "processorAuthCode": "TAS0244",
                    "processorRefNumber": "85040012",
                    "softDescriptor": "1", 
                    "tipAmount": 3.000,
                    "serviceFee": 0.40,
                    "surchargeAmount": 0.35,
                    "surchargeLabel": "4 Percent",
                    "cvvVerificationCode": "M",
                    "addressVerificationCode": "N",
                    "invoiceGuid": "4f8fa757-d607-4ba8-f56a-f6e7ae4a9fjg", 
                    "currencyCode": "USD",
                    "semiIntegrated": false, 
                    "cardDataSource": "INTERNET",
                    "generatedByCapture": false,        
                    "salesTax": 0.1, 
                    "shippingCharges": 0.12, 
                    "CustomerRefID": "Ref9977"
                },
                {
                    "transactionType": "Sale",
                    "transactionDatetime": "2020-11-29T07:30:52.72-06:00",
                    "lastFour": "0213",
                    "cardType": "Mastercard",
                    "amount": 21.49,
                    "processorResponseMessage": "Success", 
                    "deviceGuid": "9f8fa757-d607-4ba8-a56a-f6e7aw4a94af", 
                    "generatedByRecurringBilling": false, 
                    "orderNumber": "ACL47DG",
                    "orderDate": "2017-02-08T00:00:00",
                    "invoiceNumber": "Ref4166", 
                    "processorResponseCode": "A0000",
                    "processorAuthCode": "TAS0424",
                    "processorRefNumber": "85042001",
                    "softDescriptor": "1", 
                    "tipAmount": 3.000,
                    "serviceFee": 0.40,
                    "surchargeAmount": 0.35,
                    "surchargeLabel": "4 Percent",
                    "cvvVerificationCode": "M",
                    "addressVerificationCode": "N",
                    "invoiceGuid": "4f8fa757-d607-4ba8-fw6a-f6e7ae4a94jg", 
                    "currencyCode": "USD",
                    "semiIntegrated": false, 
                    "cardDataSource": "INTERNET",
                    "generatedByCapture": false,        
                    "salesTax": 0.1, 
                    "shippingCharges": 0.12, 
                    "CustomerRefID": "Ref4997"
                }
            ]
        },
        {
            "batchGuid": "b8d73296-e397-45c0-bb43-ba064e9750ac",
            "batchNumber": "160634",
            "closureDate": "2020-11-25T07:16:27.09-06:00",
            "batchTotalAmount": 19.74,
            "userName": "maxduplessy",
            "summary": {
                "saleCount": 1,
                "saleAmount": 19.74,
                "returnCount": 0,
                "returnAmount": 0.00
            },
            "transactions": [
                {
                    "transactionType": "Sale",
                    "transactionDatetime": "2020-11-15T07:15:27.28-06:00",
                    "lastFour": "0213",
                    "cardType": "Mastercard",
                    "amount": 19.74,
                    "processorResponseMessage": "Success", 
                    "deviceGuid": "9f8fa757-d607-4ba8-a54a-f6e7ae4a94af", 
                    "generatedByRecurringBilling": false, 
                    "orderNumber": "AC47DG",
                    "orderDate": "2017-07-03T00:00:00",
                    "invoiceNumber": "Ref4696", 
                    "processorResponseCode": "A0000",
                    "processorAuthCode": "TAS02497",
                    "processorRefNumber": "850400196",
                    "softDescriptor": "1", 
                    "tipAmount": 3.000,
                    "serviceFee": 0.40,
                    "surchargeAmount": 0.35,
                    "surchargeLabel": "4 Percent",
                    "cvvVerificationCode": "M",
                    "addressVerificationCode": "N",
                    "invoiceGuid": "4f8fa757-d607-4ba8-f56a-f6e75e4a94jg", 
                    "currencyCode": "USD",
                    "semiIntegrated": false, 
                    "cardDataSource": "INTERNET",
                    "generatedByCapture": false,        
                    "salesTax": 0.1, 
                    "shippingCharges": 0.12, 
                    "CustomerRefID": "Ref99760"
                }
            ]
        }
    ]
}
```

This endpoint searches batches in a date range.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Batches/search`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
deviceGuid | string | Mandatory | Device's guid.
startDate | date | Mandatory | Search's start date.<br><br>Allowed Recurring Billing format:<br><br>YYYY-MM-DD<br>For example: 2002-05-30
endDate | date | Mandatory | Search's end date.<br><br>Allowed Recurring Billing format:<br><br>YYYY-MM-DD<br>For example: 2002-05-30

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

## Get batches with transactions

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Batch
    {
        public static void GetBatchsWithTransactions()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Batches/58e8a0ad-d167-4015-a4b1-904b1e7f2b26/Transactions");
                request.Method = "GET";

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Response:

```json
[
    {
        "transactionType": "Return",
        "transactionDatetime": "2020-11-25T07:17:24.15-06:00",
        "lastFour": "0213",
        "cardType": "Mastercard",
        "amount": 19.74,
        "processorResponseMessage": "Return requested, Void successful",
        "userName": "maxduplessy", 
        "deviceGuid": "9f8fa757-d607-4ba8-a56a-f6e7ae4a94af", 
        "generatedByRecurringBilling": false, 
        "orderNumber": "AC47DjG",
        "orderDate": "2017-02-03T00:00:00",
        "invoiceNumber": "Ref466", 
        "processorResponseCode": "A0000",
        "processorAuthCode": "TAS024",
        "processorRefNumber": "8504001",
        "softDescriptor": "1", 
        "tipAmount": 3.000,
        "serviceFee": 0.40,
        "surchargeAmount": 0.35,
        "surchargeLabel": "4 Percent",
        "cvvVerificationCode": "M",
        "addressVerificationCode": "N",
        "invoiceGuid": "4f8fa757-d607-4ba8-f56a-f6e7ae4a94jg", 
        "currencyCode": "USD",
        "semiIntegrated": false, 
        "cardDataSource": "INTERNET",
        "generatedByCapture": false,        
        "salesTax": 0.1, 
        "shippingCharges": 0.12, 
        "CustomerRefID": "Ref99H7"
    },
    {
        "transactionType": "Sale",
        "transactionDatetime": "2020-11-25T07:29:06.97-06:00",
        "lastFour": "8442",
        "cardType": "Visa",
        "amount": 25.04,
        "processorResponseMessage": "Success",
        "userName": "maxduplessy", 
        "deviceGuid": "9f8fa757-d607-4ba8-a56a-f6e7ae4a94af", 
        "generatedByRecurringBilling": false, 
        "orderNumber": "AC47PDG",
        "orderDate": "2017-02-03T00:00:00",
        "invoiceNumber": "Ref466", 
        "processorResponseCode": "A0000",
        "processorAuthCode": "TAS024",
        "processorRefNumber": "8504001",
        "softDescriptor": "1", 
        "tipAmount": 3.000,
        "serviceFee": 0.40,
        "surchargeAmount": 0.35,
        "surchargeLabel": "4 Percent",
        "cvvVerificationCode": "M",
        "addressVerificationCode": "N",
        "invoiceGuid": "9f8fa757-d607-4ba8-f56a-f6e7ae4a94af", 
        "currencyCode": "USD",
        "semiIntegrated": false, 
        "cardDataSource": "INTERNET",
        "generatedByCapture": false,        
        "salesTax": 0.1, 
        "shippingCharges": 0.12, 
        "CustomerRefID": "Ref997"
    },
    {
        "transactionType": "Sale",
        "transactionDatetime": "2020-11-25T07:30:52.72-06:00",
        "lastFour": "0213",
        "cardType": "Mastercard",
        "amount": 21.49,
        "processorResponseMessage": "Success",
        "userName": "maxduplessy", 
        "deviceGuid": "9f8fa757-d607-4ba8-a56a-f6e7ae4a94af", 
        "generatedByRecurringBilling": false, 
        "orderNumber": "AC47Dj",
        "orderDate": "2017-02-09T00:00:00",
        "invoiceNumber": "Ref456", 
        "processorResponseCode": "A0000",
        "processorAuthCode": "TAS24",
        "processorRefNumber": "854401",
        "softDescriptor": "1", 
        "tipAmount": 3.000,
        "serviceFee": 0.40,
        "surchargeAmount": 0.35,
        "surchargeLabel": "4 Percent",
        "cvvVerificationCode": "M",
        "addressVerificationCode": "N",
        "invoiceGuid": "9f8fa757-d607-4ba8-f56a-f6e7ae4a94af", 
        "currencyCode": "USD",
        "semiIntegrated": false, 
        "cardDataSource": "INTERNET",
        "generatedByCapture": false,        
        "salesTax": 0.1, 
        "shippingCharges": 0.12, 
        "CustomerRefID": "Ref78997"
    }
]
```

This endpoint gets a batch with its transactions.

### HTTP Request

`GET https://sandbox.choice.dev/api/v1/Batches/<guid>/Transactions/`

### Headers using token

Key | Value
--------- | -------
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
guid | Batch’s guid to get

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

# Invoice Customer

## Create invoice customer

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class InvoiceCustomer
    {
        public static void CreateInvoiceCustomer()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/InvoiceCustomers");
                request.ContentType = "text/json";
                request.Method = "POST";

                var invoiceCustomer = new
                {
                    MerchantGuid = "24639b5f-f881-45dc-966c-4beece954e6c",
                    CompanyName = "Incutex",
                    FirstName = "John",
                    LastName = "Lock",
                    Address1 = "108 8th Av.",
                    Address2 = "8th Floor",
                    Zip = "10008",
                    City = "New York",
                    State = "NY",
                    PersonalPhone = "9727414574",
                    WorkPhone = "9177563046",
                    Fax = "8004578796",
                    Email = "john.lock@mailinator.com"
                };

                string json = JsonConvert.SerializeObject(invoiceCustomer);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
	"MerchantGuid" : "24639b5f-f881-45dc-966c-4beece954e6c",
	"CompanyName" : "Incutex",
	"FirstName": "John",
	"LastName": "Lock",
	"Address1": "108 8th Av.",
	"Address2": "8th Floor",
	"Zip": "10008",
	"City": "New York",
	"State": "NY",
	"PersonalPhone" : "9727414574",
	"WorkPhone" : "9177563046",
	"Fax" : "8004578796",
	"Email": "john.lock@mailinator.com"
}
```

> Json Example Response:

```json
{
    "guid": "33524364-727a-422b-8bbe-5eab1a849e82",
    "merchantGuid": "24639b5f-f881-45dc-966c-4beece954e6c",
    "customerCode": "LJ7482",
    "companyName": "Incutex",
    "firstName": "John",
    "lastName": "Lock",
    "address1": "108 8th Av.",
    "address2": "8th Floor",
    "zip": "10008",
    "city": "New York",
    "state": "NY",
    "personalPhone": "9727414574",
    "workPhone": "9177563046",
    "fax": "8004578796",
    "email": "john.lock@mailinator.com",
    "invoiceNumber": "000001"
}
```

This endpoint creates a invoice customer.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/InvoiceCustomers`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
MerchantGuid | string | Mandatory | Merchant’s Guid.
CompanyName | string | Optional | Company Name.
FirstName | string |  Optional | User’s first name.
LastName | string | Optional | User’s last name.
Address1 | string | Optional | User’s address.
Address2 | string | Optional | User’s address line 2.
City | string | Optional | User’s city.
State | string | Optional | User’s short name state. The ISO 3166-2 CA and US state or province code of a user. Length = 2.
Zip | string | Optional | User’s zipcode. Length = 5.
PersonalPhone | string | Optional | User’s phone number. The phone number must be syntactically correct. For example, 4152345678.
WorkPhone | string | Optional | User’s phone number. The phone number must be syntactically correct. For example, 4152345678.
Fax | string | Optional | User’s phone number. The phone number must be syntactically correct. For example, 4152345678.
Email | string | Optional | User’s valid email address

### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>













## Update invoice customer

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class InvoiceCustomer
    {
        public static void UpdateInvoiceCustomer()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/InvoiceCustomers/33524364-727a-422b-8bbe-5eab1a849e82");
                request.ContentType = "text/json";
                request.Method = "PUT";

                var invoiceCustomer = new
                {
                    CompanyName = "Incutex",
                    FirstName = "John",
                    LastName = "Lock",
                    Address1 = "108 8th Av.",
                    Address2 = "8th Floor",
                    Zip = "10008",
                    City = "New York",
                    State = "NY",
                    PersonalPhone = "9727414574",
                    WorkPhone = "9177563046",
                    Fax = "8004578796",
                    Email = "john.lock@mailinator.com"
                };

                string json = JsonConvert.SerializeObject(invoiceCustomer);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
  "CompanyName" : "Incutex",
  "FirstName": "John",
  "LastName": "Lock",
  "Address1": "108 8th Av.",
  "Address2": "8th Floor",
  "Zip": "10008",
  "City": "New York",
  "State": "NY",
  "PersonalPhone" : "9727414574",
  "WorkPhone" : "9177563046",
  "Fax" : "8004578796",
  "Email": "john.lock@mailinator.com"
}
```

> Json Example Response:

```json
{
    "guid": "33524364-727a-422b-8bbe-5eab1a849e82",
    "merchantGuid": "24639b5f-f881-45dc-966c-4beece954e6c",
    "customerCode": "LJ7482",
    "companyName": "Incutex",
    "firstName": "John",
    "lastName": "Lock",
    "address1": "108 8th Av.",
    "address2": "8th Floor",
    "zip": "10008",
    "city": "New York",
    "state": "NY",
    "personalPhone": "9727414574",
    "workPhone": "9177563046",
    "fax": "8004578796",
    "email": "john.lock@mailinator.com",
    "invoiceNumber": "000001"
}
```

This endpoint updates a invoice customer.

### HTTP Request

`PUT https://sandbox.choice.dev/api/v1/InvoiceCustomers/<guid>`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
guid | Invoice customer’s guid to update

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
CompanyName | string | Optional | Company Name.
FirstName | string |  Optional | User’s first name.
LastName | string | Optional | User’s last name.
Address1 | string | Optional | User’s address.
Address2 | string | Optional | User’s address line 2.
City | string | Optional | User’s city.
State | string | Optional | User’s short name state. The ISO 3166-2 CA and US state or province code of a user. Length = 2.
Zip | string | Optional | User’s zipcode. Length = 5.
PersonalPhone | string | Optional | User’s phone number. The phone number must be syntactically correct. For example, 4152345678.
WorkPhone | string | Optional | User’s phone number. The phone number must be syntactically correct. For example, 4152345678.
Fax | string | Optional | User’s phone number. The phone number must be syntactically correct. For example, 4152345678.
Email | string | Optional | User’s valid email address

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>















## Get invoice customer

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class InvoiceCustomer
    {
        public static void GetInvoiceCustomer()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/InvoiceCustomers/33524364-727a-422b-8bbe-5eab1a849e82");
                request.Method = "GET";

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Response:

```json
{
    "guid": "33524364-727a-422b-8bbe-5eab1a849e82",
    "merchantGuid": "24639b5f-f881-45dc-966c-4beece954e6c",
    "customerCode": "LJ7482",
    "companyName": "Incutex",
    "firstName": "John",
    "lastName": "Lock",
    "address1": "108 8th Av.",
    "address2": "8th Floor",
    "zip": "10008",
    "city": "New York",
    "state": "NY",
    "personalPhone": "9727414574",
    "workPhone": "9177563046",
    "fax": "8004578796",
    "email": "john.lock@mailinator.com",
    "invoiceNumber": "000001"
}
```

This endpoint gets a invoice customer.

### HTTP Request

`GET https://sandbox.choice.dev/api/v1/InvoiceCustomers/<guid>`

### Headers using token

Key | Value
--------- | -------
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
guid | Invoice customer’s guid to get

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>












## Get invoice customer by Merchant

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class InvoiceCustomer
    {
        public static void GetInvoiceCustomerByMerchant()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Invoice/24639b5f-f881-45dc-966c-4beece954e6c/InvoiceCustomers");
                request.Method = "GET";

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Response:

```json
[
    {
        "guid": "c6697789-6b5d-4243-bb2c-91da084b6e4a",
        "merchantGuid": "24639b5f-f881-45dc-966c-4beece954e6c",
        "customerCode": "NA1530",
        "companyName": "Incutex",
        "firstName": "Max",
        "lastName": "Tomassi",
        "address1": "Belgrano 383",
        "zip": "10016",
        "city": "New York",
        "state": "NY",
        "country": "United States",
        "province": "",
        "personalPhone": "",
        "email": "maxduplessy@mailinator.com",
        "invoiceNumber": "000001",
        "shippingAddresses": [
            {
                "isDeleted": false,
                "guid": "e45e0464-39c8-4d87-b1da-7bef8b251f29",
                "address1": "Belgrano 383",
                "zip": "10016",
                "city": "New York",
                "state": "NY",
                "country": "United States",
                "province": ""
            }
        ]
    },
    {
        "guid": "e79ec8b0-dbd3-4be0-bf4e-8c12c632be7c",
        "merchantGuid": "24639b5f-f881-45dc-966c-4beece954e6c",
        "customerCode": "TD1627",
        "companyName": "Incutex",
        "firstName": "John",
        "lastName": "Lock",
        "address1": "108 8th Av.",
        "address2": "8th Floor",
        "zip": "10008",
        "city": "New York",
        "state": "NY",
        "personalPhone": "9727414574",
        "workPhone": "9177563046",
        "fax": "8004578796",
        "email": "john.lock@mailinator.com",
        "invoiceNumber": "000001"
    },
    {
        "guid": "33524364-727a-422b-8bbe-5eab1a849e82",
        "merchantGuid": "24639b5f-f881-45dc-966c-4beece954e6c",
        "customerCode": "LJ7482",
        "companyName": "Incutex",
        "firstName": "John",
        "lastName": "Lock",
        "address1": "108 8th Av.",
        "address2": "8th Floor",
        "zip": "10008",
        "city": "New York",
        "state": "NY",
        "personalPhone": "9727414574",
        "workPhone": "9177563046",
        "fax": "8004578796",
        "email": "john.lock@mailinator.com",
        "invoiceNumber": "000001"
    }
]
```

This endpoint gets a invoice customer.

### HTTP Request

`GET https://sandbox.choice.dev/api/v1/Invoice/<merchantGuid>/InvoiceCustomers`

### Headers using token

Key | Value
--------- | -------
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
merchantGuid | Merchant’s guid to get

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>










# Invoice

## Create invoice

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Invoice
    {
        public static void CreateInvoice()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Invoice");
                request.ContentType = "text/json";
                request.Method = "POST";

                var invoice = new
                {
                    MerchantGuid = "24639b5f-f881-45dc-966c-4beece954e6c",
                    InvoiceCustomerGuid = "c6697789-6b5d-4243-bb2c-91da084b6e4a",
                    InvoiceNumber = "000006",
                    OrderNumber = "000006",
                    PaymentTerm = "Due on Receipt",
                    DueDate = "11/26/2020",
                    Details = new Detail[]
                    {
                        new Detail{
                             ItemDescription = "Wine Malbec",
                             ItemName = "Wine Malbec",
                             Rate = 14.78,
                             Quantity = 8
                        },
                        new Detail{
                             ItemDescription = "Rum",
                             ItemName = "Rum",
                             Rate = 9.85,
                             Quantity = 4
                        },
                        new Detail{
                             ItemDescription = "Brandy",
                             ItemName = "Brandy",
                             Rate = 11.80,
                             Quantity = 2
                        },
                        new Detail{
                             ItemDescription = "Cigars",
                             ItemName = "Cigars",
                             Rate = 44.89,
                             Quantity = 3
                        }
                    },
                    DiscountType = "Fixed",
                    DiscountValue = 5.00,
                    TaxZip = "10016",
                    TaxAmount = 27.5932625,
                    TaxRate = 8.875,
                    Note = "For september services",
                    TermsAndConditions = "The ones you accepted when you clicked two pages ago",
                    SendStatus = "Scheduled To be Sent",
                    SendDate = "11/25/2020",
                    InvoiceRecipientEmail = "maxduplessy@mailinator.com",
                    OtherRecipients = "maxduplessy@mailinator.com, maxduplessy123@mailinator.com",
                    SendBySMS = true,
                    SendToPhoneNumber = "9174355176",
                    SendToOthersNumber ="6384910738, 9174607489"
                };

                string json = JsonConvert.SerializeObject(invoice);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
    "merchantGuid": "24639b5f-f881-45dc-966c-4beece954e6c",
    "invoiceCustomerGuid": "c6697789-6b5d-4243-bb2c-91da084b6e4a",
    "invoiceNumber": "000006",
    "orderNumber": "000006",
    "paymentTerm": "Due on Receipt",
    "dueDate": "11/26/2020",
    "details": [
        {
            "itemName": "Wine Malbec",
            "itemDescription": "",
            "quantity": 8,
            "rate": "14.78"
        },
        {
            "itemName": "Rum",
            "itemDescription": "",
            "quantity": 4,
            "rate": "9.85"
        },
        {
            "itemName": "Brandy",
            "itemDescription": "",
            "quantity": 2,
            "rate": "11.80"
        },
        {
            "itemName": "Cigars",
            "itemDescription": "",
            "quantity": 3,
            "rate": "44.89"
        }
    ],
    "discountType": "Fixed",
    "discountValue": "5",
    "taxZip": "10016",
    "taxRate": 8.875,
    "taxAmount": 27.5932625,
    "note": "For september services",
    "termsAndConditions": "The ones you accepted when you clicked two pages ago",
    "sendStatus": "Scheduled To be Sent",
    "sendDate": "11/25/2020",
    "invoiceRecipientEmail": "maxduplessy@mailinator.com",
    "sendToPhoneNumber": "9887555656",
    "sendBySMS": true
}
```

> Json Example Response:

```json
{
    "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
    "timeStamp": "2020-11-25T09:54:46.21-06:00",
    "merchantGuid": "24639b5f-f881-45dc-966c-4beece954e6c",
    "invoiceCustomerGuid": "c6697789-6b5d-4243-bb2c-91da084b6e4a",
    "sendDate": "2020-11-25T00:00:00",
    "sendStatus": "Scheduled To be Sent",
    "paymentStatus": "Scheduled To be Sent",
    "invoiceRecipientEmail": "maxduplessy@mailinator.com",
    "paymentTerm": "Due on Receipt",
    "dueDate": "2020-11-25T09:54:46.2",
    "invoiceNumber": "000006",
    "orderNumber": "000006",
    "amountSubTotal": 315.91,
    "amountDueTotal": 338.50,
    "remainingBalance": 0.0,
    "amountDiscounted": 5.00,
    "discountValue": 5.00,
    "discountType": "Fixed",
    "taxRate": 8.87,
    "taxAmount": 27.59,
    "taxZip": "10016",
    "note": "For september services",
    "termsAndConditions": "The ones you accepted when you clicked two pages ago",
    "extensionHostedPaymentPageRequestToken": "00000000-0000-0000-0000-000000000000",
    "details": [
        {
            "guid": "f2ce15be-a6b7-4106-8660-ffd88e4219bc",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "itemName": "Wine Malbec",
            "itemDescription": "",
            "rate": 14.78,
            "quantity": 8,
            "amount": 118.24,
            "isDeleted": false,
            "discountedAmount": 0.00,
            "finallyAmount": 118.24
        },
        {
            "guid": "c9d70ca3-a571-4f5b-bb86-d52b9e28c298",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "itemName": "Rum",
            "itemDescription": "",
            "rate": 9.85,
            "quantity": 4,
            "amount": 39.40,
            "isDeleted": false,
            "discountedAmount": 0.00,
            "finallyAmount": 39.40
        },
        {
            "guid": "23074f19-969f-48d0-b2d4-99ffd2884001",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "itemName": "Brandy",
            "itemDescription": "",
            "rate": 11.80,
            "quantity": 2,
            "amount": 23.60,
            "isDeleted": false,
            "discountedAmount": 0.00,
            "finallyAmount": 23.60
        },
        {
            "guid": "0ac11bf0-1e81-4059-bb79-41e46ecbf4c1",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "itemName": "Cigars",
            "itemDescription": "",
            "rate": 44.89,
            "quantity": 3,
            "amount": 134.67,
            "isDeleted": false,
            "discountedAmount": 0.00,
            "finallyAmount": 134.67
        }
    ],
    "reminders": [
        {
            "guid": "21b7cce9-8940-47d0-b2f9-1d5c3cbe194c",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "reminderType": "5 days after due date",
            "isActive": true,
            "isCompleted": false,
            "reminderDate": "2020-11-30T09:54:46.2"
        }
    ],
    "merchant": {
        "guid": "24639b5f-f881-45dc-966c-4beece954e6c",
        "mid": "888000002849",
        "allowOpenButton": false,
        "recurringBillingSettings": [
            {
                "numReAttempt": 1,
                "nameDayOfWeek": "Friday"
            },
            {
                "numReAttempt": 2,
                "nameDayOfWeek": "Friday"
            },
            {
                "numReAttempt": 3,
                "nameDayOfWeek": "Friday"
            }
        ],
        "dba": "326Nothing",
        "legalName": "326Nothing",
        "adminUserGuid": "fad491cb-ac86-4250-ab34-ac8be77ee659",
        "email": "326Nothing@mailinator.com",
        "phone": "5656566886",
        "address1": "Belgrano 383",
        "city": "New York",
        "state": "NY",
        "zipcode": "10016",
        "logoUrl": "https://res.cloudinary.com/choice/image/upload/v1516126617/uxuxkm3odb6lltibyeyj.png",
        "defaultAllowsSurchargeOtherDevice": false,
        "reAttemptRB": true,
        "logoName": "mariano.png",
        "status": "Merchant - Active",
        "merchantOwners": [
            {
                "guid": "04ba4b19-e671-4c53-9951-1647b8d2fffa",
                "firstName": "max",
                "lastName": "tomassi",
                "email": "Alpinstar@mailinator.com",
                "phone": "5454354455",
                "address1": "Belgrano 383",
                "city": "new york",
                "state": "NY",
                "zipcode": "10016",
                "country": "US",
                "status": "MerchantOwner - Active"
            }
        ]
    },
    "invoiceCustomer": {
        "guid": "c6697789-6b5d-4243-bb2c-91da084b6e4a",
        "merchantGuid": "00000000-0000-0000-0000-000000000000",
        "customerCode": "NA1530",
        "companyName": "Incutex",
        "firstName": "Max",
        "lastName": "Tomassi",
        "address1": "Belgrano 383",
        "zip": "10016",
        "city": "New York",
        "state": "NY",
        "country": "United States",
        "province": "",
        "personalPhone": "",
        "email": "maxduplessy@mailinator.com",
        "invoiceNumber": "000001"
    },
    "sendBySMS": true,
    "sendToPhoneNumber": "9887555656",
    "recurringBilling": []
}
```

This endpoint creates a invoice.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Invoice`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
MerchantGuid | string | Mandatory | Merchant's Guid.
InvoiceCustomerGuid | string | Mandatory | InvoiceCustomer's Guid.
InvoiceNumber | string |  Mandatory | Invoice Number.
OrderNumber | string |  Optional | Order number. Length = 30.
PaymentTerm | string | Optional | The term of payments.<br><br>Allowed values:<br><br>**1. Custom**<br>**2. Due on Receipt** <br>**3. Due end of month** <br>**4. Due end of next month** <br>**5. Net 15** <br>**6. Net 30** <br>**7. Net 45** <br>**8. Net 60**
DueDate | date | Optional | Due Date.<br><br>Allowed format:<br><br>MM-DD-YYYY.<br>For example: 05-30-2018.
DueDateSpecial | date | Optional | Due Date for today.<br><br>Allowed format:<br><br>MM-DD-YYYY.<br>For example: 05-30-2018.
DiscountType | string | Optional | Discount Type.<br><br>Allowed values:<br><br>**1. Fixed**<br>**2. Percentage**
DiscountValue | decimal | Optional | Discount Value.
TaxZip | string | Optional | Tax Zip.
TaxAmount | decimal | Optional | Tax Amount.
TaxRate | decimal | Optional | Tax Rate.
Note | string | Optional | Note.
TermsAndConditions | string | Optional | Terms And Conditions.
SendStatus | string | Optional | Send Status.<br><br>Allowed values:<br><br>**1. Draft**<br>**2. Scheduled To be Sent** <br>**3. Scheduled To be SentEMAIL** <br>**4. Scheduled To be SentSMS**
SendDate | date | Optional | Send Date.<br><br>Allowed format:<br><br>MM-DD-YYYY.<br>For example: 05-30-2018.
InvoiceRecipientEmail | string | Optional | Valid email address.
SendBySMS | boolean | Optional | True or False.
SendToPhoneNumber | string | Optional | Phone number. The phone number must be syntactically correct. For example, 4152345678.
Details | object[] | Mandatory | Details.
 |  | 
**Details** | array of detail | 
ItemDescription | string | Optional | Item Description.
ItemName | string | Optional | Item Name.
Rate | decimal | Optional | Rate.
Quantity | integer | Optional | Quantity.

### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>













## Update invoice

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Invoice
    {
        public static void UpdateInvoice()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Invoice/2541692d-958f-46b7-8ca7-a127016ea3b6");
                request.ContentType = "text/json";
                request.Method = "PUT";

                var invoice = new
                {
                    OrderNumber = "000006",
                    PaymentTerm = "Due on Receipt",
                    DueDate = "11/26/2020",
                    DiscountType = "Fixed",
                    DiscountValue = "5",
                    TaxZip = "10016",
                    TaxRate = 8.875,
                    TaxAmount = 27.5932625,
                    Note = "For september services",
                    TermsAndConditions = "The ones you accepted when you clicked two pages ago",
                    SendStatus = "Scheduled To be Sent",
                    SendDate = "11/25/2020",
                    InvoiceRecipientEmail = "maxduplessy@mailinator.com",
                    SendToPhoneNumber = "9887555656",
                    SendBySMS = true
                };

                string json = JsonConvert.SerializeObject(invoice);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
    "OrderNumber": "000006",
    "PaymentTerm": "Due on Receipt",
    "DueDate": "11/26/2020",
    "DiscountType": "Fixed",
    "DiscountValue": "5",
    "TaxZip": "10016",
    "TaxRate": 8.875,
    "TaxAmount": 27.5932625,
    "Note": "For september services",
    "TermsAndConditions": "The ones you accepted when you clicked two pages ago",
    "SendStatus": "Scheduled To be Sent",
    "SendDate": "11/25/2020",
    "InvoiceRecipientEmail": "maxduplessy@mailinator.com",
    "SendToPhoneNumber": "9887555656",
    "SendBySMS": true
}
```

> Json Example Response:

```json
{
    "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
    "timeStamp": "2020-11-25T09:54:46.21-06:00",
    "merchantGuid": "24639b5f-f881-45dc-966c-4beece954e6c",
    "invoiceCustomerGuid": "c6697789-6b5d-4243-bb2c-91da084b6e4a",
    "sendDate": "2020-11-25T00:00:00",
    "sendStatus": "Scheduled To be Sent",
    "paymentStatus": "Unpaid",
    "invoiceRecipientEmail": "maxduplessy@mailinator.com",
    "paymentTerm": "Due on Receipt",
    "dueDate": "2020-11-25T13:21:30.91",
    "invoiceNumber": "000006",
    "orderNumber": "000006",
    "amountSubTotal": 315.91,
    "amountDueTotal": 338.49,
    "remainingBalance": 0.0,
    "amountDiscounted": 5.00,
    "discountValue": 5.00,
    "discountType": "Fixed",
    "taxRate": 8.87,
    "taxAmount": 27.59,
    "taxZip": "10016",
    "note": "For september services",
    "termsAndConditions": "The ones you accepted when you clicked two pages ago",
    "extensionHostedPaymentPageRequestToken": "00000000-0000-0000-0000-000000000000",
    "details": [
        {
            "guid": "f2ce15be-a6b7-4106-8660-ffd88e4219bc",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "itemName": "Wine Malbec",
            "itemDescription": "",
            "rate": 14.78,
            "quantity": 8,
            "amount": 118.24,
            "isDeleted": false,
            "discountedAmount": 0.00,
            "finallyAmount": 118.24
        },
        {
            "guid": "c9d70ca3-a571-4f5b-bb86-d52b9e28c298",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "itemName": "Rum",
            "itemDescription": "",
            "rate": 9.85,
            "quantity": 4,
            "amount": 39.40,
            "isDeleted": false,
            "discountedAmount": 0.00,
            "finallyAmount": 39.40
        },
        {
            "guid": "23074f19-969f-48d0-b2d4-99ffd2884001",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "itemName": "Brandy",
            "itemDescription": "",
            "rate": 11.80,
            "quantity": 2,
            "amount": 23.60,
            "isDeleted": false,
            "discountedAmount": 0.00,
            "finallyAmount": 23.60
        },
        {
            "guid": "0ac11bf0-1e81-4059-bb79-41e46ecbf4c1",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "itemName": "Cigars",
            "itemDescription": "",
            "rate": 44.89,
            "quantity": 3,
            "amount": 134.67,
            "isDeleted": false,
            "discountedAmount": 0.00,
            "finallyAmount": 134.67
        }
    ],
    "reminders": [
        {
            "guid": "21b7cce9-8940-47d0-b2f9-1d5c3cbe194c",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "reminderType": "5 days after due date",
            "isActive": true,
            "isCompleted": false,
            "reminderDate": "2020-11-30T13:21:30.91"
        }
    ],
    "merchant": {
        "guid": "24639b5f-f881-45dc-966c-4beece954e6c",
        "mid": "888000002849",
        "allowOpenButton": false,
        "recurringBillingSettings": [
            {
                "numReAttempt": 1,
                "nameDayOfWeek": "Friday"
            },
            {
                "numReAttempt": 2,
                "nameDayOfWeek": "Friday"
            },
            {
                "numReAttempt": 3,
                "nameDayOfWeek": "Friday"
            }
        ],
        "dba": "326Nothing",
        "legalName": "326Nothing",
        "adminUserGuid": "fad491cb-ac86-4250-ab34-ac8be77ee659",
        "email": "326Nothing@mailinator.com",
        "phone": "5656566886",
        "address1": "Belgrano 383",
        "city": "New York",
        "state": "NY",
        "zipcode": "10016",
        "logoUrl": "https://paybugstagestorage.blob.core.windows.net/images/C:%5Cinetpub%5CPayBugStage%5CApp_Data%5CBodyPart_4504fad2-18b5-48dc-9e91-20f81647485e",
        "defaultAllowsSurchargeOtherDevice": false,
        "reAttemptRB": true,
        "logoName": "mariano.png",
        "status": "Merchant - Active",
        "merchantOwners": [
            {
                "guid": "04ba4b19-e671-4c53-9951-1647b8d2fffa",
                "firstName": "max",
                "lastName": "tomassi",
                "email": "Alpinstar@mailinator.com",
                "phone": "5454354455",
                "address1": "Belgrano 383",
                "city": "new york",
                "state": "NY",
                "zipcode": "10016",
                "country": "US",
                "status": "MerchantOwner - Active"
            }
        ]
    },
    "invoiceCustomer": {
        "guid": "c6697789-6b5d-4243-bb2c-91da084b6e4a",
        "merchantGuid": "00000000-0000-0000-0000-000000000000",
        "customerCode": "NA1530",
        "companyName": "Incutex",
        "firstName": "Max",
        "lastName": "Tomassi",
        "address1": "Belgrano 383",
        "zip": "10016",
        "city": "New York",
        "state": "NY",
        "country": "United States",
        "province": "",
        "personalPhone": "",
        "email": "maxduplessy@mailinator.com",
        "invoiceNumber": "000001"
    },
    "sendBySMS": true,
    "sendToPhoneNumber": "9887555656",
    "recurringBilling": []
}
```

This endpoint updates a invoice.

### HTTP Request

`PUT https://sandbox.choice.dev/api/v1/Invoice/<guid>`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
guid | Invoice’s guid to update

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
OrderNumber | string |  Optional | Order number. Length = 30.
PaymentTerm | string | Optional | The term of payments.<br><br>Allowed values:<br><br>**1. Custom**<br>**2. Due on Receipt** <br>**3. Due end of month** <br>**4. Due end of next month** <br>**5. Net 15** <br>**6. Net 30** <br>**7. Net 45** <br>**8. Net 60**
DueDate | date | Optional | Due Date.<br><br>Allowed format:<br><br>MM-DD-YYYY.<br>For example: 05-30-2018.
DiscountType | string | Optional | Discount Type.<br><br>Allowed values:<br><br>**1. Fixed**<br>**2. Percentage**
DiscountValue | decimal | Optional | Discount Value.
TaxZip | string | Optional | Tax Zip.
TaxAmount | decimal | Optional | Tax Amount.
TaxRate | decimal | Optional | Tax Rate.
Note | string | Optional | Note.
TermsAndConditions | string | Optional | Terms And Conditions.
SendStatus | string | Optional | Send Status.<br><br>Allowed values:<br><br>**1. Draft**<br>**2. Scheduled To be Sent** <br>**3. Scheduled To be SentEMAIL** <br>**4. Scheduled To be SentSMS**
SendDate | date | Optional | Send Date.<br><br>Allowed format:<br><br>MM-DD-YYYY.<br>For example: 05-30-2018.
InvoiceRecipientEmail | string | Optional | Valid email address.
SendBySMS | boolean | Optional | True or False.
SendToPhoneNumber | string | Optional | Phone number. The phone number must be syntactically correct. For example, 4152345678.


### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>















## Get invoice

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Invoice
    {
        public static void GetInvoice()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Invoice/2541692d-958f-46b7-8ca7-a127016ea3b6");
                request.Method = "GET";

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Response:

```json
{
    "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
    "timeStamp": "2020-11-25T09:54:46.21-06:00",
    "merchantGuid": "24639b5f-f881-45dc-966c-4beece954e6c",
    "invoiceCustomerGuid": "c6697789-6b5d-4243-bb2c-91da084b6e4a",
    "sendDate": "2020-11-25T00:00:00",
    "sendStatus": "Scheduled To be Sent",
    "paymentStatus": "Unpaid",
    "invoiceRecipientEmail": "maxduplessy@mailinator.com",
    "paymentTerm": "Due on Receipt",
    "dueDate": "2020-11-25T13:21:30.91",
    "invoiceNumber": "000006",
    "orderNumber": "000006",
    "amountSubTotal": 315.91,
    "amountDueTotal": 338.49,
    "remainingBalance": 0.0,
    "amountDiscounted": 5.00,
    "discountValue": 5.00,
    "discountType": "Fixed",
    "taxRate": 8.87,
    "taxAmount": 27.59,
    "taxZip": "10016",
    "note": "For september services",
    "termsAndConditions": "The ones you accepted when you clicked two pages ago",
    "extensionHostedPaymentPageRequestToken": "e0b2ac6e-994c-4e65-888a-3cb3d1b7f974",
    "extensionDisplayCreditCard": true,
    "extensionDisplayAch": true,
    "details": [
        {
            "guid": "f2ce15be-a6b7-4106-8660-ffd88e4219bc",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "itemName": "Wine Malbec",
            "itemDescription": "",
            "rate": 14.78,
            "quantity": 8,
            "amount": 118.24,
            "isDeleted": false,
            "discountedAmount": 0.00,
            "finallyAmount": 118.24
        },
        {
            "guid": "c9d70ca3-a571-4f5b-bb86-d52b9e28c298",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "itemName": "Rum",
            "itemDescription": "",
            "rate": 9.85,
            "quantity": 4,
            "amount": 39.40,
            "isDeleted": false,
            "discountedAmount": 0.00,
            "finallyAmount": 39.40
        },
        {
            "guid": "23074f19-969f-48d0-b2d4-99ffd2884001",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "itemName": "Brandy",
            "itemDescription": "",
            "rate": 11.80,
            "quantity": 2,
            "amount": 23.60,
            "isDeleted": false,
            "discountedAmount": 0.00,
            "finallyAmount": 23.60
        },
        {
            "guid": "0ac11bf0-1e81-4059-bb79-41e46ecbf4c1",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "itemName": "Cigars",
            "itemDescription": "",
            "rate": 44.89,
            "quantity": 3,
            "amount": 134.67,
            "isDeleted": false,
            "discountedAmount": 0.00,
            "finallyAmount": 134.67
        }
    ],
    "reminders": [
        {
            "guid": "21b7cce9-8940-47d0-b2f9-1d5c3cbe194c",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "reminderType": "5 days after due date",
            "isActive": true,
            "isCompleted": false,
            "reminderDate": "2020-11-30T13:21:30.91"
        }
    ],
    "merchant": {
        "guid": "24639b5f-f881-45dc-966c-4beece954e6c",
        "mid": "888000002849",
        "allowOpenButton": false,
        "recurringBillingSettings": [
            {
                "numReAttempt": 1,
                "nameDayOfWeek": "Friday"
            },
            {
                "numReAttempt": 2,
                "nameDayOfWeek": "Friday"
            },
            {
                "numReAttempt": 3,
                "nameDayOfWeek": "Friday"
            }
        ],
        "dba": "326Nothing",
        "legalName": "326Nothing",
        "adminUserGuid": "fad491cb-ac86-4250-ab34-ac8be77ee659",
        "email": "326Nothing@mailinator.com",
        "phone": "5656566886",
        "address1": "Belgrano 383",
        "city": "New York",
        "state": "NY",
        "zipcode": "10016",
        "logoUrl": "https://paybugstagestorage.blob.core.windows.net/images/C:%5Cinetpub%5CPayBugStage%5CApp_Data%5CBodyPart_4504fad2-18b5-48dc-9e91-20f81647485e",
        "defaultAllowsSurchargeOtherDevice": false,
        "reAttemptRB": true,
        "logoName": "mariano.png",
        "status": "Merchant - Active",
        "merchantOwners": [
            {
                "guid": "04ba4b19-e671-4c53-9951-1647b8d2fffa",
                "firstName": "max",
                "lastName": "tomassi",
                "email": "Alpinstar@mailinator.com",
                "phone": "5454354455",
                "address1": "Belgrano 383",
                "city": "new york",
                "state": "NY",
                "zipcode": "10016",
                "country": "US",
                "status": "MerchantOwner - Active"
            }
        ]
    },
    "invoiceCustomer": {
        "guid": "c6697789-6b5d-4243-bb2c-91da084b6e4a",
        "merchantGuid": "00000000-0000-0000-0000-000000000000",
        "customerCode": "NA1530",
        "companyName": "Incutex",
        "firstName": "Max",
        "lastName": "Tomassi",
        "address1": "Belgrano 383",
        "zip": "10016",
        "city": "New York",
        "state": "NY",
        "country": "United States",
        "province": "",
        "personalPhone": "",
        "email": "maxduplessy@mailinator.com",
        "invoiceNumber": "000001"
    },
    "sendBySMS": true,
    "sendToPhoneNumber": "9887555656",
    "recurringBilling": []
}
```

This endpoint gets a invoice.

### HTTP Request

`GET https://sandbox.choice.dev/api/v1/Invoice/<guid>`

### Headers using token

Key | Value
--------- | -------
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
guid | Invoice’s guid to get

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

























































# Invoice Detail

## Add invoice detail

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class InvoiceDetail
    {
        public static void CreateInvoiceDetail()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Invoice/Detail");
                request.ContentType = "text/json";
                request.Method = "POST";

                var invoiceDetail = new
                {
                    InvoiceGuid = "4be52be2-bedf-4125-b7d5-2ed9ef8d6027",
                    ItemDescription = "Sparkling Wine",
                    Rate = 1.95,
                    Quantity = 3
                };

                string json = JsonConvert.SerializeObject(invoiceDetail);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
	"invoiceGuid" : "2541692d-958f-46b7-8ca7-a127016ea3b6",
	"ItemDescription":"Sparkling Wine",
    "ItemName": "Sparkling Wine",
	"Rate":1.95,
	"Quantity": 3
}
```

> Json Example Response:

```json
{
    "guid": "965b4d54-d35d-414d-b0c9-991c6ca013e7",
    "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
    "itemName": "Sparkling Wine",
    "itemDescription": "Sparkling Wine",
    "rate": 1.95,
    "quantity": 3,
    "amount": 5.85,
    "isDeleted": false,
    "discounType": null,
    "discountedAmount": 0.00,
    "finallyAmount": 5.85
}
```

This endpoint add a invoice detail.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Invoice/Detail`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
InvoiceGuid | string | Mandatory | Invoice's Guid.
ItemDescription | string | Mandatory | Item Description.
ItemName | string | Optional | Item Name.
Rate | decimal |  Mandatory | Rate.
Quantity | integer |  Mandatory | Quantity.


### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>









































## Create invoice details list

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class InvoiceDetail
    {
        public static void CreateInvoiceDetailsList()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Invoice/Detail/all/2541692d-958f-46b7-8ca7-a127016ea3b6");
                request.ContentType = "text/json";
                request.Method = "PUT";

                var invoiceDetailsList = new Detail[]
                {
                    new Detail{
                         ItemDescription = "Red Wine",
                         ItemName = "Red Wine",
                         Rate = 14.78,
                         Quantity = 8
                    },
                    new Detail{
                         ItemDescription = "Purple Wine",
                         ItemName = "Purple Wine",
                         Rate = 9.85,
                         Quantity = 4
                    }
                };

                string json = JsonConvert.SerializeObject(invoiceDetailsList);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
[
    {
        "ItemDescription": "Red Wine",
        "ItemName": "Red Wine",
        "Rate": 1.95,
        "Quantity": 3
    },
    {
        "ItemDescription": "Purple Wine",
        "ItemName": "Purple Wine",
        "Rate": 7.55,
        "Quantity": 1
    }
]
```

> Json Example Response:

```json
{
    "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
    "timeStamp": "2020-11-25T09:54:46.21-06:00",
    "merchantGuid": "24639b5f-f881-45dc-966c-4beece954e6c",
    "invoiceCustomerGuid": "c6697789-6b5d-4243-bb2c-91da084b6e4a",
    "sendDate": "2020-11-25T00:00:00",
    "sendStatus": "Scheduled To be Sent",
    "paymentStatus": "Scheduled To be Sent",
    "invoiceRecipientEmail": "maxduplessy@mailinator.com",
    "paymentTerm": "Due on Receipt",
    "dueDate": "2020-11-25T09:54:46.2",
    "invoiceNumber": "000006",
    "orderNumber": "000006",
    "amountSubTotal": 315.91,
    "amountDueTotal": 338.50,
    "remainingBalance": 0.0,
    "amountDiscounted": 5.00,
    "discountValue": 5.00,
    "discountType": "Fixed",
    "taxRate": 8.87,
    "taxAmount": 27.59,
    "taxZip": "10016",
    "note": "For september services",
    "termsAndConditions": "The ones you accepted when you clicked two pages ago",
    "extensionHostedPaymentPageRequestToken": "00000000-0000-0000-0000-000000000000",
    "details": [
        {
            "guid": "23074f19-969f-48d0-b2d4-99ffd2884001",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "itemName": "Brandy",
            "itemDescription": "",
            "rate": 11.80,
            "quantity": 2,
            "amount": 23.60,
            "isDeleted": false,
            "discountedAmount": 0.00,
            "finallyAmount": 23.60
        },
        {
            "guid": "0ac11bf0-1e81-4059-bb79-41e46ecbf4c1",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "itemName": "Cigars",
            "itemDescription": "",
            "rate": 44.89,
            "quantity": 3,
            "amount": 134.67,
            "isDeleted": false,
            "discountedAmount": 0.00,
            "finallyAmount": 134.67
        },
                {
            "guid": "f2ce15be-a6b7-4106-8660-ffd88e4219bc",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "itemName": "Red Wine",
            "itemDescription": "",
            "rate": 14.78,
            "quantity": 8,
            "amount": 118.24,
            "isDeleted": false,
            "discountedAmount": 0.00,
            "finallyAmount": 118.24
        },
        {
            "guid": "c9d70ca3-a571-4f5b-bb86-d52b9e28c298",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "itemName": "Purple Wine",
            "itemDescription": "",
            "rate": 9.85,
            "quantity": 4,
            "amount": 39.40,
            "isDeleted": false,
            "discountedAmount": 0.00,
            "finallyAmount": 39.40
        }
    ],
    "reminders": [
        {
            "guid": "21b7cce9-8940-47d0-b2f9-1d5c3cbe194c",
            "invoiceGuid": "2541692d-958f-46b7-8ca7-a127016ea3b6",
            "reminderType": "5 days after due date",
            "isActive": true,
            "isCompleted": false,
            "reminderDate": "2020-11-30T09:54:46.2"
        }
    ],
    "merchant": {
        "guid": "24639b5f-f881-45dc-966c-4beece954e6c",
        "mid": "888000002849",
        "allowOpenButton": false,
        "recurringBillingSettings": [
            {
                "numReAttempt": 1,
                "nameDayOfWeek": "Friday"
            },
            {
                "numReAttempt": 2,
                "nameDayOfWeek": "Friday"
            },
            {
                "numReAttempt": 3,
                "nameDayOfWeek": "Friday"
            }
        ],
        "dba": "326Nothing",
        "legalName": "326Nothing",
        "adminUserGuid": "fad491cb-ac86-4250-ab34-ac8be77ee659",
        "email": "326Nothing@mailinator.com",
        "phone": "5656566886",
        "address1": "Belgrano 383",
        "city": "New York",
        "state": "NY",
        "zipcode": "10016",
        "logoUrl": "https://res.cloudinary.com/choice/image/upload/v1516126617/uxuxkm3odb6lltibyeyj.png",
        "defaultAllowsSurchargeOtherDevice": false,
        "reAttemptRB": true,
        "logoName": "mariano.png",
        "status": "Merchant - Active",
        "merchantOwners": [
            {
                "guid": "04ba4b19-e671-4c53-9951-1647b8d2fffa",
                "firstName": "max",
                "lastName": "tomassi",
                "email": "Alpinstar@mailinator.com",
                "phone": "5454354455",
                "address1": "Belgrano 383",
                "city": "new york",
                "state": "NY",
                "zipcode": "10016",
                "country": "US",
                "status": "MerchantOwner - Active"
            }
        ]
    },
    "invoiceCustomer": {
        "guid": "c6697789-6b5d-4243-bb2c-91da084b6e4a",
        "merchantGuid": "00000000-0000-0000-0000-000000000000",
        "customerCode": "NA1530",
        "companyName": "Incutex",
        "firstName": "Max",
        "lastName": "Tomassi",
        "address1": "Belgrano 383",
        "zip": "10016",
        "city": "New York",
        "state": "NY",
        "country": "United States",
        "province": "",
        "personalPhone": "",
        "email": "maxduplessy@mailinator.com",
        "invoiceNumber": "000001"
    },
    "sendBySMS": true,
    "sendToPhoneNumber": "9887555656",
    "recurringBilling": []
}
```

This endpoint create a invoice details list.

### HTTP Request

`PUT https://sandbox.choice.dev/api/v1/Invoice/Detail/all/<guid>`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
guid | Invoice’s guid



### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
ItemDescription | string | Mandatory | Item Description.
Rate | decimal |  Mandatory | Rate.
Quantity | integer |  Mandatory | Quantity.


### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>























































# Invoice Reminder

## Create invoice reminder

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class InvoiceReminder
    {
        public static void CreateInvoiceReminder()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Invoice/Reminder");
                request.ContentType = "text/json";
                request.Method = "POST";

                var invoiceReminder = new
                {
                    InvoiceGuid = "d5828cfd-12a6-4a7e-9be6-7c4b11f07dbf",
                    ReminderType = "2 weeks after due date"
                };

                string json = JsonConvert.SerializeObject(invoiceReminder);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
    "InvoiceGuid" : "d5828cfd-12a6-4a7e-9be6-7c4b11f07dbf",
    "ReminderType" : "2 weeks after due date"
}
```

> Json Example Response:

```json
{
    "guid": "a4e4278b-f2de-41d9-8da5-d0aabbf5a385",
    "invoiceGuid": "d5828cfd-12a6-4a7e-9be6-7c4b11f07dbf",
    "reminderType": "2 weeks after due date",
    "isActive": true,
    "isCompleted": false,
    "reminderDate": "2019-08-08T00:00:00"
}
```

This endpoint creates a invoice reminder.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Invoice/Reminder`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
InvoiceGuid | string | Mandatory | Invoice's Guid.
ReminderType | string | Mandatory | Reminder Type.<br><br>Allowed values:<br><br>**1. X days before due date**<br>**2. 1 days before due date** <br>**3. 5 days before due date** <br>**4. 1 week before due date** <br>**5. 10 days before due date** <br>**6. 2 week before due date** <br><br>**7. X days after due date**<br>**8. 1 days after due date** <br>**9. 5 days after due date** <br>**10. 1 week after due date** <br>**11. 10 days after due date** <br>**12. 2 week after due date**
ReminderDaysValue | byte | Optional | Reminder Days Value. Only with "X days before due date" or "X days after due date"


### Response

* 201 code (created).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>













## Update invoice reminder

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class InvoiceReminder
    {
        public static void UpdateInvoiceReminder()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Invoice/Reminder/a4e4278b-f2de-41d9-8da5-d0aabbf5a385/true");
                request.ContentType = "text/json";
                request.Method = "PUT";

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Response (PUT https://sandbox.choice.dev/api/v1/Invoice/Reminder/<guid>/true):

```json
{
    "guid": "a4e4278b-f2de-41d9-8da5-d0aabbf5a385",
    "invoiceGuid": "d5828cfd-12a6-4a7e-9be6-7c4b11f07dbf",
    "reminderType": "2 weeks after due date",
    "isActive": true,
    "isCompleted": false,
    "reminderDate": "2019-08-08T00:00:00"
}
```

> Json Example Response (PUT https://sandbox.choice.dev/api/v1/Invoice/Reminder/<guid>/false):

```json
{
    "guid": "a4e4278b-f2de-41d9-8da5-d0aabbf5a385",
    "invoiceGuid": "d5828cfd-12a6-4a7e-9be6-7c4b11f07dbf",
    "reminderType": "2 weeks after due date",
    "isActive": false,
    "isCompleted": false,
    "reminderDate": "2019-08-08T00:00:00"
}
```

This endpoint updates a invoice reminder.

### HTTP Request

`PUT https://sandbox.choice.dev/api/v1/Invoice/Reminder/<guid>/true`

    or

`PUT https://sandbox.choice.dev/api/v1/Invoice/Reminder/<guid>/false`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
guid | Reminder’s guid to update


### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>















## Get invoice reminder

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class InvoiceReminder
    {
        public static void GetInvoiceReminder()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Invoice/Reminder/d5828cfd-12a6-4a7e-9be6-7c4b11f07dbf/true");
                request.Method = "GET";

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Response (GET https://sandbox.choice.dev/api/v1/Invoice/Reminder/<guid>/true):

```json
[
    {
        "guid": "00000000-0000-0000-0000-000000000000",
        "invoiceGuid": "d5828cfd-12a6-4a7e-9be6-7c4b11f07dbf",
        "reminderType": "1 day before due date",
        "isActive": false,
        "isCompleted": false,
        "reminderDate": "0001-01-01T00:00:00"
    },
    {
        "guid": "fbb2633f-0bf4-412b-a51e-edad3fb4b058",
        "invoiceGuid": "d5828cfd-12a6-4a7e-9be6-7c4b11f07dbf",
        "reminderType": "5 days before due date",
        "isActive": true,
        "isCompleted": false,
        "reminderDate": "2019-07-20T00:00:00"
    },
    {
        "guid": "00000000-0000-0000-0000-000000000000",
        "invoiceGuid": "d5828cfd-12a6-4a7e-9be6-7c4b11f07dbf",
        "reminderType": "1 week before due date",
        "isActive": false,
        "isCompleted": false,
        "reminderDate": "0001-01-01T00:00:00"
    },
    {
        "guid": "00000000-0000-0000-0000-000000000000",
        "invoiceGuid": "d5828cfd-12a6-4a7e-9be6-7c4b11f07dbf",
        "reminderType": "10 days before due date",
        "isActive": false,
        "isCompleted": false,
        "reminderDate": "0001-01-01T00:00:00"
    },
    {
        "guid": "00000000-0000-0000-0000-000000000000",
        "invoiceGuid": "d5828cfd-12a6-4a7e-9be6-7c4b11f07dbf",
        "reminderType": "2 weeks before due date",
        "isActive": false,
        "isCompleted": false,
        "reminderDate": "0001-01-01T00:00:00"
    },
    {
        "guid": "00000000-0000-0000-0000-000000000000",
        "invoiceGuid": "d5828cfd-12a6-4a7e-9be6-7c4b11f07dbf",
        "reminderType": "1 day after due date",
        "isActive": false,
        "isCompleted": false,
        "reminderDate": "0001-01-01T00:00:00"
    },
    {
        "guid": "cf7f8d0a-7540-43de-b637-09f7dec0a273",
        "invoiceGuid": "d5828cfd-12a6-4a7e-9be6-7c4b11f07dbf",
        "reminderType": "5 days after due date",
        "isActive": true,
        "isCompleted": false,
        "reminderDate": "2019-07-30T00:00:00"
    },
    {
        "guid": "00000000-0000-0000-0000-000000000000",
        "invoiceGuid": "d5828cfd-12a6-4a7e-9be6-7c4b11f07dbf",
        "reminderType": "1 week after due date",
        "isActive": false,
        "isCompleted": false,
        "reminderDate": "0001-01-01T00:00:00"
    },
    {
        "guid": "00000000-0000-0000-0000-000000000000",
        "invoiceGuid": "d5828cfd-12a6-4a7e-9be6-7c4b11f07dbf",
        "reminderType": "10 days after due date",
        "isActive": false,
        "isCompleted": false,
        "reminderDate": "0001-01-01T00:00:00"
    },
    {
        "guid": "00000000-0000-0000-0000-000000000000",
        "invoiceGuid": "d5828cfd-12a6-4a7e-9be6-7c4b11f07dbf",
        "reminderType": "2 weeks after due date",
        "isActive": false,
        "isCompleted": false,
        "reminderDate": "0001-01-01T00:00:00"
    }
]
```

> Json Example Response (GET https://sandbox.choice.dev/api/v1/Invoice/Reminder/<guid>/false):

```json
[
    {
        "guid": "fbb2633f-0bf4-412b-a51e-edad3fb4b058",
        "invoiceGuid": "d5828cfd-12a6-4a7e-9be6-7c4b11f07dbf",
        "reminderType": "5 days before due date",
        "isActive": true,
        "isCompleted": false,
        "reminderDate": "2019-07-20T00:00:00"
    },
    {
        "guid": "cf7f8d0a-7540-43de-b637-09f7dec0a273",
        "invoiceGuid": "d5828cfd-12a6-4a7e-9be6-7c4b11f07dbf",
        "reminderType": "5 days after due date",
        "isActive": true,
        "isCompleted": false,
        "reminderDate": "2019-07-30T00:00:00"
    }
]
```

This endpoint gets a invoice reminder.

### HTTP Request

`GET https://sandbox.choice.dev/api/v1/Invoice/Reminder/<guid>/true`

    or

`GET https://sandbox.choice.dev/api/v1/Invoice/Reminder/<guid>/false`

### Headers using token

Key | Value
--------- | -------
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters

Parameter | Description
--------- | -----------
guid | Invoice’s guid to get

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>


























































# Search

## Search sales

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Search
    {
        public static void SearchSales()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Search/Sales/false");
                request.ContentType = "text/json";
                request.Method = "POST";

                var search = new
                {
                    merchantGuid = "19344275-985e-4dff-81ee-cb84b8ad356c",
                    Status = "Transaction - Approved"
                };

                string json = JsonConvert.SerializeObject(search);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
    "merchantGuid": "19344275-985e-4dff-81ee-cb84b8ad356c",
    "Status": "Transaction - Approved"
}
```

> Json Example Response:

```json
{
    "pageCurrent": 1,
    "pageCurrentResults": 1,
    "pageTotal": 1,
    "pageSize": 10,
    "totalResults": 1,
    "cardSummary": null,
    "searchResultDTO": [
        {
            "status": "Transaction - Approved",
            "amount": 20.00,
            "card": {
                "cardHolderName": null,
                "cardType": "Visa",
                "card.first6": "123456",
                "card.last4": "2910",
                "cardToken": "xxxx-xxxx-xxxx-xxxx-2910"
            },
            "timeStamp": "2020-11-26T06:34:34.46-06:00",
            "processorResponseMessage": "Success",
            "effectiveAmount": 0.00,
            "batchStatus": "Batch - Open",
            "relatedVoid": {
                "guid": "0f85c12d-9f51-49b8-8b8a-38238c89b9a6"
            },
            "guid": "372d66e5-1a5d-48ec-bebc-bb9347abe200",
            "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
            "generatedByCapture": false,
            "processorRefNumber": "24823396",
            "type": "Credit",
            "surchargeType": null,
            "tipAmount": null,
            "cardDataSource": "INTERNET",
            "allowCardEmv": false,
            "addressVerificationCode": "0",
            "addressVerificationResult": "Full Match",
            "cvvVerificationResult": " ",
            "semiIntegrated": false,
            "userName": "maxduplessy"
        }
    ]
}
```

This endpoint search a sales.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Search/Sales/{exportable}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/Sales/{exportable}/{pageNumber}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/Sales/{exportable}/{pageNumber}/{pageSize}`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
Exportable | string | Mandatory | True or False. It means if you want results exportable to CSV.
PageNumber | integer | Optional | Int. Number of page of the results. Default is 1 (Page size default is 500).
PageSize | integer | Optional | Int. Size of each page of the results. Default is 500.

### Json Body:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
MerchantGuid | string | Mandatory | Merchant's Guid.
AmountFrom | decimal | Optional | Amount of the transaction. Min. amt.: $0.50
AmountTo | decimal | Optional | Amount of the transaction. Min. amt.: $0.50
CardHolderName | string | Optional | Cardholder's name.
Card.First6 | string | Optional | Card first six number.
Card.Last4 | string | Optional | Card last four number.
Card.Token | string | Optional | Card tokenized number value
CardType | string | Optional | Card type.
InvoiceNumber | string |  Optional | Sale's InvoiceNumber.
OrderNumber | string |  Optional | Sale's order number. Length = 17.
OrderDateFrom | date | Optional | Sale's order Date.
OrderDateTo | date | Optional | Sale's order Date.
TimeStampFrom | date | Optional | Sale's TimeStamp.
TimeStampTo | date | Optional | Sale's TimeStamp.
Status | string | Optional | Sale’s status.<br><br>Allowed values:<br><br>**1. Transaction - Approved**<br>**2. Transaction - Declined**<br>**3. Transaction - Created - Local**<br>**4. Transaction - Created - Error: Processor not reached**<br>**5. Transaction - Processor Error**<br>**6. Transaction - Approved - Warning**
CustomerId | string | Optional | Customer Id.
CustomData | string | Optional | Customer Data.
GeneratedByCapture | boolean | Optional | Generated By Capture.<br><br>Allowed values:<br><br>**1. true**<br>**2. false**<br>
ProcessorRefNumber | string | Optional | Processor Reference Number.
UserName | string | Optional | UserName.

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>




























## Search voids

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Search
    {
        public static void SearchVoids()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Search/Voids/false");
                request.ContentType = "text/json";
                request.Method = "POST";

                var search = new
                {
                    merchantGuid = "19344275-985e-4dff-81ee-cb84b8ad356c",
                    Status = "Transaction - Approved"
                };

                string json = JsonConvert.SerializeObject(search);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
    "merchantGuid": "19344275-985e-4dff-81ee-cb84b8ad356c",
    "Status": "Transaction - Approved"
}
```

> Json Example Response:

```json
{
    "pageCurrent": 1,
    "pageCurrentResults": 1,
    "pageTotal": 1,
    "pageSize": 500,
    "totalResults": 1,
    "cardSummary": null,
    "searchResultDTO": [
        {
            "status": "Transaction - Approved",
            "sale": {
                "amount": 10.00,
                "card": {
                    "cardHolderName": null,
                    "cardType": "Visa",
                    "card.last4": "6734"
                }
            },
            "voidReason": "POST_AUTH_USER_DECLINE",
            "timeStamp": "2020-11-26T06:40:25.7-06:00",
            "processorStatusCode": "A0000",
            "batchStatus": "Batch - Open",
            "guid": "4130888a-cb0a-41a9-9d06-d90e10535b2a",
            "allowCardEmv": false,
            "userName": "maxduplessy"
        }
    ]
}
```

This endpoint search a voids.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Search/Voids/{exportable}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/Voids/{exportable}/{pageNumber}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/Voids/{exportable}/{pageNumber}/{pageSize}`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
Exportable | string | Mandatory | True or False. It means if you want results exportable to CSV.
PageNumber | integer | Optional | Int. Number of page of the results. Default is 1 (Page size default is 500).
PageSize | integer | Optional | Int. Size of each page of the results. Default is 500.

### Json Body:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
MerchantGuid | string | Mandatory | Merchant's Guid.
VoidReason | string | Optional | Indicates the reason the transaction was voided.<br><br>Allowed values:<br><br>**1. POST_AUTH_USER_DECLINE**<br>**2. DEVICE_TIMEOUT**<br>**3. DEVICE_UNAVAILABLE**<br>**4. PARTIAL_REVERSAL**<br>**5. TORN_TRANSACTION**<br>**6. POST_AUTH_CHIP_DECLINE**
Status | string | Optional | Void’s status.<br><br>Allowed values:<br><br>**1. Transaction - Approved**<br>**2. Transaction - Declined**<br>**3. Transaction - Created - Local**<br>**4. Transaction - Created - Error: Processor not reached**<br>**5. Transaction - Processor Error**<br>**6. Transaction - Approved - Warning**
TimeStampFrom | date | Optional | Void's TimeStamp.
TimeStampTo | date | Optional | Void's TimeStamp.
UserName | string | Optional | UserName.


### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>



























## Search returns

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Search
    {
        public static void SearchReturns()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Search/Returns/false");
                request.ContentType = "text/json";
                request.Method = "POST";

                var search = new
                {
                    merchantGuid = "19344275-985e-4dff-81ee-cb84b8ad356c",
                    Status = "Transaction - Approved"                
                };

                string json = JsonConvert.SerializeObject(search);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
    "merchantGuid": "19344275-985e-4dff-81ee-cb84b8ad356c",
    "Status": "Transaction - Approved"
}
```

> Json Example Response:

```json
{
    "pageCurrent": 1,
    "pageCurrentResults": 1,
    "pageTotal": 1,
    "pageSize": 500,
    "totalResults": 1,
    "cardSummary": null,
    "searchResultDTO": [
        {
            "status": "Transaction - Approved",
            "amount": 19.74,
            "card": {
                "cardHolderName": "John Doe",
                "cardType": "Mastercard",
                "card.last4": "0213"
            },
            "timeStamp": "2020-11-25T07:17:24.15-06:00",
            "processorStatusCode": "A0014",
            "batchStatus": "Batch - Closed",
            "userName": "maxduplessy",
            "guid": "9fb17f4e-b508-48d3-bd77-bb6813b42620",
            "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
            "saleGuid": "b4084e11-884d-468c-84d9-614c5b986fde",
            "type": "CreditReturn",
            "cardDataSource": "",
            "allowCardEmv": false
        }
    ]
}
```

This endpoint search a returns.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Search/Returns/{exportable}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/Returns/{exportable}/{pageNumber}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/Returns/{exportable}/{pageNumber}/{pageSize}`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"


### URL Parameters:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
Exportable | string | Mandatory | True or False. It means if you want results exportable to CSV.
PageNumber | integer | Optional | Int. Number of page of the results. Default is 1 (Page size default is 500).
PageSize | integer | Optional | Int. Size of each page of the results. Default is 500.

### Json Body:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
MerchantGuid | string | Mandatory | Merchant's Guid.
AmountFrom | decimal | Optional | Amount of the transaction. Min. amt.: $0.50
AmountTo | decimal | Optional | Amount of the transaction. Min. amt.: $0.50
Status | string | Optional | Return’s status.<br><br>Allowed values:<br><br>**1. Transaction - Approved**<br>**2. Transaction - Declined**<br>**3. Transaction - Created - Local**<br>**4. Transaction - Created - Error: Processor not reached**<br>**5. Transaction - Processor Error**<br>**6. Transaction - Approved - Warning**
Card.Last4 | string | Optional | Card last four number.
TimeStampFrom | date | Optional | Return's TimeStamp.
TimeStampTo | date | Optional | Return's TimeStamp.
UserName | string | Optional | UserName.


### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>
























## Search authOnlys

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Search
    {
        public static void SearchAuthOnlys()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Search/AuthOnlys/false");
                request.ContentType = "text/json";
                request.Method = "POST";

                var search = new
                {
                    merchantGuid = "19344275-985e-4dff-81ee-cb84b8ad356c",
                    Status = "Transaction - Approved"
                };

                string json = JsonConvert.SerializeObject(search);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
    "merchantGuid": "19344275-985e-4dff-81ee-cb84b8ad356c",
    "Status": "Transaction - Approved"
}
```

> Json Example Response:

```json
{
    "pageCurrent": 1,
    "pageCurrentResults": 1,
    "pageTotal": 1,
    "pageSize": 10,
    "totalResults": 1,
    "cardSummary": null,
    "searchResultDTO": [
        {
            "status": "Transaction - Approved",
            "amount": 10.00,
            "effectiveAmount": 10.00,
            "card": {
                "cardHolderName": "John Doe",
                "cardType": "Mastercard",
                "card.first6": "123456",
                "card.last4": "9426",
                "cardToken": "xxxx-xxxx-xxxx-xxxx-9426"
            },
            "orderNumber": "11518",
            "orderDate": "2020-11-24T00:00:00",
            "timeStamp": "2020-11-24T14:18:54.16-06:00",
            "customerId": "xt147",
            "processorResponseMessage": "Success",
            "batchStatus": "Batch - Closed",
            "relatedCapture": {
                "guid": "284dd923-a266-4d0f-80bd-e8441154c742",
                "newAmount": 10.00
            },
            "relatedVoid": null,
            "guid": "493cffed-232a-4896-b9ca-36b543d7da13",
            "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
            "partiallyApprovedAmount": null,
            "cardDataSource": "INTERNET",
            "allowCardEmv": false,
            "addressVerificationCode": "N",
            "addressVerificationResult": "No Match",
            "cvvVerificationCode": "M",
            "cvvVerificationResult": "Passed",
            "semiIntegrated": null,
            "userName": "maxduplessy"
        }
    ]
}
```

This endpoint search an authOnlys.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Search/AuthOnlys/{exportable}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/AuthOnlys/{exportable}/{pageNumber}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/AuthOnlys/{exportable}/{pageNumber}/{pageSize}`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
Exportable | string | Mandatory | True or False. It means if you want results exportable to CSV.
PageNumber | integer | Optional | Int. Number of page of the results. Default is 1 (Page size default is 500).
PageSize | integer | Optional | Int. Size of each page of the results. Default is 500.

### Json Body:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
MerchantGuid | string | Mandatory | Merchant's Guid.
AmountFrom | decimal | Optional | Amount of the transaction. Min. amt.: $0.50
AmountTo | decimal | Optional | Amount of the transaction. Min. amt.: $0.50
Card.First6 | string | Optional | Card first six number.
Card.Last4 | string | Optional | Card last four number.
CardToken | string | Optional | Card tokenized number value.
CardHolderName | string | Optional | Cardholder's name.
CardType | string | Optional | Card Type.
InvoiceNumber | string |  Optional | AuthOnly's InvoiceNumber.
OrderNumber | string |  Optional | AuthOnly's order number. Length = 17.
OrderDateFrom | date | Optional | AuthOnly's order Date.
OrderDateTo | date | Optional | AuthOnly's order Date.
TimeStampFrom | date | Optional | AuthOnly's TimeStamp.
TimeStampTo | date | Optional | AuthOnly's TimeStamp.
Status | string | Optional | AuthOnly’s status.<br><br>Allowed values:<br><br>**1. Transaction - Approved**<br>**2. Transaction - Declined**<br>**3. Transaction - Created - Local**<br>**4. Transaction - Created - Error: Processor not reached**<br>**5. Transaction - Processor Error**<br>**6. Transaction - Approved - Warning**
MerchantCustomerId | string | Optional | Merchant Customer Id.
UserName | string | Optional | UserName.

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>





























## Search captures

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Search
    {
        public static void SearchCaptures()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Search/Captures/false");
                request.ContentType = "text/json";
                request.Method = "POST";

                var search = new
                {
                    merchantGuid = "19344275-985e-4dff-81ee-cb84b8ad356c",
                    Status = "Transaction - Approved"
                };

                string json = JsonConvert.SerializeObject(search);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
    "merchantGuid": "19344275-985e-4dff-81ee-cb84b8ad356c",
    "Status": "Transaction - Approved"
}
```

> Json Example Response:

```json
{
    "pageCurrent": 1,
    "pageCurrentResults": 1,
    "pageTotal": 1,
    "pageSize": 500,
    "totalResults": 1,
    "cardSummary": null,
    "searchResultDTO": [
        {
            "status": "Transaction - Approved",
            "authOnly": {
                "status": null,
                "amount": 10.00,
                "effectiveAmount": null,
                "card": {
                    "cardHolderName": "John Doe",
                    "cardType": "Mastercard",
                    "card.last4": "9426"
                },
                "orderNumber": "11518",
                "orderDate": "2020-11-24T00:00:00",
                "timeStamp": "0001-01-01T00:00:00",
                "customerId": null,
                "processorResponseMessage": null,
                "batchStatus": null,
                "relatedCapture": null,
                "relatedVoid": null,
                "guid": "493cffed-232a-4896-b9ca-36b543d7da13",
                "deviceGuid": "b29725af-b067-4a35-9819-bbb31bdf8808",
                "partiallyApprovedAmount": null,
                "cardDataSource": null,
                "allowCardEmv": false,
                "addressVerificationCode": null,
                "addressVerificationResult": null,
                "cvvVerificationCode": null,
                "cvvVerificationResult": null,
                "semiIntegrated": null,
                "userName": null
            },
            "newAmount": 10.00,
            "timeStamp": "2020-11-24T14:21:25.59-06:00",
            "processorResponseMessage": "Success",
            "batchStatus": "Batch - Closed",
            "guid": "284dd923-a266-4d0f-80bd-e8441154c742",
            "allowCardEmv": false,
            "userName": "maxduplessy"
        }
    ]
}
```

This endpoint search a captures.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Search/Captures/{exportable}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/Captures/{exportable}/{pageNumber}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/Captures/{exportable}/{pageNumber}/{pageSize}`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
Exportable | string | Mandatory | True or False. It means if you want results exportable to CSV.
PageNumber | integer | Optional | Int. Number of page of the results. Default is 1 (Page size default is 500).
PageSize | integer | Optional | Int. Size of each page of the results. Default is 500.

### Json Body:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
MerchantGuid | string | Mandatory | Merchant's Guid.
NewAmountFrom | decimal | Optional | NewAmount From of the transaction. Min. amt.: $0.50
NewAmountTo | decimal | Optional | NewAmount To of the transaction. Min. amt.: $0.50
TimeStampFrom | date | Optional | Capture's TimeStamp From.
TimeStampTo | date | Optional | Capture's TimeStamp To.
Status | string | Optional | Capture’s status.<br><br>Allowed values:<br><br>**1. Transaction - Approved**<br>**2. Transaction - Declined**<br>**3. Transaction - Created - Local**<br>**4. Transaction - Created - Error: Processor not reached**<br>**5. Transaction - Processor Error**<br>**6. Transaction - Approved - Warning**
UserName | string | Optional | UserName.

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>






















## Search verify

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Search
    {
        public static void SearchVerify()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Search/Verify/false");
                request.ContentType = "text/json";
                request.Method = "POST";

                var search = new
                {
                    merchantGuid = "19344275-985e-4dff-81ee-cb84b8ad356c",
                    Status = "Transaction - Approved"
                };

                string json = JsonConvert.SerializeObject(search);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
    "merchantGuid": "19344275-985e-4dff-81ee-cb84b8ad356c",
    "Status": "Transaction - Approved"
}
```

> Json Example Response:

```json
{
    "pageCurrent": 1,
    "pageCurrentResults": 1,
    "pageTotal": 1,
    "pageSize": 500,
    "totalResults": 1,
    "cardSummary": null,
    "searchResultDTO": [
        {
            "status": "Transaction - Approved",
            "card": {
                "cardHolderName": "Justin Troudeau",
                "cardType": "Visa",
                "card.first6": "123456",
                "card.last4": "7402",
                "cardToken": "xxxx-xxxx-xxxx-xxxx-7402"
            },
            "timeStamp": "2020-11-25T07:24:25.29-06:00",
            "processorStatusCode": "A0000",
            "userName": "maxduplessy"
        }
    ]
}
```

This endpoint search a verify.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Search/Verify/{exportable}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/Verify/{exportable}/{pageNumber}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/Verify/{exportable}/{pageNumber}/{pageSize}`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
Exportable | string | Mandatory | True or False. It means if you want results exportable to CSV.
PageNumber | integer | Optional | Int. Number of page of the results. Default is 1 (Page size default is 500).
PageSize | integer | Optional | Int. Size of each page of the results. Default is 500.

### Json Body:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
MerchantGuid | string | Mandatory | Merchant's Guid.
Card.First6 | string | Optional | Card first six number.
Card.Last4 | string | Optional | Card last four number.
Card.Token | string | Optional | Card tokenized number value
CardHolderName | string | Optional | Cardholder's name.
CardType | string | Optional | Card Type.
TimeStampFrom | date | Optional | Verify's TimeStamp From.
TimeStampTo | date | Optional | Verify's TimeStamp To.
Status | string | Optional | Verify’s status.<br><br>Allowed values:<br><br>**1. Transaction - Approved**<br>**2. Transaction - Declined**<br>**3. Transaction - Created - Local**<br>**4. Transaction - Created - Error: Processor not reached**<br>**5. Transaction - Processor Error**<br>**6. Transaction - Approved - Warning**
UserName | string | Optional | UserName.

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>






































## Search Recurring Billings

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Search
    {
        public static void SearchRecurringBillings()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Search/RecurringBillings/false");
                request.ContentType = "text/json";
                request.Method = "POST";

                var search = new
                {
                    merchantGuid = "19344275-985e-4dff-81ee-cb84b8ad356c",
                    Status = "Transaction - Approved"
                };

                string json = JsonConvert.SerializeObject(search);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
    "merchantGuid": "19344275-985e-4dff-81ee-cb84b8ad356c",
    "Status": "Transaction - Approved"
}
```

> Json Example Response:

```json
{
    "pageCurrent": 1,
    "pageCurrentResults": 1,
    "pageTotal": 1,
    "pageSize": 10,
    "totalResults": 1,
    "cardSummary": null,
    "searchResultDTO": [
        {
            "status": "RecurringBilling - Active",
            "interval": "monthly",
            "intervalValue": "april, may, june",
            "amount": 10.50,
            "name": "John Doe",
            "recurringBillingNumber": "64942678",
            "startDate": "2021-05-13T00:00:00",
            "endDate": "2023-04-01T00:00:00",
            "paymentCount": null,
            "processorResponseMessage": null,
            "card.first6": "123456",
            "card.last4": "6019",
            "accountNumberLastFour": null,
            "timeStamp": "2020-11-25T14:31:38.6",
            "invoiceGuid": "00000000-0000-0000-0000-000000000000",
            "invoiceNumber": null,
            "guid": "11fd2d0a-298f-4387-81ed-ffd6780f7a21",
            "scheduleNotes": "Cable",
            "pause": null,
            "accountNumber": "",
            "pauseStartDate": null,
            "pauseEndDate": null,
            "userName": "maxduplessy"
        }
    ]
}
```

This endpoint search a recurring billings.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Search/RecurringBillings/{exportable}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/RecurringBillings/{exportable}/{pageNumber}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/RecurringBillings/{exportable}/{pageNumber}/{pageSize}`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
Exportable | string | Mandatory | True or False. It means if you want results exportable to CSV.
PageNumber | integer | Optional | Int. Number of page of the results. Default is 1 (Page size default is 500).
PageSize | integer | Optional | Int. Size of each page of the results. Default is 500.

### Json Body:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
MerchantGuid | string | Mandatory | Merchant's Guid.
CardOnly | string | Optional | Card Only.
BankAccountOnly | string | Optional | Bank Account Only.
Status | string | Optional | Recurring billing status.<br><br>Allowed values:<br><br>**1. RecurringBilling -  Active**<br>**2. RecurringBilling - Created - Local** <br>**3. RecurringBilling - Created - Error: Processor not reached** <br>**4. RecurringBilling - Created - Processor Fail** <br>**5. RecurringBilling -  Deactivated** <br>**6. RecurringBilling -  Paused** <br>**7. RecurringBilling - Finished** <br>**8. RecurringBilling - Deleted** <br>**9. RecurringBilling - Active Started**
Name | string | Optional | Name.
AmountFrom | decimal | Optional |  amount from of the transaction. Min. amt.: $0.50
AmountTo | decimal | Optional |  amount to of the transaction. Min. amt.: $0.50
CreatedDateFrom | date | Optional | Created Date From RecurringBillings.
CreatedDateTo | date | Optional | Created Date To RecurringBillings.
BillingDateFrom | date | Optional | Created Billing Date From RecurringBillings.
BillingDateTo | date | Optional | Created Billing Date To RecurringBillings.
LastFour | string | Optional | LastFour.
RecurringBillingNumber | string | Optional | RecurringBillingNumber.
ScheduleNotes | string | Optional | ScheduleNotes.
ScheduleExceptions | string | Optional | ScheduleExceptions.
UserName | string | Optional | UserName.
AccountNumber | string | Optional | AccountNumber.


### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>




























## Search bank clearing

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Search
    {
        public static void SearchBankClearing()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Search/BankClearings/false");
                request.ContentType = "text/json";
                request.Method = "POST";

                var search = new
                {
                    merchantGuid = "19344275-985e-4dff-81ee-cb84b8ad356c",
                    Status = "Transaction - Approved"
                };

                string json = JsonConvert.SerializeObject(search);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
    "merchantGuid": "19344275-985e-4dff-81ee-cb84b8ad356c",
    "Status": "Transaction - Approved"
}
```

> Json Example Response:

```json
{
    "pageCurrent": 1,
    "pageCurrentResults": 1,
    "pageTotal": 1,
    "pageSize": 500,
    "totalResults": 1,
    "cardSummary": null,
    "searchResultDTO": [
        {
            "status": "Transaction - Approved - Warning",
            "amount": 3.50,
            "effectiveAmount": 0.00,
            "bankAccount": {
                "nameOnAccount": "Joe Black",
                "routingNumber": "211274450",
                "lastFour": "2020"
            },
            "orderNumber": "11518",
            "timeStamp": "2020-11-24T08:53:17.61-06:00",
            "updateTimeStamp": null,
            "fromRecurringBilling": false,
            "customerId": "xt147",
            "processorResponseMessage": "HELD",
            "relatedVoid": null,
            "relatedReturn": "f76fda9b-3632-441b-aa8c-f98ff4389ade",
            "guid": "8c7d4ce1-1cd2-4c92-ac24-86bf7ae1c0ed",
            "deviceGuid": "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
            "responseTransactionStatus": "Debit Sent",
            "responseSettlementStatus": "Credit Sent",
            "responseCode": "HELD",
            "userName": "maxduplessy"
        }
    ]
}
```

This endpoint search a bank clearings.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Search/BankClearings/{exportable}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/BankClearings/{exportable}/{pageNumber}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/BankClearings/{exportable}/{pageNumber}/{pageSize}`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
Exportable | string | Mandatory | True or False. It means if you want results exportable to CSV.
PageNumber | integer | Optional | Int. Number of page of the results. Default is 1 (Page size default is 500).
PageSize | integer | Optional | Int. Size of each page of the results. Default is 500.

### Json Body:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
MerchantGuid | string | Mandatory | Merchant's Guid.
Status | string | Optional | Bank clearing’s status.<br><br>Allowed values:<br><br>**1. Transaction - Approved**<br>**2. Transaction - Declined**<br>**3. Transaction - Created - Local**<br>**4. Transaction - Created - Error: Processor not reached**<br>**5. Transaction - Processor Error**<br>**6. Transaction - Approved - Warning**
TotalAmountFrom | decimal | Optional | Total amount from of the transaction. Min. amt.: $0.50
TotalAmountTo | decimal | Optional | Total amount to of the transaction. Min. amt.: $0.50
NameOnAccount | string | Optional | Account's name.
RoutingNumber | string | Optional | Routing's number. Must be 9 characters (example: 490000018).
AccountNumberLastFour | string | Optional | Account's number last four.
FromRecurringBilling | boolean | Optional | From RecurringBilling.
OrderNumber | string |  Optional | Merchant's order number. The value sent on this element will be returned as InvoiceNumber. Length = 17.
TimeStampFrom | date | Optional | Bank clearing's TimeStamp.
TimeStampTo | date | Optional | Bank clearing's TimeStamp.
MerchantCustomerId | string | Optional | Merchant Customer Id.
UserName | string | Optional | UserName.
SettlementStatus | string | Optional | Settlement’s status.<br><br>Allowed values:<br><br>**1.Pending**<br>**2. Debit Sent**<br>**3. Credit Sent**<br>**4. Chargeback**<br>**5. Not Available**<br>**6. No Credit**<br>**7. Voided**<br>**8. Returned**<br>**9. Cancelled**



### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>


































## Search bank clearing voids

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Search
    {
        public static void SearchBankClearingVoids()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Search/BankClearingVoids/false");
                request.ContentType = "text/json";
                request.Method = "POST";

                var search = new
                {
                    merchantGuid = "19344275-985e-4dff-81ee-cb84b8ad356c",
                    Status = "Transaction - Approved"
                };

                string json = JsonConvert.SerializeObject(search);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
    "merchantGuid": "19344275-985e-4dff-81ee-cb84b8ad356c",
    "UserName": "maxduplessy"
}
```

> Json Example Response:

```json
{
    "pageCurrent": 1,
    "pageCurrentResults": 1,
    "pageTotal": 1,
    "pageSize": 500,
    "totalResults": 1,
    "cardSummary": null,
    "searchResultDTO": [
        {
            "status": "Transaction - Approved",
            "relatedClearing": {
                "status": null,
                "amount": 3.50,
                "effectiveAmount": 0.0,
                "bankAccount": {
                    "nameOnAccount": "max tomassi",
                    "routingNumber": "211274450",
                    "lastFour": "5678"
                },
                "orderNumber": "11518",
                "timeStamp": "0001-01-01T00:00:00",
                "updateTimeStamp": null,
                "fromRecurringBilling": null,
                "customerId": null,
                "processorResponseMessage": null,
                "relatedVoid": null,
                "relatedReturn": null,
                "guid": "00000000-0000-0000-0000-000000000000",
                "deviceGuid": "00000000-0000-0000-0000-000000000000",
                "responseTransactionStatus": null,
                "responseSettlementStatus": null,
                "responseCode": null,
                "userName": null
            },
            "timeStamp": "2020-11-26T08:20:33.7-06:00",
            "processorResponseMessage": "VOIDED",
            "guid": "8fc24460-718f-4db2-a5cb-31ce722cec3a",
            "userName": "maxduplessy"
        }
    ]
}
```

This endpoint search a bank clearing voids.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Search/BankClearingVoids/{exportable}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/BankClearingVoids/{exportable}/{pageNumber}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/BankClearingVoids/{exportable}/{pageNumber}/{pageSize}`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
Exportable | string | Mandatory | True or False. It means if you want results exportable to CSV.
PageNumber | integer | Optional | Int. Number of page of the results. Default is 1 (Page size default is 500).
PageSize | integer | Optional | Int. Size of each page of the results. Default is 500.

### Json Body:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
MerchantGuid | string | Mandatory | Merchant's Guid.
OrderNumber | string |  Optional | Merchant's order number. The value sent on this element will be returned as InvoiceNumber. Length = 17.
TimeStampFrom | date | Optional | Bank clearing void's TimeStamp.
TimeStampTo | date | Optional | Bank clearing void's TimeStamp.
Status | string | Optional | Bank clearing void’s status.<br><br>Allowed values:<br><br>**1. Transaction - Approved**<br>**2. Transaction - Declined**<br>**3. Transaction - Created - Local**<br>**4. Transaction - Created - Error: Processor not reached**<br>**5. Transaction - Processor Error**<br>**6. Transaction - Approved - Warning**
UserName | string | Optional | UserName.

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>




































## Search bank clearing returns

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Search
    {
        public static void SearchBankClearingReturns()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Search/BankClearingReturns/false");
                request.ContentType = "text/json";
                request.Method = "POST";

                var search = new
                {
                    merchantGuid = "19344275-985e-4dff-81ee-cb84b8ad356c",
                    Status = "Transaction - Approved"
                };

                string json = JsonConvert.SerializeObject(search);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
    "merchantGuid": "19344275-985e-4dff-81ee-cb84b8ad356c",
    "Status": "Transaction - Approved"
}
```

> Json Example Response:

```json
{
    "pageCurrent": 1,
    "pageCurrentResults": 1,
    "pageTotal": 1,
    "pageSize": 500,
    "totalResults": 1,
    "cardSummary": null,
    "searchResultDTO": [
        {
            "status": "Transaction - Approved - Warning",
            "relatedClearing": {
                "status": null,
                "amount": 3.50,
                "effectiveAmount": 0.0,
                "bankAccount": {
                    "nameOnAccount": "Joe Black",
                    "routingNumber": "211274450",
                    "lastFour": "2020"
                },
                "orderNumber": "11518",
                "timeStamp": "0001-01-01T00:00:00",
                "updateTimeStamp": null,
                "fromRecurringBilling": null,
                "customerId": null,
                "processorResponseMessage": null,
                "relatedVoid": null,
                "relatedReturn": null,
                "guid": "00000000-0000-0000-0000-000000000000",
                "deviceGuid": "00000000-0000-0000-0000-000000000000",
                "responseTransactionStatus": null,
                "responseSettlementStatus": null,
                "responseCode": null,
                "userName": null
            },
            "timeStamp": "2020-11-24T09:05:12.05-06:00",
            "processorResponseMessage": "HELD",
            "guid": "f76fda9b-3632-441b-aa8c-f98ff4389ade",
            "userName": null
        }
    ]
}
```

This endpoint search a bank clearing returns.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Search/BankClearingReturns/{exportable}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/BankClearingReturns/{exportable}/{pageNumber}`
<br>
<br>
`POST https://sandbox.choice.dev/api/v1/Search/BankClearingReturns/{exportable}/{pageNumber}/{pageSize}`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### URL Parameters:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
Exportable | string | Mandatory | True or False. It means if you want results exportable to CSV.
PageNumber | integer | Optional | Int. Number of page of the results. Default is 1 (Page size default is 500).
PageSize | integer | Optional | Int. Size of each page of the results. Default is 500.

### Json Body:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
MerchantGuid | string | Mandatory | Merchant's Guid.
OrderNumber | string |  Optional | Merchant's order number. The value sent on this element will be returned as InvoiceNumber. Length = 17.
TimeStampFrom | date | Optional | Bank clearing void's TimeStamp.
TimeStampTo | date | Optional | Bank clearing void's TimeStamp.
Status | string | Optional | Bank clearing void’s status.<br><br>Allowed values:<br><br>**1. Transaction - Approved**<br>**2. Transaction - Declined**<br>**3. Transaction - Created - Local**<br>**4. Transaction - Created - Error: Processor not reached**<br>**5. Transaction - Processor Error**<br>**6. Transaction - Approved - Warning**
UserName | string | Optional | UserName.

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>











































## Search bank Clearing Settlement

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Search
    {
        public static void ShowSettlements()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Search/BankClearings/ShowSettlements");
                request.ContentType = "text/json";
                request.Method = "POST";

                var search = new
                {
                    DeviceGuid = "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
                    SettleDateBegin = "01/01/2010",
                    SettleDateEnd = "12/01/2020"
                };

                string json = JsonConvert.SerializeObject(search);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
    "DeviceGuid" : "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
    "SettleDateBegin": "01/01/2010",
    "SettleDateEnd": "12/01/2020"
}
```

> Json Example Response:

```json
{
    "summary": {
        "error": "",
        "message": "",
        "requestCount": 1,
        "successCount": 1,
        "failCount": 0,
        "timeStamp": "11/27/2020 8:58:20 AM",
        "executeTime": 0.0
    },
    "items": [
        {
            "settlementId": "1931",
            "settlementDate": "2020-11-17T08:22:00",
            "fileId": "1162",
            "subMerchantNum": "",
            "settlementAmount": 398.0000,
            "settlementType": "Origination File Settlement",
            "approvedDebits": 398.0000,
            "approvedCredits": 0.0,
            "debitCreditFees": 0.0,
            "nocFees": 0.0,
            "returnFees": 0.0,
            "reserve": 0.0,
            "discount": 0.0,
            "chargebacks": 0.0,
            "chargebackFees": 0.0,
            "extendedHoursFees": 0.0,
            "adminFees": 0.0,
            "deliveryMethod": 0.0,
            "offset": 0.0
        }
    ],
    "messageXml": "<?xml version='1.0'?>\n<BOPRequest>\n<ResponseSummary>\n<Error></Error>\n<Message></Message>\n<RequestCount>1</RequestCount>\n<SuccessCount>1</SuccessCount>\n<FailCount>0</FailCount>\n<TimeStamp>11/27/2020 8:58:20 AM</TimeStamp>\n<ExecuteTime>0</ExecuteTime>\n</ResponseSummary>\n<Request ID=\"1\">\n<RequestType>ShowSettlements</RequestType>\n<Status>Success</Status>\n<StatusMessage/>\n<TimeStamp>11/27/2020 8:58:20 AM</TimeStamp>\n<Results Count=\"1\">\n<Result ID=\"1\">\n<SettlementID>1931</SettlementID>\n<SettlementDate>11/17/2020 8:22:00 AM</SettlementDate>\n<FileID>1162</FileID>\n<SubMerchantNum></SubMerchantNum>\n<SettlementAmount>398.0000</SettlementAmount>\n<SettlementType>1</SettlementType>\n<ApprovedDebits>398.0000</ApprovedDebits>\n<ApprovedCredits>0</ApprovedCredits>\n<DebitCreditFees>0</DebitCreditFees>\n<NOCFees>0</NOCFees>\n<ReturnFees>0</ReturnFees>\n<Reserve>0</Reserve>\n<Discount>0</Discount>\n<Chargebacks>0</Chargebacks>\n<ChargebackFees>0</ChargebackFees>\n<ExtendedHoursFees>0</ExtendedHoursFees>\n<AdminFees>0</AdminFees>\n<DeliveryMethod>ACH</DeliveryMethod>\n<Offset>0</Offset>\n</Result>\n</Results>\n</Request>\n</BOPRequest>\n"
}
```

This endpoint searchs Bank Clearing Settlement.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Search/BankClearings/ShowSettlements`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Json Body:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device's Guid.
SettleDateBegin | string | Mandatory | Settle Date Begin.
SettleDateEnd | string | Mandatory | Settle Date End.



### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>





































## Search bank Clearing Settlement Details

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Search
    {
        public static void ShowSettlementDetails()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Search/BankClearings/ShowSettlementDetails");
                request.ContentType = "text/json";
                request.Method = "POST";

                var search = new
                {
                    DeviceGuid = "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
                    SettlementId = "1931"
                };

                string json = JsonConvert.SerializeObject(search);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
    "DeviceGuid" : "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
    "SettlementId" : 1931
}
```

> Json Example Response:

```json
{
    "summary": {
        "error": "",
        "message": "",
        "requestCount": 1,
        "successCount": 1,
        "failCount": 0,
        "timeStamp": "11/27/2020 9:00:23 AM",
        "executeTime": 0.0
    },
    "item": {
        "status": "Success",
        "statusMessage": "",
        "transactionId": "22275",
        "transactionType": "CREDIT",
        "fileId": "1162",
        "subMerchantNum": "",
        "amount": 299.0000,
        "customerId": "543f9dd4-1234-4e69-a5e9-212b9486020e",
        "accountName": "Irish City"
    },
    "messageXml": "<?xml version='1.0'?>\n<BOPRequest>\n<ResponseSummary>\n<Error></Error>\n<Message></Message>\n<RequestCount>1</RequestCount>\n<SuccessCount>1</SuccessCount>\n<FailCount>0</FailCount>\n<TimeStamp>11/27/2020 9:00:23 AM</TimeStamp>\n<ExecuteTime>0</ExecuteTime>\n</ResponseSummary>\n<Request ID=\"1\">\n<RequestType>ShowSettlementDetails</RequestType>\n<Status>Success</Status>\n<StatusMessage/>\n<TimeStamp>11/27/2020 9:00:23 AM</TimeStamp>\n<Results Count=\"2\">\n<Result ID=\"1\">\n<TransactionID>22274</TransactionID>\n<TransactionType>CREDIT</TransactionType>\n<FileID>1162</FileID>\n<SubMerchantNum></SubMerchantNum>\n<Amount>99.0000</Amount>\n<CustomerID>8337ea32-1234-4ddb-9b80-6f67c7d7048a</CustomerID>\n<AccountName>Galaxy Fun Park</AccountName>\n</Result>\n<Result ID=\"2\">\n<TransactionID>22275</TransactionID>\n<TransactionType>CREDIT</TransactionType>\n<FileID>1162</FileID>\n<SubMerchantNum></SubMerchantNum>\n<Amount>299.0000</Amount>\n<CustomerID>543f9dd4-1234-4e69-a5e9-212b9486020e</CustomerID>\n<AccountName>Irish City</AccountName>\n</Result>\n</Results>\n</Request>\n</BOPRequest>\n"
}
```

This endpoint Searchs bank Clearing Settlement Details.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Search/BankClearings/ShowSettlementDetails`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Json Body:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device's Guid.
SettlementId | string | Mandatory | Settlement Id.



### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>








## Search bank Clearing Notifications

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Search
    {
        public static void ShowReturnNotificationList()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Search/BankClearings/ShowReturnNotificationList");
                request.ContentType = "text/json";
                request.Method = "POST";

                var search = new
                {
                    DeviceGuid = "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
                    NotificationDateBegin = "2020-11-11",
                    NotificationDateEnd = "2021-11-11"
                };

                string json = JsonConvert.SerializeObject(search);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
    "DeviceGuid": "0e51ea3e-56ff-49e6-9d72-39f2058774a1",
    "NotificationDateBegin": "2020-11-11",
    "NotificationDateEnd": "2021-11-11"
}
```

> Json Example Response:

```json
{
    "summary": {
        "error": "",
        "message": "",
        "requestCount": 1,
        "successCount": 1,
        "failCount": 0,
        "timeStamp": "11/16/2021 4:44:55 PM",
        "executeTime": 0.0
    },
    "items": [
        {
            "messageXml": "<Result ID=\"1\"><NotificationID>66</NotificationID><NotificationTime>11/23/2020 5:06:39 PM</NotificationTime><Announced>True</Announced><Retrieved>False</Retrieved><Description>RETURN NOTIFICATION FOR RETURN FILE ID: 9999</Description></Result>",
            "messageJson": "{\"Result\":{\"@ID\":\"1\",\"NotificationID\":\"66\",\"NotificationTime\":\"11/23/2020 5:06:39 PM\",\"Announced\":\"True\",\"Retrieved\":\"False\",\"Description\":\"RETURN NOTIFICATION FOR RETURN FILE ID: 9999\"}}",
            "notificationId": "66",
            "notificationTime": "2020-11-23T17:06:39",
            "announced": true,
            "retrieved": false,
            "description": "RETURN NOTIFICATION FOR RETURN FILE ID: 9999"
        },
        {
            "messageXml": "<Result ID=\"2\"><NotificationID>67</NotificationID><NotificationTime>11/23/2020 5:08:08 PM</NotificationTime><Announced>True</Announced><Retrieved>False</Retrieved><Description>RETURN NOTIFICATION FOR RETURN FILE ID: 9999</Description></Result>",
            "messageJson": "{\"Result\":{\"@ID\":\"2\",\"NotificationID\":\"67\",\"NotificationTime\":\"11/23/2020 5:08:08 PM\",\"Announced\":\"True\",\"Retrieved\":\"False\",\"Description\":\"RETURN NOTIFICATION FOR RETURN FILE ID: 9999\"}}",
            "notificationId": "67",
            "notificationTime": "2020-11-23T17:08:08",
            "announced": true,
            "retrieved": false,
            "description": "RETURN NOTIFICATION FOR RETURN FILE ID: 9999"
        },
        {
            "messageXml": "<Result ID=\"3\"><NotificationID>71</NotificationID><NotificationTime>12/2/2020 2:12:02 PM</NotificationTime><Announced>True</Announced><Retrieved>False</Retrieved><Description>RETURN NOTIFICATION FOR RETURN FILE ID: 9999</Description></Result>",
            "messageJson": "{\"Result\":{\"@ID\":\"3\",\"NotificationID\":\"71\",\"NotificationTime\":\"12/2/2020 2:12:02 PM\",\"Announced\":\"True\",\"Retrieved\":\"False\",\"Description\":\"RETURN NOTIFICATION FOR RETURN FILE ID: 9999\"}}",
            "notificationId": "71",
            "notificationTime": "2020-12-02T14:12:02",
            "announced": true,
            "retrieved": false,
            "description": "RETURN NOTIFICATION FOR RETURN FILE ID: 9999"
        }
    ],
    "messageXml": "<?xml version='1.0'?>\n<BOPRequest>\n<ResponseSummary>\n<Error></Error>\n<Message></Message>\n<RequestCount>1</RequestCount>\n<SuccessCount>1</SuccessCount>\n<FailCount>0</FailCount>\n<TimeStamp>11/16/2021 4:44:55 PM</TimeStamp>\n<ExecuteTime>0</ExecuteTime>\n</ResponseSummary>\n<Request ID=\"1\">\n<RequestType>ShowReturnNotificationList</RequestType>\n<Status>Success</Status>\n<StatusMessage/>\n<TimeStamp>11/16/2021 4:44:55 PM</TimeStamp>\n<Results Count=\"3\">\n<Result ID=\"1\">\n<NotificationID>66</NotificationID>\n<NotificationTime>11/23/2020 5:06:39 PM</NotificationTime>\n<Announced>True</Announced>\n<Retrieved>False</Retrieved>\n<Description>RETURN NOTIFICATION FOR RETURN FILE ID: 9999</Description>\n</Result>\n<Result ID=\"2\">\n<NotificationID>67</NotificationID>\n<NotificationTime>11/23/2020 5:08:08 PM</NotificationTime>\n<Announced>True</Announced>\n<Retrieved>False</Retrieved>\n<Description>RETURN NOTIFICATION FOR RETURN FILE ID: 9999</Description>\n</Result>\n<Result ID=\"3\">\n<NotificationID>71</NotificationID>\n<NotificationTime>12/2/2020 2:12:02 PM</NotificationTime>\n<Announced>True</Announced>\n<Retrieved>False</Retrieved>\n<Description>RETURN NOTIFICATION FOR RETURN FILE ID: 9999</Description>\n</Result>\n</Results>\n</Request>\n</BOPRequest>\n",
    "messageJson": "{\"BOPRequest\":{\"ResponseSummary\":{\"Error\":\"\",\"Message\":\"\",\"RequestCount\":\"1\",\"SuccessCount\":\"1\",\"FailCount\":\"0\",\"TimeStamp\":\"11/16/2021 4:44:55 PM\",\"ExecuteTime\":\"0\"},\"Request\":{\"@ID\":\"1\",\"RequestType\":\"ShowReturnNotificationList\",\"Status\":\"Success\",\"StatusMessage\":null,\"TimeStamp\":\"11/16/2021 4:44:55 PM\",\"Results\":{\"@Count\":\"3\",\"Result\":[{\"@ID\":\"1\",\"NotificationID\":\"66\",\"NotificationTime\":\"11/23/2020 5:06:39 PM\",\"Announced\":\"True\",\"Retrieved\":\"False\",\"Description\":\"RETURN NOTIFICATION FOR RETURN FILE ID: 9999\"},{\"@ID\":\"2\",\"NotificationID\":\"67\",\"NotificationTime\":\"11/23/2020 5:08:08 PM\",\"Announced\":\"True\",\"Retrieved\":\"False\",\"Description\":\"RETURN NOTIFICATION FOR RETURN FILE ID: 9999\"},{\"@ID\":\"3\",\"NotificationID\":\"71\",\"NotificationTime\":\"12/2/2020 2:12:02 PM\",\"Announced\":\"True\",\"Retrieved\":\"False\",\"Description\":\"RETURN NOTIFICATION FOR RETURN FILE ID: 9999\"}]}}}}"
}
```

This endpoint Searchs bank Clearing Notifications.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Search/BankClearings/ShowReturnNotificationList`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Json Body:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device's Guid.
NotificationDateBegin | string | Mandatory | Notification Date Begin.
NotificationDateEnd | string | Mandatory | Notification Date End.



### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>









## Search bank Clearing Notifications Details

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Search
    {
        public static void ShowReturnNotificationDetails()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Search/BankClearings/ShowReturnNotificationDetails");
                request.ContentType = "text/json";
                request.Method = "POST";

                var search = new
                {
                    DeviceGuid = "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
                    NotificationId = "66"
                };

                string json = JsonConvert.SerializeObject(search);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
    "DeviceGuid": "0e51ea3e-56ff-49e6-9d72-39f2058774a1",
    "NotificationId": "66"
}
```

> Json Example Response:

```json
{
    "summary": {
        "error": "",
        "message": "",
        "requestCount": 1,
        "successCount": 1,
        "failCount": 0,
        "timeStamp": "11/16/2021 5:10:39 PM",
        "executeTime": 0.0
    },
    "items": [
        {
            "messageXml": "<Result ID=\"1\"><TransactionID>22346</TransactionID><ReturnDate>11/23/2020 5:07:00 PM</ReturnDate><ReturnCode>R01</ReturnCode><ReturnDesc>INSUFFICIENT FUNDS</ReturnDesc><CustomerID>5c9aa8e8-38d3-44c8-a97e-637ccdb7fc3a</CustomerID></Result>",
            "messageJson": "{\"Result\":{\"@ID\":\"1\",\"TransactionID\":\"22346\",\"ReturnDate\":\"11/23/2020 5:07:00 PM\",\"ReturnCode\":\"R01\",\"ReturnDesc\":\"INSUFFICIENT FUNDS\",\"CustomerID\":\"5c9aa8e8-38d3-44c8-a97e-637ccdb7fc3a\"}}",
            "transactionId": "22346",
            "returnDate": "2020-11-23T17:07:00",
            "returnCode": "R01",
            "returnDesc": "INSUFFICIENT FUNDS",
            "customerId": "5c9aa8e8-38d3-44c8-a97e-637ccdb7fc3a"
        }
    ],
    "messageXml": "<?xml version='1.0'?>\n<BOPRequest>\n<ResponseSummary>\n<Error></Error>\n<Message></Message>\n<RequestCount>1</RequestCount>\n<SuccessCount>1</SuccessCount>\n<FailCount>0</FailCount>\n<TimeStamp>11/16/2021 5:10:39 PM</TimeStamp>\n<ExecuteTime>0</ExecuteTime>\n</ResponseSummary>\n<Request ID=\"1\">\n<RequestType>ShowReturnNotificationDetails</RequestType>\n<Status>Success</Status>\n<StatusMessage/>\n<TimeStamp>11/16/2021 5:10:39 PM</TimeStamp>\n<Results Count=\"1\">\n<Result ID=\"1\">\n<TransactionID>22346</TransactionID>\n<ReturnDate>11/23/2020 5:07:00 PM</ReturnDate>\n<ReturnCode>R01</ReturnCode>\n<ReturnDesc>INSUFFICIENT FUNDS</ReturnDesc>\n<CustomerID>5c9aa8e8-38d3-44c8-a97e-637ccdb7fc3a</CustomerID>\n</Result>\n</Results>\n</Request>\n</BOPRequest>\n",
    "messageJson": "{\"BOPRequest\":{\"ResponseSummary\":{\"Error\":\"\",\"Message\":\"\",\"RequestCount\":\"1\",\"SuccessCount\":\"1\",\"FailCount\":\"0\",\"TimeStamp\":\"11/16/2021 5:10:39 PM\",\"ExecuteTime\":\"0\"},\"Request\":{\"@ID\":\"1\",\"RequestType\":\"ShowReturnNotificationDetails\",\"Status\":\"Success\",\"StatusMessage\":null,\"TimeStamp\":\"11/16/2021 5:10:39 PM\",\"Results\":{\"@Count\":\"1\",\"Result\":{\"@ID\":\"1\",\"TransactionID\":\"22346\",\"ReturnDate\":\"11/23/2020 5:07:00 PM\",\"ReturnCode\":\"R01\",\"ReturnDesc\":\"INSUFFICIENT FUNDS\",\"CustomerID\":\"5c9aa8e8-38d3-44c8-a97e-637ccdb7fc3a\"}}}}}"
}
```

This endpoint Searchs bank Clearing Notifications Details.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Search/BankClearings/ShowReturnNotificationDetails`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Json Body:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device's Guid.
NotificationId | string | Mandatory | Notification Id.



### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>









## Search bank Clearing Chargebacks

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Search
    {
        public static void Chargebacks()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Search/BankClearings/ShowChargebacks");
                request.ContentType = "text/json";
                request.Method = "POST";

                var search = new
                {
                    DeviceGuid = "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
                    ChargeBackDateBegin = "2020-11-11", 
                    ChargeBackDateEnd = "2021-11-11", 
                };

                string json = JsonConvert.SerializeObject(search);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
    "DeviceGuid": "0e51ea3e-56ff-49e6-9d72-39f2058774a1",
    "ChargeBackDateBegin": "2020-11-11",
    "ChargeBackDateEnd": "2021-11-11"
}
```

> Json Example Response:

```json
{
    "summary": {
        "error": "",
        "message": "",
        "requestCount": 1,
        "successCount": 1,
        "failCount": 0,
        "timeStamp": "11/16/2021 5:23:17 PM",
        "executeTime": 0.0
    },
    "items": [],
    "messageXml": "<?xml version='1.0'?>\n<BOPRequest>\n<ResponseSummary>\n<Error></Error>\n<Message></Message>\n<RequestCount>1</RequestCount>\n<SuccessCount>1</SuccessCount>\n<FailCount>0</FailCount>\n<TimeStamp>11/16/2021 5:23:17 PM</TimeStamp>\n<ExecuteTime>0</ExecuteTime>\n</ResponseSummary>\n<Request ID=\"1\">\n<RequestType>ShowChargebacks</RequestType>\n<Status>Success</Status>\n<StatusMessage/>\n<TimeStamp>11/16/2021 5:23:17 PM</TimeStamp>\n<Results Count=\"0\">\n</Results>\n</Request>\n</BOPRequest>\n",
    "messageJson": "{\"BOPRequest\":{\"ResponseSummary\":{\"Error\":\"\",\"Message\":\"\",\"RequestCount\":\"1\",\"SuccessCount\":\"1\",\"FailCount\":\"0\",\"TimeStamp\":\"11/16/2021 5:23:17 PM\",\"ExecuteTime\":\"0\"},\"Request\":{\"@ID\":\"1\",\"RequestType\":\"ShowChargebacks\",\"Status\":\"Success\",\"StatusMessage\":null,\"TimeStamp\":\"11/16/2021 5:23:17 PM\",\"Results\":{\"@Count\":\"0\"}}}}"
}
```

This endpoint searchs bank Clearing Chargebacks.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Search/BankClearings/ShowChargebacks`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Json Body:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device's Guid.
ChargeBackDateBegin | string | Mandatory | ChargeBack Date Begin.
ChargeBackDateEnd  | string | Mandatory | ChargeBack Date Begin.



### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>












## Search bank Clearing NOCs

```csharp
using System;
using Newtonsoft.Json;
using System.IO;
using System.Net;
using System.Text;

namespace ChoiceSample
{
    public class Search
    {
        public static void Chargebacks()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/Search/BankClearings/ShowNOCList");
                request.ContentType = "text/json";
                request.Method = "POST";

                var search = new
                {
                    DeviceGuid = "a9c0505b-a087-44b1-b9cd-0e7f75715d4d",
                    NocDateBegin = "2020-11-11", 
                    NocDateEnd = "2021-11-11", 
                };

                string json = JsonConvert.SerializeObject(search);

                request.Headers.Add("Authorization", "Bearer 1A089D6ziUybPZFQ3mpPyjt9OEx9yrCs7eQIC6V3A0lmXR2N6-seGNK16Gsnl3td6Ilfbr2Xf_EyukFXwnVEO3fYL-LuGw-L3c8WuaoxhPE8MMdlMPILJTIOV3lTGGdxbFXdKd9U03bbJ9TDUkqxHqq8_VyyjDrw7fs0YOob7bg0OovXTeWgIvZaIrSR1WFR06rYJ0DfWn-Inuf7re1-4SMOjY1ZoCelVwduWCBJpw1111cNbWtHJfObV8u1CVf0");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        string result = reader.ReadToEnd();
                        Console.Write((int)response.StatusCode);
                        Console.WriteLine();
                        Console.WriteLine(response.StatusDescription);
                        Console.WriteLine(result);
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.Write((int)errorResponse.StatusCode);
                                Console.WriteLine();
                                Console.WriteLine(errorResponse.StatusDescription);
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        } 
    }
}
```

> Json Example Request:

```json
{
    "DeviceGuid": "0e51ea3e-56ff-49e6-9d72-39f2058774a1",
    "NocDateBegin": "2020-11-11",
    "NocDateEnd": "2021-11-11"
}
```

> Json Example Response:

```json
{
    "summary": {
        "error": "",
        "message": "",
        "requestCount": 1,
        "successCount": 1,
        "failCount": 0,
        "timeStamp": "11/16/2021 5:23:17 PM",
        "executeTime": 0.0
    },
    "items": [],
    "messageXml": "<?xml version='1.0'?>\n<BOPRequest>\n<ResponseSummary>\n<Error></Error>\n<Message></Message>\n<RequestCount>1</RequestCount>\n<SuccessCount>1</SuccessCount>\n<FailCount>0</FailCount>\n<TimeStamp>11/16/2021 5:23:17 PM</TimeStamp>\n<ExecuteTime>0</ExecuteTime>\n</ResponseSummary>\n<Request ID=\"1\">\n<RequestType>ShowChargebacks</RequestType>\n<Status>Success</Status>\n<StatusMessage/>\n<TimeStamp>11/16/2021 5:23:17 PM</TimeStamp>\n<Results Count=\"0\">\n</Results>\n</Request>\n</BOPRequest>\n",
    "messageJson": "{\"BOPRequest\":{\"ResponseSummary\":{\"Error\":\"\",\"Message\":\"\",\"RequestCount\":\"1\",\"SuccessCount\":\"1\",\"FailCount\":\"0\",\"TimeStamp\":\"11/16/2021 5:23:17 PM\",\"ExecuteTime\":\"0\"},\"Request\":{\"@ID\":\"1\",\"RequestType\":\"ShowChargebacks\",\"Status\":\"Success\",\"StatusMessage\":null,\"TimeStamp\":\"11/16/2021 5:23:17 PM\",\"Results\":{\"@Count\":\"0\"}}}}"
}
```

This endpoint searchs bank Clearing NOCs.

### HTTP Request

`POST https://sandbox.choice.dev/api/v1/Search/BankClearings/ShowNOCList`

### Headers using token

Key | Value
--------- | -------
Content-Type | "application/json"
Authorization | Token. Eg: "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6E..."

### Headers using API Key

Key | Value
--------- | -------
Content-Type | "application/json"
UserAuthorization | API Key. Eg: "e516b6db-3230-4b1c-ae3f-e5379b774a80"

### Json Body:

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
DeviceGuid | string | Mandatory | Device's Guid.
NocDateBegin | string | Mandatory | Noc Date Begin.
NocDateEnd | string | Mandatory | Noc Date Begin.



### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>



# Hosted Payment Page

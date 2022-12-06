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

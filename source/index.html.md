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

<!--
## Update user

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
        public static void UpdateUser()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/users/f3db1e2c-df7f-4cbe-b0cf-25f74d17b501");
                request.ContentType = "text/json";
                request.Method = "PUT";

                var user = new
                {
                    FirstName = "John",
                    LastName = "Doe",
                    Email = "johndoe@mailinator.com",
                    Phone = "9177563666",
                    Address1 = "151 E 33rd ST",
                    Address2 = "Second Floor",
                    City = "Dallas",
                    State = "TX",
                    Zipcode = "76092"
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
 "FirstName": "John",
 "LastName": "Doe",
 "Email": "johndoe@mailinator.com",
 "Phone": "9177563666",
 "Address1": "151 E 33rd ST",
 "Address2": "Second Floor",
 "City": "Dallas",
 "State": "TX",
 "Zipcode": "76092"
}
```

> Json Example Response:

```json
{
  "guid": "f3db1e2c-df7f-4cbe-b0cf-25f74d17b501",
  "userName": "JohnDoe",
  "isoNumber": 1000,
  "firstName": "John",
  "lastName": "Doe",
  "email": "johndoe@mailinator.com",
  "phone": "9177563666",
  "status": "User - Active",
  "address1": "151 E 33rd ST",
  "address2": "Second Floor",
  "city": "Dallas",
  "state": "TX",
  "zipcode": "76092",
  "accountNumber": "10000000",
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

This endpoint updates a user.

### HTTP Request

`PUT https://sandbox.choice.dev/api/v1/users/<guid>`

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
guid | User’s guid to update

### Query Parameters

Parameter | Type |  M/C/O | Value
--------- | ------- | ------- |-----------
FirstName | string |  Optional | User’s first name.
LastName | string | Optional | User’s last name.
Email | string | Optional | User’s valid email address
Phone | string | Optional | User’s phone number. The phone number must be syntactically correct. For example, 4152345678.
Address1 | string | Optional | User’s address.
Address2 | string | Optional | User’s address line 2.
City | string | Optional | User’s city.
State | string | Optional | User’s short name state. The ISO 3166-2 CA and US state or province code of a user. Length = 2.
Zipcode | string | Optional | User’s zipcode. Length = 5.

### Response

* 200 code (ok).

<aside class="success">
Remember you will need to use an authentication token or the API Key in the header request for every transaction.
</aside>

-->














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










































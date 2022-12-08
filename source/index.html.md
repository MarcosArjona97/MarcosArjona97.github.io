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

## Hosted Payment Page

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
        public static void HostedPaymentPage()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://sandbox.choice.dev/api/v1/HostedPaymentPageRequests");
                request.ContentType = "text/json";
                request.Method = "POST";

                var payment = new
                {
                    DeviceCreditCardGuid = "4b5013f7-b275-4929-8e83-0167c6edf639",
                    DeviceAchGuid = "386ac1e6-250d-4866-b283-248c1e9340ef",
                    Merchantname = "StarCoffe",
                    Description = "coffe latte",
                    Amount = 7.50,
                    OtherURL = "http://StarCoffe/other",
                    SuccessURL = "http://StarCoffe/success",
                    CancelURL = "http://StarCoffe/cancel"
                    OtherInfo = "Energy tax",
                    Customer = new
                    {
                        FirstName = "Max",
                        LastName = "Tomassi",
                        Phone = "8741234745",
                        City = "New York",
                        State = "NY",
                        Email = "maxduplessy@mailinator.com",
                        Address1 = "110 10th Av.",
                        Address2 = "",
                        Zip = "10016"
                    }
                };

                string json = JsonConvert.SerializeObject(payment);

                request.Headers.Add("Authorization", "Bearer eHSN5rTBzqDozgAAlN1UlTMVuIT1zSiAZWCo6EBqB7RFjVMuhmuPNWcYM7ozyMb3uaDe0gyDL_nMPESbuM5I4skBOYcUM4A06NO88CVV3yBYee7mWB1qT-YFu5A3KZJSfRIbTX9GZdrZpi-JuWsx-7GE9GIYrNJ29BpaQscTwxYDr67WiFlCCrsCqWnCPJUjCFRIrTDltz8vM15mlgjiO0y04ZACGOWNNErIVegX062oydV7SqumGJEbS9Av4gdy");
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
    "DeviceCreditCardGuid": "4b5013f7-b275-4929-8e83-0167c6edf639",
    "DeviceAchGuid": "386ac1e6-250d-4866-b283-248c1e9340ef",
    "Merchantname": "StarCoffe",
    "Description": "coffe latte",
    "Amount": 7.50,
    "OtherURL": "http://StarCoffe/other",
    "SuccessURL": "http://StarCoffe/success",
    "CancelURL": "http://StarCoffe/cancel",
    "OtherInfo": "Energy tax",
    "Customer": {
        "FirstName": "Max",
        "LastName": "Tomassi",
        "Phone": "8741234745",
        "City": "New York",
        "State": "NY",
        "Email": "maxduplessy@mailinator.com",
        "Address1": "110 10th Av.",
        "Address2": "",
        "Zip": "10016"
    }
}
```

> Json Example Response:

```json
{
    "merchantname": "StarCoffe",
    "description": "coffe latte",
    "amount": 7.50,
    "otherURL": "http://StarCoffe/other",
    "successURL": "http://StarCoffe/success",
    "cancelURL": "http://StarCoffe/cancel",
    "tempToken": "69337810-182e-4afe-a9d9-7268def789c7",
    "expiration": "2120-11-26T15:01:06.75",
    "otherInfo": "Energy tax",
    "customer": {
        "guid": "8144d441-acf2-4549-98cb-bab762675423",
        "firstName": "Max",
        "lastName": "Tomassi",
        "address1": "110 10th Av.",
        "address2": "",
        "zip": "10016",
        "city": "New York",
        "state": "NY",
        "phone": "8741234745",
        "email": "maxduplessy@mailinator.com"
    }
}
```

> **If the transaction gets approved the user will be redirected back to your site, sending a POST to the SuccessURL with the following parameters.**

```json
{
 "PaymentType": "Credit Card",
 "SaleGuid": "fa101801-1b87-495d-87b9-aeae745e9c85",
 "TokenizedCard": "84yXtM3EcGze0103",
 "OtherInfo": "",
 "Status": "Success",
 "AccountNumberLastFour": "1234",
 "Receipt": "CHOICE MERCHANT SOLUTIONS<br/>8320 S HARDY DRIVE<br/>TEMPE AZ 85284<br/>07/07/2017 07:23:55<br/><br/>CREDIT - SALE<br/><br/>CARD # : **** **** **** 0103<br/>CARD TYPE : VISA<br/>Entry Mode : MANUAL<br/><br/>REF # : 13259222<br/>Invoice number : 13259222<br/>AUTH CODE : TAS869<br/>Subtotal:                       $13.50<br/>--------------------------------------<br/>Total:                          $13.50<br/>--------------------------------------<br/>Andres Ordonez<br/><br/>CUSTOMER ACKNOWLEDGES RECEIPT OF<br/>GOODS AND/OR SERVICES IN THE AMOUNT<br/>OF THE TOTAL SHOWN HEREON AND AGREES<br/>TO PERFORM THE OBLIGATIONS SET FORTH<br/>BY THE CUSTOMER`S AGREEMENT WITH THE<br/>ISSUER<br/>APPROVED<br/>Customer Copy<br/>"
}
```

> **If the transaction goes wrong the user will be redirected back to your site, sending a POST to OtherURL with the following parameters**

```json
{
 "Error": "The Sale could not be processed correctly. Error code D2020. Error message: CVV2 verification failed"
}
```



The hosted payment page feature allows online and small merchants to redirect via a checkout button at their website visitors who wish to purchase items, pay invoices or make any type or transaction credit or ACH (if enabled).

To do this, simply POST to the following address using the following parameters and get your temporal token back. Then GET to our Hosted Payment Page using that temporal token and we care about the rest. This streamlines the checkout process and helps protect shoppers’ sensitive payment account data.

Enjoy a seamless checkout experience that automatically routes them to a secure page, branded with your company name. The customer enters their payment data directly into our server, releasing you of the responsibility of receiving, storing and transmitting sensitive cardholder data.

### HTTP POST

`POST https://sandbox.choice.dev/api/v1/HostedPaymentPageRequests`

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

Parameter | Type | M/C/O | Value
--------- | ------- | ------- | -----------
DeviceCreditCardGuid | string | Mandatory | Device's guid Credit Card.
DeviceAchGuid | string | Mandatory | Device's guid Ach.
Merchantname | string | Mandatory | Merchant's name.
Description | string | Mandatory | Items description.
Amount | string | Mandatory | Items total amount.
OtherURL | string | Mandatory | Web page you want to redirect the user in case something failed.
SuccessURL | string | Mandatory | Web page you want to redirect the user when transaction was successful.
CancelURL | string | Mandatory | Web page you want to redirect the user in case he decided to cancel the transaction.
OtherInfo | string | optional | Any alphanumeric code you might want to send to see it on the confirmation's response to keep track of your sales.
IsButton | boolean | optional | Determines if the hosted payment page request will be used as a button (allows multiple payments for the same request or not).
ButtonLabel | string | optional | This will be the text shown on your button (For example: "Pay here", "Donate").
Html | string | optional | Provide the html code you will use to share the button on your website.
Customer | object | optional | Customer.
RecurringBilling | object | optional | See [Recurring Billing](#recurring-billing) for details.
Invoice | object | optional | See [Invoice](#create-invoice) for details.



### Response

* 200 code (ok).


### HTTP POST BACK SUCCESS

If the transaction gets approved the user will be redirected back to your site, sending a POST to the SuccessURL with the parameters on the Json sample.


### HTTP POST BACK ERROR

If the transaction goes wrong the user will be redirected back to your site, sending a POST to OtherURL with the parameters on the Json sample.

### PAY YOUR REQUEST

Go to `https://webportalsandbox.choice.dev/HostedPaymentPage/{{tempToken}}`






































# Declined Response Codes

### Codes

Code | Response Message | Description
--------- | ------- | -------
D0001 | Duplicate Request (Approved previously) | The transaction was already performed and approved. Verify if the request was submitted twice for the same transaction ID or external reference number.
D0003 | Duplicate Request (Declined previously) | The transaction was already performed and declined. Verify if the request was submitted twice for the same transaction ID or external reference number.
D0004 | Reversal Not Allowed    | The transaction is not authorized for reversal. This error may occur because the transaction was not settled, was declined, or already reversed.
D0005 | Return Not Allowed  | The transaction is not authorized for return. This error may occur because the transaction was not settled, was declined, or already reversed.
D0006 | Supervisor Override Required     
D0007 | Modify Transaction Not Allowed  | The transaction is not authorized for modification. This error may occur because the transaction was already settled, or was declined.
D0008 | Possible Duplicate Request  | This is a duplicate request. The credentials for this transaction (i.e. amount, card number or same service) are the same as another transaction submitted less than one minute apart.
D0009 | Duplicate Request (Reversed previously) | The request with the same credentials (amount, card number, or same service) hit the server twice within a minute.
E0010 | Inactive Device (Terminal)  | The device is not registered, or is inactive in the system.
E0011 | Device (Terminal) Configuration missing | The configuration parameter is missing.
E0012 | Insufficient privileges  
E0013 | Incremental Auth Not Allowed     
E0015 | Unable to process your request. Settlement InProgress.  | The transaction settlement is in progress.
E0016 | Functionality currently not available.  | The functionality is not supported.
E0020 | Inactive Merchant (Account) | The merchant is not registered, or is inactive in the system.
E0021 | Merchant (Account) configuration missing    | The configuration parameter is missing.
E0022 | Processor configuration missing | The processor parameter is missing.
D0023 | Merchant already active  
E0030 | Unique ID Error The terminal unique ID is invalid, or is not registered in the system.
D0050 | Inactive terminal (Backend) | The device is inactive, or is not registered at the host.
D0060 | Inactive account (Backend)  | The account is inactive, or is not registered at the host.
D0070 | Unique ID Error (Backend)   | The terminal unique ID is invalid, or is not registered at the host.
D0080 | Duplicate Request (Backend) | This is a duplicate transaction. This transaction was already approved and processed.
D0090 | Reversal Not Allowed (Backend)  | The transaction is not authorized for reversal. This error may occur because the transaction was settled, declined, or already reversed.
D0091 | Return Not Allowed (Backend)    | The transaction is not authorized for return. This error may occur because the transaction was settled, declined, or already reversed.
D0092 | Request Format Error (Backend) | 
D0093 | Encryption failure from host  |    
D0094 | Return not allowed, Card number requested does not match with original transaction card number  |  
D0095 | Invalid taskID  |  
D0096 | Currency code mismatch with original transaction    |  
D0097 | Multiple amount format in single request not supported |   
D0098 | Multiple tax with same tax type is not allowed. | A request includes multiple tax with same tax type. |
E0110 | System Error (BillParam)  |    
E0111 | System Error (UBillACC)  | 
E0200 | System Error (Tran)  | 
E0201 | System Error (BillpayTran)  |  
E0202 | System Error (CardTran)  | 
E0203 | System Error (CheckTran) |     
E0204 | System Error (MTTran) |    
E0205 | System Error (MOTran)  |   
E0206 | System Error (AccTran)   | 
E0207 | System Error (Shipping_Info Tran)   |  
E0208 | System Error (Products Tran) |     
E0209 | System Error (Override Tran)  |    
E0210 | System Error (PayMode Tran)  | 
E0300 | System Error (UTran)  |    
E0301 | System Error (BillpayUTran) |  
E0302 | System Error (CardUTran) |     
E0303 | System Error (CheckUTran)  |   
E0304 | System Error (MTUTran) |   
E0305 | System Error (MoUTran)  |  
E0306 | System Error (ACCUTran)  | 
E0310 | System Error (BillPay WAY UTran) |
E0311 | System Error (BillPay WAY Seq) |
E0350 | System Error (UTranStatus) |
E0360 | System Error (PERIUTran) |
E0370 | System Error (SearchTran) |
E0380 | System Error (chkc history) |
E0400 | System Error (Login) |
E0450 | System Error (NoFee) |
E0451 | System Error (GetFEE) |
E0460 | System Error (EXRate) |
E0470 | System Error (PhCountry) |
E0480 | System Error (PrePay Number) |
E0481 | System Error (PrePay update) |
E0482 | System Error (PrePay List) |
E0490 | System Error (Bin Lookup) |
E0491 | System Error (Merchant Bin Lookup) |
E0500 | System Error (BrdCorp) |
E0501 | System Error (BrdMer) |
E0502 | System Error (Upate DeviceProc) |
E0503 | System Error (Upate MerchProductProc) |
E0504 | System Error (Upate LogoProc) |
E0510 | System Error (Upate MerchantProc) |
E0511 | System Error (Upate OperatorProc) |
E0550 | System Error (Search Corporation) |
E0551 | System Error (Search Merchant) |
E0560 | System Error (Modify Schedule) |
E0561 | System Error (Modify Payment) |
E0600 | System Error (CCust) |
E0601 | System Error (CCustID) |    
E0610 | System Error (UCust) |      
E0611 | System Error (UCustID) |    
E0620 | System Error (SCust) |      
E0621 | System Error (CustDt)  |    
E0630 | System Error (ECustACC) |   
E0631 | System Error (ECust) |      
E0632 | System Error (Deactivate Cust Account) |    
E0650 | System Error (CRec) |   
E0651 | System Error (CRecID) |     
E0660 | System Error (URec) |   
E0661 | System Error (URecID) |     
E0670 | System Error (SRec) |   
E0671 | System Error (RecDt) |      
E0672 | System Error (CAdminTran) |     
E0673 | System Error (BoardFee) |   
E0713 | Transaction Key Expired  | Transaction Key provided in request is expired. Register new key with our system.
E0720 | System Error(Async Insert) |    
E0721 | System Error (Async Update) |   
E0722 | System Error (Async Call Fail) |    
E0723 | System Error (Async Select Fail) |      
E0724 | System Error (Key Gen Fail)  | System Error. Please contact help desk.
E0800 | System Error (KeyNox Error) |   
E0910 | Time out |      
E0911 | System Error |      
E0912 | Error on Host |     
D1001 | Account Number Invalid   | Account number provided in request is not a valid account number.
D1002 | Valid Account, Cash payments only.  |   
D1003 | Amount invalid. |   
D1004 | Biller ID Invalid. |    Biller ID provided in request is not valid.
D1005 | Cash only biller. |     
D1006 | Bill Pay Processor Code is missing or is incorrect. Processing host is not configured please contact help desk. | 
D1007 | One or more Fields missing or incorrect. |      
D1020 | Pre Pay Number not available. |     
D1201 | Unable to determine merchant ID. |  Merchant is not register with Mobilozophy.
D1202 | Unable to process your request. |   
D1203 | Invalid redemption code. |  Redemption code provided in request is invalid.
D1204 | Unable to determine coupon ID. |    Unable to determine coupon ID.
D1205 | Coupon not valid at this location. |    Coupon not valid at this location.
D1206 | Minimum Purchase Amount criteria not met. |     Minimum Purchase Amount criteria not met.
D1207 | Either end user ID or registration ID is required. |    Either end user ID or registration ID is required.
D1208 | Unable to modify coupon.  | Modification of coupon data is not allowed.
D1209 | Unable to modify coupon.  | Modification of coupon data is not allowed.
D1210 | Unable to modify coupon.  | Modification of coupon data is not allowed.
D1211 | Unable to modify coupon.  | Modification of coupon data is not allowed.
D1212 | This code has already been redeemed.  | This code has already been redeemed.
D1213 | This code has been deleted.   | This code has been deleted.
D1214 | Invalid store ID. |     Invalid store ID.
D1215 | Invalid amount. |   Amount provided in request is invalid.
D1217 | Coupon service is temporarily unavailable. |  Coupon service is temporarily unavailable.
D1999 | General Bill Pay Decline. |     General declined please contact help desk.
D2001 | Refer to Issuer.  | The merchant must call the issuer to obtain verbal authorization.
D2002 | Suspected Card (pick-up, hot-card)  | This credit card has been flagged for fraud. the merchant should call the number on the back of the card to obtain further instructions. Suspected card error occurs in the following scenarios: 1-The card is restricted by the issuer 2-Loss of card is reported 3-Theft of card is reported
D2003 | Honor with identification?   | The card is not identified.
D2004 | Invalid Amount   | The amount exceeds the limits established by the issuer for this type of transaction.
D2005 | Invalid Card     | The issuer indicates that this card is invalid.
D2006 | No such issuer   | The card issuer number is invalid.
D2007 | Invalid fee  | The transaction fee is unacceptable.
D2008 | Incorrect Pin    | The PIN entered by the cardholder is incorrect.
D2009 | Pin attempts exceeded    | The number of attempts to enter the PIN has exceeded.
D2010 | Key synchronization failed from the host     | The failure of a key synchronization from the host.
D2011 | Expired Card     | The card has expired.
D2012 | Insufficient Funds   | The credit limit for this account has exceeded, or the amount is not enough to perform the transaction.
D2013 | Invalid From Account     | The transaction account is invalid.
D2014 | Invalid To Account   | The transaction account is invalid.
D2015 | Withdrawal Limit exceeded    | The withdrawal limit on an account is exceeded.
D2016 | Withdrawal frequency exceeded    | The withdrawal frequency on an account is exceeded.
D2017 | Time limit for Pre-Auth reached  | The time for Pre-Auth has reached its limit.
D2018 | AVS FAILED   | The address verification has failed and the merchant is configured for auto decline on AVS failure.
D2019 | Billing ZIP Mismatch     | The zip provided does not match the billing address on file and merchant is configured for auto decline on ZIP code mismatch.
D2020 | CVV2 verification failed     | The V code provided is invalid or does not match what is on file and merchant set up for auto decline on CVV2 failure.
D2021 | Issuer or Switch inoperative     | The bank is unavailable to authorize this transaction.
D2022 | Duplicate transaction ( Same amount / Account)   | The transaction with same amount and account is performed twice.
D2023 | Balance unavailable for inquiry  | The balance cannot be validated.
D2024 | Check Digit Err  | The credit card number entered did not pass validation. Correct and re-enter the credit card number.
D2025 | Excluded Bin ID for Merchant     | Card is not allowed to do transaction at this merchant.
D2026 | Do not honor     | The transaction was declined by the issuer.
D2027 | AVS and CVV2 failed  | The address verification and V code verification failed and merchant set up for auto decline on AVS anc CVV2 failure.
D2028 |  Invalid Date    | The credit card expiration date is invalid. Verify and re-enter the expiration date.
D2029 |  Invalid Service | The service provided by the card is invalid.
D2030 |  Host Validation Error   | The host is an invalid host.
D2031 |  Activity Limit exceeded | The daily card activity limit has been exceeded.
D2032 |  Cannot complete because of Violation    | The transaction cannot be completed because the credit card account has been flagged with a violation.
D2033 |  Debit Pin Required |   
D2034 |  Debit Pin Required  | The BIN is blocked by the issuer.
D2035 |  Check Service authentication failure |     
D2039 |  Could Not Retrieve a Valid Card Number for Token |     
E2042 |  No Card found for the BIN No Card found for the BIN |  
D2200 |  UNKNOWN_ERROR |    
D2201 |  CONTENT_TYPE_NOT_SET |     
D2202 |  UNKNOWN_CONTENT_TYPE |     
D2203 |  CONTENT_LENGTH_NOT_SET |   
D2204 |  INCOMING_REQUEST_READ_ERROR  | 
D2205 |  OUTGOING_RESPONSE_SEND_ERROR |     
D2206 |  INPUT_VALIDATION_ERROR  |  
D2208 |  OCT_FAILED |   
D2209 |  AFT_FAILED |   
D2210 |  AFTR_FAILED |  
D2211 |  REMOTE_VPP_ERROR |     
D2212 |  INVALID_ISSUER_COUNTRY_CODE |  
D2213 |  FAST_FUNDS_NOT_ENABLED |   
D2214 |  INTERNAL_ERROR |   
D2215 |  ACNL_FAILED |  
D2216 |  ReceiverLimitExceeded |    
D2800 |  Invalid FCS ID |   
D2801 |  Invalid Voucher Serial Number |    
D2802 |  Invalid Voucher Approval Code |    
D2803 |  Electronics Benefit Transactions cannot contain Fee or Tax |  
D2998 |  PreFraudScout Decline   | Transaction is declined in Pre Fraud rules.
D2999 |  General Card Auth Decline   | This is a general decline error.
D3001 |  Invalid Bank Routing Number | Invalid routing number in the request message.
D3002 |  Invalid Bank Account Number | The bank account number in the request message is invalid.
D3003 |  Invalid MICR Data   | The MICR data in the request message is invalid.
D3004 |  Invalid Account Type    | The account type in the request message is invalid.
D3005 |  Invalid Check Type  | The check type in the request message is invalid.
D3006 |  Invalid Amount  | The amount for a transaction is invalid.
D3007 |  Missing Signature |     
D3008 |  Missing Endorsement  | 
D3009 |  Invalid Check Date  | The date format in the request message is invalid.
D3010 |  Car Lar Mismatch    | Mismatch between the check amount written in numbers (courtesy amount) and letters (legal amount) provided on check image.
D3011 | CallNox Timeout | 
D3012 | Duplicate Check  |
D3013 | Blocked Account | The account provided in transaction is blocked.
D3014 | Blocked Check   | The check provided in transaction is blocked.
D3015 | Cannot Process Image  |   
D3016 | Invalid Check Number    | The check number in the request message is invalid.
D3017 | Bank Account Closed | The bank account does not exist.
D3018 | Decline NSF  |
D3019 | Check Image Decline  |
D3020 | Invalid SEC  |
D3101 | Maker Check Return Stop Pay Limit Exceeded  | 
D3102 | Maker Check Return No Auth Limit Exceeded |   
D3103 | Maker Check Return No Settlement Limit Exceeded  | 
D3104 | Maker Check Return NSF/Other Limit Exceeded  | 
D3105 | Maker Check Return Limit Exceeded|     
D3106 | Customer Check Return Stop Pay Limit Exceeded |    
D3107 | Customer Check Return No Auth Limit Exceeded|      
D3108 | Customer Check Return No Settlement Limit Exceede|   
D3109 | Customer Check Return NSF/Other Limit Exceeded |   
D3110 | Customer Check Return Limit Exceeded |     
D3111 | Check Image Processing Error |     
D3112 | Customer Check Cashing Limit Exceeded|    
D3200 | Record(s) Processed Successfully |     
D3201 | Duplicate Custom Fields Not Allowed.  | Duplicate Custom Field Not Allowed.
D3202 | Item code already exists.   | Item code already exists.
D3203 | Custom Field Type cannot be modified during update. | Custom Field Type cannot be modified during update.
D3204 | Could not find Product for Update.  | Product is not registered in the system.
D3205 | Could not find Product for Removal. | Product is not registered in the system.
D3206 | Unidentified Tax Category   | Tax Category is not set in our system.
D3207 |  Some Record(s) Processed Successfully  |  
D3208 |  No Records Processed    | No records are processed further.
D3211 |  Parsing Failed  | Issue with request parameter.
D3212 |  Product Enroll Fail at Merchant Level   | Merchant level data is not added or updated in the system.
D3213 |  Item code not provided  | The item code in the request message is invalid.
D3214 |  Product Enroll Fail at Merchant Custom Level    | Merchant level custom data is not added and updated in the system.
D3215 |  Product Enroll Fail at Global Level | The UPC level data is not added and updated in our system.
D3216 | Product Removal Failed  | Product Removal Failed.
D3217 | No Tax Category Found   | No Tax Category Found.
D3218 | Category already exists | 
D3219 | Invalid Category Code |   
D3220 | Modifier already exists | 
D3221 | Invalid Modifier Code |   
D3222 | Variation already exists  |   
D3223 | Invalid Variation |   
D3224 | Invalid Product Code |    
D3225 | Duplicate Variation Option Fields Not Allowed |   
D3226 | Discount already exists | 
D3227 | Start Date should be current date or future date |    
D3228 | End Date should be current date or future date |  
D3229 | Invalid Discount Code |   
D3230 | No Product found for given search criteria. | No product is found for given search criteria.
D3231 | End Date should be greater than Start Date |  
D3232 | Discount amount should be less than Max Discount amount |  
D3233 | Discount percentage should be less than 100  |
D3234 | Max Discount amount should be less than Discount Qualifying amount |  
D3235 | Discount Code already removed  |  
D3236 | Already Associated |  
D3237 | Invalid role |    
D3238 | Invalid Operation |   
D3239 | Role Already Exist |  
D3240 | Operation Type Already Exist  |   
D3241 | Role does not Exist | 
D3242 | Role can not be Deleted  |
D3243 | Default Role can not be Modified |    
D3250 | Invalid modifierOptionDetails   | Invalid modifierOptionDetails
D3253 | Order service date can not be a previous date |   
E3254 | Order creation failed |   
E3255 | OrderID not found |    
E3256 | Order updation failed |   
E3257 | Order can not be modified |   
D3259 | Invalid modifier categoryCode  | Invalid modifier categoryCode
D3260 | Invalid product categoryCode   | Invalid product categoryCode
D3264 | currentPaymentSequenceNumber should be less than and equal totalPaymentCount   | The currentPaymentSequenceNumber value entered does not meet the required criteria.
D3999 | Check Auth Decline |  
D4000 | Invalid content, one of {encodedCardData, keyedCardData, returnTransactionData} group is required  |  
E4001 | Invalid Source Country Code | 
E4002 | Invalid Source Currency Code |     
E4003 | Invalid Destination Location |    
E4004 | Invalid Destination Currency Code   | 
E4005 | Invalid Source Agent |    
E4006 | Invalid Destination Agent |   
E4007 | Invalid Conversion Rate | 
E4008 | Invalid Fee | 
E4009 | Missing/Invalid Amount |  
E4010 | Missing/Invalid Payout Amount |   
E4011 | Invalid MTCN |    
E4012 | Duplicate transaction ( Same amount/ Account ). | 
E4050 | Missing /Invalid Sender Name  |   
E4051 | Invalid Sender ID Type |  
E4052 | Invalid Sender ID  |  
E4053 | Invalid Sender Address  | 
E4054 | Invalid Sender phone number | 
E4055 | Missing /Invalid Receiver Name |  
E4056 | Invalid Receiver ID Type |    
E4057 | Invalid Receiver ID  |
E4058 | Invalid Receiver Address |    
E4059 | Invalid Receiver phone number |   
E4060 | Missing / Invalid Input  |
E4999 | General Money Transfer Decline |  
E5000 | Invalid Money order Number.|  
E5001 | Invalid Amount.  |
E5002 | Missing Payee Name. | 
D5201 | Invalid Page size in the request  |   
D5202 | Invalid Report column name for requested report  |
D5203 | Invalid date range, redefine your search |    
D5204 | Invalid search column for requested report  | 
D5205 | Invalid optional column for requested report  |   
D5206 | Invalid Report column name for requested report | 
D5207 | Invalid/Expired Report data identifier  | 
D5208 | Invalid search column value for requested report |    
D5209 | No data found, please redefine your search  | 
D5210 | One or more duplicate columns used for search, sort or for optional columns  |
D5211 | Invalid search condition, transactionID is required  
D5212 | Invalid search condition, productCode is required    
D5213 | Service is temporarily unavailable.Please try later  
E5213 | Service is temporarily unavailable.Please try later  
D5214 | Invalid search condition, dateRange is required  
E5500 | Invalid Payroll Info     
E5599 | General Payroll Decline.     
E5999 | General Money order Decline  
E6000 | Missing /Invalid Name    
E6001 | Invalid ID Type  
E6002 | Invalid ID Number    
E6004 | Invalid Address | The address provided in the transaction is invalid.
E6005 | Invalid phone number    | The phone number provided in the transaction is Invalid.
E6006 | Invalid SSN | The SSN provided in the transaction is Invalid.
E6007 | Invalid DOB | The DOB provided in the transaction is Invalid.
E6008 | Missing/Invalid Gender   
E6009 | Missing/Invalid customer Image   
E6010 | Missing/Invalid ID Image     
E6011 | Missing/Invalid Finger Print Image   
E6012 | Biometric Auth failure   
E6013 | BFD failed   
E6014 | OTP failed   
E6050 | Duplicate Enrollment | The Customer is enrolled. Verify if the request is send twice for the same customer.
E6051 | OFAC Match   
E6052 | Blocked Customer     
E6053 | Blocked Biometrics   
E6054 | Declined Score below threshold.  |
E6055 | Customer Not Enrolled.  | The customer code provided in request is not registered.
E6056 | Financial Account Not Enrolled. | 
E6057 | Customer requested stop of specific recurring payment   | Customers request to stop recurring payments.
E6058 | Customer requested stop of all recurring payments from specific merchant    | Customers request to stop recurring payments from specific merchant.
E6059 | Missing Customer ID/External Customer Number    | Customer ID or external customer number is not provided in request.
E6060 | Inactive Customer   | Inactive Customer
E6061 | Invalid UID | UID number provided in request is invalid.
E6062 | Incorrect or No Card Indicator Value |    
E6063 | Customer Group Name already exists |  
E6064 | Invalid Customer Group code | 
E6065 | Customer Code already associated |    
E6066 | Invalid Customer Code  |  
E6067 | Search criteria not found.  | The search request does not include any search criteria fields.The search criteria includes the firstName, lastName, paymentInstrumentID, or customerID fields.
E6068 | External Customer number is already available. |   
E6069 | Invalid Search Criteria  |
E6071 | Customer modification not allowed, payment is in process.  | The application is unable to delete a customer record while a recurring transaction for the customer is processing.
E6072 | Transaction is in process. Please try again after some time.   | Simultaneous actions cannot be performed on the same customer record. The application is unable to perform edits on a customer record while the record is in use.
E6100 | Inactive Customer   | Inactive Customer.
E6901 | Duplicate Schedule Billing Reference Number | Duplicate schedule billing reference number.
E6902 | Payment Count Cannot be Greater than Processed Count    | Payment count cannot be greater than processed count.
E6903 | Next start date cannot be earlier than Current Date | Next start date cannot be earlier than current date.
E6904 | Schedule cannot be added without a Payment Methods (i.e. Card, Account...) |   
E6905 | Schedule not found   | 
E6906 | Invalid Schedule string |  
E6999 | General Customer Auth Decline   | General declined.
D7000 | Record not found    | The transaction requested is not available.
E7001 | Invalid User ID | The user Id provided in request message is invalid.
E7002 | Record Not Found.   | The Transaction is not present in the system.
E7003 | User Locked. Call CSR  |   
E7004 | Invalid Security Question/Answer    | Invalid security question and answer.
E7005 | User Already Logged in  | User Already Logged in try after some time.
E7006 | Your Password has Expired, Please change the password.  | Change your password.
E7007 | User Inactive. Call CSR  | 
E7008 | Operator Not Found  | The user ID is not registered in the system.
E7009 | Expired Client Password | The client password is expired.
E7010 | Invalid Host ID | The Host Id provided in the request message is invalid.
E7011 | Client Authentication Failed    | This message may occur for more than one reason. 1.  The manifest included in the request is not configured properly. 2.  The Domain Key included in the request manifest is expired. 3.  The Host Password included in the request is expired.
E7012 | Invalid user or password    | The User id and password is invalid.
D7013 | Multiple users with same email. Enter Login ID  |  
E7013 | Multiple users with same email. Enter Login ID  | There are multiple users with the same email ID. Enter your login ID.
E7014 | Invalid Manifest    | Manifest provided in the request message is invalid.
E7015 | Invalid Transaction Key | The transaction key provided in the request message is invalid.
E7016 | Invalid UserID or EmailID |    
D7017 | User Modification Request Failed   |   
E7018 | The provided authentication credentials are not correct |  
E7019 | Duplicate Questions/answers not allowed |  
E7020 | The question cannot be same as any of the answers |    
E7021 | Unable to process, retry with terminalNumber |     
E7022 | Unable to process, retry with profileName or profileID |   
E7023 | Unable to find profile|    
E7024 | Security question expired, please fetch a new question|    
E7027 | Unable to process, retry with userID    | Multiple entries found for the merchantID and emailID search criteria used.
E7100 | General Login Decline   | General Login Decline.
E7101 | User_ID already exists  | User_ID already exists.
E7102 | Operation not allowed   | Operation not allowed.
E7103 | Operator not Register/Present   | Operator is not Register or Present in the system.
E7104 | Last active admin operator in the system    | The last Active Admin operator in the system. At least one Admin operator should be active for a merchant.
E7105 | Admin operator cannot change his own status or type | The Admin operator cannot change his own profile details.
E7106 | Not allowed to add Administrator    | The operator is not allowed to add Admin operator.
E7107 | Input Password does not adhere to complexity norms  | The Input Password does not adhere to complexity norms.
E7108 | New password must not match previous password. Please enter a unique new password   | The new password must not match previous password. Please enter a unique new password.
E7109 | Suspended/Inactive User | The User Id provided in request is Suspended or Inactive. Please reactivate to perform transaction.
E7110 | Invalid password length | The password length must be between 8 and 20 characters.
E7111 | Parameter validation Error |   
E7112 | User already exists |  
E7113 | User Credential not active |   
E7114 | Security question not set for user  |  
E7200 | General User Admin Decline  | General User Admin Decline.
E7201 | Client not registered to our system.    | The client domain name or unique ID is not registered.
E7202 | Invalid Client Key  | The client key is invalid. Re-enter the correct key and resubmit the transaction.
E7203 | Client Validity Expired, Please re-register | The client validity is expired. Re-register the domain or the unique ID.
E7204 | Invalid merchant details |     
E7251 | Invalid one time password   | The one time password is invalid. Re-enter the correct password and resubmit the transaction.
E7252 | Duplicate one time password | The one time password is a duplicate. Re-enter the correct password and resubmit the transaction.
E7253 | One time password validity expired  | The one time password has expired. Generate a new password and resubmit the transaction.
E7254 | Host Operator not allowed   | The host operator is not allowed.
E7255 | Operator is not Host    | The operator ID for this transaction is invalid.
E7256 | Token services cannot be enabled until the merchant account is set up with a token zone | Tokenization service is not enabled for the merchant.
E7257 | Tokenization service not enabled    | Tokenization service not enabled for the merchant.
E7259 | De-tokenization service not enabled | De-tokenization service is not enabled for the merchant.
E7260 | De-tokenization UnSuccessful    | The token is invalid. Resubmit with a valid token number.
E7261 | Tokenization UnSuccessful   | Service is not available. Resubmit the transaction.
D7500 | record not found (backend)  | The record was not found.
D7501 | Chargeback Protection is not allowed    | The transaction is not eligible for Chargeback Protection.
E8000 | Customer not found  | Customer is not register in our system.
E8001 | Customer not enrolled   | Customer is not register in our system.
E8002 | Customer Declined   | Customer enrollment declined.
E8003 | Customer Locked | Customer is locked in system.
E8004 | Invalid user or password    | Invalid user or password.
E8005 | Credit Limit Reached    | Max Credit Limit Reached.
E8006 | Local Opt Out   | Local Opt Out.
E8007 | Invalid Message | Invalid Message
E8008 | Globally Opted Out phone number | Globally Opted Out phone number
F8009 | Invalid Email   | Invalid Email
E8900 | System Error    | System Error.
E8999 | General Notify Decline. | General Notify Decline.
D9000 | Amount Limit Exceeded   | Amount Limit Exceeded for transaction.
D9001 | Transaction count Limit Exceeded    | Transaction count Limit Exceeded.
D9002 | Device activity Limit Exceeded  | Device activity Limit Exceeded.
D9003 | Amount per days Limit Exceeded  | Amount per days Limit Exceeded.
D9004 | Excluded Customer   | Customer is excluded to perform transaction at this merchant.
E9005 | Invalid Cashback amount | Cash back amount provided in request is invalid. Cash back should always be less then transaction amount.
E9006 | Cashback Amount is not allowed for this type of transaction | Cashback Amount is not allowed for this type of transaction.
D9007 | Maximum Line Items Exceeded Maximum Line Items Limit Exceeded |    
D9008 | BC limit not set for merchant. BC limit not set for merchant. |    
D9009 | BC buffer percent was not set for merchant. BC buffer percent was not set for merchant |   
D9010 | Invalid Transaction_Info/Service_Code.  | The error occurs in the scenarios as follows: 1-Service code is missing in the request. 2-Service code is invalid. 3-Service code is inadequate for this type of transaction.
D9011 | Net Balance is less than zero |    
D9012 | Invalid Merchant_Info/Agent_Chain_Number must be 6 bytes  |    
D9013 | Invalid Transaction type Invalid Transaction type  |   
D9014 | Merchant Per Transaction Deposit Limit Exceeded Transaction Deposit Limit Exceeded  |  
D9015 | Head Quarter merchant not found. Head Quarter merchant not found. |    
D9018 | No Valid Data Found,Please Generate Token First.  |    
D9019 | Invalid Token|     
D9020 | Invalid Transaction_Info/SubServiceCode | The error occurs in the scenarios as follows: 1- Sub service code is missing in request. 2- Sub service code is invalid. 3- Sub service code is inadequate for this type of transaction.
D9021 | Invalid Transaction_Info/Type.  | The error occurs in the scenarios as follows: 1-Type is missing in request. 2-Type is invalid. 3-Type is inadequate for this type of transaction.
D9022 | Invalid Transaction_Info/Transaction_ID | Transaction ID is expected in request for this transaction. Re-enter the Transaction ID, and resend the transaction.
D9030 | Invalid Device_Info/Device_Type | Device is inadequate to do this type of transaction.
D9040 | Invalid Processor_Info/Acquirer_Institute must be 6 bytes   | Acquirer_Institute must be 6 bytes.
D9050 | Invalid Processor_Info/Proc_Merchant_Id must be 12 bytes    | Proc_Merchant_Id must be 12 bytes.
D9070 | Invalid Processor_Info/Processor_Term must be 4 bytes   | Processor_Term must be 4 bytes.
D9080 | Invalid Processor_Info/Store_Number must be 4 bytes | Store_Number must be 4 bytes.
D9090 | Invalid Merchant_Info/MerchantType  | The merchant name is invalid or is not present.
D9091 | Invalid Merchant_Info/Name  | 
D9092 | Invalid Merchant_Info/City |  
D9093 | Invalid Merchant_Info/State|  
D9094 | Invalid Merchant_Info/TimeZone must be 3 bytes   |
D9096 | Invalid Transaction_Info/Time_Stamp must be [MMDDYY HHMMSS]|  
D9100 | Fee Configuration Level must be mentioned |   
D9101 | Fee not configured |  
D9110 | Invalid Merchant_Info/SICCODE must be 4 bytes. |  
D9111 | Invalid Processor_Info/Sequence_Number must be 4 bytes|    
D9120 | Invalid Card_Info/PIN   |  
D9130 | Invalid Transaction_Info/Country_Code must be at least 2 bytes. |  
D9140 | Invalid Card_Info/Type|    
D9210 | Invalid Card_Info/PIN |    
D9211 | Invalid Card_Info/Token |  
D9212 | Invalid Card_Info/KSN must be at least 16 chars  | 
D9240 | Invalid Merchant_Info/Agent_Bank_Number must be 6 bytes. |     
D9250 | Invalid Merchant_Info/Agent_Chain_Number must be 6 bytes |     
D9260 | Invalid Processor_Info/Batch_Number must be 3 bytes |  
D9270 | Invalid Merchant_Info/Reimburse_Attr must be 1 byte|   
D9280 | Invalid Merchant_Info/ABA_Number must be 9 bytes |     
D9282 | Invalid Merchant_Info/Settle_Agent_Number must be 4 bytes|     
D9283 | Check Out date can not be less than or equal to Check In date |    
D9284 | Transaction amount should not be greater than authorized amount |  
D9286 | Invalid CheckoutID  | The checkOutID provided is incorrect. Provide the correct checkOutID.
D9287 | Duplicate card sequence number |   
E9288 | Invalid Wallet Identifier Format |     
E9289 | Encoded Data is not allowed with checkoutID  | 
D9290 | Invalid Transaction_Info/Orig_Purchase_Date must be MMDDHHMM |     
E9291 | Keyed Card Data is not allowed with checkoutID  |  
E9292 | Mandatory Tags are missing |   
E9293 | Card Type not supported for requeted service |     
E9294 | terminalData {terminalCapability, terminalOperatingEnvironment, cardholderAuthenticationMethod, terminalAuthenticationCapability, terminalOutputCapability,maxPinLength} group is required |   
E9295 | cardholderAuthenticationMethod must be PIN |   
D9500 | Encryption services not enabled for the device  | Encryption services are not enabled for the device.
D9501 | Encryption service requested not enabled for the device | The device has encryption service but the encryption service requested is not enabled.
D9502 | Encryption method could not be determined   | The device is configured with more than one encryption service. The request does not indicate which service to use.
D9503 | Decryption unsuccessful | The decryption failed.
D9504 | Invalid Format Id  |   
D9505 | Product Details is required to perform this action |   
D9510 | Invalid Encryption Type | An invalid encryption type was included in the request.
D9511 | A unique KSN not generated or not sent in request   | The application was unable to generate or submit the key serial number.
D9610 | Data Parsing fail   | Data parsing failed.
D9611 | Encrypted Data not generated or not sent in request | Encrypted data was not received from the host or the application was unable to include this data in the response.
F9900 | XSD Format Error    | XSD Format Error
F9901 | Format Error field details  | Format Error field details
F9902 | Group encodedCardData is not allowed with cardDataSource value MANUAL  |  
F9903 | Group encodedCardData is not allowed with cardDataSource value PHONE |    
F9904 | Group encodedCardData is not allowed with cardDataSource value EMAIL|     
F9905 | Group encodedCardData is not allowed with cardDataSource value INTERNET|  
F9906 | Group keyedCardData is not allowed with cardDataSource value SWIPE |  
F9907 | Invalid cardDataSource for requested service.|    
F9908 | Sum of elements of group additionalCharges should not be greater than transactionAmount.|     
F9909 | cashTendered must not be less than transactionAmount|     
F9910 | lastChipRead is Mandatory with Fallback Swipe (Icc Terminal Error) transaction|   
F9911 | lastchipRead is not Allowed with Fallback Swipe (Empty Candidate List) transaction |  
F9912 | Invalid content, one of {track1Data, track2Data, track3Data} is required |    
F9913 | Invalid content, {encodedCardData, keyedCardData or swipedCardData} is not Allowed with Chip Card |   
F9914 | emvFallbackCondition is Mandatory with Fallback Swipe transaction  |  
F9915 | voidReason is Mandatory for Chip Card transaction |   
F9916 | Fallback Swipe allowed with track2Data only  |
F9917 | Invalid emvTags, {9F1F or 9F20 or 57 or 5A} | Tags 9F1F, 9F20, 57 and 5A are not allowed if encryptionType is VOLTAGE
F9918 | Invalid content, {track1Data, track3Data, emulatedTrackData} is not Allowed with Chip Card and encryptionType   | track1Data, track3Data, and emulatedTrackData are not allowed if encryptionType is VOLTAGE











# Other Errors

Error Description | Reason | Location
--------- | ------- | -------
"Unable to decode the Model" | The Json you sent doesn’t match a valid input | Any endpoint.
“The (entity) could not be saved because of an unmanaged exception” | An unknown reason didn’t allow the system to save the entity. Please contact support. | Any endpoint
“Update (entity) did not finish correctly because of an unmanaged exception”    | An unknown reason didn’t allow the system to update the entity. Please contact support.   | Any endpoint
“The (entity) could not be retrieved because of an unmanaged exception” | An unknown reason didn’t allow the system to retrieve the entity. Please contact support. | Any endpoint
“No Devices were found for the given User/Merchant relationship”    | You sent a device GUID that does not belong to a merchant administrated by the logged user. Please try with a different GUID. | Any transaction endpoint
“No Merchant was found for the current User”    | You sent a device GUID that does not belong to a merchant administrated by the logged user. Please try with a different GUID. | Any transaction endpoint
“Invalid Routing Number: invalid digit” | You sent and invalid Routing Number. Please try with a different one. | Create Bank Clearing
“Invalid Routing Number: digit check failed”    | You sent and invalid Routing Number. Please try with a different one. | Create Bank Clearing
“An error occurred while executing Transaction General Check”   | There is some inconsistence between your devices, Merchant Processor accounts and Merchants. Please contact support.  | Any transaction endpoint.
“Invalid Device GUID”   | You sent a device GUID that does not exist at all. Please try with a different GUID.  | Any transaction endpoint.
“BankAccount reference not found.”  | You sent an invalid BankAccount GUID. Please try with a different GUID.   | Create Bank Clearing.
“At least one of SSN4 or DateOfBirth are required.” | You attempted to create a bank clearing without providing SSN4 or Date of Birth of the bank account owner. Please provide any of those two.   | Create Bank Clearing.
The DriverLicenseState and DriverLicenseNumber fields are required. | You didn’t provide Driver License State nor Driver License Number.    | Create Bank Clearing.
“The transaction was not originated in any device of the current user.” | You are trying to void or refund a sale or bank clearing that was run on a device that is not administrated by the logged user.   | Void Bank Clearing, Refund Bank Clearing, Void Sale, Refund Sale
“There was a database error”    | There was some inconsistence on the database, please contact support. | Any endpoint.
“The related clearing is not settled. It can be voided only”    | You attempted to refund a not settled bank clearing.  | Refund Bank Clearing.
“The related clearing is already voided”    | You attempted to void or refund an already voided Bank Clearing.  | Void Bank Clearing, Refund Bank Clearing.
“The related clearing is already returned”  | You attempted to void or refund an already refunded Bank Clearing.    | Void Bank Clearing, Refund Bank Clearing.
“The related clearing was not processed, therefore it cannot be voided” | You attempted to void or refund a Bank Clearing that didn’t run successfully. | Void Bank Clearing, Refund Bank Clearing.
“Invalid (entity) GUID” | You attempted to void or refund a Bank Clearing or a Sale or to capture an Auth Only using an invalid Guid.   | Capture Auth Only, Void Sale, Refund Sale, Void Bank Clearing, Refund Bank Clearing.
“No (entity) could be found for the given Id”   | You attempted to updated or retrieve an entity using an invalid Guid. | Any endpoint.
“Credit Cards not allowed for this processor”   | You attempted to run a Sale or Auth Only on a Merchant Processor Account that is Debit Only.  | Create Auth Only, Create Sale.
“Debit Cards not allowed for this processor”    | You attempted to run a Sale or Auth Only on a Merchant Processor Account that is Credit Only. | Create Auth Only, Create Sale.
“The (transaction) could not be processed correctly. Error code (error code). Error message: (error message)”   | The transaction didn’t run for reason provided by the processor. If you have any question regarding this error, please contact support.   | Any transaction endpoint.
“No open batch” | For some reason, there was no open batch when you attempted to run a sale or auth only. Please contact support.   | Create Auth Only, Create Sale.
“Invalid CardDataSource”    | You provided an invalid card data source. | Create Auth Only, Create Sale.
“The (transaction)  amount must be greater than zero”   | You provided a negative or zero amount for the transaction.   | Any transaction endpoint.
“The (transaction) could not be fetched because of an unmanaged exception”  | Something when wrong when trying to retrieve the entity. Please contact support.  | Any endpoint.
“There is an approved Capture for this AuthOnly already”    | You attempted to capture an already captured Auth Only.   | Create Capture.
“The referenced AuthOnly and this Capture devices are not from the same MerchantAccount”    | You attempted to capture an AuthOnly using a device guid belonging to a different merchant processor account. | Create Capture.
“The Capture AuthOnlyGuid value does not have matching results.”    | You attempted to capture an AuthOnly using an invalid Auth Only Guid. | Create Capture.
“There is no Batch for the given guid.” | For some reason, there was no open batch when you attempted to run the transaction. Please contact support.|  Any transaction endpoint.
“Only one of Bank Account or Card can be used.” | You sent both, a Credit Card and a Bank Account, when trying to create a Recurring Billing    | Create Recurring Billing.
“The device hierarchy and settings could not be loaded” | Something is wrong with the merchant processor account of the device you provided. Please contact support.    | Create Recurring Billing.
“The current Merchant Processor Account does not allow Tokenization. Tokenization is required to create Recurring Billings.”    | You provided a device guid belonging to a merchant processor account that does not have Tokenization activated.   | Create Recurring Billing.
“The setup of this device does not allow this operation. Processor Transaction Type is not Card nor ACH”    | You provided a Credit Card for an ACH Merchant Processor Account or a Bank Account for a Credit Card Merchant Processor Account.  | Create Recurring Billing.
“Required data for a Recurring Billing with Card are: FirstName, LastName, Email and Phone” | You’re missing at least one of First Name, Last Name, Email or Phone when trying to create a Recurring Billing using a credit card.   | Create Recurring Billing.
“An Email is required for a Recurring Billing with ACH” | You’re missing e-mail address when trying to create a Recurring Billing using a Bank Account  | Create Recurring Billing.
“There are no payments to be generated for the given parameters (StartDate, EndDate, PaymentCount, Interval, IntervalValue)”    | You sent a combination of parameters that didn’t result in any possible payment.  | Create Recurring Billing.
“Invalid Card or Bank Account”  | You provided an invalid credit card or an invalid bank account when trying to create a recurring billing. | Create Recurring Billing.
“Customer data not present” | You provided a Credit Card or Bank Account without the customer’s information.    | Create Recurring Billing.
“Invalid Interval. The 'every' value must be the only value.”   | You sent both, an “every” interval and a list of custom dates | Create Recurring Billing.
“Invalid Interval/IntervalValue”    | You provided an invalid combination of the interval and its values.   | Create Recurring Billing.
“An error occurred while parsing the CustomDates”   | At least one of the custom dates you provided is not properly formatted   | Create Recurring Billing.
“There is a date in the CustomDates smaller than the StartDate” | At least one of the custom dates you provided is previous to the StartDate you provided.|     Create Recurring Billing.
“The EndDate must be greater than the greatest CustomDate”  | At least one of the custom dates you provided is posterior to the EndDate you provided.   | Create Recurring Billing.
“EndDate must be submitted for CustomDates” | You provided a list of CustomDates but not an EndDate | Create Recurring Billing.
“Invalid InertervalValue: this interval does not allow values”  | You selected an interval that does not require interval values.   | Create Recurring Billing.
“Invalid Interval, you must explicit an IntervalValue”  | You selected an interval that requires IntervalValue and you did not send it. | Create Recurring Billing.
“Invalid Interval”  | You sent a wrong Interval name.   | Create Recurring Billing.
“End Date must be after start date” | You sent an End Date that is posterior to the Start Date you sent | Create Recurring Billing.
“The Start Date cannot be earlier than today”   | You sent a Start Date previous to the current date.   | Create Recurring Billing.
“Just one of End Date or Payment Count values are required, not both”   | You sent both “EndDate” and “PaymentCount”. Please send one or the other one, but not the both together.  | Create Recurring Billing.
“Either one of End Date or Payment Count values are needed” | You didn’t send “EndDate” neither “PaymentCount”. Please send at least one of them.   | Create Recurring Billing.
“The Payment Count value must be greater than 0”    | You sent PaymentCount with a value equal or less than 0   | Create Recurring Billing.
“The current status of the Recurring Billing is final and does not allow any updates.”  | The Recurring Billing finished on scheduled, was deactivated or wasn’t created correctly so its status cannot be change.  | Update Recurring Billing.
“Invalid Status”    | You sent a wrong status name. | Any Update endpoint.
“Invalid Amount, it cannot be 0. If you wish to cancel the Recurring Billing, set the status to Deactivated”    | You sent Amount with a 0 value.   | Update Recurring Billing.
“A database related error occurred B.RBB.04”    | Something went wrong when trying to retrieve the scheduled payments. Please contact support.  | Get Recurring Billing.
“Either the SaleGuid or SaleReferenceNumber are required”   | You didn’t send SaleGuid neither SaleReferenceNumber. Please send one of them.    | Create Return.
“You can't return a sale run more than 180 days ago”    | You attempted to return a sale that was run 180 days ago. | Create Return.
“The Return Amount must be greater than zero”   | You sent Amount with a value equal or less than 0 | Create Return.
“Original Amount exceeded”  | You attempted to return an amount greater than the original sale amount   | Create Return.
“Sale has been voided”  | You attempted to return a voided sale.    | Create Return.
“Sale has not been settled” | You attempted to return a sale of which batch has not been closed.    | Create Return.
“The referenced Sale and this Return devices are not from the same MerchantAccount” | You sent a device guid belonging to a merchant processor account different from the one where the sale was run.   | Create Return.
“The current invoice status does not allow a payment”   | Invoice was already paid, cancelled or has not been sent yet so it cannot be paid.    | Create Sale.
“The sale amount does not match the invoice amount” | You sent and Invoice Guid, but the sale amount does not match the amount of that invoice. | Create Sale.
“The calculated amount after Discount and/or ServiceFee and GrossAmount does not match the Amount value”    | You sent GrossAmount and Discount and/or ServiceFee, but the amount you sent does not match the result of Amount=GrossAmount-Discount+ServiceFee  | Create Sale.
“If Discount and/or ServiceFee are submitted, the GrossAmount value is required”    | You sent Discount and or ServiceFee but you didn’t send GrossAmount.  | Create Sale.
“Unable to find the device where the original sale was run.”  | You’re trying to charge a fee (or to reattempt a sale) over sale we cannot find the device where it ran. Please contact support.  | Charge Fee, Reattempt sale.
“There are no active Fee Charger devices for your Merchant Processor Account”   | You’re trying to charge a fee but you haven’t setup a Fee Charger device yet. | Charge Fee.
“Unable to charge fee for sale” | System was not able to charge the fee for the selected sale for some unknown reason. Please contact support.  | Charge Fee.
“Unable to run sale again”  | System was not able to run the selected sale again for some unknown reason. Please contact support.   | Reattempt sale.
“Original Sale not found”   | You sent an invalid Sale Guid | Reattempt sale.
“Card for original Sale not found”  | We were not able to find the card used on the original sale. Please contact support.  | Reattempt sale.
“Tax Rate submitted without declaring Tax Type” | You sent TaxRate but not TaxType. | Create Sale, Create AuthOnly, Create Capture.
“Tax Rate submitted without declaring Tax Amount”   | You sent TaxRate but not TaxAmount.   | Create Sale, Create AuthOnly, Create Capture.
“An error occurred while saving the EnhancedData”   | Something went wrong when trying to save tax information. Please contact support. | Create Sale, Create AuthOnly, Create Capture.
“An error occurred while retrieving the EnhancedData”   | Something went wrong when trying to get tax information. Please contact support.  | Get Sale, Get AuthOnly, Get Capture.
“The underlying Sale was not processed correctly. A Tip Adjustment cannot be made”  | You attempted to add a tip to a sale that didn’t run successfully.    | Create Tip Adjustment.
“The TipAdjustment could not be processed correctly B.TA.C01”   | Something went wrong when trying to add the tip to the sale. Please contact Support.  | Create Tip Adjustment.
“The Sale belongs to a closed batch. Tip Adjustments can only be made on open batches B.TA.C02” | You attempted to create a tip for a sale that is already settled. | Create Tip Adjustment.
“Sale not found B.TA.C03”   | You sent an invalid Sale Guid | Create Tip Adjustment.
“Invalid Device GUID B.TA.C04”  | You sent an invalid Device Guid.  | Create Tip Adjustment.
“Card Verification does not allow Swiped requests. B.VB.C06”    | You sent track1 and/or track2. Please send a keyed card.  | Create Verify.
“Card Verification does not allow EMV requests. B.VB.C07”   | You sent EMV data. Please send a keyed card.  | Create Verify.
“There is no card match for the given Token. B.VB.C01”  | You sent a tokenized card but we were not able to find the original card number.  | Create Verify.
“The Verify could not be processed correctly. B.VB.C05” | The Verify didn’t run successfully for an unknown reason. Please contact support. | Create Verify.
“One of SaleGuid, AuthOnlyGuid, ReturnGuid, SaleReferenceNumber, AuthOnlyReferenceNumber or ReturnReferenceNumber fields is required”   | You didn’t say any Guid or Reference Number of the transaction you want to void.  | Create Void.
“Only one of SaleGuid, AuthOnlyGuid, ReturnGuid, SaleReferenceNumber, AuthOnlyReferenceNumber or ReturnReferenceNumber fields can be accepted”  | You sent more than one Guid belonging to different kind of transactions.  | Create Void.
“The AuthOnly cannot be voided because it has been captured already”    | You attempted to void an AuthOnly that has been already captured. Please try voiding the Sale generated instead.  | Create Void.
“No device found for the underlying transaction”    | No active device was found for the transaction you’re trying to void. | Create Void.
“Sale cannot be voided because it was not processed”    | You attempted to void a sale that didn’t run successfully.  | Create Void.
“AuthOnly cannot be voided because it was not processed”    | You attempted to void an auth only that didn’t run successfully.    | Create Void.
“Return cannot be voided because it was not processed”  | You attempted to void a return that didn’t run successfully.    | Create Void.
“Transaction already settled”   | You attempted to void a transaction of which batch has already been closed  | Create Void.
“No open batch available”   | For some reason, there is no open batch to process your transaction. Please contact support.    | Any Create Transaction endpoint.
“Expiration date is required if the Card number is being sent. B.CB.GS01”  |  You sent a Card Number but no expiration date.  | Create Sale, Create AuthOnly, Create Recurring Billing, Create Verify.
“Expiration date is required if the Card is being swiped. B.CB.GS02”    | You sent track1 and/or track2 but no expiration date.   | Create Sale, Create AuthOnly.
“The current Merchant Processor Account does not allow Swiped transactions. B.CB.GS03”  | You provided a Device Guid that belongs to a Merchant Processor Account that does not allow swiped transactions. Please try with a different device Guid.   | Create Sale, Create Auth Only.
“The current Merchant Processor Account does not allow EMV transactions. B.CB.GS04” | You provided a Device Guid that belongs to a Merchant Processor Account that does not allow EMV transactions. Please try with a different device Guid.  | Create Sale, Create Auth Only.
“For EMV, required fields are EMVTags and ExpirationDate B.CB.GS05” | You’re missing either EmvTags or ExpirationDate fields (or both) when trying to run an EMV transaction. | Create Sale, Create Auth Only.
“Tags missing B.CB.GS06”    | You’re missing at least one required tag for an EMV transaction.    | Create Sale, Create Auth Only.
“Unexpected error: Card without token.”|  We were not able to tokenize the card. Please contact support.  | Create Sale, Create AuthOnly, Create Recurring Billing, Create Verify.
“The Card tokenization could not be processed.”|  Processor were not able to tokenize the card. Please contact support.   | Create Sale, Create AuthOnly, Create Recurring Billing, Create Verify.
“Card number, track1data or track2data missing” | You didn’t say any of cardNumber, track1data or track2data. Please provide at least one of them.  | Create Sale, Create AuthOnly, Create Recurring Billing, Create Verify.
“Invalid DriverLicenseState name”   | You provided a wrong short name for Driver License State  | Create Bank Clearing, Create Bank Account.
“As a Credit Card device, the ProcessorId, ProcessorOperatingUserId and ProcessorPassword data are required.”   | You’re missing at least one of ProcessorId, ProcessorOperating UserId or ProcessorPassword    | Create Device.
“As an ACH device, the ProcessorLocationId data is required.”   | You’re missing ProcessorLocationId    | Create Device.
“As an ACH device, the TerminalNumber data is required.”    | You’re missing TerminalNumber.    | Create Device.
“Only one Fee Charger active device is allowed per Merchant Processor Account”  | You’re trying to create a device with FeeCharger=true for a Merchant Processor Account that already has an active Fee Charger Device  | Create Device.
“Only one Mobile active device is allowed per Merchant Processor Account”   | You’re trying to create a device with IsMobile=true for a Merchant Processor Account that already has an active Mobile Device.    | Create Device.
“In order to create a Virtual Terminal, the Merchant Processor Account where you're attempting to create this device must have setup an Auto Close Batch time.”|    You’re trying to create a device with IsVirtualTerminal=true for a Merchant Processor Account that hasn’t set an Auto Close Batch time yet. | Create Device.
“The processor failed to return the Device Parameters. B.DB.04” | Processor was not able to return the device data. Please contact support. | Create Device.
“An error occurred while requesting the processor parameters for this device. B.DB.02”  | System was not able retrieve device data from the processor. Please contact support.  | Create Device.
“An error occurred while requesting the processor parameters for this device. B.DB.03”  | Something went wrong when system sent device parameters to processor. Please contact support. | Create Device.
“Device updated. Device status changed to Paused because of an error.”  | Device was updated, but something went wrong so it was paused. Please contact support.|   Create Device.
“There are no active Fee Charger devices for your Merchant Processor Account”   | No active device with FeeCharger=true was found.  | Charge Fee.
“checkIpIsAllowed failed to check if endpoint is allowed to be run from IP” | You tried to run an endpoint that is not allowed to be run from your IP address.|     Protected endpoints.
“The user does not have permission to do this”  | You attempted to run an endpoint that is not allowed for your user profile.   | Any endpoint.
“A server error has occurred. The operation could not be finished.” | A general error occurred. Please contact support. | Any endpoint.
“Hosted Payment Page Request expired”   | You attempted to see the preview or to confirm the transaction of a Hosted Payment Page request of which token has already expired.   | HostedPaymentPageRequests – Get Preview, HostedPaymentPageRequests – Confirm Transaction
“Hosted Payment Page Request TempToken already used”    | You attempted to see the preview or to confirm the transaction of a Hosted Payment Page request of which token has already been used. | HostedPaymentPageRequests – Get Preview, HostedPaymentPageRequests – Confirm Transaction
“SendDate cannot be earlier than today” | You sent a SendDate previous to current date. | Create Invoice.
“PaymentDate cannot be earlier than today”  | You sent a PaymentDate previous to CurrentDate    | Create Invoice.
“Send Status not allowed”   | You sent a SendSatus different from “Draft” or “Scheduled To be Sent” | Create Invoice.
“Invalid SendStatus”    | You sent a wrong SendStatus   | Create Invoice.
“Invalid PaymentTerm”   | You sent a wrong PaymentTerm  | Create Invoice.
“Forbidden SendStatus”  | You’re trying to update the SendStatus of an invoice that has been already sent and cannot be updated.    | Update Invoice.
“Invalid PaymentStatus” | You sent a wrong PaymentStatus    | Update Invoice.
“A database related error occurred B.Iv.01” | We were not able to retrieve the invoice because of a database error. Plase contact support.  | Get Invoice
“The current invoice status does not allow modifications”   | Invoice has been already sent and cannot be updated.  | Update Invoice, Update Invoice Detail
“The current detail is deleted, no changes are allowed” | Invoice Detail has been deleted and cannot be updated.    | Update Invoice Detail.
“A database related error occurred B.IvD.01”    | Invoice Detail or Invoice Reminder could not be retrieved because of a database error. Please contact support. | Get Invoice Detail, Get Invoice Reminder.
“For a custom reminder, date is required”   | You sent a customized reminder but you didn’t include the reminder’s date.    | Create Invoice Reminder.
“The current invoice payment status does not allow a new reminder”  | Invoice is already paid and a new reminder cannot be created. | Create Invoice Reminder.
“Reminder date cannot be earlier than send date”    | You attempted to generate a reminder of which date is previous to the send date of the invoice.   | Create Invoice Reminder.
“Reminder date cannot be earlier than today”    | You attempted to generate a reminder of which date is previous to current date.   | Create Invoice Reminder.
“For this invoice there is an active reminder for this date already”    | You attempted to generate a reminder of which date is the same one another active reminder for the same invoice has.  | Create Invoice Reminder.
“The reminder is completed, it cannot be updated”   | You’re trying to update a reminder that has already been executed.    | Update Invoice Reminder.
“Days not specified”    | Something went wrong when trying to calculate the date of the reminder. Please try with a different value.    | Create Invoice Reminder, Update Invoice Reminder.
“Invalid Parent Iso Number” | You provided a wrong Iso Number for ParentIso | Create Iso
“Iso Fees and Discount rate can be null or zero, but cannot be negative”    | You sent a negative amount for at least one of the fees.  | Create Iso
“User does not have the Merchant Admin role assigned.”  | You assigned as merchant’s admin a user that does not have the Merchant Admin Role    | Create Merchant
“Merchant Processor Account Fees and Discount rate can be null or zero, but cannot be negative” | You sent a negative amount for at least one of the fees.  | Create Merchant Processor Account
“AutoClose is available for Credit Card only”   | You attempted to set “AutoClose=true” for an ACH Merchant Processor Account.  | Create Merchant Processor Account, Update Merchant Processor Account
“Subisos cannot setup fees lower than those established by its Parent Iso”  | You attempted to create or update a merchant processor account and setup fees lower than the ones your parent iso charges.    | Create Merchant Processor Account, Update Merchant Processor Account
“Invalid context. Allowed values are: Invoice, i, RecurringBilling, rb” | You sent an invalid context   | Get Merchant Product, Get Merchant Product List
“A database related error occurred B.MPB.01”    | Something went wrong at the database level when trying to retrieve something related with merchant product.|      Get Merchant Product, Get Merchant Product List, Get Merchant Product List Detail.
“The authenticated user does not have permission to perform this operation.”    | Your user role does not have permission to run the endpoint you’re pointing to. Please try with a different endpoint or contact support to get a new role.    | Any endpoint.
“Invalid State name”    | You sent a wrong US State short name. | Any endpoint.
“Invalid Invoice Customer”  | You sent a wrong InvoiceCustomer Guid | Create Invoice, Update Invoice.
“Iso number not found”  | You sent a wrong Iso Number   | Register Account
“Unable to remove Merchant Admin Profile. User is still admin of the following merchants:”  | You’re trying to remove the Merchant Admin profile to a user who is still admin of some merchants. Remove the user from the admin list on each of the merchants listed    | Update User Role
“Unable to decode the Model”    | You sent a wrong Json for the endpoint you pointed to | Any endpoint.
“Invalid ModelState”    | You sent a wrong Json for the endpoint you pointed to | Any endpoint.
“NoResultFound204”  | You sent a search or called a Get All endpoint that generated no results  | Any get all endpoint, Any search endpoint.
“Unable to reach entity”    | You sent an invalid Guid  | Any get endpoint.
“Wrong emailType”   | You sent an invalid e-mail address    | Resend Email.
“Invalid TempToken” | You sent a wrong temporal token   | HostedPaymentPageRequests – Confirm Transaction
“Operation is only available for Sandbox url”   | You attempted to create a user using an endpoint only available for Sandbox   | Create User
“Iso is not active.”    | You sent an Iso Number belonging to an inactive iso   | Register Account.
“Username does not exist”   | You sent a username that does not exist at all    | Reset Password Request.
“Key expired”   | You attempted to reset a password using a key that is expired.    | Reset Password.
“Key does not exist”    | You attempted to reseat a password using a key that does not exist at all.    | Reset Password.
“Invalid State” | You sent a wrong US State short name. | Any endpoint.
“Invalid Status”    | You sent a wrong status name. | Any endpoint.

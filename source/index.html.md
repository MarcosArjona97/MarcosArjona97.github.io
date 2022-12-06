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

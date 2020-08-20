# Using the Payment-Integration Library for .Net Framework and .Net Core

Supports .NET Framework, .NET Core 1.1, .NET Core 2.0 runtimes, .NET Core 2.1 runtimes
## Installation

To install Payment Integration, run the following command in the Package Manager Console

```
Install-Package PaymentIntegration -Version 1.0.1
```
https://www.nuget.org/packages/PaymentIntegration/1.0.1

# Usage
### Auth

```csharp
//For security
var paymentOptions = new PaymentOptions(
                "url",
                "yourApiKey",
                "yourSecretKey");
                
AuthRequest req = new AuthRequest();
authReq.Amount = 2;
authReq.CardNo = "1111111111111111";
authReq.Currency = 978;
authReq.Cvv2 = "111";
authReq.Ecommerce = true;
authReq.Expiry = YYMM;
authReq.Lang = "TR";

Payment paymentManager = new Payment(paymentOptions);
var payment = await paymentManager.Auth(authReq);
```
### Auth3D

```csharp
var paymentOptions = new PaymentOptions(
                "url",
                "yourApiKey",
                "yourSecretKey");
                
var auth3DReq = new Auth3DRequest();
auth3DReq.Amount = 2;
auth3DReq.CardNo = "1111111111111111";
auth3DReq.Currency = 978;
auth3DReq.Cvv2 = "111";
auth3DReq.Ecommerce = true;
auth3DReq.Expiry = YYMM;
auth3DReq.Lang = "TR";

Payment paymentManager = new Payment(paymentOptions);
var payment = await paymentManager.Auth(auth3DReq);

if (resp.IsConnectionSuccess)//responseCode:200
{
    var res = resp.Result;
    if (res.State == PaymentState.Success)//Payment success
    {
      //Please save orderId and processId
      return Content(resp.Result.Result.HtmlContent, "text/html");//Redirect for MVC project
    }
}

The return url should receive the token and reach the payment result by checking.
Example MVC;
public async Task<IActionResult> Index(string token)
{
  var paymentOptions = new PaymentOptions(
                  "url",
                  "yourApiKey",
                  "yourSecretKey");
                  
  Payment paymentManager = new Payment(paymentOptions);
  var checkReq=new CheckPaymentRequest() { Token = token }
  var checkResp = await paymentManager.CheckPayment(checkReq);
  //you can save response data or show your screen
}
```

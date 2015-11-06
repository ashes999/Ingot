# Ingot

Ingot is a tool for writing functional/integration tests against web applications. GET/POST URLs, fill out forms, and more!

Currently, the only viable tool in the .NET ecosystem is NHtmlUnit, which is a .NET port of a Java tool. Ingot hopes to fill that gap, and provide a more user-friendly testing library.

# Authenticating to an ASP.NET MVC Application

ASP.NET MVC requires you to have the anti-forgery token. It's not hard to authenticate, but you have to GET the login page first (to get the token), and then POST your authentication credentials.

```
// Log in
var client = new HttpClient("http://mysite.com");
var loginPage = client.Request("GET", "/Account/Login");
var tokenRegex = new Regex(@"<input name=""__RequestVerificationToken"".*value=""([^""]+)""");
var token = tokenRegex.Match(loginPage.Content()).Groups[1].Value;
var isAuthorized = client.Request("POST", "/Account/Login", new Dictionary<string, string>() {
  { "__RequestVerificationToken", antiForgeryToken },
  { "UserName", "user name" },
  { "Password", "password" },
  { "RememberMe", "false" }
});
Assert.That(isAuthorized, Is.EqualTo(true));

// Request secured resources
var html = client.Request("GET", "/AuthorizedOnly").Content();
```


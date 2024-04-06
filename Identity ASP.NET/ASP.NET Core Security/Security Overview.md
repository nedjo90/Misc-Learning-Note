Objectifs:
- learning about security on high level
- Security context
- authentication
- authorization

![[Screenshot 2024-04-03 at 09.10.15.png]]`

Imagine attempting to enter a military base:

- You must present your identity card to the security guard for entry.
- Once the guard verifies your identity, you receive an access card containing information such as expiration date, granting you access through the gate.
- To enter a building, you must confirm your permission, as different buildings may have varying requirements.
- Upon expiration of your access card, you must exit the facility.

We have three processes:

1. **Authentication**: Confirming your identity; the security guard verifies your identity and issues an internal access card.
2. **Security Context**: Comprising all relevant identity information for the facility, including permissions to enter buildings.
3. **Authorization**: Ensuring that the security context meets access requirements.


### Authentication & Authorization Flow

![[Screenshot 2024-04-03 at 10.36.14.png]]

When a user logs in, the web server verifies their credentials against the database (e.g., ID and password). If validated, the web server generates a Security Context and returns it to the browser, representing the authentication process. Subsequently, the browser stores this authentication to determine whether the user is logged in or not (identifying "who you are"), and utilizes it for each request to the web server. Now, it simply needs to deserialize the security context to identify the user. The web server then determines whether the user is authorized to access the requested information or page.

### Quick cover of basics ASP.NET CORE

ASP.NET  Core is a platform for creating web applications that includes web APIs.

ASP.NET Core provides a robust framework for developing web applications, including web APIs.

Web applications operate through HTTP requests, which carry extensive information:

- **URL**
- **Headers**
- **Body**
- **Other areas**

Both the request and response primarily consist of data exchanged between the front end (browser) and the back end (server).

Within the server, there are designated stages for processing requests and generating responses.

This process involves multiple steps, including:

- **Authentication**
- **Authorization**
- **Exception Handling**
- **Logging**
- **Routing**
- **Validation**

Each of these steps is essential and must be executed effectively to ensure the smooth functioning of the application.

To handle this process effectively, we can employ various software design patterns, one of which is the "chain of responsibility." This involves organizing multiple functions that are interconnected. When a request is made, it passes through these functions sequentially, from the first to the Nth, following a predefined order. Afterwards, the response is returned in reverse order, from the Nth to the first, until it reaches the browser. Each function modifies the message (request) to transform it into a response for the browser. The message is encapsulated within the HTTP context object, containing both the HTTP request and response. This structure is commonly referred to as the ==middleware pipeline==. Each middleware (function/process) within the pipeline assumes different responsibilities. Typically, the final middleware is responsible for processing business logic and may have its own pipeline with specific filters tailored to individual requests.

![[Screenshot 2024-04-03 at 11.10.35.png]]

### Security Context

The security context encompasses all the pertinent information related to a user for security purposes, which includes:

- **User Information:**
  - User name
  - User email
  - Other relevant data

These details are encapsulated within a single object known as a claims principle. Conceptually, we can perceive this principle as representing the user. Within the principle, there exists one or more identities associated with the user. Delving deeper, identities are comprised of multiple claims. Claims, essentially, are key-value pairs that convey specific user information.

![[Screenshot 2024-04-03 at 11.48.13.png]]

### Anonymous identity

Come back to the first diagram:

![[Screenshot 2024-04-03 at 10.36.14.png]]

We can investigate each and every single step.

Let's examine login page to verify the credentials.

Regardless whether you're logged in or not, you have a security context, you have a primary identity.

### Creating login page

We are going to create a login page so that we can have a login identity instead  of anonymous one.

```cs
@page  
@model WebApp_UnderTheHood.Pages.Account.Login  
  
@{  
}  
  
<div class="container border" style="padding: 20px">  
    <form method="post">  
        <div class="text-danger" asp-validation-summary="All"></div>  
        <div class="mb-3 row">  
            <div class="col-2">  
                <label asp-for="Credential.UserName"></label>  
            </div>            <div class="col-5">  
                <input type="text" asp-for="Credential.UserName" class="form-control"/>  
                <span class="text-danger" asp-validation-for="Credential.UserName"></span>  
            </div>        </div>        <div class="mb-3 row">  
            <div class="col-2">  
                <label asp-for="Credential.Password"></label>  
            </div>            <div class="col-5">  
                <input type="password" asp-for="Credential.Password" class="form-control"/>  
                <span class="text-danger" asp-validation-for="Credential.Password"></span>  
            </div>        </div>        <div class="mb-3 row">  
            <div class="col-2">  
                <input type="submit" class="btn btn-primary" value="login"/>  
            </div>            <div class="col-5">  
            </div>        </div>    </form>  
</div>
```

```cs
using System.ComponentModel.DataAnnotations;  
using Microsoft.AspNetCore.Mvc;  
using Microsoft.AspNetCore.Mvc.RazorPages;  
  
namespace WebApp_UnderTheHood.Pages.Account;  
  
public class Login : PageModel  
{  
    [BindProperty]  
    public Credential Credential { get; set; } = new Credential();  
    public void OnGet()  
    {  
          
    }  
  
    public void OnPost()  
    {  
          
    }  
}  
  
public class Credential  
{  
    [Required]  
    [Display(Name = "User Name")]  
    public string UserName { get; set; } = string.Empty;  
    [Required]  
    [DataType(DataType.Password)]  
    public string Password { get; set; } = string.Empty;  
}
```


The security context is stored in the class Credential.

Using the debugger, if we put a breakpoint on `OnPost` and try to display the credential data after passing user name and password on the login page we have :

```cs
  this.Credential
  {WebApp_UnderTheHood.Pages.Account.Credential}
    Password: "test"
    UserName: "admin"
```

### Generate Cookie with Cookie Authentication Handler

Now we are ready to verify Credential to generate the security context.

First we verify if we have a credential
```cs
public async Task<IActionResult> OnPostAsync()  
{  
    //It return the view   
if (!ModelState.IsValid) return Page();  
      
    //verify the credential  
    if (Credential.UserName == "admin" && Credential.Password == "admin")  
    {  
        //Creating the security context  
        List<Claim> claims = new List<Claim>  
        {  
            new Claim(ClaimTypes.Name, "admin"),  
            new Claim(ClaimTypes.Email, "admin@mywebsite.com")  
        };  
        //Claims in Identity  
        ClaimsIdentity identity = new ClaimsIdentity(claims, "MyCookieAuth");  
        //Identity in Principal  
        ClaimsPrincipal claimsPrincipal = new ClaimsPrincipal(identity);  
        //Now we can encrypt and serialize the security context and store it as a cookie  
        await HttpContext.SignInAsync(  
           "MyCookieAuth", claimsPrincipal  
        );    
        return RedirectToPage("/Index");  
    }  
  
    return Page();  
}
```

```cs
var builder = WebApplication.CreateBuilder(args);  
  
// Add services to the container.  
builder.Services.AddRazorPages();
//We add Authentication services with the authentication scheme
builder.Services.AddAuthentication().AddCookie("MyCookieAuth", options =>  
{  
    options.Cookie.Name = "MyCookieAuth";  
});  
  
var app = builder.Build();  
  
// Configure the HTTP request pipeline.  
if (!app.Environment.IsDevelopment())  
{  
    app.UseExceptionHandler("/Error");  
    // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.  
    app.UseHsts();  
}  
  
app.UseHttpsRedirection();  
  
app.UseStaticFiles();  
  
app.UseRouting();  
  
app.UseAuthorization();  
  
app.MapRazorPages();  
  
app.Run();
```


### Read Cookie with Authentication Middleware


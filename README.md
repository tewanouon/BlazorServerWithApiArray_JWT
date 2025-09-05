Net core 8.0
----------------------------------------
Blazor Server API + JWT Quick Start
Steps to Use:
1 Extract the project files.
2 Open BlazorServerWithApiArray.sln in Visual Studio.
3 Build and Run the project.
4 Navigate to /swagger in the browser.
5 Call POST /api/auth/login with JSON body: { "Username": "admin", "Password": "P@ssw0rd" }.
6 You will receive an access_token in response.
7 Click 'Authorize' in Swagger and enter: Bearer .
8 Now you can test GET, POST, PUT, DELETE on /api/products (requires token).

Added Features & Configuration:
• NuGet package: Microsoft.AspNetCore.Authentication.JwtBearer
• appsettings.json: contains Jwt:Issuer, Jwt:Audience, Jwt:Key (■ change Key before production).
• Program.cs: configured AddAuthentication().AddJwtBearer(...) and AddAuthorization().
• Added app.UseAuthentication(); app.UseAuthorization();
• New endpoint: POST /api/auth/login (demo login: admin / P@ssw0rd).
• Protected API: app.MapGroup("/api/products").RequireAuthorization().
• Swagger: configured to support Bearer Token authentication.
• Pages/Products.razor: demo auto-login to call protected API with token.

Important Code Snippet (Program.cs):

builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme) .AddJwtBearer(o
=> { o.TokenValidationParameters = new TokenValidationParameters { ValidateIssuer = true,
ValidateAudience = true, ValidateIssuerSigningKey = true, ValidateLifetime = true,
ValidIssuer = issuer, ValidAudience = audience, IssuerSigningKey = new
SymmetricSecurityKey(Encoding.UTF8.GetBytes(key)), ClockSkew = TimeSpan.FromMinutes(1) };
}); builder.Services.AddAuthorization(); app.UseAuthentication(); app.UseAuthorization();
app.MapPost("/api/auth/login", (LoginRequest req) => { /* issue JWT */ }); var api =
app.MapGroup("/api/products").RequireAuthorization();

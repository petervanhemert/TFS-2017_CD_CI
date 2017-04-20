# Continuous Integration & Continuous Delivery with Visual Studio TeamS ervices 2017, Visual Studio & Azure. 

- Setup Visual Studio Team Services 2017.
   - General.
      - Create Account
      - Create Project.
   - With TFSC.
   - With local Git repository.
   - With external Git repository.
   
- Setup Visual studio 2017.
   - Add extensions.
   - Project Add to source control

- Setup Azure as Publish environment.

## Setup VSTS 2017.

https://www.visualstudio.com/team-services/

Sign in with your Microsoft account.


   ### General.
   
      Create an Account.
<img src="/MD_Images/VSTS2017/CreateAccountVSTS.PNG" width="600"/> 

<img src="/MD_Images/VSTS2017/CreatingAccount.PNG" width="600"/> 

      Create a Project.
<img src="/MD_Images/VSTS2017/CreateNewProject.PNG" width="600"/> 

<img src="/MD_Images/VSTS2017/AfterCreated.PNG" width="600"/> 

<img src="/MD_Images/VSTS2017/AfterCreated2.PNG" width="600"/> 

<img src="/MD_Images/VSTS2017/AfterCreated3.PNG" width="600"/> 

Before we create a Build, we need a project. We use Visual Studio IDE.

## Setup Visual studio 2017.

- Setup VS 2017(add extension).
- Share your existing code with Visual Studio 2017 and Team Services Git.
- Add VS2017 solution "Add to source control" to GIT project in VSTS 2017(select project in Advanced).

### Add extensions.
Tools > Extensions and Updates > (search for "Continuous Delivery Tools for Visual Studio")

<p align="center">
  <img src="/MD_Images/Capture%201.PNG" width="600"/> 
</p>



##---------------------------------------------------------------------------------------------##

## Setup Azure.


   ### With local Git repository.

   ### With external repository.

# TFS-2017_CD_CI
TFS straat
## Create the Azure app service.
- Create App Service Plan
- Create Web App
- Link App Service Plan to VSTS.
- Finish...............GoTo Setup Visual Studio Team Services 2017(VSTS)
- Create development stack 

#### Create Web App.

#### Link Team Service account to Azure subscription.

#### Create development stack from Azure portal. 


## Setup Visual Studio Team Services 2017(VSTS)
- Create VSTS account selecting Git to manage code.
- Create VSTS 2017 project selecting Git.
   - initialize with a README or gitignore.
   - build code from an external repository.
   
#### Initialize with a README or gitignore.
- Set up Build

#### Build code from an external repository.
- Set up Build

### Set up Build.
- Visual Studio
- ASP.NET
- ASP.NET Core


## Setup Visual Studio 2017.










- [Setup TFS 2017](#setup-tfs)
   - [Create account](#project-directory)
   - [Create team project](#nuget-package-manager)
  
- [Setup Azure](#setup-azure)
   - [Create account](#project-directory)
   - [Create team project](#nuget-package-manager)
  
- [Setup Visual Studio 2017](#setup-visual-studio)
   - [Add extensions](#add-extensions)
   - [Create local Git repo](#create-local-git-repo)
   - [Create new Git repo](#create-new-git-repo)
   - [Create account](#project-directory)
   - [Create team project](#nuget-package-manager)

- [Creating an MVC client](#creating-an-mvc-client)
 - [Configuring the stores](#configuring-the-stores)
 - [ConfigurationStore](#configurationstore)
 - [OperationalStore](#operationalstore)

## Setup TFS

## Setup Azure

## Setup Visual Studio



## Share your code with Visual Studio 2017 and Team Services Git

- Create repo from new project.
- Create repo from existing code on your computer.
- Clone an existing repo.

### Create local Git repo.


#### Create a local Git repo for your project from an empty folder starting a new project.

<p align="center">
  <img src="/MD_Images/Demo2_1.PNG" width="600"/> 
</p>



## Create new Git repo.

























### Setup TFS



 [*Based on this repository.*](https://github.com/petervanhemert/ASP.NET-CORE-1.1-Development-with-SSL/blob/master/README.md)
 
###Project Directory
 
 - In Data/Migrations Add Folders
  - [x] Configuration
  - [x] Identity
  - [x] PersistedGrants
 
 Move the files of ApplicationDbContext Migration to Identity.
 ```ruby
 ApplicationDbContextModelSnapshot.cs
 00000000000000_CreateIdentitySchema.cs
 ```
 
###Nuget Package Manager
 
 Update Packages
 
 Add to project:
 ```ruby
 IdentityServer4
 IdentityServer4.AspNetIdentity
 IdentityServer4.EntityFramework
 ```
 
###Connectionstring to DB

 Open appsettings.json
 
 Change DefaultConnection in ConnectionStrings in the file appsettings.json
  ```ruby
 {
  "ConnectionStrings": {
    "DefaultConnection": "Server=<Server name>;Database=<Database name>;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
  "Logging": {
    "IncludeScopes": false,
    "LogLevel": {
      "Default": "Warning"
    }
  }
}
  ```
###Configure IdentityServer and the stores  
  
####Startup

As before, IdentityServer needs to be configured in both ConfigureServices and in Configure in Startup.cs.

#####ConfigureServices

Modify your ConfigureServices in the Startup.cs to look like this:
```ruby
        public void ConfigureServices(IServiceCollection services)
        {
            // Add framework services.
            services.AddDbContext<ApplicationDbContext>(options =>
                options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

            var migrationsAssembly = typeof(Startup).GetTypeInfo().Assembly.GetName().Name;

            services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

            services.AddMvc();

            // Add application services.
            services.AddTransient<IEmailSender, AuthMessageSender>();
            services.AddTransient<ISmsSender, AuthMessageSender>();
            services.AddTransient<IProfileService, IdentityWithAdditionalClaimsProfileService>();

            // add for identityserver
            services.AddIdentityServer()
                .AddTemporarySigningCredential()
                .AddOperationalStore(
                    builder => builder.UseSqlServer(Configuration.GetConnectionString("DefaultConnection"), options =>                  options.MigrationsAssembly(migrationsAssembly)))
                .AddConfigurationStore(
                    builder => builder.UseSqlServer(Configuration.GetConnectionString("DefaultConnection"), options => options.MigrationsAssembly(migrationsAssembly)))
                .AddAspNetIdentity<ApplicationUser>()
                .AddProfileService<IdentityWithAdditionalClaimsProfileService>();
        }
```

#####Configure

This shows both the template code generated for ASP.NET Identity, plus the additions needed for IdentityServer (just after UseIdentity). It’s important when using ASP.NET Identity that IdentityServer be registered after ASP.NET Identity in the pipeline because IdentityServer is relying upon the authentication cookie that ASP.NET Identity creates and manages.

Modify your Configure in the Startup.cs to look like this:
```ruby
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
        {
            loggerFactory.AddConsole(Configuration.GetSection("Logging"));
            loggerFactory.AddDebug();

            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
                app.UseDatabaseErrorPage();

                // Browser Link is not compatible with Kestrel 1.1.0
                // For details on enabling Browser Link, see https://go.microsoft.com/fwlink/?linkid=840936
                // app.UseBrowserLink();
            }
            else
            {
                app.UseExceptionHandler("/Home/Error");
            }
           
            app.UseIdentity();
            
            app.UseIdentityServer();

            app.UseStaticFiles();
            
            app.UseMvc(routes =>
            {
                routes.MapRoute(
                    name: "default",
                    template: "{controller=Home}/{action=Index}/{id?}");
            });
        }
```

  


#####Adding migrations

```ruby
PM> Add-Migration SetMigrationName -c ApplicationDbContext -o Data/Migrations/Identity

PM> Add-Migration SetMigrationName -c PersistedGrantDbContext -o Data/Migrations/PersistedGrants

PM> Add-Migration SetMigrationName -c ConfigurationDbContext -o Data/Migrations/Configuration

PM> Update-Database -c ApplicationDbContext

PM> Update-Database -c PersistedGrantDbContext

PM> Update-Database -c ConfigurationDbContext
```

     Add-Migration What is what?

     `-c ApplicationDbContext`
     Is the DbContext we are targeting

     `-o <ProjectName>\Data\Migrations\<DestinationFolder>`
     Is the file destination

     `SetMigrationName`
     Is the name we give of the Migration

     Update-Database What is what?

     `-c ApplicationDbContext`
     Is the DbContext we are targeting


## Creating an MVC client

Add an MVC application to your solution. Use the ASP.NET Core “Web Application” template for that. No authentication.


### Client Nuget Package Manager 
 
 Update Packages
 
 Add to project:
 ```ruby
Microsoft.AspNetCore.Authentication.Cookies
Microsoft.AspNetCore.Authentication.OpenIdConnect
 ```
Next add both middlewares to your pipeline - the cookies one is simple:
```ruby
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationScheme = "Cookies"
});
```
The OpenID Connect middleware needs slightly more configuration. You point it to your IdentityServer, specify a client ID and tell it which middleware will do the local signin (namely the cookies middleware). As well, we’ve turned off the JWT claim type mapping to allow well-known claims (e.g. ‘sub’ and ‘idp’) to flow through unmolested. This “clearing” of the claim type mappings must be done before the call to UseOpenIdConnectAuthentication():
```ruby
JwtSecurityTokenHandler.DefaultInboundClaimTypeMap.Clear();

app.UseOpenIdConnectAuthentication(new OpenIdConnectOptions
{
    AuthenticationScheme = "oidc",
    SignInScheme = "Cookies",

    Authority = "http://localhost:5000",
    RequireHttpsMetadata = false,

    ClientId = "mvc",
    SaveTokens = true
});
```

Both middlewares should be added before the MVC in the pipeline.

The last step is to trigger the authentication handshake. For that go to the home controller and add the `[Authorize]` on one of the actions. Also modify the view of that action to display the claims of the user, e.g.:


```ruby
<dl>
    @foreach (var claim in User.Claims)
    {
        <dt>@claim.Type</dt>
        <dd>@claim.Value</dd>
    }
</dl>
```


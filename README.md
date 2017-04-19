# TFS-2017_CD_CI
TFS straat

- Create VSTS account selecting Git to manage code.
- Create VSTS 2017 project selecting Git.
- Setup VS 2017(add extension).
- Share your existing code with Visual Studio 2017 and Team Services Git.
- Add VS2017 solution "Add to source control" to GIT project in VSTS 2017(select project in Advanced).

   - Git VSTS 2017
   - Git Github
   - Git Remote repo







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

### Add extensions.
Tools > Extensions and Updates > (search for "Continuous Delivery Tools for Visual Studio")

<p align="center">
  <img src="/MD_Images/Capture%201.PNG" width="600"/> 
</p>

## Share your code with Visual Studio 2017 and Team Services Git

- Create repo from new project.
- Create repo from existing code on your computer.
- Clone an existing repo.

### Create local Git repo.

#### Create a local Git repo for your project from a folder containing existing code on your computer.

Create a new local Git repo for your project, after you opend or created your project, by selecting <img src="/MD_Images/addsrccontrol.png" width="150"/> on the status bar in the lower right hand corner of Visual Studio. This will create a new repo in the folder the solution is in and commit your code into that repo.

Once you have a local repo, select items in the status bar to quickly navigate between Git tasks in Team Explorer.

<p align="center">
  <img src="/MD_Images/vsstatusbar.png" width="600"/> 
</p>
<p>

- <img src="/MD_Images/unpublished_changes.png"> shows the number of unpublished commits in your local branch. Selecting this will open the Sync view in Team Explorer.

  
- <img src="/MD_Images/pending_changes.png"/> shows the number of uncommitted file changes. Selecting this will open the Changes view in Team Explorer.
    
- <img src="/MD_Images/current_repo.png"/> shows the current Git repo. Selecting this will open the Connect view in Team Explorer.
    
- <img src="/MD_Images/branch_picker.png"/> shows your current Git branch. Selecting this displays a branch picker to quickly switch between Git branches or create new branches.
</p>  

#### Create a local Git repo for your project from an empty folder starting a new project.

<p align="center">
  <img src="/MD_Images/Demo2_1.PNG" width="600"/> 
</p>

### Publish your code to Team Services

In the Push view in Team Explorer, select the Publish Git Repo button under Push to Visual Studio Team Services.
<p align="center">
  <img src="/MD_Images/Demo2_2.PNG" width="600"/> 
</p>

Verify your email and select your account in the Team Services Domain drop down.

Enter your repository name and select Publish Repository. 

<p align="center">
  <img src="/MD_Images/Demo2_3.PNG" width="600"/> 
</p>

This creates a new Team Project in your account with the same name as the repository. To create the repo in an existing Team Project, click Advanced next to Repository name and select a team project.

<p align="center">
  <img src="/MD_Images/Demo2_4.PNG" width="600"/> 
</p>
Your code is now in a Team Services repo. You can view your code on the web by selecting See it on the web.

### Commit and push updates.

1. As you write your code, your changes are automatically tracked by Visual Studio. You can commit changes to your local Git repository by selecting the pending changes icon ( <img src="/MD_Images/pending_changes.png"/>  ) from the status bar.

2. On the Changes view in Team Explorer, add a message describing your update and commit your changes.

<p align="center">
  <img src="/MD_Images/Demo2_5.png" width="600"/> 
</p>

3. Select the unpublished changes status bar icon ( <img src="/MD_Images/unpublished_changes.png"> ) or the Sync view in Team Explorer. Select Push to update your code in Team Services/TFS.

<p align="center">
  <img src="/MD_Images/vspush.gif" width="600"/> 
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


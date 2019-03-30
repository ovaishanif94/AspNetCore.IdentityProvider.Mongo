#AspNetCore.IdentityProvider.Mongo

MongoDB provider for the ASP.NET Core 2.2 Identity framework. 

### Download
The latest stable release is available on NuGet: https://www.nuget.org/packages/AspNetCore.IdentityProvider.Mongo/

####Example
`Install-Package AspNetCore.IdentityProvider.Mongo -Version 1.0.1`

**Startup.cs**

	 using AspNetCore.IdentityProvider.Mongo;
	 
	 public void ConfigureServices(IServiceCollection services)
     {
            services.AddIdentityMongoDbProvider<MongoUser, MongoRole>(identityOptions =>
            {
                identityOptions.Password.RequiredLength = 8;
                identityOptions.Password.RequireNonAlphanumeric = true;
                identityOptions.Password.RequireDigit = true;
                identityOptions.Lockout.MaxFailedAccessAttempts = 3;
                identityOptions.Lockout.DefaultLockoutTimeSpan = TimeSpan.FromMinutes(5);
            }, mongoIdentityOptions =>
            {
                mongoIdentityOptions.ConnectionString = "mongodb://localhost:27017/mongo";
            });

            services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
     }
		
	public void Configure(IApplicationBuilder app, IHostingEnvironment env)
     {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            else
            {
                app.UseHsts();
            }

            app.UseHttpsRedirection();
            app.UseMvc();
            app.UseAuthentication();
      }

**Controller**

	public class ValuesController : ControllerBase
    {
        private readonly SignInManager<MongoUser> signInManager;
        private readonly UserManager<MongoUser> userManager;

        public ValuesController(SignInManager<MongoUser> signInManager,
            UserManager<MongoUser> userManager)
        {
            this.signInManager = signInManager;
            this.userManager = userManager;
        }
    }
	

**Extend User Entity**

	public class User : MongoUser
     {
            // code here
     }
	 
**Extend User Roles**

	public class Roles : MongoRole
     {
            // code here
     }

####More Documentation
https://docs.microsoft.com/en-us/aspnet/core/security/authentication/identity?view=aspnetcore-2.2&tabs=visual-studio

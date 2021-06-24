### Features
- DI Container
- FluentMigrator
- AutoMapper
- Fluent Validator
- EF Code First
- MediatR
# How To Use
`Install-Package Web.Framework`

**Modify Startup.cs**
	{
        public void Configuration(IAppBuilder app)
        {
            MiddlewareConfig.Configure(app);
        }
    }
	
**Modify Global.asax**
    public class MvcApplication : GlobalConfig
    {
        protected override void Application_Start()
        {
            AreaRegistration.RegisterAllAreas();
            FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
            RouteConfig.RegisterRoutes(RouteTable.Routes);
            BundleConfig.RegisterBundles(BundleTable.Bundles);
            base.Application_Start();
        }
        protected override void ConfigureService(ServiceCollection serviceCollection)
        {
            serviceCollection.AddScoped<ApplicationUserManager>(sp => HttpContext.Current.GetOwinContext().GetUserManager<ApplicationUserManager>());
            serviceCollection.AddScoped<ApplicationSignInManager>(sp => HttpContext.Current.GetOwinContext().Get<ApplicationSignInManager>());
            serviceCollection.AddTransient<ModelDb>();
            serviceCollection.AddFluentMigrator();
            serviceCollection.AddAutoMapperConfig();
            serviceCollection.RegisterDependencies();
            
            base.ConfigureService(serviceCollection);
        }
    }

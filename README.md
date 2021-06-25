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
```sh
{
        public void Configuration(IAppBuilder app)
        {
            MiddlewareConfig.Configure(app);
        }
}


```
**Modify Global.asax**
```sh
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
         protected override void ConfigureService(IServiceCollection services)
        {
            //register autofac Container
            service.AutofacRegister();
            base.ConfigureService(services);
        }
        //using LighInject Container
        protected override void ConfigureContainer(ServiceContainer container)
        {
            container.RegisterControllers();
            container.RegisterScoped<IEmployeeService, EmployeeService>();
            base.ConfigureContainer(container);
        }
    }
```

How to Use Autofac DI container

```sh
public class DependencyRegistrar: DependencyRegistrarModule
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<EmployeeService>().AsImplementedInterfaces();
            builder.RegisterType<EmployeeValidtor>().Keyed<IValidator>(typeof(EmployeeValidtor));
            base.Load(builder);
        }
    }
```

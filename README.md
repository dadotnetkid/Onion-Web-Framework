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

**How to Use Autofac DI container**

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
**How to use FluentMigrator**
```sh
**Employee is a Entity which Inherit BaseEntity**
public class EmployeeDbBuilder: TSEntityBuilder<Employees>
    {
        public override void MapEntity(CreateTableExpressionBuilder table)
        {
           
        }
    }
    ///Here is create migration for the employee Table which create all the properties in the entity
     [Migration(06242021144,"Create Employee table")]
    public class EmployeeMigration:AutoReversingMigration
    {
        private readonly IMigrationManager _migrationManager;

        public EmployeeMigration(IMigrationManager migrationManager)
        {
            _migrationManager = migrationManager;
        }
        public override void Up()
        {
            _migrationManager.BuildTable<Employees>(Create);
        }
    }
```
**How to use Repository Pattern**
```sh
        just use IRepository<TEntity> in contructor just that
        private readonly IRepository<Employees> _employeeRepository;
        public EmployeeService(IRepository<Employees> employeeRepository)
        {
            _employeeRepository = employeeRepository;
        }

```
**How to use AutoMapper**
```sh
ViewModel must Inherit BaseModel
public class ModelMappingConfiguration: AutoMapperAbstraction
    {
        public ModelMappingConfiguration()
        {
            CreateMap<Employee, EmployeeModel>();
            CreateMap<EmployeeModel, Employee   >();
        }
    }
     public  IQueryable<EmployeeModel> GetAll()
        {
        //.ToModel<DTO>();
            return _employeeRepository.GetAll().ToModel<EmployeeModel>();
        }
    
```

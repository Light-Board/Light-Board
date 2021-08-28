3번째 시간에는 환경 구축 중 초기단계인 라이브러리부터 추가하는 작업을 정리했다.
이번 시간 부터는 추가한 라이브러리를 가지고, 설정하는 부분을 정리하려고한다.
참고로, SpringFramework는 설정 방법은 3가지 방법이 있다.

1. Xml으로 설정
2. java config로 설정
3. Xml + java config로 설정

위와 같이 설정 방법에 대해선 3가지 방법이 있지만, 2번째 방법을 선택해서 구현하려고 한다.
~~그 이유는 java config로만 설정하게 될 경우 불편함을 직접(?) 느껴보고 싶어서이다.~~

아무튼 시작해보자.

### 🏳 ApplicationContext

#### 1. RootApplicationContext / WebApplicationContext

![](https://images.velog.io/images/seyoung_jeon/post/ad6c32e3-db23-45dd-b0e6-5bff367f5ed9/image.png)

**RootAppContext**는 Business Layer와 Data Access Layer에 대한 필요한 Bean 생성과 관계설정등에 대한 내용을 담고 있고 **WebAppContext**는 PresentationLayer인 SpringMVC와 관련된 설정 내용을 담고 있다.


#### 2. ServletContext 설정 및 Servlet 생성과정
Servlet은 3.0 이후부터 web.xml 없이도 `Servlet Context` 초기화 작업이 가능해졌는데, 프레임워크 레벨에서 직접 초기화할 수 있게 도와주는 `ServletContainerInitializer API`를 제공하기 때문이다.

> **Servlet Context 초기화란?**
web.xml에서 했던 Servlet 등록/매핑, 리스너 등록, 필터 등록 같은 작업을 의미

Spring은 웹 모듈내에 이미 `ServletContainerInitializer`를 구현한 클래스가 포함되어 있고, 이는 `WebApplicationInitializer` 인터페이스를 구현한 클래스를 찾아 초기화 작업을 위임하는 역할을 수행한다.

즉, `WebApplicationInitializer` 인터페이스를 구현한 클래스를 `main`이라고 생각하면 될 것이다.

```
public class WebApplication implements WebApplicationInitializer {
	
    @Override
    public void onStartup(ServletContext servletContext) {
    // RootApplicationContext 생성 및 설정정보 등록
    AnnotationConfigWebApplicationContext rootContext = new AnnotationConfigWebApplicationContext();
    rootContext.register(RootAppConfig.class);
    
    // RootApplicationContext 라이프 사이클 설정
    servletContext.addListener(new ContextLoaderListener(rootContext));
    
    // WebApplicationContext 생성 및 설정정보 등록
    AnnotationConfigWebApplicationContext dispatcherContext = new AnnotationConfigWebApplicationContext();
    dispatcherContext.register(WebAppConfig.class);
    
    // DispatcherServlet 생성 및 기타 옵션정보 설정
    ServletRegisteration.Dynamic dispatcher = servletContext.addServlet("dispatcher", new DispatcherServlet(dispatcherContext));
    dispatcher.setLoadOnStartUp(1);
    dispatcher.addMapping("/");
    }
}
```
위의 코드는 RootContext와 WebApplicationContext를 역할에 따라, 나누어 Context를 생성한 경우이다.

필자는, 한 Context안에서 모두 설정하는 방법을 선택했는데, 코드는 아래와 같다.
```
public class WebApplication implements WebApplicationInitializer {
	@Override
	public void onStartup(ServletContext servletContext) throws ServletException {
		AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();
		context.register(WebConfig.class);
		context.register(DBConfig.class);
        
		servletContext.addListener(new ContextLoaderListener(context));

		// Servlet 설정
		setServletDispatcherContext(context, servletContext);

		// Filter 설정
		setCharacterEncodingFilter(servletContext);
	}

	// Servlet 설정
	private void setServletDispatcherContext(AnnotationConfigWebApplicationContext context,
		ServletContext servletContext) {
		ServletRegistration.Dynamic dispatcher = servletContext.addServlet("dispatcher",
			new DispatcherServlet(context));
		dispatcher.setLoadOnStartup(1);
		dispatcher.addMapping("/");
	}

	// Filter 설정 > encoding UTF-8
	private void setCharacterEncodingFilter(ServletContext servletContext) {
		FilterRegistration.Dynamic characterEncodingFilter = servletContext.addFilter("characterEncodingFilter",
			new CharacterEncodingFilter());
		characterEncodingFilter.setInitParameter("encoding", "UTF-8");
		characterEncodingFilter.setInitParameter("forceEncoding", "true");
	}
}

```

### 🐯 DB Config
위에서 필자가 작성한 코드에는 `DBConfig.class` 라는 파일이 있는데, 해당 클래스를 통해서 DB 관련 설정을 진행하고 있다.

먼저 코드를 살펴보자.
```
@Configuration
@EnableJpaRepositories("com.lb.lightboard.repository")
@EnableTransactionManagement
public class DBConfig {
	@Bean
	public DataSource dataSource() {
		BasicDataSource dataSource = new BasicDataSource();

		dataSource.setDriverClassName(ConfigConstants.DATASOURCE_DRIVER_CLASS_NAME);
		dataSource.setUrl(ConfigConstants.DATASOURCE_URL);
		dataSource.setUsername(ConfigConstants.DATASOURCE_USER_NAME);
		dataSource.setPassword(ConfigConstants.DATASOURCE_PASSWORD);
		dataSource.setInitialSize(100);
		dataSource.setMaxIdle(30);
		dataSource.setMinIdle(20);
		dataSource.setMaxWaitMillis(10000);

		return dataSource;
	}

	@Bean
	public EntityManagerFactory entityManagerFactory() {
		HibernateJpaVendorAdapter hibernateJpaVendorAdapter = new HibernateJpaVendorAdapter();
		hibernateJpaVendorAdapter.setGenerateDdl(true);
		LocalContainerEntityManagerFactoryBean factory = new LocalContainerEntityManagerFactoryBean();
		factory.setJpaVendorAdapter(hibernateJpaVendorAdapter);
		factory.setJpaProperties(properties());
		factory.setPackagesToScan("com.lb.lightboard.model");
		factory.setDataSource(dataSource());
		factory.afterPropertiesSet();
		return factory.getObject();
	}

	@Bean
	public PlatformTransactionManager transactionManager() {
		JpaTransactionManager txManager = new JpaTransactionManager();
		txManager.setEntityManagerFactory(entityManagerFactory());
		return txManager;
	}

	private Properties properties() {
		Properties properties = new Properties();
		properties.put("hibernate.dialect", "org.hibernate.dialect.MySQLDialect");
		properties.put("hibernate.show_sql", "true");
		properties.put("hibernate.hbm2ddl.auto", "none");
		return  properties;
	}
}
```

#### 1. BasicDataSource
이전 3번째 시간에서 DataSource에 대해서 알아보았는데, 그 때에도 언급했다 시피, 구현체로 대중적으로 많이 사용되는 `org.apache.commons.dbcp.BasicDataSource`를 사용한다. 
데이터베이스 연결 관련 설정을 하는 Bean이다.

drvierClassName, url, password 같은 속성 이외에 생소한 속성에 대해서 알아보겠다.

**initialSize** : 연결 풀이 최초 생성될 때 같이 설정된 숫자만큼 데이터베이스 연결을 미리 생성한다. 기본 값은 0

**maxIdle** : 풀에서 사용되지 않은 상태로 존재할 수 있는 최대 연결의 숫자로 음수이면 제한이 없다. 기본값은 8

**minIdle** : 풀에서 사용되지 않는 상태로 존재할 수 있는 최소 연결의 숫자이다. 기본 값은 0

**maxWait** : 풀에 사용 가능할 연결이 없을 때, 대기하는 최대 시간을 밀리초 단위로 나타낸다. 대기시간이후에도 사용가능한 연결이 없으면 예외를 발생시킨다. -1은 무한대기를 나타내고, 기본 값은 -1

#### 2. entityManagerFactory
JPA는 `Entity`라는 형태로 데이터베이스의 데이터를 오브젝트화한다. `Entity`를 관리하는 것이 `EntityManager`이다.

- **LocalContainerEntityManagerFactoryBean**
> 
SessionFactoryBean과 동일하게 DataSource와 Hibernate Property, Entity가 위치한 Package를 지정한다.
또한, Hibernate 기반으로 동작하는 것을 지정하는 JpaVendor 설정이 필요하다.

- **HibernateJpaVendorAdapter**
> 
Hibernate vendor와 JPA 간의 Adapter를 설정한다.
간단히, showSql 정도의 Propert만 설정하면 된다.

#### 3. transactionManager
JPA를 지원하는 TransactionManager를 등록한다.

- **JpaTransactionManager**
> 
DataSource와 EntityManagerFactoryBean에서 생성되는 EntityManagerFactory를 지정하는 Bean

이렇게 설정한 DBConfig.class를 WebApplication.class의 context에 다음과 같은 코드로 등록하여 Jpa-Hibernate DB 설정을 할 수 있다.
```
context.register(DBConfig.class);
```


### 🚩 결론
1. Spring 설정 방법에 대해선 3가지 (xml, java config, xml+java config) 방법이 있다.
2. ApplicationContext는 역할에 따라, RootApplicationContext와 WebApplicationContext로 나눌수도 있지만, 한 개의 Context로 관리할 수 도 있다.
3. Sevlet 3.0 부터는 Java Config로 Servlet 초기화를 진행할 수 있다.
4. DBConfig.class를 통해, DataSource 설정, EntityManage 설정, TransactionManager 설정 등과 같은  DB 설정을 진행할 수 있다.
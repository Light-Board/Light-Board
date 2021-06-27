- 참고 링크

    [[spring] WebApplicationInitializer](https://joont92.github.io/spring/WebApplicationInitializer/)

- Servlet 3.0 버전 이후부터는 `web.xml` 없이도 **서블릿 컨텍스트 초기화** 작업이 가능해짐
    - **서블릿 컨텍스트 초기화**
        - `web.xml` 에서 했던 서블릿 등록/매핑, 리스너 등록, 필터 등록 같은 작업을 말함
    - 프레임워크 레벨에서 직접 초기화할 수 있게 도와주는 `ServletContainerInitializer API` 를 제공하기 때문임
- 스프링은 웹 모듈내에 이미 `ServletContainerInitializer` 를 구현한 클래스가 포함되어 있어, `WebApplicationInitializer` 인터페이스를 구현한 클래스를 찾아 초기화 작업을 위임하는 역할을 수행함
    - 해당 인터페이스를 구현한 클래스를 만들면 웹 어플리케이션이 시작할 때, 자동으로 `onStartup()` 메소드가 실행되는데, 여기서 초기화 작업을 수행하면 됨

## 리스너

- 역할
    - 서블릿 컨텍스트가 생성하는 이벤트를 전달받는 역할을 수행
    - 서블릿 컨텍스트가 만드는 이벤트는 **컨텍스트 초기화 이벤트**와 **종료 이벤트**임
        - 즉, 웹 어플리케이션이 시작되고 종료되는 시점에 이벤트가 발생하고, 리스너를 등록해두면 이를 받을 수 있음
        - 종료 시점 이벤트도 `onStartup` 메소드에서 등록을 해줘야함 안그러면, 애플리케이션이 종료되어도 리소스가 반환되지 못하므로 메모리 누수가 일어날 수 있음

- **리스너 등록**
    - web.xml

        ```xml
        <listener>
        	<!-- /WEB-INF/applicationContext.xml 생성 -->
        	<listener-class>org.springframework.web.context.ContextLoaderListener</listenr-class>
        </listner>
        ```

    - Java Code

        ```java
        ServletContextListener listener = new ContextLoaderListener();
        servletContext.addListener(listener);
        ```

- **디폴트 루트 웹 애플리케이션 컨텍스트 클래스(`XmlWebApplicationContext`)나 디폴트 XML 설정파일 위치(`WEB-INF/applicationContext.xml`) 를 변경하고 싶은 경우**
    - web.xml

        ```xml
        <!-- 애플리케이션 컨텍스트 클래스 변경 -->
        <context-param>
        	<param-name>contextClass</param-name>
        	<param-value>
        		org.springframework.web.context.support.AnnotationWebApplicationContext
        	</param-value>
        </context-param>

        <!-- 설정파일 위치 변경 -->
        <context-param>
        	<param-name>contextConfigLocation</param-name>
        	<param-value>/WEB-INF/root-context.xml</param-value>
        </context-param>
        ```

    - Java Code

        ```java
        // 애플리케이션 컨텍스트 클래스 변경
        servletContext.setInitParameter("contextClass", "org.springframework.web.context.support.AnnotationWebApplicationContext");
        // 설정파일 위치 변경
        servletContext.setInitParameter("contextConfigLocation", "/WEB-INF/root-context.xml");
        ```

        - 만약 Java Config를 사용하면 더 깔끔하게 처리할 수 있음

            ```java
            AnnotationConfigWebApplicationContext rootContext = new AnnotationConfigWebApplicationContext();
            rootContext.register(RootConfig.class);

            servletContext.addListenr(new ContextLoaderListener(rootContext));
            ```

## 서블릿 웹 애플리케이션 컨텍스트 등록 (servlet-context.xml)

- 서블릿 웹 애플리케이션 컨텍스트는 서블릿 안에서 초기화되고 서블릿이 종료될 때 같이 종료됨
    - 이 때, 사용되는 서블릿이 `DispatcherServlet` 임

- DispatcherServlet 등록
    - xml

        ```xml
        <servlet>
        	<servlet-name>appServlet</servlet-name>
        	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet>
        	<init-param>
        		<param-name>contextConfigLocation</param-name>
        		<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
        	</init-param>
        	<load-on-startup>1</load-on-startup>
        </servlet>

        <servlet-mapping>
        	<servlet-name>appServlet</servlet-name>
        	<url-pattern>/</url-pattern>
        </servlet-mapping>
        ```

    - Java Code

        ```java
        ServletRegistration.Dynamic dispatcher = servletContext.addServlet("appServlet", new DispatcherServlet());
        dispatcher.setInitParameter("contextConfigLocation", "/WEB-INF/spring/appServle/servlet-context.xml");
        dispatcher.setLoadOnStartUp(1);
        dispatcher.addMapping("/");
        ```

        - Java Config

            ```java
            AnnotationConfigWebApplicationContext sac = new AnnotationConfigWebApplicationContext();
            sac.register(WebConfig.class);

            ServletRegistration.Dynamic dispatcher = servletContext.addServlet("appServlet", new DispatcherServlet(sac));
            dispatcher.setLoadOnStartUp(1);
            dispatcher.addMapping("/");
            ```
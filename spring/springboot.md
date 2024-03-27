
- [1. applicationContext](#1-applicationcontext)
- [2. tomcat](#2-tomcat)
- [3. spring boot](#3-spring-boot)



# 1. applicationContext

```plantuml
class SpringApplication {
    WebApplicationType webApplicationType
    List<ApplicationContextInitializer<?>> initializers
    List<ApplicationListener<?>> listeners
    ApplicationContextFactory applicationContextFactory
    ConfigurableApplicationContext run(String... args)
    void prepareContext()
}

interface  ApplicationContextFactory {

}

interface ConfigurableApplicationContext {
    void refresh()
    void setEnvironment(ConfigurableEnvironment environment)
    void setApplicationStartup(ApplicationStartup applicationStartup)
}

interface ConfigurableWebServerApplicationContext extends ConfigurableApplicationContext, WebServerApplicationContext {
    void setServerNamespace(String serverNamespace)
}
class GenericApplicationContext extends AbstractApplicationContext implements BeanDefinitionRegistry {
    DefaultListableBeanFactory beanFactory
}

class GenericWebApplicationContext extends GenericApplicationContext implements ConfigurableWebApplicationContext, ThemeSource {
}
interface ConfigurableWebApplicationContext extends WebApplicationContext, ConfigurableApplicationContext {
    void setServletContext(@Nullable ServletContext servletContext)
    void setServletConfig(@Nullable ServletConfig servletConfig)
    void setConfigLocations(String... configLocations)
    String[] getConfigLocations()
}

abstract class AbstractApplicationContext extends DefaultResourceLoader implements ConfigurableApplicationContext


interface WebServerApplicationContext extends ApplicationContext {
    WebServer getWebServer()
    String getServerNamespace()
}
interface ApplicationContext extends EnvironmentCapable, ListableBeanFactory, HierarchicalBeanFactory,MessageSource, ApplicationEventPublisher, ResourcePatternResolver

interface ConfigurableApplicationContext extends ApplicationContext, Lifecycle, Closeable
interface WebApplicationContext extends ApplicationContext {
    ServletContext getServletContext()
}

interface ApplicationContext {

AutowireCapableBeanFactory getAutowireCapableBeanFactory()
}

interface ResourcePatternResolver {

Resource[] getResources(String locationPattern)
} 
class ServletWebServerApplicationContext extends GenericWebApplicationContext implements ConfigurableWebServerApplicationContext {
    volatile WebServer webServer
    void createWebServer()
}

class AnnotationConfigServletWebServerApplicationContext extends ServletWebServerApplicationContext implements AnnotationConfigRegistry {
     final AnnotatedBeanDefinitionReader reader
	 final ClassPathBeanDefinitionScanner scanner
}

SpringApplication o-- ApplicationContextFactory

ApplicationContextFactory --> ConfigurableApplicationContext:create

```

# 2. tomcat 

```plantuml

class ServletWebServerApplicationContext {
    void createWebServer()
}




class TomcatServletWebServerFactory {
WebServer getWebServer(ServletContextInitializer... initializers)
}

ServletWebServerApplicationContext --> TomcatServletWebServerFactory: getFactory




```


```plantuml

package tomcat {
    frame server {
        frame service {
                component connector
                
                    component Engine {
                        component Host {
                            component context {
                                component servlet
                            }
                        }
                    }
                
                connector -left-> Engine
        }
    }
    

}


```


# 3. spring boot

```plantuml
class SpringApplication {
    List<ApplicationContextInitializer<?>> initializers
    List<ApplicationListener<?>> listeners
    Collection<T> getSpringFactoriesInstances(Class<T> type, Class<?>[] parameterTypes, Object... args)
    ConfigurableApplicationContext run(String... args)
    ConfigurableApplicationContext createApplicationContext()
    void refreshContext(ConfigurableApplicationContext context) 

}


class DefaultApplicationContextFactory {
    ConfigurableApplicationContext create(WebApplicationType webApplicationType)
}

class AnnotationConfigServletWebServerApplicationContext {
    AnnotatedBeanDefinitionReader reader
	ClassPathBeanDefinitionScanner scanner
	Set<Class<?>> annotatedClasses = new LinkedHashSet<>();
	String[] basePackages
    class Factory 
    void refresh()
}

abstract class AbstractApplicationContext {
    void refresh()
    void invokeBeanFactoryPostProcessors(ConfigurableListableBeanFactory beanFactory)
    List<BeanFactoryPostProcessor> getBeanFactoryPostProcessors()
}



class  PostProcessorRegistrationDelegate {
invokeBeanFactoryPostProcessors(beanFactory,beanFactoryPostProcessors)
}
class  ConfigurationClassPostProcessor {
    void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry registry)
}

class ConfigurationClassParser {
    ComponentScanAnnotationParser componentScanParser
    parse(Set<BeanDefinitionHolder> configCandidates)
    processConfigurationClass(ConfigurationClass configClass, Predicate<String> filter)
    SourceClass doProcessConfigurationClass(configClass,  sourceClass,  filter)
}

SpringApplication o-- DefaultApplicationContextFactory
DefaultApplicationContextFactory --> AnnotationConfigServletWebServerApplicationContext:create

AbstractApplicationContext <|-- AnnotationConfigServletWebServerApplicationContext
AbstractApplicationContext --> PostProcessorRegistrationDelegate: invoke
PostProcessorRegistrationDelegate --> ConfigurationClassPostProcessor:invoke
ConfigurationClassPostProcessor --> ConfigurationClassParser
```
configurationclass Parser priority
1. component
2. propertysource
3. componentscan
4. import
5. importresource
6. bean
7. superclass
# TS-LOGGER

Uygulamaların log ihtiyacı için geliştirilmiş kütüphanedir.

## Content

- [Features](#Features)
- [Install](#install)
- [Usage](#usage)
- [Maintainers](#maintainers)
- [Contributing](#contributing)
- [License](#license)

## Features

1. Logları Json Formatında tutması sayesinde arama ve gruplama işlemleri yapılabilmektedir.

```json
{
  "@timestamp": "2023-06-13T15:42:50.802+03:00",
  "message": "Başarılı şekilde Kayıt çekildi - ****",
  "logger_name": "tr.com.tslogger.demo.tsloggerdemo.controller.DemoController",
  "level": "INFO",
  "context": {
    "traceId": "cb274fc31236ece1",
    "spanId": "cb274fc31236ece1",
    "method": "GET",
    "session_id": "SERVICE",
    "ip_address": "0:0:0:0:0:0:0:1",
    "request_url": "http://localhost:8081/info",
    "username": "PUSULA_USER"
  },
  "application_name": "deneme",
  "trace": {
    "caller_class_name": "tr.com.tslogger.demo.tsloggerdemo.controller.DemoController",
    "caller_method_name": "info",
    "caller_file_name": "DemoController.java",
    "caller_line_number": 26
  }
}
```

2. Uygulama Hatalarını try-catch bloklarına alma ihtiyacı gerekmeksizin Stack Trace ile birlikte log olarak kaydediyor.

```json

{
  "@timestamp": "2023-06-13T16:33:51.82+03:00",
  "message": "Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception [Request processing failed; nested exception is java.lang.Exception: Hata alındı!] with root cause",
  "logger_name": "org.apache.catalina.core.ContainerBase.[Tomcat].[localhost].[/].[dispatcherServlet]",
  "level": "ERROR",
  "stack_trace": "java.lang.Exception: Hata alındı!\n\tat tr.com.tslogger.demo.tsloggerdemo.controller.DemoController.error(DemoController.java:40)\n\tat jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(NativeMethodAccessorImpl.java)\n\tat jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:77)\n\tat jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)\n\tat java.lang.reflect.Method.invoke(Method.java:568)\n\tat org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:205)\n\tat org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:150)\n\tat org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:117)\n\tat org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:895)\n\tat org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:808)\n\tat org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:87)\n\tat org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:1072)\n\tat org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:965)\n\tat org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1006)\n\tat org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:898)\n\tat javax.servlet.http.HttpServlet.service(HttpServlet.java:529)\n\tat org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:883)\n\tat javax.servlet.http.HttpServlet.service(HttpServlet.java:623)\n\tat org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:209)\n\tat org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:153)\n\tat org.apache.tomcat.webs...\n",
  "context": {
    "traceId": "7ff67688ea4584c4",
    "spanId": "7ff67688ea4584c4",
    "method": "GET",
    "session_id": "SERVICE",
    "ip_address": "0:0:0:0:0:0:0:1",
    "request_url": "http://localhost:8081/error1",
    "username": "PUSULA_USER"
  },
  "application_name": "deneme",
  "trace": {
    "caller_class_name": "org.apache.juli.logging.DirectJDKLog",
    "caller_method_name": "log",
    "caller_file_name": "DirectJDKLog.java",
    "caller_line_number": 175
  }
}
```

3. Console çıktıları daha sade ve okunaklı


4. Hassas veriler maskelenme sayesinde log üzerinde gözükmemesi sağlanabiliyor.(Ör: kullanıcının parolası,kart bilgileri
   ,kimlik bilgileri vs.)

```java
log.info("Parola başarılı şekilde değiştirildi - password"+"545454545");
log.info("Parola Kaydedildi", kv("password", "dadadwwewew646464"));

```

Çıktı :

```json
{
  "@timestamp": "2023-06-13T16:37:13.429+03:00",
  "message": "Parola Kaydedildi",
  "logger_name": "tr.com.tslogger.demo.tsloggerdemo.controller.DemoController",
  "level": "INFO",
  "context": {
    "traceId": "64380f4d7ce49dc0",
    "spanId": "64380f4d7ce49dc0",
    "method": "GET",
    "session_id": "SERVICE",
    "ip_address": "0:0:0:0:0:0:0:1",
    "request_url": "http://localhost:8081/info",
    "username": "PUSULA_USER"
  },
  "password": "****",
  "application_name": "deneme",
  "trace": {
    "caller_class_name": "tr.com.tslogger.demo.tsloggerdemo.controller.DemoController",
    "caller_method_name": "info",
    "caller_file_name": "DemoController.java",
    "caller_line_number": 25
  }
}

```

6. Distributed tracing sayesinde dağıtık uygulamaları izlememize, özellikle bir hatanın yada performans sıkıntının
   nerede olduğunu takip etmemize yarar.
7. StructuredLog yapısı sunmaktadır. Logların daha standart halde log tutulmasını sağlamaktadır.(Geliştirme aşamasında)

``` java
 BusinessLogger.createLog(BusinessLog.forClass(this.getClass()).builder().message("ödeme alındı.").costumerInfo("Customer").category(Category.POLICE).logLevel(ELogLevel.INFO).build());
```

```json
{
  "@timestamp": "2023-06-13T16:38:36.261+03:00",
  "message": "ödeme alındı.",
  "logger_name": "tr.com.trs.logger.businesslog.BusinessLogger",
  "level": "INFO",
  "context": {
    "traceId": "6599f894d4a26eae",
    "spanId": "6599f894d4a26eae",
    "sessionInfo": "Keycloak.sessionInfo",
    "session.username": "Keycloak.security.username",
    "method": "GET",
    "session_id": "SERVICE",
    "ip_address": "0:0:0:0:0:0:0:1",
    "category": "POLICE",
    "request_url": "http://localhost:8081/business",
    "username": "PUSULA_USER",
    "customer": "Customer"
  },
  "customer": "Customer",
  "sessionInfo": "Keycloak.sessionInfo",
  "category": "POLICE",
  "customer": "Customer",
  "application_name": "deneme",
  "trace": {
    "caller_class_name": "tr.com.trs.logger.businesslog.BusinessLogger",
    "caller_method_name": "createLog",
    "caller_file_name": "BusinessLogger.java",
    "caller_line_number": 33
  }
}
```
8. Uygulamanın Profili yalnızca stage ve master olması halinde dosya yazma işlemini gerçekleştirmektedir.

        

## Tech Stack
Logstash-logback-encoder : https://github.com/logfellow/logstash-logback-encoder

| Spring Boot-2.7.12                                                                            | Spring Cloud Sleuth                                                                                          | Logback-Ecs-Encoder                                                                                                           | Maven                                                                              | 
|-----------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| <img height="60px" src="https://blog.knoldus.com/wp-content/uploads/2019/05/spring_boot.png"> | <img height="80px" src="https://miro.medium.com/v2/resize:fit:490/format:webp/1*ea1lfm4Y5x7hahaL_HYEcg.png"> | <img height="60px" src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRjOJRm6KdwwovEEdS9kKy6v0mNPVdP1zMoLQ&usqp=CAU"> | <img height="20px" src="https://maven.apache.org/images/apache-maven-project.png"> | 

<br>

## Install

Projeye maven dependency olarak eklenip kullanılabilmektedir.

```sh
<dependency>  
<groupId>tr.com.trs</groupId>  
<artifactId>ts-logger</artifactId>  
<version>1.1.0-RELEASE</version>  
</dependency>
```

## Usage

1. Application Log kullanılacaksa standarta uygun şekilde kullanır.Diğer parametreler arka planda eklenmektedir.

```java
    log.info("İşlem Başlatıldı");
```


2. Structured Log kullanılacaksa

BusinessLogger sınıfı inject edilerek kullanılır:

```java
BusinessLogger.createLog(BusinessLog.forClass(this.getClass()).builder().message("ödeme alındı.").costumerInfo("Customer").category(Category.POLICE).logLevel(ELogLevel.INFO).build());
```


## Parametreler

| Parametre                       | Açıklama                                                 | Default Değeri |
|---------------------------------|----------------------------------------------------------|----------------|
| logback.path                    | LogPath                          | ./logs         |
| spring.application.name         | Applicarion Name | deneme         |
| org.springframework.security    | Spring security Logs                               | LogLevel ERROR |
| org.springframework.cache       | Spring Cache Logs                                     | LogLevel ERROR |
| tr.com.trs                      | TRS paket Logs                                   | LogLevel INFO  |
| org.apache.kafka                | Kafka Logs                                         | LogLevel ERROR |
| org.apache.kafka.clients        | Kafka Logs                                         | LogLevel ERROR |
| org.apache.kafka.common.metrics | Kafka Logs                                         | LogLevel ERROR |
| org.liquibase.core              | Liquibase Logs                                     | LogLevel ERROR |
| org.hibernate                   | Hibernate Logs                                        | LogLevel ERROR |
| org.hibernate.SQL               | SQL Logs                                              | LogLevel ALL   |

 

```yml
logging:
   level:
     root: INFO
     org.springframework.web: DEBUG
```


# spring-boot-tomcat-externo
Usando *springBoot* com tomcat embeded apenas no desenvolvimento e tomcat externo para hospedar sua aplicação.

As vezes queremos desenvolver uma aplicação usando as facilidades do springboot com tomcat embeded, contudo quando vamos colocar em produção o cliente exige que seja um tomcat externo, ou tomcat já instalado na empresa.

As alterações são simples:

1) Adicionar no POM.XML 
```
<packaging>war</packaging>
````
2) Adicionar a dependência que usará o tomcat embeded apenas em desenvolvimento, quando for empacotado com WAR não terá o tomcat no pacote

```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-tomcat</artifactId>
	<scope>provided</scope>
</dependency>
```

3) Depois temos que abrir a classe principal da aplicação springboot, onde tem o famoso  `public static void main` e extender  `SpringBootServletInitializer`

*Alterações feitas, quando subir sua aplicação em desenvolvimento com intelij ou eclipse que seja, usando a classe springboot, que tem o void main, ele vai ter um tomcat embeded, agora no momento que rodar o comando `mvn clean package` a aplicação será empacotada em WAR e poderá ser colocada dentro de um container java tomcat externo.*

**OBS:** Quando estamos em desenvolvimento teremos os path apenas com /, tipo `http://localhost:8080/clientes` por exemplo, quando implantamos no tomcat externo como ele tem diversas aplicações temos que dizer qual queremos acessar, desta forma ficaria assim, `http://localhost:8080/AQUINOMEDOWARGERADO/clientes`

Você pode ter o seguinte erro usando o intelij
```
Caused by: java.lang.IllegalStateException: Failed to introspect Class [org.springframework.boot.web.servlet.support.SpringBootServletInitializer] from ClassLoader [jdk.internal.loader.ClassLoaders$AppClassLoader@77556fd]
	at org.springframework.util.ReflectionUtils.getDeclaredMethods(ReflectionUtils.java:481) ~[spring-core-5.3.8.jar:5.3.8]
	at org.springframework.util.ReflectionUtils.getDeclaredMethods(ReflectionUtils.java:455) ~[spring-core-5.3.8.jar:5.3.8]
	at org.springframework.core.type.StandardAnnotationMetadata.getAnnotatedMethods(StandardAnnotationMetadata.java:151) ~[spring-core-5.3.8.jar:5.3.8]
	... 17 common frames omitted
Caused by: java.lang.NoClassDefFoundError: javax/servlet/ServletContext
	at java.base/java.lang.Class.getDeclaredMethods0(Native Method) ~[na:na]
	at java.base/java.lang.Class.privateGetDeclaredMethods(Class.java:3167) ~[na:na]
	at java.base/java.lang.Class.getDeclaredMethods(Class.java:2310) ~[na:na]
	at org.springframework.util.ReflectionUtils.getDeclaredMethods(ReflectionUtils.java:463) ~[spring-core-5.3.8.jar:5.3.8]
	... 19 common frames omitted
Caused by: java.lang.ClassNotFoundException: javax.servlet.ServletContext
	at java.base/jdk.internal.loader.BuiltinClassLoader.loadClass(BuiltinClassLoader.java:583) ~[na:na]
	at java.base/jdk.internal.loader.ClassLoaders$AppClassLoader.loadClass(ClassLoaders.java:178) ~[na:na]
	at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:521) ~[na:na]
	... 23 common frames omitted
```

**Corrija assim**

No IntelliJ as dependências fornecidas não são adicionadas ao classpath. Supondo que você queira manter o IDEA, faça isso:

![bugIntelijj](https://user-images.githubusercontent.com/8472133/126052352-3a72ab06-e3c9-4810-8bba-5e7f3275a47d.png)

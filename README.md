# spring-boot-tomcat-externo
Usando *springBoot* com tomcat embeded apenas no desenvolvimento e tomcat externo para hospedar app

As vezes queremos desenvolver uma aplicação usando as facilidades do springboot com tomcat embeded, contudo quando vamos colocar em produção o cliente exige quse seja um tomcat externo, ou tomcat já instalado na empresa.

As alterações são simples:

1) Adicionar no POM.XML `<packaging>war</packaging>`
2) Adicionar a dependencia que usará o tomcat embeded apenas em desenvolvimento, quando for empacotado com WAR não terá o tomcat no pacote
`		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
			<scope>provided</scope>
		</dependency>`

3) Depois temos que abrir a classe principal da aplicação springboot, onde tem o famoso  `public static void main` e extender  `SpringBootServletInitializer`

*Alterações feitas, quando subir sua aplicação em desenvolvimento com intelij ou eclipse que seja, usando a classe springboot ele vai ter um tomcat embeded, agora no momento que rodar o comando `mvn clean package` a aplicação será empacotada em WAR e poderá ser colocada dentro de um container java tomcat externo.*

**OBS:** Quando estamos em desenvolvimento teremos os path apenas com /, tipo `http://localhost:8080/clientes` por exemplo, quando implantamos no tomcat externo como ele tem diversas aplicações temos que dizer qual queremos acessar, desta forma ficaria assim, `http://localhost:8080/AQUINOMEDOWARGERADO/clientes`

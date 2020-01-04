Abrimos una consola de Azure Cloud, y clonamos el repositorio:

```sh
git clone https://github.com/eugeniogarcia/AzureSpring
```

Ejecutamos:

```sh
cd AzureSpring

mvn spring-boot:run
```

Abrimos otra consola de Azure Cloud y ejecutamos:

```sh
curl http://localhost:8083/
```

Todo funciona. Procedamos ahora a desplegar el codigo en Azure. Para configurar el plugin hacemos:

```sh
mvn azure-webapp:config
```

Nos ira preguntando una serie de cosas que se volcaran al pom, y especificamente a las propiedades del plugin de despliegue en Azure. 




```yml
<build> 
<plugins> 
  <plugin> 
	<groupId>com.microsoft.azure</groupId>  
	<artifactId>azure-webapp-maven-plugin</artifactId>  
	<version>1.8.0</version>  
	<configuration>
	  <schemaVersion>V2</schemaVersion>
	  <resourceGroup>prueba_webaps</resourceGroup>
	  <appName>gs-spring-boot-1578054422803</appName>
	  <pricingTier>F1</pricingTier>
	  <region>westeurope</region>
	  <runtime>
		<os>linux</os>
		<javaVersion>jre8</javaVersion>
		<webContainer>jre8</webContainer>
	  </runtime>
	  <!-- Begin of App Settings  -->
	  <appSettings>
		<property>
			<name>JAVA_OPTS</name>
			<value>-D server.port=8083</value>
		</property>
	  </appSettings>
	   <!-- End of App Settings  -->
	  <deployment>
		<resources>
		  <resource>
			<directory>${project.basedir}/target</directory>
			<includes>
			  <include>*.jar</include>
			</includes>
		  </resource>
		</resources>
	  </deployment>
	</configuration>
  </plugin>  
``` 
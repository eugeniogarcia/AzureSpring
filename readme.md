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
...

  <plugin> 
	<groupId>com.microsoft.azure</groupId>  
	<artifactId>azure-webapp-maven-plugin</artifactId>  
	<version>1.8.0</version>  
	<configuration>
	  <schemaVersion>V2</schemaVersion>
	  <resourceGroup>prueba_webapp</resourceGroup>
	  <appName>testSpringBoot</appName>
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
``` 

Especificamente fijemonos en:

- resourceGroup. Nombre del resource group en Azure
- appName. Nombre que tendra nuestra Web App en Azure
- pricingTier. Plan que elegimos para crear nuestra aplicacion. En este caso F1, que es el plan gratuito
- region. Region de Azure

En el `<runtime>` estamos especificando que la Web App queremos que corra en Linux con el runtime de Java 8.

Tambien podemos pasar parametros a la java vm con `<appSettings>`. En este caso especificamos que el MiSe escuchara en el 80 - cuando se ejecuto con `mvn spring-boot:run` toma el puerto del `Application.yaml`; con este setup lo que hacemos es que cuando se ejecute la Web App escuche en el 80:

```yml
  <appSettings>
	<property>
		<name>JAVA_OPTS</name>
		<value>-D"server.port"=80</value>
	</property>
  </appSettings>
```

Para hacer el despliegue hacemos:

```sh
mvn clean package

mvn azure-webapp:deploy
```

__Notese__ que si el _resource group_ y/o la __web app_ existiesen ya en Azure, se actualizaran. De lo contrario, se crean nuevas.

El despliegue deja en `/wwwroot/` el archivo `app.jar`.


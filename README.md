# CONFIG
How to centralize the configuration with spring-cloud-config server

create a config service
<pre>
  <code>
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.config.server.EnableConfigServer;

@SpringBootApplication
@EnableConfigServer
public class ConfigServerApplication {

	public static void main(String[] args) {
		SpringApplication.run(ConfigServerApplication.class, args);
	}

}
  </code>
</pre>

- add properties
	- server port
 	- application name
  	- directory of all configurations files
	  	- application.yml for globals configurations
	  	- service-name.yml for his specific config
  	  
<pre>
<code>
server:
  port: 8888
spring:
  application:
    name: config-server
spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/pounct/CONFIG
</code>
</pre>

- We can use local directory using uri: file://C:/.../config-directory

- And start config server service (here port:8888)
- in consul :

  <img src="images/Captura de pantalla1.png"/>

- the service is in register service just with the dependency consul-discovery
- but we can enable discovery explicitment because in this version of consul
	is enable as default...
- for all service we need to register at consul discovery sevice with:

<pre>
  <code>
@EnableDiscoveryClient
  </code>
</pre>

<pre>
  <code>
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.config.server.EnableConfigServer;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableConfigServer
@EnableDiscoveryClient
public class ConfigServerApplication {

	public static void main(String[] args) {
		SpringApplication.run(ConfigServerApplication.class, args);
	}

}
  </code>
</pre>

and we can consult ower configuration
samples:
<pre>
  <code>
	  http://localhost:8888/application/default
	  http://localhost:8888/challenge/default
	  http://localhost:8888/challenge/dev
	  http://localhost:8888/challenge/prod
  </code>
</pre>
<img src="images/cofglob.png"/>
<img src="images/cofcha1.png"/>
<img src="images/cofcha2.png"/>
<img src="images/cofcha3.png"/>

- now that everything is fine
- we must indicate to each service where to look for its global and specific configuration (in our case: config_server)
- Before there was only the internal configuration "application.yml/.properties" and now in its local configuration there is a dependency towards an external one in our case the challenge service has the property:
<pre>
  <code>
server:
   port:9001
spring:
   application:
     name:challenge
   config:
     import:optional:configserver:http://localhost:8888
  </code>
</pre>

and to test that it is looking for its configuration from config_server we will create an endpoint with RestController as follows:

<img src="images/Captura de pantalla2.png"/>
<img src="images/challengetest.png"/>

when we use the values of the configuration file we use the @value annotation

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

</code>
</pre>

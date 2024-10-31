 To create a Spring Boot application that acts as a proxy to another REST API, you can use `RestTemplate` or `WebClient` to forward the request to the downstream API and return the response payload exactly as received. Here’s how to implement it using `WebClient`:

1. **Setup Spring Boot Application**:
   - Add dependencies for Spring Web and WebFlux in your `pom.xml`.

2. **Create a Controller to Handle the Proxy Request**:
   - Define an endpoint in your Spring Boot app that receives the JSON payload and forwards it to the downstream API.

3. **Use WebClient for Proxying**:
   - Set up `WebClient` to call the downstream API, passing along the received payload.

### Step-by-Step Code Implementation

#### 1. Add Dependencies in `pom.xml`

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-webflux</artifactId>
    </dependency>
</dependencies>
```

#### 2. Configure WebClient Bean

In your `@Configuration` class, define a `WebClient` bean:

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.reactive.function.client.WebClient;

@Configuration
public class WebClientConfig {

    @Bean
    public WebClient.Builder webClientBuilder() {
        return WebClient.builder();
    }
}
```

#### 3. Create a Proxy Controller

Create a REST controller with an endpoint that receives the JSON payload, forwards it to the downstream API, and returns the exact response.

```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.reactive.function.client.WebClient;
import reactor.core.publisher.Mono;

@RestController
@RequestMapping("/api")
public class ProxyController {

    private final WebClient.Builder webClientBuilder;

    @Value("${downstream.api.url}")
    private String downstreamApiUrl;

    public ProxyController(WebClient.Builder webClientBuilder) {
        this.webClientBuilder = webClientBuilder;
    }

    @PostMapping("/proxy")
    public Mono<ResponseEntity<String>> proxyRequest(@RequestBody String payload) {
        return webClientBuilder.build()
                .post()
                .uri(downstreamApiUrl)
                .bodyValue(payload)
                .retrieve()
                .bodyToMono(String.class)
                .map(response -> ResponseEntity.ok().body(response));
    }
}
```

#### 4. Configure the Downstream API URL

Add the downstream API URL in `application.properties`:

```properties
downstream.api.url=https://example.com/target-api
```

### Explanation

1. **Endpoint**: `/api/proxy` accepts POST requests with any JSON payload.
2. **Forwarding**: The controller uses `WebClient` to send a POST request to the downstream API, passing the exact JSON payload it received.
3. **Returning the Response**: It returns the response from the downstream API directly to the client.

#### Example Usage

If you send a POST request to `http://localhost:8080/api/proxy` with a JSON payload, this Spring Boot application will forward it to `https://example.com/target-api` and return the response exactly as received from the downstream API.


To add an HTTP proxy with basic authentication (username and password) to the `WebClient`, we can configure a `HttpClient` instance with the proxy settings and then use it in the `WebClient` setup. Here’s how to modify the setup to include these details:

### Step-by-Step Configuration

1. **Add Dependencies for Reactor Netty**:
   - Make sure you have the `spring-boot-starter-webflux` dependency, as it includes the necessary libraries for configuring a `WebClient` with a proxy.

2. **Configure the Proxy Settings in the WebClient Bean**:
   - Set up the proxy with the URL, host, port, username, and password.

### Updated Code with Proxy Configuration

#### 1. Add Required Dependencies

If not already present, add the `Reactor Netty` dependency for low-level HTTP customization:

```xml
<dependency>
    <groupId>io.projectreactor.netty</groupId>
    <artifactId>reactor-netty-http</artifactId>
</dependency>
```

#### 2. Configure the Proxy in `WebClientConfig`

In the `WebClientConfig` configuration class, set up a `WebClient` bean with the proxy configuration:

```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.reactive.function.client.WebClient;
import reactor.netty.http.client.HttpClient;
import reactor.netty.transport.ProxyProvider;
import java.nio.charset.StandardCharsets;
import java.util.Base64;

@Configuration
public class WebClientConfig {

    @Value("${downstream.proxy.host}")
    private String proxyHost;

    @Value("${downstream.proxy.port}")
    private int proxyPort;

    @Value("${downstream.proxy.username}")
    private String proxyUsername;

    @Value("${downstream.proxy.password}")
    private String proxyPassword;

    @Bean
    public WebClient.Builder webClientBuilder() {
        // Encode username and password in Base64 for Basic Authentication
        String authValue = "Basic " + Base64.getEncoder()
                .encodeToString((proxyUsername + ":" + proxyPassword).getBytes(StandardCharsets.UTF_8));

        // Configure HttpClient with Proxy settings
        HttpClient httpClient = HttpClient.create()
                .proxy(proxy -> proxy.type(ProxyProvider.Proxy.HTTP)
                        .host(proxyHost)
                        .port(proxyPort))
                .headers(headers -> headers.add("Proxy-Authorization", authValue));

        return WebClient.builder().clientConnector(new ReactorClientHttpConnector(httpClient));
    }
}
```

#### 3. Add Proxy Configuration in `application.properties`

Add your proxy details to the `application.properties`:

```properties
downstream.proxy.host=your-proxy-host
downstream.proxy.port=8080
downstream.proxy.username=your-username
downstream.proxy.password=your-password
```

### Explanation

1. **Proxy Authentication**: The username and password are encoded in Base64 to set up basic authentication for the proxy.
2. **HttpClient Configuration**: The `HttpClient` is configured to use the proxy with the provided host and port and include the `Proxy-Authorization` header.
3. **WebClient Setup**: The configured `HttpClient` is then set in the `WebClient` configuration, so every call made with this `WebClient` will route through the proxy with authentication.

This setup allows your `WebClient` to route requests through the specified HTTP proxy with basic authentication.

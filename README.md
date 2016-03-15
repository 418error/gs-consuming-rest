# gs-consuming-rest

#### Built using...
* A Mac
* Atom
* Java JDK 1.8
* Gradle 2.10

#### The steps...

1. Create a project folder

  ```
  mkdir gs-consuming-rest
  cd gs-consuming-rest
  ```
2. Create a Gradle build script
  ```
  touch build.gradle
  atom build.gradle
  ```
  Copy the contents from this [Initial Gradle build script](  https://github.com/spring-guides/gs-consuming-rest/blob/master/initial/build.gradle) to the build.gradle file

3. Create the project directory structure
  ```
  mkdir -p src/main/java/hello
  ```

4. Create a domain class
  ```
  touch src/main/java/hello/Quote.java
  atom src/main/java/hello/Quote.java
  ```
  Add the following

  ```java
  package hello;

  import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

  @JsonIgnoreProperties(ignoreUnknown = true)
  public class Quote {

    private String type;
    private Value value;

    public Quote() {
    }

    public String getType() {
        return type;
    }

    public void setType(String type) {
        this.type = type;
    }

    public Value getValue() {
        return value;
    }

    public void setValue(Value value) {
        this.value = value;
    }

    @Override
    public String toString() {
        return "Quote{" +
                "type='" + type + '\'' +
                ", value=" + value +
                '}';
    }
  }
  ```

5. Create an additional class to embed the inner quotation itself.
  ```
  touch src/main/java/hello/Value.java
  atom src/main/java/hello/Value.java
  ```
  Add the following

  ```java
  package hello;

  import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

  @JsonIgnoreProperties(ignoreUnknown = true)
  public class Value {

      private Long id;
      private String quote;

      public Value() {
      }

      public Long getId() {
          return this.id;
      }

      public String getQuote() {
          return this.quote;
      }

      public void setId(Long id) {
          this.id = id;
      }

      public void setQuote(String quote) {
          this.quote = quote;
      }

      @Override
      public String toString() {
          return "Value{" +
                  "id=" + id +
                  ", quote='" + quote + '\'' +
                  '}';
      }
  }
  ```  
6. Make it executable
  ```
  touch src/main/java/hello/Application.java
  atom src/main/java/hello/Application.java
  ```
  Add the following

  ```java
  package hello;

  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;
  import org.springframework.boot.CommandLineRunner;
  import org.springframework.boot.SpringApplication;
  import org.springframework.boot.autoconfigure.SpringBootApplication;
  import org.springframework.web.client.RestTemplate;

  @SpringBootApplication
  public class Application implements CommandLineRunner {

      private static final Logger log = LoggerFactory.getLogger(Application.class);

      public static void main(String args[]) {
          SpringApplication.run(Application.class);
      }

      @Override
      public void run(String... args) throws Exception {
          RestTemplate restTemplate = new RestTemplate();
          Quote quote = restTemplate.getForObject("http://gturnquist-quoters.cfapps.io/api/random", Quote.class);
          log.info(quote.toString());
      }
  }
  ```
7. Run it
  * The one liner
  ```
  gradle bootRun
  ```
  * Build and run
  ```
  gradle Build
  java -jar build/libs/gs-consuming-rest-0.1.0.jar
  ```

8. Test the thing

  When running the above the console should output...
  ```
  2016-03-15 17:27:39.992  INFO 25167 --- [main] hello.Application : Quote{type='success', value=Value{id=6, quote='It embraces convention over configuration, providing an experience on par with frameworks that excel at early stage development, such as Ruby on Rails.'}}
  ```

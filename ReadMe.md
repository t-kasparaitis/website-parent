# Website Project

This project is a multi-module Maven project that includes a Spring Boot back-end and a React front-end, packaged together as a single deployable artifact.

## Prerequisite Setup

- Java 21+
    - Check info on https://start.spring.io/ for supported Java versions
    - Download & install from https://www.oracle.com/java/technologies/downloads/#java21
- Maven 3.9.9+
    - Download binary from https://maven.apache.org/download.cgi
    - Extract to C:\Program Files\Apache\Maven\apache-maven-3.9.9 or similar
    - Add as env variable "C:\Program Files\Apache\Maven\apache-maven-3.9.9\bin" under System Variables\Path
- Node.js and Yarn
    - Download Node.js LTS using Prebuilt Installer from https://nodejs.org/en/download/package-manager
    - Make sure to close all other programs before running the Node installer. This one fails if other stuff is running... Check it is installed afterwards with `node -v` & `npm -v`.
    - Install yarn via npm: `npm install --global yarn` and check it with `yarn -v`.
        - If there is an Execution Policy issue, run Powershell as Admin, then `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`.

## Project Structure & Setup

- `website-parent`: Parent Maven project that manages common configurations and dependencies.
  - `spring-boot-app`: Spring Boot application module.
  - `react-app`: React front-end module.

### Creating `website-parent`
Run the following in Powershell at the folder in which you want to create the parent:
```powershell
mvn archetype:generate "-DgroupId=io.tkasparaitis.website" "-DartifactId=website-parent" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"
```
Delete the App.java and AppTest.java files generated by the archetype. You won't need them in `website-parent`.

Modify the `website-parent` `pom.xml`. Below is the original:
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>io.tkasparaitis.website</groupId>
  <artifactId>website-parent</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>website-parent</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```
After changes it should look like this (ChatGPT-generated):
```
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>io.tkasparaitis.website</groupId>
    <artifactId>website-parent</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging> <!-- Change this from 'jar' to 'pom' to act as a parent project -->

    <name>website-parent</name>
    <url>http://maven.apache.org</url>

    <!-- Once these modules are created you can uncomment them: -->
    <!--
    <modules>
        <module>website-spring-boot-app</module>
        <module>website-react-app</module>
    </modules>
    -->

    <!-- Manage dependencies for child modules -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.5.4</version> <!-- You can use the latest version of Spring Boot -->
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <properties>
      <java.version>21</java.version>
    </properties>


    <build>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                    <!-- Additional configurations for the Spring Boot Maven plugin can go here -->
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
</project>
```

### Creating `website-spring-boot-app`

```
cd website-parent
mvn archetype:generate "-DgroupId=io.tkasparaitis.website" "-DartifactId=website-spring-boot-app" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"
```

Modify the `website-spring-boot-app` `pom.xml`. Below is the original:
```
<?xml version="1.0"?>
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>io.tkasparaitis.website</groupId>
    <artifactId>website-parent</artifactId>
    <version>1.0-SNAPSHOT</version>
  </parent>
  <groupId>io.tkasparaitis.website</groupId>
  <artifactId>website-spring-boot-app</artifactId>
  <version>1.0-SNAPSHOT</version>
  <name>website-spring-boot-app</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```
After changes it should look like this (ChatGPT-generated):
```
<?xml version="1.0"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  
  <modelVersion>4.0.0</modelVersion>
  
  <!-- Inherit from the parent project -->
  <parent>
    <groupId>io.tkasparaitis.website</groupId>
    <artifactId>website-parent</artifactId>
    <version>1.0-SNAPSHOT</version>
  </parent>
  
  <groupId>io.tkasparaitis.website</groupId>
  <artifactId>website-spring-boot-app</artifactId>
  <version>1.0-SNAPSHOT</version>
  <name>website-spring-boot-app</name>
  <url>http://maven.apache.org</url>

  <!-- Spring Boot and other dependencies -->
  <dependencies>
    <!-- Spring Boot Starter for building web applications -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- WebJars locator for serving WebJars -->
    <dependency>
      <groupId>org.webjars</groupId>
      <artifactId>webjars-locator</artifactId>
    </dependency>

    <!-- jQuery WebJar as an example -->
    <dependency>
      <groupId>org.webjars</groupId>
      <artifactId>jquery</artifactId>
      <version>3.5.1</version>
    </dependency>

    <!-- JUnit for unit testing -->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.13.2</version>
      <scope>test</scope>
    </dependency>

    <!-- Add the React module as a dependency once it's created -->
    <!-- <dependency>
      <groupId>io.tkasparaitis.website</groupId>
      <artifactId>website-react-app</artifactId>
      <version>1.0-SNAPSHOT</version>
    </dependency> -->
  </dependencies>

  <!-- Build configuration for Spring Boot -->
  <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>

</project>
```

### Creating `website-react-app`

Modify the `website-react-app` `pom.xml`. Below is the original:
```
mkdir website-react-app
cd website-react-app
yarn create next-app .
```

Installation choices:
  - √ Would you like to use TypeScript? ... No / `Yes`
  - √ Would you like to use ESLint? ... No / `Yes`
  - √ Would you like to use Tailwind CSS? ... No / `Yes`
  - √ Would you like to use `src/` directory? ... No / `Yes`
  - √ Would you like to use App Router? (recommended) ... No / `Yes`
  - √ Would you like to customize the default import alias (@/*)? ... `No` / Yes

Don't forget to uncomment the modules in `website-parent` `pom.xml`!
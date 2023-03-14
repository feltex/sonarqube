# sonarqube



## Projeto Base

Server status
[https://github.com/feltex/server-status](https://github.com/feltex/server-status)


---


- Como instalar o SonarQube: https://docs.sonarqube.org/9.8/setup-and-upgrade/install-the-server/

- Adicionar dependencia Jacoco
- Configurar o plugin Jacoco
- Gerar o Token no SonarQube
- Gerar o relatório do Sonar
- Gerar o relatório Jacoco


## Configuração do projeto

```

  <properties>
        <java.version>11</java.version>
        <jacoco.version>0.8.6</jacoco.version>
        <sonar.java.coveragePlugin>jacoco</sonar.java.coveragePlugin>
        <sonar.dynamicAnalysis>reuseReports</sonar.dynamicAnalysis>
        <sonar.jacoco.reportPath>${project.basedir}/../target/jacoco.exec</sonar.jacoco.reportPath>
        <sonar.language>java</sonar.language>
    </properties>


      <dependency>
            <groupId>org.jacoco</groupId>
            <artifactId>jacoco-maven-plugin</artifactId>
            <version>${jacoco.version}</version>
      </dependency>

<build>
....
 <plugin>
                <groupId>org.jacoco</groupId>
                <artifactId>jacoco-maven-plugin</artifactId>
                <version>${jacoco.version}</version>
                <executions>
                    <execution>
                        <id>jacoco-initialize</id>
                        <goals>
                            <goal>prepare-agent</goal>
                        </goals>
                    </execution>
                    <execution>
                        <id>jacoco-site</id>
                        <phase>package</phase>
                        <goals>
                            <goal>report</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
</build>


```

### Comandos do Maven


`mvn clean install`

`mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.login=sqa_8d7e8e6913f1c2a7e103b14f86910e33cb9afee8`


### SonarQube no Docker

---



```
version: "3.8"
services:
  sonarqube:
    image: sonarqube:9.9.0-community
    hostname: sonarqube
    container_name: sonarqube
    depends_on:
      - db
    environment:
      SONAR_JDBC_URL: jdbc:postgresql://db:5432/sonar
      SONAR_JDBC_USERNAME: sonar
      SONAR_JDBC_PASSWORD: sonar
    volumes:
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_extensions:/opt/sonarqube/extensions
      - sonarqube_logs:/opt/sonarqube/logs
    ports:
      - "9000:9000"
  db:
    image: postgres:13
    hostname: postgresql
    container_name: postgresql
    environment:
      POSTGRES_USER: sonar
      POSTGRES_PASSWORD: sonar
      POSTGRES_DB: sonar
    volumes:
      - postgresql:/var/lib/postgresql
      - postgresql_data:/var/lib/postgresql/data

volumes:
  sonarqube_data:
  sonarqube_extensions:
  sonarqube_logs:
  postgresql:
  postgresql_data:

```
 

### O que pode dar errado


 Caso você não consiga iniciar o SonarQube por problema de memória virtual
 
```

Feb 15 14:53:46 scratchpad elasticsearch: [1]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

https://stackoverflow.com/questions/51445846/elasticsearch-max-virtual-memory-areas-vm-max-map-count-65530-is-too-low-inc

```

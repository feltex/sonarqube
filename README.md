# SonarQube

  Como configurar o SonarQube no seu projeto Java.

![alt text](Sonarqube.png)


## Vídeo no youtube

[sonar](https://www.youtube.com/watch?v=dnvHgocLupM)

## Projeto Base

Server status
[https://github.com/feltex/server-status](https://github.com/feltex/server-status)


---

- Como instalar o SonarQube: https://docs.sonarqube.org/9.8/setup-and-upgrade/install-the-server/

- Acessar o SonarQube
- Gerar o Token no SonarQube
- Gerar o relatório do Sonar
- Adicionar dependência Jacoco
- Configurar o plugin Jacoco
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
                       <execution>
                        <id>jacoco-check</id>
                        <goals>
                            <goal>check</goal>
                        </goals>
                        <configuration>
                            <rules>
                                <rule>
                                    <element>PACKAGE</element>
                                    <limits>
                                        <limit>
                                            <counter>LINE</counter>
                                            <value>COVEREDRATIO</value>
                                            <minimum>0.20</minimum>
                                        </limit>
                                    </limits>
                                </rule>
                            </rules>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
</build>

```

### Comandos do Maven

```shell
mvn clean install
```

```shell
mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.login=sqa_8d7e8e6913f1c2a7e103b14f86910e33cb9afee8
```

### SonarQube no Docker

Iniciar o SonarQube

```shell
docker-compose -f docker/docker-compose.yaml up 
```

[Acesse](http://localhost:9000)

Parar o SonarQube

```shell
docker-compose -f docker/docker-compose.yaml down 
```

### O que pode dar errado

#### Warning

```
[WARNING] The artifact org.codehaus.mojo:sonar-maven-plugin:jar:3.9.1.2184 has been relocated to org.sonarsource.scanner.maven:sonar-maven-plugin:jar:3.9.1.2184: SonarQube plugin was moved to SonarSource organisation

```

Atualize a dependência

Caso você não consiga iniciar o SonarQube por problema de memória virtual

```

Feb 15 14:53:46 scratchpad elasticsearch: [1]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

https://stackoverflow.com/questions/51445846/elasticsearch-max-virtual-memory-areas-vm-max-map-count-65530-is-too-low-inc

```

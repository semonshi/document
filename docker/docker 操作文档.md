# Docker 操作文档

## docker安装

http://www.runoob.com/docker/centos-docker-install.html

## 私有服务及其配置

1.安装

```
docker pull registry		
```

2.启动（此为http服务，https需生产证书并配置，用到是百度）

```
docker run -d -p 5000:5000 -v /mnt/registry:/var/lib/registry --restart always --name registry registry
```

3.配置客户端 etc/daemon.json ->insecure registries -> 添加 ip:port

4.上传下载(可以将ip修改为方便的域名)

```
docker pull java
docker tag java ip:port/myjava
docker push ip:port/myjava
docker pull ip:port/myjava
```

## IDEA编辑器添加dockerfile-maven

可以参考地址 https://github.com/spotify/dockerfile-maven

```
 <plugin>
                <groupId>com.spotify</groupId>
                <artifactId>dockerfile-maven-plugin</artifactId>
                <version>1.4.4</version>
                <executions>
                    <execution>
                        <id>default</id>
                        <goals>
                            <goal>build</goal>
                            <goal>push</goal>
                        </goals>
                    </execution>
                </executions>

                <configuration>
                    <!--//上下文路径配置，此处设置为项目根路径-->
                    <contextDirectory>${project.basedir}</contextDirectory>
                    <useMavenSettingsForAuth>true</useMavenSettingsForAuth>
                    <repository>${registryUrl}/scz/${project.artifactId}</repository>
                    <tag>${project.version}</tag>
                    <!--//作为Dockerfile文件传入-->
                    <buildArgs>
                        <JAR_FILE>${project.build.finalName}.jar</JAR_FILE>
                    </buildArgs>
                </configuration>
            </plugin>
```





###Docker-compose 操作文档

- docker-compose 是针对对单机的

- docker-compose 命令 docker-compose -f *.yml up

- docker-compose.yml 配置样例

- ```
  version: '2'
  services:
    web:
      build: .
      ports:
       - "5000:5000"
      volumes:
       - .:/code
      networks:
        - front-tier
        - back-tier
    redis:
      image: redis
      volumes:
        - redis-data:/var/lib/redis
      networks:
        - back-tier
  volumes:
    redis-data:
      driver: local
  networks:
    front-tier:
      driver: bridge
    back-tier:
      driver: bridge
  ```




## docker-machine创建容器主机

Linux 安装

```
sudo curl -L https://github.com/docker/machine/releases/download/v0.13.0/docker-machine-`uname -s`-`uname -m` > /usr/local/bin/docker-machine
$ sudo chmod +x /usr/local/bin/docker-machine
```


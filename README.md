# Docker + Alpine + MariaDB(Mysql) + Nginx + RabbitMQ + Redis
基于 Dockerfile 文件，使用 Docker 容器服务的方式搭建 mariadb(mysql)/nginx/rabbitmq/redis 环境，易于维护、升级。

> 持续更新中...

# 1. 目录结构
```
docker-alpine
    |-- dockerfiles                 # docker 相关服务
    |-- **.yml                      # 通用配置文件
    |-- .env                        # 环境变量
```

# 2. 配置（关键）
可使用以下两种方式：
- 使用docker-compose.yml文件自定义构建项目组合（复杂，集中式）

- 使用.env文件自定义构建项目组合（简单，分离式）**《推荐》**

示例：
```
# 只需新增或修改所需的项目，目前配置中为mysql、redis和nginx
# docker compose
COMPOSE_FILE=mysql.yml:redis.yml:nginx.yml
```

# 3. 使用（关键）
进入项目所在目录：
执行命令：
```
docker-compose up
```

如果没问题，下次启动时可以以守护模式启用，所有容器将在后台运行：  
```
docker-compose up -d
``` 

除此之外，常见的docker-compose命令如下：
```
# 停止运行并移除容器
docker-compose down

# 启动单个服务
docker-compose up -d 服务名

# 查看当前运行的服务
docker-compose ps

# 构建镜像，--no-cache表示不用缓存，否则在重新编辑Dockerfile后再build可能会直接使用缓存而导致新编辑内容不生效
docker-compose build --no-cache

# 查看镜像
docker-compose images

# 查看日志
docker-compose logs

# 启动/停止服务
docker-compose start/stop 服务名

# 拉取镜像
docker-compose pull 镜像名
```

# 4. 常用指令
- 帮助
    ```
    docker-compose --help
    ```
- 列出网络 (包括跨群集中多个主机的网络)
    ```
    docker network ls
    ```
- 运行时，指定配置文件  
    ```
    docker-compose -p java -f docker-compose-tomcat.yml up -d
    ```
    **参数**:
    - `-p` 工程名称, 这里为 java, 代表java 相关配置
    - `-f` 配置文件
    - `-d` 后台运行

- 常用`shell`组合

    ```
    # 删除所有容器
    docker stop `docker ps -q -a` | xargs docker rm

    # 删除所有标签为none的镜像
    docker images|grep \<none\>|awk '{print $3}'|xargs docker rmi

    # 查找容器IP地址
    docker inspect 容器名或ID | grep "IPAddress"

    # 创建网段, 名称: mynet, 分配两个容器在同一网段中 (这样子才可以互相通信)
    docker network create mynet
    docker run -d --net mynet --name container1 my_image
    docker run -it --net mynet --name container1 another_image
    ```

> 更多帮助信息 `docker-compose -h|--help`      

# 5. 扩展知识
Version 3 (docker-composer) 不再支持参数说明**depends_on**

> 笔者解读: 通过配置 `networks` 参数更好地改进

- `depends_on` does not wait for `db` and `redis` to be “ready” before starting `web` - only until they have been started. If you need to wait for a service to be ready, see [Controlling startup order](https://docs.docker.com/compose/startup-order/) for more on this problem and strategies for solving it.
- Version 3 no longer supports the `condition` form of `depends_on`.
- The `depends_on` option is ignored when [deploying a stack in swarm mode](https://docs.docker.com/engine/reference/commandline/stack_deploy/) with a version 3 Compose file.

> @https://docs.docker.com/compose/compose-file/

**links**

**Warning**: The `--link` flag is a legacy feature of Docker. It may eventually be removed. Unless you absolutely need to continue using it, we recommend that you use [user-defined networks](https://docs.docker.com/engine/userguide/networking//#user-defined-networks) to facilitate communication between two containers instead of using `--link`. One feature that user-defined networks do not support that you can do with `--link` is sharing environmental variables between containers. However, you can use other mechanisms such as volumes to share environment variables between containers in a more controlled way.

# 6. 参考资料
- [[官方文档] Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
- [[官方文档] Compose file version 3 reference](https://docs.docker.com/compose/compose-file/)
- [[官方文档] Use volumes](https://docs.docker.com/storage/volumes/)
- [[官方文档] env-file](https://docs.docker.com/compose/env-file/)
- [[官方安装包] alpine-packages](https://pkgs.alpinelinux.org/packages)
- [[镜像] mysql 镜像说明](https://hub.docker.com/_/mysql/)
- [[镜像] php 镜像说明](https://hub.docker.com/_/php/)
- [[镜像站] 阿里云开源镜像站](https://opsx.alibaba.com/mirror)
- [[镜像站] 腾讯开源镜像站](https://mirrors.cloud.tencent.com/index.html)
- [[镜像站] 网易开源镜像站](http://mirrors.163.com/)
- [[镜像站] 清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/debian/)
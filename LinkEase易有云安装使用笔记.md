# 易有云安装使用笔记

## 用Docker 安装

```bash
docker run -d \
    -p 8897:8897 \                # Maps port 8897 on the host to port 8897 on the container
    --network host \              # Uses the host's network stack
    --name linkease \             # Names the container "linkease"
    --restart unless-stopped \    # Restarts the container unless it is explicitly stopped
    -v /home/johnson/wddisk/nas_data:/linkease-data \  # Mounts the host directory /home/johnson/wddisk/nas_data to /linkease-data in the container
    -v /home/johnson/wddisk/nas_config:/linkease-config \  # Mounts the host directory /home/johnson/wddisk/nas_config to /linkease-config in the container
    -v /etc/localtime:/etc/localtime:ro \  # Mounts the host's localtime file to the container to sync time settings, read-only
    -v /home/johnson/wddisk/nas_storage:/My-storage \  # Mounts the host directory /home/johnson/wddisk/nas_storage to /My-storage in the container
    -e PUID=1000 \      # Sets an environment variable PUID with the user ID 1000
    -e PGID=1000 \      # Sets an environment variable PGID with the group ID 1000
    linkease/linkease:latest      # Specifies the image to use, in this case, the latest version of linkease/linkease
```

### 命令解释

docker run -d：以分离模式（后台运行）运行容器。
-p 8897:8897：将主机的8897端口映射到容器的8897端口，允许访问容器在此端口上的服务。
--network host：使用主机的网络栈，这可以提高性能，但可能有安全隐患。
--name linkease：为容器分配名称“linkease”，以便于管理。
--restart unless-stopped：确保容器自动重启，除非明确停止。
-v /home/johnson/wddisk/nas_data:/linkease-data：将主机目录/home/johnson/wddisk/nas_data挂载到容器中的/linkease-data，允许数据持久化和共享。
-v /home/johnson/wddisk/nas_config:/linkease-config：将主机目录/home/johnson/wddisk/nas_config挂载到容器中的/linkease-config，用于配置文件。
-v /etc/localtime:/etc/localtime:ro：将主机的本地时间文件挂载到容器中，以同步时间设置，只读模式。
-v /home/johnson/wddisk/nas_storage:/My-storage：将主机目录/home/johnson/wddisk/nas_storage挂载到容器中的/My-storage。
-e PUID=1000 和 -e PGID=1000：设置用户ID和组ID的环境变量为1000，这可以用于容器内的权限管理。
linkease/linkease:latest：指定要使用的Docker镜像，在本例中是linkease/linkease的最新版本。

如果容器由于某种原因停止或中止，您可以使用 `docker start` 命令重新启动它。由于您已经指定了 `--restart unless-stopped`，Docker 会尝试自动重启容器，除非它被明确停止。然而，如果您需要手动重启它，可以使用以下命令：
docker start linkease

此命令将使用所有先前的设置重新启动名为 "linkease" 的容器。

如果您需要从头重新创建容器并使用相同的设置，可以再次使用 `docker run` 命令。以下是您提供的命令，您可以重复使用它来启动具有相同设置的容器：
```bash
docker run -d \
    -p 8897:8897 \
    --network host \
    --name linkease \
    --restart unless-stopped \
    -v /home/johnson/wddisk/nas_data:/linkease-data \
    -v /home/johnson/wddisk/nas_config:/linkease-config \
    -v /etc/localtime:/etc/localtime:ro \
    -v /home/johnson/wddisk/nas_storage:/My-storage \
    -e PUID=1000 \
    -e PGID=1000 \
    linkease/linkease:latest
```

重新启动容器的步骤：
自动重启：由于 --restart unless-stopped 选项，Docker 会自动重启容器。
手动重启：使用 docker start linkease 手动重启容器。
重新创建容器：如果需要，使用相同的设置再次运行 docker run 命令来重新创建容器。

当您在运行代码时看到 `WARNING: Published ports are discarded when using host network mode` 这条警告信息时，这意味着在使用 `--network host` 模式时，指定的端口映射（例如 `-p 8897:8897`）将被忽略。

- **`--network host` 模式**：在这种模式下，容器将直接使用主机的网络栈。这意味着容器中的服务将直接使用主机的网络接口和端口，而不需要通过 Docker 的端口映射机制。
- **端口映射被忽略**：由于容器直接使用主机的网络接口，指定的端口映射（例如 `-p 8897:8897`）将不起作用，因为容器已经可以直接访问主机的所有端口。
- 如果您确实需要使用端口映射而不是直接使用主机网络，可以去掉 `--network host` 选项，并保留 `-p` 选项。
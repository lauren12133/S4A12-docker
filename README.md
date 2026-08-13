## Docker

本地构建docker images，下载项目docker目录下dockerfile

```bash
mkdir -p dxf/Data/Pvf/ && cd dxf
docker build --no-cache -t dxf-server:latest . #当项目更新时重新该命令以构建最新docker镜像
```

**当你docker编译缓存占用过多的时候** 

```bash
docker system prune -a
```

将你的inventory.db上传在/Data，将你的Script.pvf上传自/Data/Pvf

dockr-compose运行

```bash
cat > docker-compose.yml <<EOF
services:
  dxf-server:
    image: dxf-server:latest
    container_name: dxf-server

    network_mode: host

    volumes:
      - ./Data:/dxf/Data

    environment:
      SERVER_IP: 这里改成你机器ip
      TZ: Asia/Shanghai
EOF
```

先执行“docker compose up”运行无问题ctrl+c终止，在执行“docker compose up -d”后台运行


直接运行命令

```bash
docker run -d \
  --name dxf-server \
  --network host \
  -e SERVER_IP=这里改成你机器ip \
  -e TZ=Asia/Shanghai \
  -v "$(pwd)/Data:/dxf/Data" \
  --restart unless-stopped \
  dxf-server:latest
```

更新内容直接替换inventory.db和Script.pvf即可。

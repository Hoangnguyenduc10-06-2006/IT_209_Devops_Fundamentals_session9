# Session 09 - Bài 2: Chạy Nginx Web Server với Docker

## Mục tiêu
- Khởi chạy Nginx (image nhẹ `nginx:alpine`) lắng nghe trên cổng **8080** của máy cá nhân.
- Tên container: `my-web`, ánh xạ cổng `8080:80`.
- Kiểm tra bằng `curl`, sau đó dừng và xóa container.

## Các lệnh thực hiện

```bash
# 1. Khởi chạy container
docker run -d -p 8080:80 --name my-web nginx:alpine

# (Kiểm tra container đang chạy)
docker ps

# 2. Kiểm tra trang web
curl http://localhost:8080

# 3. Dọn dẹp
docker stop my-web && docker rm my-web
```

| Tham số | Ý nghĩa |
|---|---|
| `-d` | Chạy container ở chế độ nền (detached) |
| `-p 8080:80` | Ánh xạ cổng 8080 của host → cổng 80 trong container |
| `--name my-web` | Đặt tên container |
| `nginx:alpine` | Image Nginx dựa trên Alpine Linux (rất nhẹ) |

---

## Kết quả

### 1. Khởi chạy container

```console
$ docker run -d -p 8080:80 --name my-web nginx:alpine
Unable to find image 'nginx:alpine' locally
alpine: Pulling from library/nginx
e2de96513ba9: Pulling fs layer
e72112c14215: Pulling fs layer
d9aae54b5831: Pulling fs layer
6c53d0b2a666: Pulling fs layer
745dfb2690dd: Pulling fs layer
9a9a644fdd6a: Pulling fs layer
64c8194480fe: Pulling fs layer
e76228b47809: Pulling fs layer
745dfb2690dd: Download complete
6c53d0b2a666: Download complete
64c8194480fe: Download complete
9a9a644fdd6a: Download complete
e2de96513ba9: Download complete
d9aae54b5831: Download complete
e76228b47809: Download complete
e2de96513ba9: Pull complete
6c53d0b2a666: Pull complete
d9aae54b5831: Pull complete
745dfb2690dd: Pull complete
64c8194480fe: Pull complete
9a9a644fdd6a: Pull complete
e76228b47809: Pull complete
d54d3939625e: Download complete
0deea27e58d9: Download complete
e72112c14215: Download complete
e72112c14215: Pull complete
Digest: sha256:df221db836e1754089190208cee7eeda94f233197056426eda74a43ab1abeac2
Status: Downloaded newer image for nginx:alpine
be88e68b1133faac75fb70f26627860cbbfda26a1e225df63fbb22d244b888a4
```

### Kiểm tra bằng `docker ps`

```console
$ docker ps --filter name=my-web
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                                     NAMES
be88e68b1133   nginx:alpine   "/docker-entrypoint.…"   3 seconds ago   Up 3 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-web
```

### 2. `curl http://localhost:8080`

```console
$ curl http://localhost:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, nginx is successfully installed and working.
Further configuration is required for the web server, reverse proxy, 
API gateway, load balancer, content cache, or other features.</p>

<p>For online documentation and support please refer to
<a href="https://nginx.org/">nginx.org</a>.<br/>
To engage with the community please visit
<a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
For enterprise grade support, professional services, additional 
security features and capabilities please refer to
<a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

✅ Lệnh `curl` trả về mã HTML chứa dòng **"Welcome to nginx!"**.

### 3. Dọn dẹp

```console
$ docker stop my-web && docker rm my-web
my-web
my-web
```

✅ Container `my-web` đã được dừng và xóa.

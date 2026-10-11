# Session 09 - Bài 3: Đóng gói trang web cá nhân vào Nginx image

## Mục tiêu
- Tạo trang `index.html` đơn giản.
- Viết `Dockerfile` dựa trên `nginx:alpine` để đóng gói trang web.
- Build image `my-html-app:v1` và chạy container trên cổng **8081**.

## Cấu trúc thư mục

```
ex3/
├── Dockerfile
├── index.html
└── README.md
```

### `index.html`

```html
<h1>Hello Docker Session 09!</h1>
```

### `Dockerfile`

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

- `FROM nginx:alpine`: dùng image Nginx nền Alpine làm base.
- `COPY ...`: ghi đè trang mặc định của Nginx (`/usr/share/nginx/html/index.html`) bằng trang của mình.

## Các lệnh thực hiện

```bash
docker build -t my-html-app:v1 .
docker run -d -p 8081:80 --name html-app my-html-app:v1
curl http://localhost:8081
```

---

## Kết quả

### 1. Build image

```console
$ docker build -t my-html-app:v1 .
#0 building with "desktop-linux" instance using docker driver

#1 [internal] load build definition from Dockerfile
#1 transferring dockerfile: 104B 0.0s done
#1 DONE 0.0s

#2 [internal] load metadata for docker.io/library/nginx:alpine
#2 DONE 0.0s

#3 [internal] load .dockerignore
#3 transferring context: 2B done
#3 DONE 0.0s

#4 [internal] load build context
#4 transferring context: 71B done
#4 DONE 0.0s

#5 [1/2] FROM docker.io/library/nginx:alpine@sha256:df221db836e1754089190208cee7eeda94f233197056426eda74a43ab1abeac2
#5 resolve docker.io/library/nginx:alpine@sha256:df221db836e1754089190208cee7eeda94f233197056426eda74a43ab1abeac2 0.0s done
#5 DONE 0.2s

#6 [2/2] COPY index.html /usr/share/nginx/html/index.html
#6 DONE 0.0s

#7 exporting to image
#7 exporting layers
#7 exporting layers 0.7s done
#7 exporting manifest sha256:342c8979404110853352537b8822f0102ec4495be1307aa3bfb8769fa0467518
#7 exporting manifest sha256:342c8979404110853352537b8822f0102ec4495be1307aa3bfb8769fa0467518 0.4s done
#7 exporting config sha256:173aacb8cac8126e3cb6978fba428a30f4e43c447196030b05ac40dc6d47b7b6
#7 exporting config sha256:173aacb8cac8126e3cb6978fba428a30f4e43c447196030b05ac40dc6d47b7b6 0.3s done
#7 exporting attestation manifest sha256:d861a53e8f52b7924f06d7778d2405f9fe0d44ee7e37598ace958fd294dd3a13 0.0s done
#7 exporting manifest list sha256:b2d69e991abdb8d3cfd1632acedac98a5a30793186b61a4139685ffd32343598 0.0s done
#7 naming to docker.io/library/my-html-app:v1 done
#7 unpacking to docker.io/library/my-html-app:v1 0.0s done
#7 DONE 1.4s

View build details: docker-desktop://dashboard/build/desktop-linux/desktop-linux/dlhtharppmk1mjz7d07pzo5is
```

### 2. Chạy container

```console
$ docker run -d -p 8081:80 --name html-app my-html-app:v1
e705f7260c91786a703fde573f1d9c85c2571c990a0e5e8161e13575bbdeb133

$ docker ps --filter name=html-app
CONTAINER ID   IMAGE            COMMAND                  CREATED         STATUS         PORTS                                     NAMES
e705f7260c91   my-html-app:v1   "/docker-entrypoint.…"   3 seconds ago   Up 3 seconds   0.0.0.0:8081->80/tcp, [::]:8081->80/tcp   html-app
```

### 3. Kiểm tra

```console
$ curl http://localhost:8081
<h1>Hello Docker Session 09!</h1>
```

✅ Lệnh `curl` trả về đúng chuỗi: `<h1>Hello Docker Session 09!</h1>`.

## Dọn dẹp (tùy chọn)

```bash
docker stop html-app && docker rm html-app
docker rmi my-html-app:v1
```

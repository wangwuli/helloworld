# FROM ... AS builder : 表示依赖的镜像只是使用在编译阶段
FROM golang:1.20.4 AS builder

# 编译阶段的工作目录，也可以作为全局工作目录
WORKDIR /app

# 把当前目录的所有内容copy到 WORKDIR指定的目录中
COPY . .

# 定义go build的工作环境，
# 例如GOOS=linux、GOARCH=amd64，
# 这样编译出来的 'main可执行文件' 就只能在linux的amd64架构中使用
ARG TARGETOS
ARG TARGETARCH

ENV GOPROXY=https://goproxy.cn

RUN GOOS=${TARGETOS} GOARCH=${TARGETARCH} go build -o main

FROM rockylinux:9

WORKDIR /home

# 把执行builder阶段文件拷贝到新阶段环境内
COPY --from=builder /app/main /home/app.exe
COPY --from=builder /app/config.json /home/config.json

# 运行main命令，启动项目
CMD ["/home/app.exe"]

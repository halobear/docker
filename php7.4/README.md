# 构建PHP7.4镜像

## 构建

```shell
docker build -t halobear/php:7.4-fpm-alpine .
```

## 推送

```shell
docker push halobear/php:7.4-fpm-alpine
```

## 多平台构建
```shell
# 创建
docker buildx create --name mybuilder
# 使用
docker buildx use mybuilder

# 构建
docker buildx build --push --platform linux/arm/v6,linux/arm/v7,linux/arm64,linux/amd64 -t halobear/php:7.4-fpm-alpine .
```

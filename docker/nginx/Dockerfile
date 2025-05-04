# ci-cd/docker/Dockerfile.nginx
FROM nginx:alpine

# Thiết lập timezone
RUN apk add --no-cache tzdata && \
    ln -sf /usr/share/zoneinfo/Asia/Ho_Chi_Minh /etc/localtime && \
    echo "Asia/Ho_Chi_Minh" > /etc/timezone

# Copy file cấu hình Nginx
COPY docker/nginx/nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
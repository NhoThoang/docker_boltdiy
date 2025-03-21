# Bolt.DIY - Hướng dẫn chạy bằng Docker Compose

## Giới thiệu

Bolt.DIY là một dự án chạy trên nền tảng Docker, giúp bạn triển khai nhanh chóng môi trường phát triển hoặc thử nghiệm. Dự án sử dụng hình ảnh (`image`) từ GitHub Container Registry.

## Yêu cầu hệ thống

- Docker
- Docker Compose

## Cách chạy dự án bằng Docker Compose

### 1. Tạo file `docker-compose.yml`

Dưới đây là nội dung file `docker-compose.yml`:

```yaml
services:
  bolt:
    image: ghcr.io/stackblitz-labs/bolt.diy:latest
    container_name: bolt-diy
    ports:
      - "7500:5173"
    volumes:
      - bolt_data:/app/data
    environment:
      - NODE_ENV=production
    restart: unless-stopped
    networks:
      - bolt_diy_network

volumes:
  bolt_data:

networks:
  bolt_diy_network:
    driver: bridge
```

### 2. Khởi động dịch vụ

Chạy lệnh sau để khởi động container:

```sh
docker compose up -d
```

Lệnh này sẽ:
- Tải hình ảnh `ghcr.io/stackblitz-labs/bolt.diy:latest` nếu chưa có.
- Tạo và chạy container với tên `bolt-diy`.
- Liên kết cổng `7500` của máy host với `5173` trong container.
- Lưu dữ liệu vào volume `bolt_data` để tránh mất dữ liệu khi container bị xóa.
- Tạo mạng `bolt_diy_network` để container có thể giao tiếp nội bộ.

### 3. Kiểm tra container

Kiểm tra container có chạy không:
```sh
docker ps
```

Nếu container không chạy, xem logs bằng lệnh:
```sh
docker logs bolt-diy
```

### 4. Dừng và xóa container

Dừng container:
```sh
docker compose down
```

Xóa container và volume nếu không cần thiết nữa:
```sh
docker compose down -v
```

## Mount dữ liệu ra ngoài (tùy chọn)

Nếu muốn lưu dữ liệu vào thư mục trên host thay vì volume Docker, sửa `docker-compose.yml` như sau:

```yaml
volumes:
  - ./data:/app/data
```

Điều này giúp bạn dễ dàng truy cập dữ liệu ngay trên máy host.

## Kết luận

Dự án Bolt.DIY có thể dễ dàng triển khai bằng Docker Compose với cách thiết lập trên. Nếu có vấn đề, hãy kiểm tra logs hoặc kiểm tra lại cấu hình mạng và volume.

Chúc bạn thành công! 🚀
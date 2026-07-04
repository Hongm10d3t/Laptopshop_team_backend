# LaptopHub Backend

Backend REST API cho website bán laptop và phụ kiện máy tính **LaptopHub**.

Dự án hiện tại đang ở giai đoạn **khởi tạo nền backend**: đã có cấu trúc package theo module, cấu hình Spring Boot, MySQL, JPA, Flyway, Docker Compose, response format chung, exception handler chung, base entity và tài liệu quy ước phát triển.

---

## 1. Tổng quan dự án

LaptopHub là hệ thống thương mại điện tử chuyên bán laptop và phụ kiện máy tính.

Các nhóm người dùng dự kiến:

| Actor     | Mô tả                                                                                     |
| --------- | ----------------------------------------------------------------------------------------- |
| Guest     | Khách vãng lai, có thể xem, tìm kiếm, lọc sản phẩm                                        |
| Customer  | Khách hàng đã đăng nhập, có thể quản lý giỏ hàng, đặt hàng, thanh toán, đánh giá sản phẩm |
| Admin     | Quản trị viên, có thể quản lý sản phẩm, đơn hàng, tồn kho, voucher, khách hàng, dashboard |
| ChatbotAI | Hỗ trợ tư vấn sản phẩm cho khách hàng và hỗ trợ Admin tra cứu dữ liệu                     |

---

## 2. Trạng thái hiện tại của backend

Dự án hiện tại **chưa có nghiệp vụ hoàn chỉnh** như đăng nhập, quản lý sản phẩm, giỏ hàng, đặt hàng. Tuy nhiên, phần nền ban đầu đã được chuẩn bị tương đối đầy đủ.

### Đã có

| Nhóm                | Nội dung                                                      |
| ------------------- | ------------------------------------------------------------- |
| Spring Boot project | Có class main `LaptopHubApplication`                          |
| Build tool          | Maven Wrapper `mvnw`, `mvnw.cmd`                              |
| Database config     | MySQL qua biến môi trường                                     |
| JPA                 | Spring Data JPA, Hibernate, `ddl-auto=validate`               |
| Migration           | Flyway đã được cấu hình                                       |
| Docker              | `docker-compose.yml` chạy MySQL 8                             |
| API prefix          | `/api/v1`                                                     |
| Response format     | `ApiResponse<T>`                                              |
| Error handling      | `ErrorCode`, `AppException`, `GlobalExceptionHandler`         |
| Base entity         | `BaseEntity` với `id`, `createdAt`, `updatedAt`               |
| JPA Auditing        | `JpaAuditingConfig`                                           |
| CORS                | Cho phép frontend local ở `localhost:3000`, `localhost:5173`  |
| Pagination DTO      | `PageResponse<T>`                                             |
| Health check        | `GET /api/v1/health`                                          |
| Docs                | Có tài liệu quy ước API, database, module plan, project rules |

### Chưa có

| Nhóm                  | Trạng thái                              |
| --------------------- | --------------------------------------- |
| Auth/Security/JWT     | Chưa triển khai                         |
| Entity nghiệp vụ      | Chưa có `User`, `Product`, `Order`, ... |
| Repository nghiệp vụ  | Chưa có                                 |
| Service nghiệp vụ     | Chưa có                                 |
| Controller nghiệp vụ  | Chưa có                                 |
| Migration SQL thật    | Chưa có file migration tạo bảng         |
| CRUD API              | Chưa có                                 |
| Transaction nghiệp vụ | Chưa có                                 |
| File upload           | Chưa có                                 |
| ChatbotAI             | Chưa triển khai                         |

---

## 3. Công nghệ sử dụng

| Công nghệ              | Vai trò                                     |
| ---------------------- | ------------------------------------------- |
| Java 25                | Ngôn ngữ lập trình chính                    |
| Spring Boot 3.5.0      | Framework backend                           |
| Maven                  | Quản lý dependency và build project         |
| Spring Web             | Xây dựng REST API                           |
| Spring Data JPA        | Làm việc với database qua Repository/Entity |
| Hibernate              | ORM implementation cho JPA                  |
| MySQL 8                | Database chính                              |
| Flyway                 | Quản lý version database bằng migration SQL |
| Lombok                 | Giảm code lặp như getter/setter             |
| Validation             | Validate request DTO                        |
| Docker Compose         | Chạy MySQL local nhanh hơn                  |
| JUnit/Spring Boot Test | Kiểm thử cơ bản                             |

---

## 4. Cấu trúc thư mục chính

```text
backend
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com
│   │   │       └── laptophub
│   │   │           ├── LaptopHubApplication.java
│   │   │           ├── common
│   │   │           ├── config
│   │   │           ├── security
│   │   │           ├── auth
│   │   │           ├── user
│   │   │           ├── catalog
│   │   │           ├── cart
│   │   │           ├── order
│   │   │           ├── payment
│   │   │           ├── voucher
│   │   │           ├── review
│   │   │           ├── inventory
│   │   │           ├── dashboard
│   │   │           └── chatbot
│   │   └── resources
│   │       ├── application.yml
│   │       └── db
│   │           └── migration
│   └── test
│       └── java
├── docs
│   ├── API_CONVENTION.md
│   ├── DATABASE_DESIGN.md
│   ├── MODULE_PLAN.md
│   └── PROJECT_RULES.md
├── docker-compose.yml
├── pom.xml
├── mvnw
├── mvnw.cmd
├── .env.example
├── .gitignore
└── README.md
```

---

## 5. Ý nghĩa các package chính

| Package     | Vai trò                                                                                                       |
| ----------- | ------------------------------------------------------------------------------------------------------------- |
| `common`    | Chứa thành phần dùng chung toàn hệ thống: response format, mã lỗi, exception handler, base entity, pagination |
| `config`    | Cấu hình hạ tầng: CORS, JPA Auditing, các bean dùng chung                                                     |
| `security`  | Dự kiến chứa cấu hình Spring Security, JWT filter, phân quyền                                                 |
| `auth`      | Dự kiến chứa đăng ký, đăng nhập, refresh token                                                                |
| `user`      | Dự kiến chứa hồ sơ khách hàng, địa chỉ giao hàng, quản lý khách hàng                                          |
| `catalog`   | Dự kiến chứa sản phẩm, danh mục, thương hiệu, tìm kiếm/lọc                                                    |
| `cart`      | Dự kiến chứa giỏ hàng                                                                                         |
| `order`     | Dự kiến chứa đặt hàng, xem/hủy đơn, quản lý đơn                                                               |
| `payment`   | Dự kiến chứa thanh toán COD/chuyển khoản                                                                      |
| `voucher`   | Dự kiến chứa voucher giảm giá                                                                                 |
| `review`    | Dự kiến chứa đánh giá sản phẩm                                                                                |
| `inventory` | Dự kiến chứa quản lý tồn kho                                                                                  |
| `dashboard` | Dự kiến chứa thống kê cho Admin                                                                               |
| `chatbot`   | Dự kiến chứa ChatbotAI tư vấn và tra cứu dữ liệu                                                              |

---

## 6. Kiến trúc code dự kiến cho từng module

Mỗi module nghiệp vụ nên tự chứa các layer của riêng nó.

Ví dụ module `catalog`:

```text
catalog
├── controller
│   └── ProductController.java
├── service
│   └── ProductService.java
├── repository
│   └── ProductRepository.java
├── entity
│   └── Product.java
├── dto
│   ├── ProductCreateRequest.java
│   ├── ProductUpdateRequest.java
│   └── ProductResponse.java
└── mapper
    └── ProductMapper.java
```

Không nên tạo các package layer cấp gốc như:

```text
controller
service
repository
entity
```

Lý do: dự án lớn sẽ khó quản lý, các file nghiệp vụ bị trộn lẫn với nhau.

Luồng xử lý chuẩn:

```text
Client
  ↓
Controller
  ↓
Service
  ↓
Repository
  ↓
Database
```

---

## 7. Các file nền quan trọng

### `LaptopHubApplication.java`

Class main của Spring Boot application.

```java
@SpringBootApplication
public class LaptopHubApplication {
    public static void main(String[] args) {
        SpringApplication.run(LaptopHubApplication.class, args);
    }
}
```

Đây là điểm bắt đầu khi chạy backend.

---

### `application.yml`

File cấu hình chính của ứng dụng.

Các cấu hình quan trọng:

```yaml
server:
  port: 8080
  servlet:
    context-path: /api/v1
```

Nghĩa là API sẽ có prefix:

```text
/api/v1
```

Ví dụ:

```text
GET http://localhost:8080/api/v1/health
```

Database được cấu hình qua biến môi trường:

```yaml
spring:
  datasource:
    url: jdbc:mysql://${DB_HOST:localhost}:${DB_PORT:3306}/${DB_NAME:laptophub}
    username: ${DB_USERNAME}
    password: ${DB_PASSWORD}
```

JPA đang dùng chế độ:

```yaml
spring:
  jpa:
    hibernate:
      ddl-auto: validate
```

Nghĩa là Hibernate **không tự tạo bảng**, chỉ kiểm tra Entity có khớp database không. Việc tạo/sửa bảng phải đi qua Flyway migration.

---

### `ApiResponse.java`

Chuẩn hóa response API toàn hệ thống.

Response thành công:

```json
{
  "success": true,
  "message": "OK",
  "data": {},
  "timestamp": "2026-07-04T00:00:00Z"
}
```

Response lỗi:

```json
{
  "success": false,
  "message": "Không tìm thấy dữ liệu",
  "errorCode": "RESOURCE_NOT_FOUND",
  "timestamp": "2026-07-04T00:00:00Z"
}
```

---

### `ErrorCode.java`

Định nghĩa các mã lỗi chuẩn.

Một số mã lỗi hiện có:

| ErrorCode            | HTTP Status | Ý nghĩa                |
| -------------------- | ----------: | ---------------------- |
| `VALIDATION_ERROR`   |         400 | Dữ liệu không hợp lệ   |
| `BAD_REQUEST`        |         400 | Request sai            |
| `UNAUTHENTICATED`    |         401 | Chưa xác thực          |
| `UNAUTHORIZED`       |         403 | Không có quyền         |
| `RESOURCE_NOT_FOUND` |         404 | Không tìm thấy dữ liệu |
| `RESOURCE_CONFLICT`  |         409 | Dữ liệu xung đột       |
| `INTERNAL_ERROR`     |         500 | Lỗi hệ thống           |

---

### `AppException.java`

Exception dùng cho lỗi nghiệp vụ.

Ví dụ:

```java
throw new AppException(ErrorCode.RESOURCE_NOT_FOUND, "Không tìm thấy sản phẩm");
```

---

### `GlobalExceptionHandler.java`

Bộ xử lý lỗi toàn cục.

Nhiệm vụ:

```text
- Bắt AppException
- Bắt lỗi validate request
- Bắt lỗi validate param/path
- Bắt lỗi hệ thống không mong muốn
- Trả lỗi theo format ApiResponse
```

Nhờ file này, controller/service không cần tự `try-catch` ở mọi nơi.

---

### `BaseEntity.java`

Class cha cho các entity sau này.

Các field dùng chung:

```text
id
createdAt
updatedAt
```

Ví dụ entity sau này:

```java
@Entity
@Table(name = "products")
public class Product extends BaseEntity {
    private String name;
    private BigDecimal price;
}
```

---

### `JpaAuditingConfig.java`

Bật JPA Auditing để tự động set:

```text
createdAt
updatedAt
```

khi tạo/cập nhật entity.

---

### `CorsConfig.java`

Cho phép frontend local gọi backend.

Hiện tại cho phép:

```text
http://localhost:3000
http://localhost:5173
```

Phù hợp với React/Next/Vite local development.

---

### `PageResponse.java`

DTO dùng chung cho API phân trang.

Ví dụ response phân trang:

```json
{
  "content": [],
  "page": 0,
  "size": 10,
  "totalElements": 100,
  "totalPages": 10,
  "last": false
}
```

---

### `HealthController.java`

API kiểm tra server đang chạy.

Endpoint:

```text
GET /api/v1/health
```

Ví dụ:

```bash
curl http://localhost:8080/api/v1/health
```

---

## 8. Yêu cầu môi trường

Trước khi chạy dự án, cần cài:

| Công cụ          | Phiên bản khuyến nghị      | Ghi chú                              |
| ---------------- | -------------------------- | ------------------------------------ |
| JDK              | 25                         | Project hiện đang cấu hình Java 25   |
| Docker Desktop   | Mới nhất                   | Dùng để chạy MySQL local             |
| Git              | Mới nhất                   | Clone source code                    |
| IDE              | IntelliJ IDEA hoặc VS Code | IntelliJ khuyến nghị cho Spring Boot |
| Postman/Insomnia | Tùy chọn                   | Test API                             |

Kiểm tra Java:

```bash
java -version
```

Kiểm tra Docker:

```bash
docker --version
docker compose version
```

Kiểm tra Git:

```bash
git --version
```

---

## 9. Cấu hình biến môi trường

Dự án dùng file `.env` để cấu hình thông tin nhạy cảm như database password, JWT secret, API key.

Tạo file `.env` từ file mẫu:

```bash
cp .env.example .env
```

Trên Windows PowerShell, nếu không dùng được `cp`, dùng:

```powershell
Copy-Item .env.example .env
```

Nội dung mẫu nên có dạng:

```env
DB_HOST=localhost
DB_PORT=3306
DB_NAME=laptophub
DB_USERNAME=root
DB_PASSWORD=changeme

JWT_SECRET=changeme

OPENAI_API_KEY=changeme
```

Lưu ý:

```text
Không commit file .env lên Git.
Không gửi .env cho người khác.
Không hardcode password, JWT secret, API key trong code.
```

---

## 10. Chạy MySQL bằng Docker Compose

Nếu máy chưa có MySQL local, có thể chạy bằng Docker Compose.

Tại thư mục gốc project:

```bash
docker compose up -d
```

Kiểm tra container:

```bash
docker ps
```

Container dự kiến:

```text
laptophub-mysql
```

Dừng MySQL container:

```bash
docker compose down
```

Dừng và xóa luôn volume dữ liệu:

```bash
docker compose down -v
```

Chỉ dùng `down -v` khi muốn xóa sạch database local.

---

## 11. Chạy backend

### Cách 1: Chạy bằng Maven Wrapper

Trên Linux/Mac/Git Bash:

```bash
./mvnw spring-boot:run
```

Trên Windows PowerShell:

```powershell
.\mvnw.cmd spring-boot:run
```

Nếu gặp lỗi permission trên Linux/Mac:

```bash
chmod +x mvnw
./mvnw spring-boot:run
```

---

### Cách 2: Chạy bằng IDE

Mở project bằng IntelliJ IDEA hoặc VS Code.

Chạy class:

```text
com.laptophub.LaptopHubApplication
```

Nếu dùng VS Code, cần đảm bảo IDE đọc được file `.env` hoặc đã set biến môi trường tương ứng.

---

## 12. Kiểm tra backend đã chạy chưa

Sau khi chạy app, mở:

```text
http://localhost:8080/api/v1/health
```

Hoặc dùng cURL:

```bash
curl http://localhost:8080/api/v1/health
```

Kết quả mong muốn:

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "status": "UP",
    "service": "laptophub-backend",
    "timestamp": "..."
  },
  "timestamp": "..."
}
```

---

## 13. Chạy test

Trên Linux/Mac/Git Bash:

```bash
./mvnw clean test
```

Trên Windows PowerShell:

```powershell
.\mvnw.cmd clean test
```

Test hiện tại chủ yếu kiểm tra Spring context có load được không.

Lưu ý: khi bắt đầu có Entity, Repository, Migration và test tích hợp database, có thể cần MySQL đang chạy.

---

## 14. Quy ước API

Tất cả API có prefix:

```text
/api/v1
```

Ví dụ:

```text
GET /api/v1/health
GET /api/v1/products
POST /api/v1/auth/login
```

Response thành công luôn dùng:

```json
{
  "success": true,
  "message": "OK",
  "data": {},
  "timestamp": "..."
}
```

Response lỗi luôn dùng:

```json
{
  "success": false,
  "message": "Lỗi",
  "errorCode": "ERROR_CODE",
  "timestamp": "..."
}
```

Không trả Entity trực tiếp ra ngoài API. Mọi request/response nên dùng DTO riêng.

---

## 15. Quy ước database

Dự án dùng:

```text
MySQL + Flyway + JPA validate
```

Nghĩa là:

```text
Flyway chịu trách nhiệm tạo/sửa bảng.
Hibernate/JPA chỉ kiểm tra Entity có khớp database không.
```

File migration đặt tại:

```text
src/main/resources/db/migration
```

Quy tắc đặt tên:

```text
V1__init_schema.sql
V2__create_users_table.sql
V3__create_catalog_tables.sql
```

Lưu ý có **2 dấu gạch dưới** giữa version và mô tả.

Ví dụ migration tạo bảng:

```sql
CREATE TABLE users (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(255) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL,
    full_name VARCHAR(255) NOT NULL,
    role VARCHAR(50) NOT NULL,
    created_at DATETIME NOT NULL,
    updated_at DATETIME NOT NULL
);
```

Quy tắc quan trọng:

```text
Migration đã chạy rồi thì không sửa trực tiếp.
Nếu cần thay đổi schema, tạo migration mới.
```

---

## 16. Quy ước phát triển code

Khi tạo module mới, nên tuân thủ:

```text
Controller chỉ nhận request và trả response.
Service xử lý nghiệp vụ.
Repository giao tiếp database.
Entity ánh xạ database.
DTO dùng cho request/response.
Mapper chuyển Entity sang DTO và ngược lại nếu cần.
```

Không nên:

```text
- Viết business logic nặng trong Controller
- Controller gọi Repository trực tiếp
- Trả Entity trực tiếp ra API
- Hardcode secret trong code
- Sửa migration cũ đã chạy
- Xóa vật lý dữ liệu quan trọng nếu có thể soft delete
```

Nên:

```text
- Dùng ApiResponse cho mọi API
- Dùng AppException cho lỗi nghiệp vụ
- Dùng GlobalExceptionHandler để xử lý lỗi tập trung
- Dùng @Transactional cho nghiệp vụ ghi nhiều bảng
- Dùng BigDecimal cho tiền
- Dùng DTO riêng cho request/response
- Cập nhật docs khi thay đổi API/database
```

---

## 17. Tài liệu trong thư mục `docs`

| File                      | Nội dung                                                |
| ------------------------- | ------------------------------------------------------- |
| `docs/API_CONVENTION.md`  | Quy ước API, response, error, pagination                |
| `docs/PROJECT_RULES.md`   | Bối cảnh nghiệp vụ, quy tắc kiến trúc, quy tắc làm việc |
| `docs/DATABASE_DESIGN.md` | Quy ước thiết kế database và kế hoạch bảng              |
| `docs/MODULE_PLAN.md`     | Roadmap phát triển các module                           |

Trước khi code module mới, nên đọc các file này.

---

## 18. Roadmap phát triển dự kiến

Thứ tự nên phát triển tiếp theo:

```text
1. Auth/Security/User
2. Catalog: sản phẩm, danh mục, thương hiệu
3. Cart
4. Voucher
5. Order + Payment
6. Inventory
7. Review
8. Dashboard
9. ChatbotAI
```

Lý do nên làm Auth/Security trước:

```text
Các module sau như quản lý sản phẩm, đơn hàng, voucher, dashboard đều cần phân quyền Admin/Customer.
```

---

## 19. Một số lỗi thường gặp

### Lỗi thiếu `DB_USERNAME` hoặc `DB_PASSWORD`

Nguyên nhân: chưa tạo `.env` hoặc IDE chưa đọc được `.env`.

Cách xử lý:

```bash
cp .env.example .env
```

Sau đó kiểm tra file `.env` đã có:

```env
DB_USERNAME=root
DB_PASSWORD=changeme
```

---

### Lỗi không kết nối được MySQL

Kiểm tra MySQL container:

```bash
docker ps
```

Nếu chưa chạy:

```bash
docker compose up -d
```

Kiểm tra port trong `.env`:

```env
DB_PORT=3306
```

---

### Lỗi CORS khi frontend gọi backend

Kiểm tra frontend đang chạy ở port nào.

Backend hiện cho phép:

```text
http://localhost:3000
http://localhost:5173
```

Nếu frontend chạy port khác, cần sửa `CorsConfig.java`.

---

### Lỗi Flyway checksum

Nguyên nhân thường là sửa nội dung file migration đã chạy.

Cách xử lý đúng:

```text
Không sửa migration cũ.
Tạo migration mới để thay đổi schema.
```

---

### Lỗi `Permission denied` khi chạy `./mvnw`

Trên Linux/Mac:

```bash
chmod +x mvnw
```

Sau đó chạy lại:

```bash
./mvnw spring-boot:run
```

---

## 20. Ghi chú bảo mật

Không đưa các file sau lên Git hoặc gửi cho người khác:

```text
.env
target/
.vscode/
.git/
```

Không đặt password thật trong:

```text
.env.example
README.md
application.yml
source code
```

Các giá trị nhạy cảm phải lấy từ biến môi trường:

```text
DB_PASSWORD
JWT_SECRET
OPENAI_API_KEY
```

---

## 21. Gợi ý cho người mới tham gia dự án

Nếu mới clone project, hãy làm theo thứ tự:

```text
1. Cài JDK 25
2. Cài Docker Desktop
3. Clone project
4. Copy .env.example thành .env
5. Điền DB_PASSWORD trong .env
6. Chạy docker compose up -d
7. Chạy ./mvnw clean test
8. Chạy ./mvnw spring-boot:run
9. Mở http://localhost:8080/api/v1/health
10. Đọc docs/PROJECT_RULES.md và docs/MODULE_PLAN.md trước khi code
```

---

## 22. Lệnh nhanh

```bash
# Copy env
cp .env.example .env

# Chạy MySQL
docker compose up -d

# Chạy test
./mvnw clean test

# Chạy backend
./mvnw spring-boot:run

# Kiểm tra health
curl http://localhost:8080/api/v1/health

# Dừng MySQL
docker compose down
```

Trên Windows PowerShell:

```powershell
Copy-Item .env.example .env

docker compose up -d

.\mvnw.cmd clean test

.\mvnw.cmd spring-boot:run
```

---

## 23. Kết luận

Backend hiện tại là skeleton sạch cho dự án LaptopHub

```text
- Module-based package structure
- Common response format
- Global exception handling
- JPA base entity
- Flyway migration setup
- MySQL Docker setup
- API convention documents
- Database design convention
- Module roadmap
```

Giai đoạn tiếp theo nên bắt đầu từ:

```text
Auth/Security/User + migration database đầu tiên
```

Sau khi phần xác thực và phân quyền ổn định, tiếp tục phát triển các module nghiệp vụ như Catalog, Cart, Order, Payment.

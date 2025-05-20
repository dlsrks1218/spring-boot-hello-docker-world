# Spring Boot Dockerized “Hello Docker World”

A minimal Spring Boot web application, packaged into a Docker image and run locally.

## 📦 구성
- **`src/main/java`**: 간단한 `@SpringBootApplication` + REST 컨트롤러

## ⚙️ 사용법

1. **앱 빌드 & 패키징**  
   ```bash
   # java 설치
   brew install temurin@17
   export JAVA_HOME=$(/usr/libexec/java_home -v17)
   export PATH="$JAVA_HOME/bin:$PATH"
   java -version

   # Gradle을 통한 앱 빌드
   chmod +x gradlew
   ./gradlew clean bootJar
   ```

2. **도커 이미지 빌드 & 푸시**
   ```bash
   # ECR 로그인
   aws ecr get-login-password --region ap-northeast-2 | docker login --username AWS \
      --password-stdin <ACCOUNT_ID>.dkr.ecr.ap-northeast-2.amazonaws.com

   # 사전 요구사항으로 ECR이 생성되어 있어야 합니다.
   docker buildx build \
     --platform linux/amd64 \
     --tag <AWS_ACCOUNT_ID>.dkr.ecr.ap-northeast-2.amazonaws.com/hello-docker-world:latest \
     --push \
     .
   ```
3. **로컬에서 컨테이너 실행 & 테스트**
   ```bash
   docker run -d -p 8080:8080 \
     <AWS_ACCOUNT_ID>.dkr.ecr.ap-northeast-2.amazonaws.com/test:latest
   curl http://localhost:8080
   # → Hello Docker World
   ```

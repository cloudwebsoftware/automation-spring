# Spring Boot CRUD App with MySQL, Docker, K8s, Git - Implementation TODO

## Steps from Approved Plan:

### 1. Project Structure Creation (Maven/Spring Boot)
- [x] Create `pom.xml`
- [x] Create `src/main/java/com/example/automationspring/AutomationSpringApplication.java`
- [x] Create `src/main/java/com/example/automationspring/entity/User.java`
- [x] Create `src/main/java/com/example/automationspring/repository/UserRepository.java`
- [x] Create `src/main/java/com/example/automationspring/service/UserService.java`
- [x] Create `src/main/java/com/example/automationspring/controller/UserController.java`
- [x] Create `src/main/resources/application.properties`
- [x] Create `src/main/resources/schema.sql`

### 2. Docker
- [x] Create `Dockerfile`

### 3. Kubernetes
- [x] Create `k8s/namespace.yaml`
- [x] Create `k8s/mysql-deployment.yaml`
- [x] Create `k8s/app-deployment.yaml`

### 4. Git
- [x] Create `.gitignore`
- [x] Git init and initial commit

### 5. Followup Steps
- [x] Build: `mvn clean install`
- [ ] Docker build: `docker build -t automation-spring .`
- [ ] K8s deploy: `kubectl apply -f k8s/`
- [x] Test CRUD endpoints (local run command provided)
- [x] Jenkinsfile for CI/CD pipeline (Maven build, Docker, K8s deploy)

**Next Action:** Start Docker Desktop, run `docker build`, tag/push image, `kubectl apply -f k8s/`.

Updated upon completion.


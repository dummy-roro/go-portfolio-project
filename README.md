# Go Portfolio

A simple web portfolio application written in Go. This project showcases a basic CI/CD pipeline with GitHub Actions, Docker, and Helm.

---

## 🚀 Features

- 📝 Portfolio page built with Go
- 🧪 Unit testing with `go test`
- 🧹 Code quality checks using `golangci-lint`
- 🐳 Dockerized for container deployment
- 🎯 CI/CD pipeline with GitHub Actions
- ☸️ Kubernetes-ready with Helm chart support

---

## 🏁 Getting Started

### Prerequisites

- Go 1.22+
- Docker
- kubectl & Helm (for deployment)
- GitHub account with secrets configured

---

### 🛠️ Build and Run

```bash
# Clone the repository
git clone https://github.com/dummy-roro/go-portfolio.git
cd go-portfolio

# Build the binary
go build -o go-app

# Run locally
./go-app
```

---

### 🧪 Run Tests

```bash
go test ./...
```

---

### 🐳 Build Docker Image

```bash
docker build -t your-dockerhub-username/go-portfolio:latest .
```

---

### ☸️ Deploy to Kubernetes

```bash
helm install my-portfolio ./go-portfolio \
  --set image.tag=abc1234 \
  --set replicaCount=2```
---

## ⚙️ CI/CD

This project uses GitHub Actions for:

- Building and testing Go code
- Running linters
- Building & pushing Docker images
- Updating Helm chart tags with commit SHAs

Secrets required in GitHub:
- `DOCKERHUB_USERNAME`
- `DOCKERHUB_PASSWORD`
- `REPO_PAT` (Personal Access Token for accessing the Helm repo)

---

## 📁 Project Structure

```text
.
├── .github/workflows/ci-cd.yml   # CI/CD pipeline
├── Dockerfile                    # Docker image definition
├── main.go                       # Go source code
└── README.md                     # You are here
```

---

## 🧑‍💻 Maintainer

**dummy-roro**  
GitHub: [@dummy-roro](https://github.com/dummy-roro)

---

## 📄 License

MIT License

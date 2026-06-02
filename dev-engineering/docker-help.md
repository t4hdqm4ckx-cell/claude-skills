---
name: docker-help
description: Write or debug a Dockerfile or docker-compose file for a described stack
---

You are helping with Docker configuration. The user will describe their stack or provide an existing file to debug.

**FOR NEW DOCKERFILES — follow this checklist:**

```dockerfile
# 1. Always pin the base image tag — never use :latest
FROM python:3.11-slim

# 2. Set WORKDIR before any COPY/RUN
WORKDIR /app

# 3. Copy dependency files FIRST (layer caching)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# 4. THEN copy application code
COPY . .

# 5. Use non-root user for security
RUN useradd --create-home appuser
USER appuser

# 6. Declare ports and volumes explicitly
EXPOSE 8080
VOLUME ["/app/data"]

# 7. Use ENTRYPOINT for the binary, CMD for default args
ENTRYPOINT ["python"]
CMD ["-m", "myapp"]
```

**COMMON ISSUES TO CHECK:**
- `.dockerignore` missing → builds include `node_modules`, `.git`, `*.pyc`, secrets
- `RUN apt-get update && apt-get install` without `rm -rf /var/lib/apt/lists/*` → bloated image
- Secrets in `ENV` or `ARG` → use Docker secrets or environment injection at runtime
- `pip install` without `--no-cache-dir` → unnecessary cache in layer
- Running as root → security risk
- No healthcheck for long-running services

**FOR docker-compose — follow this checklist:**

```yaml
version: '3.9'
services:
  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      - LOG_LEVEL=INFO
    env_file:
      - .env          # secrets here, never hardcode
    volumes:
      - ./data:/app/data
    depends_on:
      db:
        condition: service_healthy
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8080/health"]
      interval: 30s
      timeout: 10s
      retries: 3

  db:
    image: postgres:15-alpine
    volumes:
      - pgdata:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]

volumes:
  pgdata:
```

**OUTPUT FORMAT**
Provide the complete file(s) with inline comments explaining non-obvious choices.
After the file, list:
```
DECISIONS MADE:
- [Why you chose this base image]
- [Why this layer order]

SECURITY NOTES:
- [Any security considerations]

.dockerignore (add this file):
__pycache__/
*.pyc
.env
.git/
node_modules/
*.log
```

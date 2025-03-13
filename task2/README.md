# Task 2: Optimize and Deploy a Docker Image

## Overview
In this task, I optimized a Dockerfile for the application, built the Docker image, and pushed it to Docker Hub. The optimized Docker image is publicly available at: [zuhairhossain/todo-app](https://hub.docker.com/r/zuhairhossain/todo-app).

---

## Steps to Optimize and Deploy the Docker Image

### 1. **Clone the Repository**
- Cloned the repository from GitHub:
    ```bash
    git clone https://github.com/zahido/devops-exam.git
    cd devops-exam
    ```

### 2. **Application Structure Review**

- **Programming Language and Dependencies**: 
    - Identified the use of Node.js with `package.json` and `yarn.lock`.

- **Entry Point**: 
    - Checked the entry point of the application at `src/index.js`.

- **Test Scripts**: 
    - Reviewed the test scripts to ensure they are included in the build process.

### 3. **Optimization Opportunities**

- **Multi-stage Builds**: 
    - To reduce the final image size by separating the build environment from the runtime environment.

- **Lightweight Base Image**: 
    - Using `node:16-alpine` for both build and runtime stages to minimize the image size.

- **Dependency Management**: 
    - Installing only production dependencies in the final image to reduce unnecessary bloat.

### 4. **Create an Optimized Dockerfile**

#### Multi-stage Build Approach:

**Stage 1 (Builder):**

- Used `node:16-alpine` as the base image for the build stage.
- Installed dependencies using `yarn install`.
- Ran tests to ensure the application is working as expected.
- Copied only the necessary files (e.g., `src`, `node_modules`, `package.json`) to the final stage.

**Stage 2 (Production):**

- Used `node:16-alpine` as the base image for the runtime stage.
- Copied only the required files from the build stage (e.g., `node_modules`, `src`, `package.json`).
- Set the environment variable `NODE_ENV=production` to optimize the application for production.
- Exposed port `3000` for the application to run.
- Defined the command to start the application (`node src/index.js`).

#### Key Optimizations:

- Reduced the final image size by excluding unnecessary build tools and dependencies.
- Ensured that only production-ready files are included in the final image.
- Used a lightweight Alpine-based Node.js image to minimize the image size.

### 5. **Build the Docker Image**
Built the Docker image using the optimized Dockerfile:
```bash
docker build -t zuhairhossain/todo-app:latest .
```

### 6. **Push the Docker Image to Docker Hub**
Logged in to Docker Hub:
```bash
docker login -u zuhairhossain
```
Pushed the Docker image to Docker Hub:
```bash
docker push zuhairhossain/todo-app:latest
```

### 7. **Make the Docker Image Public**
The Docker image was made public on Docker Hub. You can access it here: [zuhairhossain/todo-app](https://hub.docker.com/r/zuhairhossain/todo-app).

### Verification
Verified that the Docker image is publicly available on Docker Hub.

### Challenges Faced
- **Docker Image Size**: Initially, the Docker image was large due to unnecessary dependencies. This was resolved by using multi-stage builds and a lightweight base image.
- **Docker Hub Authentication**: Had to ensure proper authentication with Docker Hub before pushing the image.

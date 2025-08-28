# Extract tar.gz, .tar, .rpm, and .deb Files

This folder contains instructions to set up a **Linux container** that automatically extracts all archives placed in the `files/` folder.

## Usage

### Starting the Container

1. **Navigate to this folder**  
   Open your terminal and go to the folder containing the `docker-compose.yml` file.

2. **Run the command**:

        docker compose up -d

**Explanation**: This command will download the necessary image, mount the `files/` folder into the container, extract all supported files, and leave the results in the mounted folder.

## Explanation

### docker-compose.yml

```yaml
services:
  extractor:
    build: .
    volumes:
      - ./files:/files
```

**Explanation**:

* `build: .` → Builds the container image from the Dockerfile in this directory.
* `volumes` → Mounts the local `files/` folder into the container at `/files` so extracted files remain accessible on the host.

---

### Dockerfile

The `Dockerfile` defines the extraction image:

```dockerfile
# Use Ubuntu base image
FROM ubuntu:latest

# Install tools for .deb, .rpm, and tar.gz files
RUN apt-get update && \
    apt-get install -y \
    dpkg \
    rpm2cpio \
    cpio \
    tar \
    gzip && \
    apt-get clean

# Set working directory
WORKDIR /files

# Run extraction script at container startup
CMD bash -c "for file in *; do \
    case \"\$file\" in \
        *.deb) mkdir \"\${file%.deb}\" && dpkg-deb -x \"\$file\" \"\${file%.deb}\" && rm \"\$file\" ;; \
        *.rpm) mkdir \"\${file%.rpm}\" && rpm2cpio \"\$file\" | cpio -idmv -D \"\${file%.rpm}\" && rm \"\$file\" ;; \
        *.tar.gz) mkdir -p \"\${file%.tar.gz}\" && echo \"Extracting: \$file\" && tar -xzf \"\$file\" -C \"\${file%.tar.gz}\" && rm \"\$file\" ;; \
        *.tar) mkdir -p \"\${file%.tar}\" && echo \"Extracting: \$file\" && tar -xf \"\$file\" -C \"\${file%.tar}\" && rm \"\$file\" ;; \
    esac; done"
```

**Key Points**:

* Installs necessary extraction tools: `dpkg`, `rpm2cpio`, `cpio`, `tar`, and `gzip`.
* Uses a loop in the `CMD` to detect file types and extract them into folders named after each archive.
* Removes the original archive after extraction.
* The `files/` folder on the host will contain the extracted contents automatically.

---

This setup provides a **ready-to-use Linux container** for quickly extracting multiple archive types, useful for learning Docker volume mounting, automation scripts, and Linux package management.

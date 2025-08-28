# Utilities Docker Lab

This folder contains utility containers for various tasks in the Docker Learning Lab. Each utility is documented in its own README.

## Available Utilities

### Compressed Files Extractor [ToExtract](./ToExtract/README-compressed.md)

- **Description**: A Linux container that automatically extracts `.tar.gz`, `.tar`, `.rpm`, and `.deb` files from the `files/` folder.  

## How to Use

1. Navigate to the specific utility folder (for example, `ToExtract`).  
2. Open its README to follow the setup and usage instructions.  
3. Run the container using `docker compose up -d` or the specific commands provided.  

> [!NOTE]
> Each utility runs in its own container with isolated volumes for persistent data.  
# Flask-MDict Docker Image

A Dockerized version of [flask-mdict](https://github.com/liuyug/flask-mdict) with significant improvements for modern deployment.
- **GitHub Repository**: [nxxxsooo/docker-flask-mdict](https://github.com/nxxxsooo/docker-flask-mdict)
- **DockerHub**: [tardivo/flask-mdict](https://hub.docker.com/r/tardivo/flask-mdict)

## Features

*   **Web Interface**: Access your dictionaries from any browser.
*   **Format Support**: Fully supports `.mdx` and `.mdd` files.
*   **Persistence**: Keep your dictionaries and configuration safe across restarts.
*   **Multi-Arch**: Supports `amd64` and `arm64`.

> [!IMPORTANT]
> **No dictionaries are included.** You must provide your own `.mdx` and `.mdd` files. Place them in a folder on your host machine (e.g., `./library`) and mount it to `/app/content`.

## Quick Start

**Bash (Mac/Linux):**
```bash
docker run -d \
  --name flask-mdict \
  -p 5248:5248 \
  -v $(pwd)/library:/app/content \
  tardivo/flask-mdict:latest
```

**PowerShell (Windows):**
```powershell
docker run -d `
  --name flask-mdict `
  -p 5248:5248 `
  -v ${PWD}/library:/app/content `
  tardivo/flask-mdict:latest
```

**Command Prompt (Windows CMD):**
```cmd
docker run -d ^
  --name flask-mdict ^
  -p 5248:5248 ^
  -v %cd%\library:/app/content ^
  tardivo/flask-mdict:latest
```

## Configuration

### Volumes

| Container Path | Description | Suggested Host Path |
| :--- | :--- | :--- |
| `/app/content` | Stores dictionary files (`.mdx`, `.mdd`) and the database. | `./library` |
| `/config` | Stores the `flask_mdict.json` configuration file. | `./config` |

### Custom Configuration File

To use a custom `flask_mdict.json`, map a volume to `/config` and override the command:

```yaml
version: '3.8'
services:
  flask-mdict:
    image: tardivo/flask-mdict:latest
    ports:
      - "5248:5248"
    volumes:
      - ./library:/app/content
      - ./config:/config
    command: ["python", "app.py", "--config-file", "/config/flask_mdict.json"]
```

## Improvements & Changes in this Fork

This version includes several critical fixes and enhancements not present in the original:

1.  **Reverse Proxy Support**: Added `ProxyFix` middleware to correctly handle `X-Forwarded-Proto` headers. Sites behind Nginx/Traefik will now load CSS/assets correctly via HTTPS.
2.  **LZO Compression Support**: Native support for LZO-compressed MDX files. This resolves common "unknown compression type" or decoding errors ensuring a wider range of dictionaries (especially older or Chinese dictionaries) load correctly.
3.  **Modernized Build**:
    -   Reduced image size by removing broken/unused translator plugins.
    -   Bind address set to `0.0.0.0` by default for Docker compatibility.
    -   Fixed `AttributeError: 'bytes' object has no attribute 'decode'` in MDX decoding logic.
4.  **Dictionary Tools**: Includes `tools/organize_dicts.py` to help bulk-rename and organize your dictionary library.

## Unraid

An XML template is available in the GitHub repository (`flask-mdict.xml`) for easy installation on Unraid.

## Build Workflow (GitHub Actions)

This repository contains a GitHub Actions workflow to automatically build and push the Docker image.

The workflow is defined in `.github/workflows/build.yml`.

**Triggers:**
- **Schedule:** Runs daily at 2:00 AM UTC.
- **Manual:** Can be triggered manually via the "Run workflow" button in the Actions tab.

**Steps:**
1. Check out the upstream repository (`liuyug/flask-mdict`).
2. Log in to Docker Hub using GitHub Secrets.
3. Build the Docker image from `upstream/Dockerfile`.
4. Push the image to `tardivo/flask-mdict:latest`.
5. Supports multi-architecture builds (`linux/amd64`, `linux/arm64`).

## License

Based on `flask-mdict` (MIT License).

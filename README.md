# Dev Container Demo - Python Development Environment

A Docker-based development container setup for Python projects with pre-installed dependencies, designed for both online and offline development scenarios.

## Overview

This project provides a complete development environment using VS Code Dev Containers with:

- **Python 3.11** (Debian Bookworm)
- **Pre-installed dependencies** from `requirements.txt`
- **VS Code Dev Container** support
- **Offline distribution** capability for air-gapped environments

## Quick Start

### Prerequisites

- [Docker](https://www.docker.com/get-started) installed and running
- [VS Code](https://code.visualstudio.com/) with the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

### Using the Dev Container (Recommended)

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd devcontainersdemo
   ```

2. **Open in VS Code:**
   ```bash
   code .
   ```

3. **Open the Dev Container:**
   - Press `F1` (or `Cmd+Shift+P` on macOS / `Ctrl+Shift+P` on Windows/Linux)
   - Select **"Dev Containers: Reopen in Container"**
   - VS Code will build and start the container automatically

4. **Start developing:**
   - The container includes all Python dependencies pre-installed
   - Your workspace is mounted at `/workspaces/app`
   - Python and all packages are ready to use

### First-Time Setup

On first launch, VS Code will:
1. Build the Docker image from `.devcontainer/Dockerfile`
2. Install all dependencies from `requirements.txt`
3. Set up the development environment
4. Connect you to the container

This process takes a few minutes the first time. Subsequent launches are faster.

## Project Structure

```
devcontainersdemo/
├── .devcontainer/
│   ├── devcontainer.json      # VS Code Dev Container configuration
│   ├── Dockerfile             # Container image definition
│   └── requirements.txt       # Python dependencies
├── main.py                    # Example Python script
├── requirements.txt           # Root requirements (for reference)
├── save-container.sh          # Build script (Linux/macOS)
├── save-container.ps1         # Build script (Windows PowerShell)
├── save-container.bat         # Build script (Windows CMD)
├── DISTRIBUTION.md            # Offline distribution guide
└── README.md                  # This file
```

## Development Workflow

### Adding New Dependencies

1. **Update requirements:**
   ```bash
   # Edit .devcontainer/requirements.txt
   # Add your new package, e.g.:
   # new-package==1.0.0
   ```

2. **Rebuild the container:**
   - Press `F1` → **"Dev Containers: Rebuild Container"**
   - Or manually: `docker build -t devcontainer-offline:latest -f Dockerfile .devcontainer`

### Running Python Scripts

Once inside the container:

```bash
# Run a Python script
python main.py

# Start Python interactive shell
python

# Install additional packages (temporary, lost on rebuild)
pip install package-name
```

### Using Python Packages

All packages from `requirements.txt` are pre-installed and available:

```python
import pandas
import numpy
import duckdb
# ... and all other dependencies
```

## Building Offline Container

For distributing the container to offline/air-gapped environments, see [DISTRIBUTION.md](./DISTRIBUTION.md) for detailed instructions.

### Quick Build

**Linux/macOS:**
```bash
./save-container.sh
```

**Windows (PowerShell):**
```powershell
.\save-container.ps1
```

**Windows (CMD):**
```cmd
save-container.bat
```

This creates `devcontainer-offline.tar` that can be distributed to disconnected stations.

## Installed Dependencies

The container includes the following Python packages (from `requirements.txt`):

- `duckdb` - In-process SQL OLAP database
- `dlt` - Data load tool
- `pandas` - Data manipulation and analysis
- `numpy` - Numerical computing
- `oracledb` - Oracle database driver
- `pymongo` - MongoDB driver
- `requests` - HTTP library
- `openpyxl` - Excel file support
- `pyarrow` - Apache Arrow integration
- `scikit-learn` - Machine learning
- `matplotlib` - Plotting library
- `sqlalchemy` - SQL toolkit
- `pyodbc` - ODBC database drivers
- `mssql-python` - Microsoft SQL Server driver

## Troubleshooting

### Container Won't Start

1. **Check Docker is running:**
   ```bash
   docker ps
   ```

2. **Rebuild the container:**
   - Press `F1` → **"Dev Containers: Rebuild Container Without Cache"**

3. **Check logs:**
   - Open the **Output** panel in VS Code
   - Select **"Dev Containers"** from the dropdown

### Dependencies Not Found

1. **Verify requirements.txt is correct:**
   ```bash
   cat .devcontainer/requirements.txt
   ```

2. **Rebuild the container** to reinstall dependencies

3. **Check Python path:**
   ```bash
   python -c "import sys; print(sys.path)"
   ```

### Container Exits Immediately

The container should stay running. If it exits:
- Check the Dockerfile includes `CMD ["sleep", "infinity"]`
- Rebuild the container

### Permission Issues

If you encounter permission errors:
- The container runs as user `vscode` (UID 1000)
- Files created in the container will have appropriate permissions
- Use `updateRemoteUserUID: "on"` in devcontainer.json to match your host UID

## Advanced Usage

### Customizing the Container

Edit `.devcontainer/Dockerfile` to:
- Add system packages: `RUN apt-get update && apt-get install -y package-name`
- Configure environment variables: `ENV VAR_NAME=value`
- Add custom setup scripts

### Using Pre-built Image

If you have a pre-built image:

1. Load it:
   ```bash
   docker load -i devcontainer-offline.tar
   ```

2. Update `.devcontainer/devcontainer.json`:
   ```json
   {
     "image": "devcontainer-offline:latest",
     // Remove or comment out the "build" section
   }
   ```

### Multiple Python Versions

To use a different Python version, edit `.devcontainer/Dockerfile`:
```dockerfile
FROM mcr.microsoft.com/devcontainers/python:1-3.12-bookworm
# Change 3.12 to your desired version
```

## Contributing

1. Make your changes
2. Test in the dev container
3. Update documentation if needed
4. Submit a pull request

## Resources

- [VS Code Dev Containers Documentation](https://code.visualstudio.com/docs/devcontainers/containers)
- [Docker Documentation](https://docs.docker.com/)
- [Python Documentation](https://docs.python.org/3/)
- [Offline Distribution Guide](./DISTRIBUTION.md)

## License

[Add your license here]

## Support

For issues or questions:
- Open an issue in the repository
- Check [DISTRIBUTION.md](./DISTRIBUTION.md) for offline setup help
- Review VS Code Dev Containers documentation


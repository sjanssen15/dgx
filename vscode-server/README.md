# Visual Studio Code Server
## How to build
```bash
docker build . --tag vscode-server:latest
```

## How to run
```bash
docker run --publish 8787:8080 -dit --name vscode-server vscode-server:latest
```
FROM mcr.microsoft.com/devcontainers/base:noble

## Intall ollama
ARG OLLAMA_VERSION=0.5.7
RUN curl -fsSL https://ollama.com/install.sh | OLLAMA_VERSION=${OLLAMA_VERSION} sh
COPY --chmod=0755 <<EOF /etc/init.d/ollama
#!/bin/sh
case "\$1" in
    start)
        HOME=/home/vscode nohup ollama serve &
        sleep 1
        ;;
    stop)
        killall ollama
        ;;
    *)
        exit 1
        ;;
esac

exit 0
EOF

## Install Fabric
ARG FABRIC_VERSION=1.4.131
ADD --chown=vscode:vscode --chmod=0755 \
    https://github.com/danielmiessler/fabric/releases/download/v${FABRIC_VERSION}/fabric-linux-amd64 \
    /usr/bin/fabric

RUN mkdir -p /home/vscode/.ollama && chown -R vscode:vscode /home/vscode/.ollama
VOLUME /home/vscode/.ollama
USER vscode

COPY --chown=vscode:vscode <<EOF /home/vscode/.config/fabric/.env
DEFAULT_VENDOR=Ollama
PATTERNS_LOADER_GIT_REPO_URL=https://github.com/danielmiessler/fabric.git
PATTERNS_LOADER_GIT_REPO_PATTERNS_FOLDER=patterns
OLLAMA_API_URL=http://localhost:11434
EOF

RUN <<EOF
git clone -n --depth=1 --filter=tree:0 https://github.com/danielmiessler/fabric.git /tmp/fabric
(cd /tmp/fabric && git sparse-checkout set --no-cone /patterns && git checkout)
cp -r /tmp/fabric/patterns /home/vscode/.config/fabric/
rm -rf /tmp/fabric
EOF
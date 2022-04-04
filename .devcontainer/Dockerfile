# OS
ARG VARIANT=bullseye
FROM --platform=linux/amd64 mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}

# Deno install
ENV DENO_INSTALL=/deno
RUN mkdir -p /deno \
    && curl -fsSL https://deno.land/x/install/install.sh | sh \
    && chown -R vscode /deno

ENV PATH=${DENO_INSTALL}/bin:${PATH} \
    DENO_DIR=${DENO_INSTALL}/.cache/deno

RUN deno install -qAn vr https://deno.land/x/velociraptor/cli.ts \
    && echo 'export PATH="$HOME/.deno/bin:$PATH"' >> ~/.bashrc

# Vercel install
RUN curl -fsSL https://deb.nodesource.com/setup_17.x | bash -\
    && apt-get update \
    && apt-get install -y nodejs build-essential \
    && npm i -g vercel

# Tiled install
COPY tiled/default.tiled-session /home/vscode/.local/share/tiled/
RUN wget -O /tmp/Tiled.AppImage https://github.com/mapeditor/tiled/releases/download/v1.7.2/Tiled-1.7.2-x86_64.AppImage \
    && chmod +x /tmp/Tiled.AppImage \
    && /tmp/Tiled.AppImage --appimage-extract \ 
    && mv squashfs-root /tiled \
    && chown -R vscode /tiled \
    && chmod +x /tiled/AppRun \
    && mkdir -p /home/vscode/.cache/tiled/sentry \
    && echo 1 > /home/vscode/.cache/tiled/sentry/user-consent


# File: .devcontainer/Dockerfile
# Usamos Node 20 (LTS) sobre Debian Bookworm para máxima estabilidad con Astro V5
#
FROM node:24-bookworm

# 1. Instalación de herramientas del sistema y ZSH
# sudo: Necesario para que el usuario 'node' pueda instalar cosas si hace falta.
# git/ssh: Vitales para el control de versiones.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
    git zsh vim nano locales build-essential sudo \
    && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && locale-gen \
    && apt-get clean -y && rm -rf /var/lib/apt/lists/*

# 2. Configurar usuario 'node' con permisos sudo sin contraseña
# Esto evita errores de permisos al instalar paquetes globales o herramientas.
RUN echo "node ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/node \
    && chmod 0440 /etc/sudoers.d/node

# 3. Activar PNPM (Gestor de paquetes elegido)
RUN corepack enable && corepack prepare pnpm@latest --activate

# 4. Instalar Starship (Prompt visual para la terminal)
# - Mantenemos esta herramienta visual excelente.
RUN curl -sS https://starship.rs/install.sh | sh -s -- -y
RUN echo 'eval "$(starship init zsh)"' >> /home/node/.zshrc

# Configuración básica de Starship
RUN mkdir -p /home/node/.config && \
    echo 'scan_timeout = 500' > /home/node/.config/starship.toml && \
    chown -R node:node /home/node/.config
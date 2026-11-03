FROM python:3.12-slim

# Instalar herramientas del sistema
RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    make \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Directorio de trabajo
WORKDIR /opt

# Descargar versión concreta del proyecto
RUN git clone --depth 1 --branch v2.34.2 \
    https://github.com/psf/requests.git

WORKDIR /opt/requests

# Instalar dependencias de desarrollo y testing
RUN python -m pip install --upgrade pip \
    && make

# Ejecutar la batería de pruebas
CMD ["make", "ci"]
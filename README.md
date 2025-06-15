# Microservisios-Taller
NO

# dockerfile.python.flask

# Stage 1: Build
FROM python:3.9-slim-buster AS builder

WORKDIR /app

# Instalar dependencias del sistema si son necesarias
# RUN apt-get update && apt-get install -y --no-install-recommends \
#     build-essential \
#     && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

# Puedes ejecutar pruebas aquí si quieres
# RUN pytest

# Stage 2: Run
FROM python:3.9-slim-buster

WORKDIR /app

# Copiar las dependencias instaladas y el código
COPY --from=builder /usr/local/lib/python3.9/site-packages /usr/local/lib/python3.9/site-packages
COPY --from=builder /app /app

# Variables de entorno específicas de la aplicación
ENV FLASK_APP=app.py
ENV PORT 8080

EXPOSE 8080

# Comando para iniciar la aplicación usando Gunicorn o similar
# Asegúrate de que tu app.py o wsgi.py esté configurado correctamente
CMD ["gunicorn", "--bind", "0.0.0.0:8080", "app:app"]
# Para Django: CMD ["gunicorn", "--bind", "0.0.0.0:8080", "your_project.wsgi:application"]

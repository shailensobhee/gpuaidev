# Start from the pre-built ROCm image
FROM vllm/vllm-openai-rocm:latest

# Install your additional system packages
USER root
RUN apt-get update && apt-get install -y \
    wget \
    unzip \
    && rm -rf /var/lib/apt/lists/*
	
# 2. Set Environment Variables for Python stability
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# Install your additional pip packages
RUN pip install --no-cache-dir \
    httpx \
	google-adk[extensions]==1.25.0 \
	openfoodfacts==3.4.3 \
	mlflow==3.6.0 \
	opentelemetry-distro \
	opentelemetry-exporter-otlp

# 4. Bootstrap OpenTelemetry (Installs necessary instrumentations)
RUN opentelemetry-bootstrap -a install

# Reset to the default user if necessary (vLLM images often use root)
# Set the entrypoint back to vLLM's default
ENTRYPOINT ["python3", "-m", "vllm.entrypoints.openai.api_server"]

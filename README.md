# OLLAMA DOCKER

This repository provides a simple and production-ready Docker setup for running **OLLAMA**, an AI language model runtime.
It supports CPU and NVIDIA GPU acceleration and allows basic performance tuning through environment variables.

## Requirements

- Docker `>= 24.x`
- Docker Compose `>= 2.x`
- (Optional) NVIDIA GPU with **NVIDIA Container Toolkit** installed

Verify GPU support (optional):

```bash
docker run --rm --gpus all nvidia/cuda:12.2.0-base nvidia-smi
```

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/qidamqolby/ollama-docker
cd ollama-docker
```

### 2. Environment Configuration

Copy the example environment file:

```bash
cp .env.example .env
```

Default configuration:

```env
# Ollama
OLLAMA_PORT=11434
OLLAMA_MODEL=llama3.1

# Performance tuning
OLLAMA_NUM_THREADS=8
OLLAMA_GPU_LAYERS=999

# GPU control
NVIDIA_VISIBLE_DEVICES=all
NVIDIA_DRIVER_CAPABILITIES=compute,utility
```

#### Configuration Notes

- **OLLAMA_PORT**
  Port exposed by the OLLAMA API.

- **OLLAMA_MODEL**
  Default model that will be pulled and executed.

- **OLLAMA_NUM_THREADS**
  Number of CPU threads allocated for inference.

- **OLLAMA_GPU_LAYERS**
  Number of model layers offloaded to GPU.
  `999` effectively means “use GPU as much as possible”.

- **NVIDIA_VISIBLE_DEVICES**
  GPU visibility control (`all`, `0`, `1`, etc).

- **NVIDIA_DRIVER_CAPABILITIES**
  Required for compute workloads.

### 3. Start the Container

Run OLLAMA using Docker Compose:

```bash
docker compose up -d
```

Check container status:

```bash
docker ps
```

### 4. Pull the Model

Pull the configured model from OLLAMA Hub:

```bash
docker exec -it ollama ollama pull llama3.1
```

You may pull additional models if needed:

```bash
docker exec -it ollama ollama pull mistral
docker exec -it ollama ollama pull qwen
```

### 5. Run the Model

Start an interactive session:

```bash
docker exec -it ollama ollama run llama3.1
```

## API Usage

The OLLAMA API will be available at:

```text
http://localhost:11434
```

Example API call:

```bash
curl http://localhost:11434/api/tags
```

---

## Data Persistence

- Models are stored in a Docker volume
- Container restarts will **not** remove downloaded models

## GPU Support

GPU acceleration is automatically enabled when:

- NVIDIA Container Toolkit is installed
- `NVIDIA_VISIBLE_DEVICES` is set
- Docker Compose includes GPU reservation

No additional runtime flags are required.

## Stop and Cleanup

Stop containers:

```bash
docker compose down
```

Stop and remove volumes (models will be deleted):

```bash
docker compose down -v
```

## Intended Use

- Local development
- Self-hosted inference
- Internal AI services
- API-based LLM workloads

## License

This project follows the same license as the official **OLLAMA** project.

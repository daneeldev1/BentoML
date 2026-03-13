# BentoML

BentoML is a Python framework for building, serving, packaging, and deploying AI applications and machine-learning inference systems.

It is designed for production workloads including traditional ML models, large language models, multimodal applications, RAG systems, agents, embedding services, and multi-model inference pipelines.

## Overview

BentoML turns Python inference code into production-ready APIs while keeping service logic close to standard Python.

A BentoML application can define:

- Inference Services
- HTTP API endpoints
- Background and long-running tasks
- Python and system dependencies
- Model references
- CPU and GPU requirements
- Traffic and timeout settings
- Scaling behavior
- Container build configuration
- Observability integrations

The resulting application can be served locally, packaged as a Bento, containerized as an OCI-compatible image, or deployed to managed infrastructure.

Typical use cases include:

- Model inference APIs
- LLM endpoints
- OpenAI-compatible servers
- RAG applications
- Embedding services
- Image-generation APIs
- Classification systems
- Recommendation APIs
- Multi-model pipelines
- GPU inference services
- Batchable inference workloads
- Agentic AI applications

## Features

### Service Definition

BentoML uses Python classes to define deployable services.

```python
import bentoml


@bentoml.service(
    resources={"cpu": "2"},
    traffic={"timeout": 60},
)
class PredictionService:

    @bentoml.api
    def predict(self, value: float) -> float:
        return value * 2
```

Services can configure:

- Runtime images
- Python packages
- Environment variables
- CPU resources
- GPU resources
- Memory requirements
- Traffic timeouts
- Concurrency
- Autoscaling behavior
- Service dependencies

### API Endpoints

Methods decorated with `@bentoml.api` become callable service endpoints.

Endpoints can support:

- Python type hints
- Structured input and output
- File and binary data
- Streaming responses
- Batchable requests
- Synchronous methods
- Asynchronous methods
- Custom business logic

### Dynamic Batching

Inference methods can be marked as batchable:

```python
@bentoml.api(batchable=True)
def predict(self, inputs: list[str]) -> list[str]:
    ...
```

Batching can improve throughput and accelerator utilization for compatible workloads.

### Framework Flexibility

BentoML can wrap standard Python inference code and is not limited to one model framework.

Common integrations include:

- PyTorch
- TensorFlow
- scikit-learn
- MLflow
- Hugging Face
- vLLM
- ONNX Runtime
- NVIDIA Triton
- custom inference code
- external model APIs

### Model Management

BentoML provides APIs for:

- Creating model records
- Listing models
- Loading models
- Deleting models
- Exporting models
- Importing models
- Pushing models
- Pulling models

Versioned model references improve reproducibility across environments.

### Hugging Face Models

BentoML supports referencing Hugging Face model artifacts directly from a Service.

This is useful for:

- LLMs
- Embedding models
- Vision models
- Diffusion models
- Tokenizers
- Transformer pipelines

### Bento Packaging

A Bento is the deployable artifact produced from a BentoML project.

It can include:

- Service code
- Application files
- Dependency definitions
- Runtime configuration
- Model references
- Environment metadata
- Build instructions

Build a Bento:

```bash
bentoml build
```

List local Bentos:

```bash
bentoml list
```

### Containerization

Create an OCI-compatible container image:

```bash
bentoml containerize <bento-tag>
```

Run the image:

```bash
docker run --rm -p 3000:3000 <image-tag>
```

### Distributed Services

Larger inference systems can be split into multiple Services.

This is useful when components require different:

- Hardware
- Dependencies
- Scaling policies
- Concurrency settings
- Deployment lifecycles

Example architecture:

```text
API Service
    ↓
Embedding Service
    ↓
Retrieval Layer
    ↓
LLM Service
```

### GPU Workloads

Services can declare accelerator requirements.

```python
@bentoml.service(
    resources={"gpu": 1},
)
class GPUModel:
    ...
```

### Autoscaling

Cloud deployments can specify minimum and maximum replicas:

```bash
bentoml deploy \
  --scaling-min 1 \
  --scaling-max 3
```

## Tech Stack

| Area | Technologies |
| --- | --- |
| Primary language | Python |
| Supported Python | Python 3.9+ |
| Service API | BentoML |
| Async networking | aiohttp, httpx |
| CLI | Click |
| Serialization | attrs, cattrs, cloudpickle |
| Numerical runtime | NumPy |
| Configuration | Python, YAML, TOML |
| Storage abstraction | fsspec |
| Observability | OpenTelemetry, Prometheus |
| GPU monitoring | NVIDIA ML Python bindings |
| Optional gRPC | grpcio, Protocol Buffers |
| Optional image IO | Pillow |
| Optional tabular IO | pandas, PyArrow |
| Optional cloud storage | s3fs |
| Optional Triton integration | Triton client |
| Packaging | Hatchling |
| Testing | Pytest |
| Coverage | pytest-cov |
| Parallel tests | pytest-xdist |
| Type checking | Pyright |
| Linting and formatting | Ruff |
| Containerization | Docker / OCI |
| Managed deployment | BentoCloud |

## Installation

### Requirements

BentoML currently requires:

```text
Python 3.9+
```

### Create a Virtual Environment

```bash
python -m venv .venv
```

Activate on Linux or macOS:

```bash
source .venv/bin/activate
```

Activate on Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

### Install BentoML

```bash
pip install -U bentoml
```

Verify the CLI:

```bash
bentoml --help
```

## Quick Start

Create `service.py`:

```python
import bentoml


@bentoml.service
class Calculator:

    @bentoml.api
    def double(self, value: float) -> float:
        return value * 2
```

Start the server:

```bash
bentoml serve
```

The local server runs on port `3000` by default.

When the Service is defined in another module:

```bash
bentoml serve mymodule:MyService
```

## Client Usage

A Python client can call the running service:

```python
import bentoml


with bentoml.SyncHTTPClient("local_service_origin") as client:
    result = client.double(5)

print(result)
```

The generated HTTP endpoints can also be called with standard HTTP clients.

## Runtime Environment

A Service can define its environment directly in Python.

```python
import bentoml


image = (
    bentoml.images.Image(python_version="3.11")
    .python_packages(
        "torch",
        "transformers",
    )
)


@bentoml.service(image=image)
class TextService:
    ...
```

Runtime configuration can define:

- Python version
- Python packages
- System packages
- Environment variables
- Setup commands
- Model references

## Build Configuration

Project dependencies can be defined through `pyproject.toml`.

```toml
[project]
dependencies = [
    "numpy",
    "torch",
    "transformers",
]
```

Bento build configuration can also specify model references and Python packages.

A `.bentoignore` file can exclude unnecessary build files.

Typical exclusions:

```text
.git
.venv
tests
local-data
checkpoints
*.log
```

## Bento Lifecycle

A common workflow is:

```text
Python Service
     ↓
Local Serve
     ↓
Bento Build
     ↓
Container Image
     ↓
Deployment
```

### Serve

```bash
bentoml serve
```

### Build

```bash
bentoml build
```

### List

```bash
bentoml list
```

### Containerize

```bash
bentoml containerize <bento-tag>
```

## Model Store

BentoML exposes model management through Python APIs.

Typical operations include:

- Create
- List
- Get
- Delete
- Export
- Import
- Push
- Pull

Models can use versioned tags:

```text
model-name:version
```

Explicit versions are recommended for reproducible production deployments.

## Deployment

### Local

```bash
bentoml serve
```

### Docker

Build:

```bash
bentoml build
```

Containerize:

```bash
bentoml containerize <bento-tag>
```

Run:

```bash
docker run --rm -p 3000:3000 <image-tag>
```

### BentoCloud

Authenticate:

```bash
bentoml cloud login
```

Deploy:

```bash
bentoml deploy
```

Deploy with a custom name:

```bash
bentoml deploy -n my-inference-service
```

Managed deployment performs the Bento build, upload, image creation, infrastructure provisioning, and Service startup workflow.

## Scaling

A Service can declare resources:

```python
@bentoml.service(
    resources={
        "cpu": "4",
        "memory": "8Gi",
    }
)
class ModelService:
    ...
```

Cloud replica scaling can be configured with:

```bash
bentoml deploy \
  --scaling-min 1 \
  --scaling-max 4
```

GPU workloads can separately specify accelerator requirements.

## Observability

BentoML includes production observability integrations.

Capabilities include:

- OpenTelemetry
- Prometheus metrics
- Distributed tracing
- HTTP instrumentation
- Optional gRPC instrumentation
- Request and inference metrics
- Service health endpoints

Production systems should monitor:

- Request latency
- Error rate
- Throughput
- Queue depth
- CPU usage
- GPU usage
- GPU memory
- Model initialization time
- Autoscaling behavior

## gRPC

Install optional gRPC support:

```bash
pip install "bentoml[grpc]"
```

Additional extras support:

- Health checking
- Reflection
- Channelz
- OpenTelemetry instrumentation

## Optional IO Support

Image dependencies:

```bash
pip install "bentoml[io-image]"
```

Tabular dependencies:

```bash
pip install "bentoml[io-pandas]"
```

Install the broader optional feature set:

```bash
pip install "bentoml[all]"
```

## LLM and Generative AI

BentoML can serve workloads such as:

- Chat models
- Completion models
- Embeddings
- Rerankers
- Diffusion models
- Multimodal models
- RAG pipelines
- Tool-calling applications
- Agents
- OpenAI-compatible endpoints

Large model deployments should consider:

- GPU memory
- Quantization
- Model precision
- Tensor parallelism
- Request batching
- Streaming
- Context length
- Concurrency
- Cold-start time
- Autoscaling

## Production Considerations

Before deploying a Service:

- Pin critical dependencies
- Use explicit model versions
- Define CPU and memory limits
- Declare GPU requirements
- Configure timeouts
- Add health checks
- Enable monitoring
- Validate container builds
- Keep secrets outside source code
- Validate incoming data
- Limit file upload sizes
- Test concurrent requests
- Monitor accelerator memory
- Test model startup time
- Configure scaling limits
- Keep build artifacts reproducible

## Development

Clone the repository:

```bash
git clone <repository>
cd BentoML
```

Install BentoML in editable mode:

```bash
pip install -e .
```

Install framework development dependencies:

```bash
pip install -e ".[testing,tooling]"
```

## Testing

Run all tests:

```bash
pytest
```

Run a specific test:

```bash
pytest tests/path/to/test_file.py
```

Run tests in parallel:

```bash
pytest -n auto
```

Run coverage:

```bash
pytest --cov=bentoml
```

The repository recognizes both `test_*.py` and `*_test.py` files.

## Code Quality

Run Ruff:

```bash
ruff check .
```

Format code:

```bash
ruff format .
```

Check formatting:

```bash
ruff format --check .
```

Type checking is configured with Pyright.

## Protocol Buffers

When BentoML gRPC protocol definitions change, regenerate the Python stubs:

```bash
./scripts/generate_grpc_stubs.sh
```

Generated stubs should remain synchronized with the source protocol definitions.

## Contributing

Create a focused branch and follow existing API, typing, testing, and backward-compatibility conventions.

Before submitting changes:

- Add tests for fixes and new features
- Run targeted Pytest suites
- Run the full test suite when practical
- Run Ruff checks
- Run type checking for affected code
- Regenerate gRPC stubs when protocol definitions change
- Preserve public API compatibility where possible
- Keep optional integrations behind dependency extras
- Avoid committing credentials or private model data
- Update documentation when user-facing behavior changes
- Test Bento creation for packaging changes
- Test containerization for deployment changes
- Keep changes focused and clearly documented

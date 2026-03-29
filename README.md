# OpenTelemetry SiteCustomize Instrumentation

Automatic OpenTelemetry instrumentation via Python's `sitecustomize` mechanism. Designed for environments where modifying the application startup command is difficult or impossible: managed runtimes, serverless platforms, job schedulers, and similar constrained settings. Install the package and instrumentation is applied automatically at Python startup, no wrapper command required.

## When to Use This

The `opentelemetry-instrument` CLI is the standard way to auto-instrument a Python application. This package is for situations where wrapping your command with the CLI is not feasible, or requires tedious workarounds.

> **Note:** If you control your application's startup command, prefer the `opentelemetry-instrument` CLI. This package is specifically designed for cases where that is not practical.

## Installation

```bash
pip install opentelemetry-sitecustomize
```

or

```bash
uv add opentelemetry-sitecustomize
```

You will also need to install the OpenTelemetry instrumentors for the libraries you use. For example:

```bash
pip install opentelemetry-instrumentation-flask opentelemetry-instrumentation-requests
```

## Usage

With the package installed, simply run your application as usual:

```bash
python app.py
```

No need for:

```bash
opentelemetry-instrument python app.py
```

OpenTelemetry instrumentation is applied automatically at Python startup.

## Configuration

This package uses the same initialization path as the `opentelemetry-instrument` CLI, so all standard OpenTelemetry environment variables apply:

- `OTEL_SERVICE_NAME` - set the service name for your traces
- `OTEL_TRACES_EXPORTER` - choose the trace exporter (e.g., `otlp`, `console`, `none`)
- `OTEL_METRICS_EXPORTER` - choose the metrics exporter
- `OTEL_LOGS_EXPORTER` - choose the logs exporter
- `OTEL_EXPORTER_OTLP_ENDPOINT` - set the OTLP collector endpoint
- `OTEL_PYTHON_DISABLED_INSTRUMENTATIONS` - comma-separated list of instrumentations to skip

See the [OpenTelemetry Python documentation](https://opentelemetry-python.readthedocs.io/en/latest/sdk/environment_variables.html) for the full list of supported environment variables.

## How It Works

This package registers a [sitecustomize entrypoint](https://pypi.org/project/sitecustomize-entrypoints/) that calls `opentelemetry.instrumentation.auto_instrumentation:initialize` during Python's site initialization. This is the same initialization function used by the `opentelemetry-instrument` CLI, but triggered automatically via the [`sitecustomize-entrypoints`](https://pypi.org/project/sitecustomize-entrypoints/) library.

## Requirements

- Python >= 3.9
- [sitecustomize-entrypoints](https://pypi.org/project/sitecustomize-entrypoints/) ~= 1.0
- [opentelemetry-api](https://pypi.org/project/opentelemetry-api/) >= 1.25
- [opentelemetry-instrumentation](https://pypi.org/project/opentelemetry-instrumentation/) >= 0.46b0

## License

Apache-2.0

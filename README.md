# opentelemetry-sitecustomize

Automatic OpenTelemetry instrumentation via Python's `sitecustomize` mechanism. This package removes the need to wrap your application with the `opentelemetry-instrument` CLI, just install it and run your Python application normally.

## Why Use This Instead of the CLI?

The `opentelemetry-instrument` CLI requires wrapping your application command, which can be inconvenient/cumbersome in certain scenarios. Here are a few reasons you may opt to use this package instead of the CLI:

- **No wrapper script needed** — no need to prefix every command with `opentelemetry-instrument`
- **Works with any execution method** — `python app.py`, `gunicorn`, `celery worker`, IDE debuggers, and test runners all get instrumented without special configuration
- **Simpler containerization** — no need to change `CMD`/`ENTRYPOINT` in Dockerfiles; just add the package to your requirements

> **Note:** The `opentelemetry-instrument` CLI should still be the preferred method of instrumentation when possible. Use this package in situations where wrapping your command with the CLI is not practical.

## Installation

```bash
pip install opentelemetry-sitecustomize
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

## How It Works

This package registers a [sitecustomize entrypoint](https://pypi.org/project/sitecustomize-entrypoints/) that calls `opentelemetry.instrumentation.auto_instrumentation:initialize` during Python's site initialization. This is the same initialization function used by the `opentelemetry-instrument` CLI, but triggered automatically via the [`sitecustomize-entrypoints`](https://pypi.org/project/sitecustomize-entrypoints/) library.

## Requirements

- Python >= 3.9
- [sitecustomize-entrypoints](https://pypi.org/project/sitecustomize-entrypoints/) ~= 1.0
- [opentelemetry-api](https://pypi.org/project/opentelemetry-api/) >= 1.25
- [opentelemetry-instrumentation](https://pypi.org/project/opentelemetry-instrumentation/) >= 0.46b0

## License

Apache-2.0

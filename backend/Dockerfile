FROM python:3.10

ENV PYTHONUNBUFFERED=1

# Install git for cloning
RUN apt-get update && apt-get install -y git && rm -rf /var/lib/apt/lists/*

# Clone the FastAPI template repository
RUN git clone https://github.com/fastapi/full-stack-fastapi-template.git /tmp/template

# Set working directory to the backend
WORKDIR /app/

# Install uv
COPY --from=ghcr.io/astral-sh/uv:0.5.11 /uv /uvx /bin/

ENV PATH="/app/.venv/bin:$PATH"
ENV UV_COMPILE_BYTECODE=1
ENV UV_LINK_MODE=copy

# Copy backend files from cloned repo and install dependencies
RUN cp /tmp/template/backend/pyproject.toml ./ && \
    cp /tmp/template/uv.lock ./ && \
    uv sync --frozen --no-install-project

ENV PYTHONPATH=/app

# Copy all backend source files
RUN cp -r /tmp/template/backend/scripts/ /app/ && \
    cp /tmp/template/backend/alembic.ini /app/ && \
    cp -r /tmp/template/backend/app/ /app/

# Install the project
RUN uv sync

CMD ["fastapi", "run", "--workers", "4", "app/main.py"]
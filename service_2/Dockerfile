FROM python:3.11-slim
WORKDIR /app
COPY . .
RUN pip install --no-cache-dir uv \
    && uv pip compile pyproject.toml -o requirements.txt \
    && uv pip install --system --requirement requirements.txt
EXPOSE 8002
CMD ["uv", "run", "app.py"] 
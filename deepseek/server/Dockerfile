FROM python:3.13-slim AS builder

ENV PYTHONDONTWRITEBYTECODE=1 PYTHONUNBUFFERED=1

WORKDIR /build
COPY pyproject.toml ./
COPY src ./src

RUN pip wheel --no-cache-dir --wheel-dir /wheels .


FROM python:3.13-slim

ENV PYTHONDONTWRITEBYTECODE=1 PYTHONUNBUFFERED=1

COPY --from=builder /wheels /wheels
RUN pip install --no-cache-dir --no-index --find-links=/wheels deepseek-receiver \
    && rm -rf /wheels

RUN useradd -m -u 1000 -s /bin/sh app
USER app
WORKDIR /home/app

EXPOSE 8080

CMD ["python", "-m", "deepseek_receiver"]

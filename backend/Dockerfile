FROM python:3.11-slim

RUN adduser --disabled-password --gecos '' appuser

WORKDIR /app

COPY app.py .

RUN chown -R appuser:appuser /app

USER appuser

EXPOSE 8080

CMD ["python", "app.py"]
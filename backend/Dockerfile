 FROM python:3.11-slim

 WORKDIR /app

 ENV PYTHONDONTWRITEBYTECODE=1 \
     PYTHONUNBUFFERED=1

 RUN apt-get update && apt-get install -y --no-install-recommends build-essential && rm -rf /var/lib/apt/lists/*

 COPY pyproject.toml ./
 RUN pip install --upgrade pip && pip install -e .

 COPY ./src ./src

 EXPOSE 8000
 CMD ["uvicorn", "linkwise_api.main:app", "--host", "0.0.0.0", "--port", "8000"]



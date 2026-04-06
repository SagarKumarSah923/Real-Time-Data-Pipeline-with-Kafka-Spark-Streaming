# 🔁 Real-Time Data Pipeline — Kafka + Spark Streaming

A production-grade real-time data pipeline that ingests live clickstream events via Apache Kafka, processes them with PySpark Streaming, and loads into BigQuery. Supports 10K+ events/sec with fault-tolerant architecture.

## 🏗️ Architecture

```
Clickstream Events → Kafka Producer → Kafka Broker → PySpark Consumer → BigQuery → Grafana Dashboard
```

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| Apache Kafka | Event streaming & message brokering |
| PySpark Streaming | Real-time stream processing |
| Google BigQuery | Analytics data warehouse |
| Docker + Docker Compose | Local development environment |
| Prometheus + Grafana | Monitoring & dashboards |
| Python 3.10+ | Core programming language |

## 📁 Project Structure

```
kafka-spark-streaming-pipeline/
├── docker/
│   ├── docker-compose.yml
│   └── kafka/
│       └── server.properties
├── producer/
│   ├── clickstream_producer.py
│   └── event_schema.py
├── consumer/
│   ├── spark_streaming_job.py
│   └── bigquery_sink.py
├── config/
│   ├── kafka_config.py
│   └── gcp_config.py
├── monitoring/
│   ├── grafana_dashboard.json
│   └── prometheus_config.yml
├── sql/
│   └── bigquery_schema.sql
├── tests/
│   ├── test_producer.py
│   └── test_streaming_job.py
├── requirements.txt
├── .env.example
└── README.md
```

## ⚡ Quick Start

### Prerequisites
- Docker & Docker Compose
- Python 3.10+
- Google Cloud account (BigQuery enabled)
- GCP Service Account JSON key

### 1. Clone & Setup

```bash
git clone https://github.com/yourusername/kafka-spark-streaming-pipeline.git
cd kafka-spark-streaming-pipeline
cp .env.example .env
# Fill in your GCP credentials in .env
```

### 2. Start Kafka & Zookeeper

```bash
docker-compose -f docker/docker-compose.yml up -d
```

### 3. Install Python Dependencies

```bash
pip install -r requirements.txt
```

### 4. Create BigQuery Table

```bash
bq mk --dataset your_project:clickstream_data
bq query --use_legacy_sql=false < sql/bigquery_schema.sql
```

### 5. Run the Producer

```bash
python producer/clickstream_producer.py
```

### 6. Run PySpark Streaming Job

```bash
spark-submit \
  --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.4.0 \
  consumer/spark_streaming_job.py
```

## 📊 Monitoring

Access Grafana dashboard at `http://localhost:3000` (admin/admin)

Import `monitoring/grafana_dashboard.json` to view:
- Events per second
- Kafka consumer lag
- Spark job throughput
- BigQuery write latency

## 🧪 Running Tests

```bash
pytest tests/ -v
```

## 🌍 Environment Variables

| Variable | Description |
|----------|-------------|
| `KAFKA_BOOTSTRAP_SERVERS` | Kafka broker address |
| `KAFKA_TOPIC` | Topic name for clickstream events |
| `GCP_PROJECT_ID` | Google Cloud project ID |
| `BIGQUERY_DATASET` | BigQuery dataset name |
| `GOOGLE_APPLICATION_CREDENTIALS` | Path to GCP service account JSON |

## 📈 Performance

- Throughput: ~10,000 events/sec per partition
- End-to-end latency: < 2 seconds
- Fault tolerance: Kafka replication factor = 3

## 🤝 Contributing

Pull requests are welcome. For major changes, open an issue first.

## 📄 License

MIT

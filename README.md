# Observability Stack with OpenTelemetry

A complete observability stack using Docker Compose that includes OpenTelemetry Collector, Jaeger for distributed tracing, Prometheus for metrics collection, and Grafana for visualization.

## 🏗️ Architecture

This stack provides a comprehensive observability solution with the following components:

- **OpenTelemetry Collector**: Receives, processes, and exports telemetry data
- **Jaeger**: Distributed tracing system for monitoring microservices
- **Prometheus**: Time-series database for metrics collection
- **Grafana**: Visualization and dashboards for metrics and traces

## 📋 Prerequisites

- Docker installed on your system
- Docker Compose v2.0 or higher
- At least 4GB of available RAM

## 🚀 Quick Start

1. Clone this repository:

```bash
git clone https://github.com/jayjirat/observation-stack.git
cd observation-stack
```

2. Start the stack:

```bash
docker compose up -d
```

3. Verify all services are running:

```bash
docker compose ps
```

## 🌐 Service Access

After starting the stack, you can access the following services:

| Service    | URL                    | Description                                |
| ---------- | ---------------------- | ------------------------------------------ |
| Grafana    | http://localhost:3000  | Dashboards and visualization (admin/admin) |
| Prometheus | http://localhost:9090  | Metrics database and query interface       |
| Jaeger UI  | http://localhost:16686 | Distributed tracing interface              |
| OTLP gRPC  | localhost:4317         | OpenTelemetry gRPC endpoint                |
| OTLP HTTP  | localhost:4318         | OpenTelemetry HTTP endpoint                |

## 📊 Default Credentials

### Grafana

- **Username**: `admin`
- **Password**: `admin`

You'll be prompted to change the password on first login.

## 🔌 Sending Data to the Stack

### Using OpenTelemetry SDKs

Configure your applications to send telemetry data to:

- **Traces & Metrics (gRPC)**: `http://localhost:4317`
- **Traces & Metrics (HTTP)**: `http://localhost:4318`

## 📈 Setting Up Grafana Dashboards

1. Access Grafana at http://localhost:3000
2. Log in with admin/admin
3. Add Prometheus as a data source:
   - URL: `http://prometheus:9090`
4. Add Prometheus Datasource **Connection** `http://prometheus:9090`
5. Import pre-built dashboards from `HTTP_Metrics_OpenTelemetry.json`

## 🧪 Test API to Generate Telemetry Data

### Testing Endpoints to Produce Metrics and Traces

Once the application is running, test these endpoints to generate OpenTelemetry metrics and traces. Use tools like **Postman**, **curl**, or any HTTP client:

#### 1. Get All Users

```http
GET http://localhost:8000/api/users
```

#### 2. Register New User

```http
POST http://localhost:8000/register
Content-Type: application/json

{
  "email": "email@example.com"
}
```

#### 3. Transfer Money (Batch Transaction)

```http
POST http://localhost:8000/transfer
Content-Type: application/json

[
  {
    "fromId": 45,
    "toId": 43,
    "amount": 99
  },
  {
    "fromId": 45,
    "toId": 44,
    "amount": 100
  },
  {
    "fromId": 45,
    "toId": 46,
    "amount": 100
  }
]
```

**Requirements:**

- `fromId` and `toId` must exist in the database
- If any user ID doesn't exist, the entire transaction will fail with an error

### 💡 Testing Purpose

The goal is to **generate telemetry data** that you can observe in:

- **Jaeger UI** - View distributed traces and spans
- **Grafana** - Monitor metrics and performance dashboards
- **Prometheus** - Check raw metrics data

1. **Start with user registration** to create test data and see database traces
2. **Call get users** to generate HTTP request metrics
3. **Test transfers** to see complex transaction traces and error handling
4. **Use invalid user IDs** to generate error traces and metrics

### 📊 Expected Telemetry Output

After testing these endpoints, you should see:

- **HTTP request spans** with response times and status codes
- **Database query spans** showing SQL execution times
- **Transaction spans** for money transfer operations
- **Error spans and metrics** when using invalid user IDs
- **Performance metrics** like request count, duration, and throughput

## 📚 Additional Resources

- [OpenTelemetry Documentation](https://opentelemetry.io/docs/)
- [Jaeger Documentation](https://www.jaegertracing.io/docs/)
- [Prometheus Documentation](https://prometheus.io/docs/)
- [Grafana Documentation](https://grafana.com/docs/)

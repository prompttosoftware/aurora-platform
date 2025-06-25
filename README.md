# aurora-platform

## 🚀 Project Overview

The Aurora platform aims to revolutionize energy management by providing businesses and individuals with a comprehensive solution to track, monitor, and manage their energy usage, with a particular focus on renewable energy sources. Our goal is to foster greater energy efficiency and sustainability through real-time data, predictive insights, and actionable recommendations.

### Key Features:

*   **Real-time Energy Monitoring**: Live tracking of energy consumption and production from diverse sources.
*   **Historical Data Analysis**: Comprehensive historical data for trend analysis and reporting.
*   **Energy Forecasting**: Predictive models for future energy consumption and renewable energy generation.
*   **Personalized Recommendations**: Tailored suggestions for improving energy efficiency based on usage patterns and forecasts.
*   **User Management**: Secure user registration, authentication, and profile management.
*   **Data Ingestion**: Robust mechanisms for integrating with various energy data sources (e.g., smart meters, IoT devices, utility APIs).
*   **Alerting & Notifications**: Proactive alerts for anomalies or significant energy events.

### Target Audience:

*   Small to medium-sized businesses focused on optimizing energy costs and reducing carbon footprint.
*   Homeowners with solar panels or smart home energy systems.
*   Energy consultants and researchers.

## 🏗️ Architecture

Aurora is built as a microservices-oriented platform to ensure scalability, resilience, and maintainability. Components primarily communicate via asynchronous message queues (Kafka) and synchronous REST APIs.

```mermaid
graph TD
    A[User Devices<br>(Web Browser, IoT)] --> B(API Gateway<br>(Nginx/AWS API GW));
    B --> C1(Authentication & User Service);
    B --> C2(Data Ingestion Service);
    B --> C3(Real-time Processing Service);
    B --> C4(Forecasting & Recommendation Engine Services);
    B --> C5(Historical Data API);
    B --> C6(Reporting & Analytics Service);

    C1 --> DB1[Relational Database<br>(PostgreSQL)];
    C2 --Kafka Producer--> MQ[Message Queue<br>(Kafka)];
    MQ --Kafka Consumer--> C3;
    MQ --Kafka Consumer--> C4;
    C3 --> DB2[Time-Series Database<br>(InfluxDB)];
    C4 --> DB2;
    C4 --> DB1;
    C5 --> DB2;
    C6 --> DB2;
    C6 --> DB1;
```

## ⚙️ Technology Stack

*   **Frontend**: React (with Next.js for SSR/SEO)
*   **Backend (API Services)**: Python with FastAPI
*   **Messaging Queue**: Apache Kafka (AWS MSK for managed service)
*   **Databases**:
    *   **PostgreSQL**: For user management, authentication, recommendations, and metadata (AWS RDS).
    *   **InfluxDB**: For time-series energy consumption and production data (raw and processed).
*   **Machine Learning**: Python libraries (scikit-learn, Prophet, TensorFlow/PyTorch)
*   **Containerization**: Docker
*   **Deployment**: AWS (ECS/Fargate, RDS, MSK, S3, CloudWatch, ElastiCache, Secrets Manager, IAM, Route 53, ALB, VPC)
*   **CI/CD**: GitHub Actions
*   **Infrastructure as Code**: Terraform

## 📁 Repository Structure (Monorepo)

```
aurora-platform/
├── frontend/                     # React/Next.js application
├── services/
│   ├── auth_service/             # User authentication and authorization
│   ├── data_ingestion_service/   # Ingests raw energy data
│   ├── real_time_processing_service/ # Processes streaming data
│   ├── forecasting_service/      # Energy prediction engine
│   ├── recommendation_service/   # Personalized recommendations
│   ├── reporting_service/        # Aggregated data, historical trends
│   └── shared_libs/              # Common utilities, data models, client SDKs
├── infrastructure/               # Terraform configurations for AWS
├── .github/                      # CI/CD workflows (GitHub Actions)
└── docs/                         # Project documentation
```

## 🚀 Getting Started

### Prerequisites

*   Docker & Docker Compose
*   Python 3.9+ and `pip`
*   Node.js 16+ and `npm` or `yarn`
*   AWS CLI (configured with appropriate credentials)
*   Terraform (if provisioning infrastructure locally)

### Setup & Installation

**1. Clone the repository:**

```bash
git clone https://github.com/your-org/aurora-platform.git
cd aurora-platform
```

**2. Set up environment variables:**

Create a `.env` file at the root of the project and populate it with necessary environment variables (e.g., database connection strings, JWT secrets, Kafka bootstrap servers). Refer to `docs/env_vars.md` for a complete list.

**3. Infrastructure (Development/Local):**

For local development, you can spin up the required databases and Kafka using `docker-compose`.

```bash
docker-compose -f infra-dev.yml up -d
```

This will provision:
*   PostgreSQL
*   InfluxDB
*   Kafka
*   Redis

**4. Backend Services:**

Each service in `services/` is a separate FastAPI application.

```bash
# Example for auth_service
cd services/auth_service
pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```

Repeat for other services, adjusting ports as needed or use a development-specific `docker-compose` setup.

**5. Frontend Application:**

```bash
cd frontend
npm install # or yarn install
npm run dev # or yarn dev
```

The frontend application will typically be available at `http://localhost:3000`.

## 🧪 Testing

The project employs a comprehensive testing strategy:

*   **Unit Tests**: Focused on individual functions and components. (Pytest for Python, Jest/React Testing Library for Frontend).
*   **Integration Tests**: Verifying interactions between services/components, utilizing `testcontainers` for external dependencies like databases and Kafka. (`fastapi.testclient`, `msw` for Frontend).
*   **End-to-End Tests**: Simulating full user journeys via the UI interacting with the real backend. (Playwright/Cypress).

To run all tests:

```bash
# From project root
# Backend tests
pytest

# Frontend tests
cd frontend
npm test
```

## 📈 Monitoring & Logging

*   **Centralized Logging**: All services send structured (JSON) logs to **AWS CloudWatch Logs**.
*   **Metrics**: Custom application metrics exposed via Prometheus client libraries, visualized in **Grafana**. Infrastructure metrics provided by **AWS CloudWatch**.
*   **Alerting**: **AWS CloudWatch Alarms** for critical thresholds, **Sentry.io** for real-time error tracking.

## 🤝 Contributing

We welcome contributions! Please refer to our [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on how to set up your development environment, run tests, and submit pull requests.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Support

For any questions, issues, or feature requests, please open an issue on the [GitHub Issues page](https://github.com/your-org/aurora-platform/issues).

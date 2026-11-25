# 🌱 Cloud-Based Smart Agriculture and Crop Monitoring Platform

A scalable, cloud-native agricultural monitoring solution built on **Amazon Web Services (AWS)**. This platform validates enterprise cloud ingestion pipelines, containerized microservices, and real-time data visualization using a **100% software-based Simulated Sensor Data Generator**—eliminating the need for physical IoT hardware during initial prototyping.

---

## 📋 Table of Contents
- [Overview](#-overview)
- [Key Features](#-key-features)
- [System Architecture](#-system-architecture)
- [Tech Stack & AWS Services](#-tech-stack--aws-services)
- [Getting Started](#-getting-started)
- [Future Roadmap](#-future-roadmap)
- [Contributors](#-contributors)

---

## 📖 Overview

Traditional manual agricultural monitoring suffers from high labor costs, delayed tracking of rapid environmental changes, and fragmented historical records. This project addresses these challenges by providing a centralized, automated, and elastic cloud platform.

Using a deterministic **Simulated Sensor Data Generator**, the system continuously produces realistic agricultural telemetry—**soil moisture**, **ambient temperature**, and **relative humidity**—and ingests it into an auto-scaled AWS backend for storage, threshold alerting, and real-time visualization.

---

## ✨ Key Features

- **Simulated Sensor Data Generation:** Software-driven script producing realistic environmental readings (`Soil Moisture %`, `Temperature °C`, `Humidity %`) with zero physical hardware dependencies.
- **Containerized Backend API:** Dockerized REST API that collects, validates, and processes incoming sensor payloads.
- **High Availability & Elasticity:** Deployed across multiple Availability Zones (Multi-AZ) using an Application Load Balancer (ALB) and EC2 Auto Scaling Groups (ASG) to handle fluctuating ingestion rates.
- **Persistent Historical Storage:** Structured time-series observations stored securely in an isolated Amazon RDS PostgreSQL database.
- **Interactive Monitoring Dashboard:** Web-based UI displaying live metric cards, historical trend charts, and threshold-based alert indicators.
- **End-to-End Observability:** Integrated with Amazon CloudWatch for EC2 metrics, application container logs, and automated scaling alarms.

---

## 🏗️ System Architecture

```text
+-----------------------------------+       +-----------------------------------+
|  Simulated Sensor Data Generator  |       |   Web-Based Monitoring Dashboard  |
| (Moisture, Temp, Humidity Script) |       |    (Farmer & Admin Browser UI)    |
+-----------------+-----------------+       +-----------------+-----------------+
                  |                                           ^
                  | HTTPS POST                                | HTTPS GET
                  v                                           |
+-------------------------------------------------------------------------------+
|                     Amazon VPC (10.0.0.0/16) — Multi-AZ                       |
|                                                                               |
|  +-------------------------------------------------------------------------+  |
|  | Public Subnets (AZ-1 & AZ-2)                                            |  |
|  |   [ Internet Gateway ]  --->  [ Application Load Balancer (ALB) ]       |  |
|  +------------------------------------+------------------------------------+  |
|                                       |                                       |
|                                       v                                       |
|  +-------------------------------------------------------------------------+  |
|  | Private Application Subnets (EC2 Auto Scaling Group across AZ-1 & AZ-2) |  |
|  |   [ EC2 Instance (AZ-1): Docker API ]   [ EC2 Instance (AZ-2): Docker ] |  |
|  +------------------------------------+------------------------------------+  |
|                                       |                                       |
|                                       v                                       |
|  +-------------------------------------------------------------------------+  |
|  | Private Database Subnets (Isolated via Security Groups)                 |  |
|  |   [ Amazon RDS for PostgreSQL ] (Historical & Current Telemetry)        |  |
|  +-------------------------------------------------------------------------+  |
+-------------------------------------------------------------------------------+
                                        |
                                        v
                        +-------------------------------+
                        |       Amazon CloudWatch       |
                        | (Metrics, App Logs, & Alarms) |
                        +-------------------------------+
```

### Data Flow
1. **Ingestion:** The Simulated Sensor Data Generator sends periodic `HTTPS POST` requests containing JSON telemetry payloads to the Application Load Balancer.
2. **Processing:** The ALB routes traffic to Dockerized REST API containers running on EC2 instances within private application subnets.
3. **Persistence:** Validated readings are written to an isolated Amazon RDS (PostgreSQL) instance in private database subnets.
4. **Visualization:** The Web Dashboard queries the backend via `HTTPS GET` to render live status cards, historical charts, and safe-threshold alerts.

---

## 🛠️ Tech Stack & AWS Services

| Layer | Technologies / Services |
| :--- | :--- |
| **Compute & Containers** | Amazon EC2, Docker, Auto Scaling Groups (ASG) |
| **Networking & Security** | Amazon VPC (`10.0.0.0/16`), Public/Private Subnets, Internet Gateway, NAT Gateway, Security Groups, Application Load Balancer (ALB) |
| **Database** | Amazon RDS for PostgreSQL *(Alternative: Amazon DynamoDB)* |
| **Monitoring & Logging** | Amazon CloudWatch (Metrics, Container Logs, Alarms) |
| **Simulation & Backend** | Python / Node.js REST API, Simulated Telemetry Script |
| **Frontend** | HTML/CSS/JS Web Monitoring Dashboard |

---

## 🚀 Getting Started

### Prerequisites
- An active **AWS Account** with permissions to provision VPC, EC2, RDS, ALB, and CloudWatch resources.
- **Docker** & **Docker Compose** installed locally (for local testing).
- **Python 3.x** or **Node.js** (depending on your API & generator runtime).

### Local Development Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/<your-username>/<your-repo-name>.git
   cd <your-repo-name>
   ```

2. **Configure environment variables:**
   Create a `.env` file in the root directory:
   ```env
   DB_HOST=localhost
   DB_PORT=5432
   DB_NAME=smart_agriculture
   DB_USER=postgres
   DB_PASSWORD=your_secure_password
   API_PORT=8000
   ```

3. **Run the Backend API with Docker:**
   ```bash
   docker build -t agri-backend-api ./backend
   docker run -d -p 8000:8000 --env-file .env agri-backend-api
   ```

4. **Start the Simulated Sensor Data Generator:**
   ```bash
   cd simulator
   python sensor_generator.py --endpoint http://localhost:8000/api/telemetry --interval 5
   ```

5. **Open the Monitoring Dashboard:**
   Navigate to `http://localhost:8000` (or open `dashboard/index.html` in your browser) to view live simulated metrics.

---

## 🔮 Future Roadmap

- [ ] **Hardware IoT Integration:** Transition from software simulation to physical field sensors (ESP32, LoRaWAN, AWS IoT Core).
- [ ] **AI Crop Recommendations:** Integrate machine learning models to suggest irrigation schedules and fertilizer usage.
- [ ] **Predictive Analytics:** Forecast soil moisture depletion and weather impacts using historical RDS datasets.
- [ ] **Mobile Application:** Build a cross-platform mobile companion app with push notifications for critical threshold alerts.

---

## 👥 Contributors

**Department of Computer Science and Engineering — KLH University**

| Name | Roll Number |
| :--- | :--- |
| **P. Pranaw Reddy** | `2520030136` |
| **P. Laxman Reddy** | `2520030097` |
| **Y. Yashwanth** | `2520030506` |

---


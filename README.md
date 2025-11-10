# 🚀 Alien Spy – AWS-Powered Unity Game

[![Unity](https://img.shields.io/badge/Engine-Unity-000000?logo=unity)]()
[![AWS](https://img.shields.io/badge/Cloud-AWS-232F3E?logo=amazonaws)]()
[![Serverless](https://img.shields.io/badge/Architecture-Serverless-success)]()
[![License](https://img.shields.io/badge/License-MIT-lightgrey)]()
[![Status](https://img.shields.io/badge/Status-In%20Development-yellow)]()

> **Alien Spy** is a serverless, multiplayer Unity WebGL game integrating AWS Lambda, DynamoDB, and Cognito.  
> Designed to bridge **cloud learning** and **game development**, it enables students to gain *hands-on AWS experience* through a fun, interactive environment.

---

## 🎯 Executive Summary
**Alien Spy** transforms traditional cloud computing education into an interactive experience.  
Built as a **1v1 endless runner**, it leverages **AWS serverless architecture** to deliver scalable multiplayer gameplay, AI-powered avatar personalization, and real-time leaderboard updates.

**Key Features**
- ⚡ **1v1 Real-Time Race Mode**
- 🧠 **AI Avatar Personalization**
- 🕹️ **Item & Boss Mechanics**
- ☁️ **Fully Serverless AWS Backend**
- 🏆 **Real-Time Leaderboard & Events**

**Educational Impact**
- Students: Build cloud-native portfolios  
- Faculty: Deploy reusable AWS-based lab modules  
- Institutions: Strengthen AWS partnership & employability outcomes

  ---

## 🧩 Architecture Overview

### High-Level Design
Unity WebGL (S3 + CloudFront)
↓
API Gateway (REST/WebSocket)
↓
Lambda Functions
↓
DynamoDB (Player Data & Leaderboards)
↓
DynamoDB Streams → SNS → Lambda (Real-time Updates)  


### AWS Services
| Layer | Services Used |
|--------|----------------|
| Frontend | S3 · CloudFront · Route 53 · WAF |
| Backend | API Gateway · Lambda · DynamoDB |
| Authentication | AWS Cognito |
| CI/CD | CodePipeline · CodeBuild · CloudFormation |
| AI / Events | Python Lambda · DynamoDB Streams · SNS |

**Security**
- TLS 1.2+ encryption  
- AES-256 S3 encryption  
- IAM least-privilege access  

---

## 🛠️ Technical Stack
| Category | Technology |
|-----------|-------------|
| Game Engine | Unity 2022 (WebGL build) |
| Cloud Platform | AWS Serverless |
| Backend Logic | AWS Lambda (Python / Node.js) |
| Database | DynamoDB |
| Authentication | AWS Cognito |
| CI/CD | AWS CodePipeline & CodeBuild |
| Infra-as-Code | AWS CloudFormation |
| Testing | NUnit, Postman, CloudWatch Metrics |

---

## 🚧 Development Timeline
| Phase | Weeks | Deliverables |
|-------|--------|--------------|
| Pre-production | 1–3 | Game Design Doc, AWS Architecture |
| Core Gameplay | 4–7 | Map, obstacles, items, leaderboard |
| Boss & Avatar | 8–9 | Boss AI, AI avatar pipeline |
| Testing & Optimization | 10–11 | QA, performance tuning |
| Deployment & Demo | 12 | WebGL demo + documentation |

---

## 💰 Budget Overview
| Category | Estimated Cost |
|-----------|----------------|
| AWS Free Tier | $0–5 / month |
| S3 + CloudFront | ~$0.15 / month |
| API Gateway + Lambda | ~$0.01 / month |
| Total | ≈ $0–100 max (over Free Tier) |

**Development Hours:** ~600 hours (12 weeks × 5 members × 10 hours/week)

---

## ⚠️ Risk Assessment
| Risk | Impact | Mitigation |
|------|---------|------------|
| AWS Misconfiguration | High | CloudFormation IaC templates |
| WebSocket instability | Medium | Auto-reconnect & REST fallback |
| Free Tier exceeded | Medium | Budget alerts & payload optimization |
| Integration delays | Medium | Agile sprints + weekly review |
| Data exposure | High | Cognito auth, S3 private access |

---

## 🎮 Game Design Highlights
- **Theme:** Sci-fi Vietnam  
- **Gameplay:** Endless runner, 1v1 online race  
- **Mechanics:** Dashing, item buffs/debuffs, boss fights  
- **Avatar:** AI-generated, customizable look  
- **Leaderboard:** Local & global real-time ranking  
- **Events:** Live challenges via SNS + DynamoDB Streams  

---

## 📚 Learning Objectives
Alien Spy is more than a game — it’s a **learning framework** for students to:
- Understand AWS serverless architecture (Lambda, DynamoDB, API Gateway)
- Integrate Unity WebGL with real-world cloud infrastructure
- Design scalable multiplayer logic using event-driven systems
- Deploy, monitor, and optimize cloud workloads
- Practice CI/CD and Infrastructure as Code principles

---

## 🌍 Expected Outcomes
- API latency <100ms  
- Real-time updates <2s  
- 50+ concurrent players  
- AI avatar processing <5s  
- Test pass rate ≥90%  
- Monthly cost < $30  

---

## 🧑‍💻 Team & Roles
| Member | Role | Focus |
|---------|------|-------|
| Lead Developer | Cloud Architect | AWS Infra, Lambda API |
| Game Developer | Gameplay Mechanics | Unity C# scripting |
| AI Engineer | Avatar Pipeline | Python Lambda & ML |
| DevOps Engineer | CI/CD | CodePipeline & CloudFormation |
| QA Lead | Testing & Docs | Postman, Unit Tests, Reports |

---

## ⚙️ Deployment
**Manual:**
"```bash"
# Build Hugo (for docs if used)
hugo

# Build WebGL
Unity → Build Settings → WebGL → Build to /Build

# Deploy to AWS
aws s3 sync ./Build s3://alien-dash-webgl
# CI/CD (Auto):
Push to main → GitHub triggers CodePipeline → Deploys to S3 + CloudFront.

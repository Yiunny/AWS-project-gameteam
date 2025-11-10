---
title: "Proposal"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Alien Spy: AWS-Powered Gaming Infrastructure
## A Serverless Unity Game for Cloud Skills Development

### 1. Executive Summary
**Alien Spy** is a serverless, endless runner game developed with Unity and AWS to serve as a hands-on learning framework for cloud integration. The platform addresses the critical skills gap in Vietnamese universities: while students learn AWS services like Lambda, DynamoDB, and API Gateway, they rarely experience realistic multi-service integration.  

Key features include:  
- **1v1 Real-time Race Mode**  
- **AI Avatar Personalization**  
- **Item & Boss System**  
- **Serverless Backend**  
- **Real-time Leaderboard and Events**  

**Benefits & ROI**  
- Students gain hands-on AWS portfolio projects.  
- Faculty get ready-to-deploy lab modules.  
- Institution enhances employability & AWS partnership opportunities.  
- Investment: $0–100 infrastructure cost, 600 total team hours (12 weeks × 5 members × 10 hours/week).  

---

### 2. Problem Statement
#### What’s the Problem?
- Most endless runners are mobile-only.  
- Students rarely get integrated cloud projects.  
- Avatar personalization and competitive features lacking → lower engagement.  

#### The Solution
- **Serverless AWS architecture** integrated with Unity WebGL.  
- Real-time multiplayer (WebSocket), AI avatars, items & boss mechanics.  
- Scalable DynamoDB backend, S3 asset storage, CloudFront CDN, Cognito auth.  

#### Stakeholders
| Stakeholder | Concern |
|------------|---------|
| Players | Competitive, personalized desktop gaming |
| Developers | Academic project integrating Unity & AWS |
| Mentors | Technically sound, reusable cloud project |

---

### 3. Solution Architecture
**Overall Structure**
![Alien Spy Architecture Placeholder](/images/2-Proposal/aws.png)
#### High-Level Architecture
- **Frontend:** Route 53, WAF, CloudFront, S3 hosting WebGL
- **Backend:** API Gateway (REST/WebSocket), Lambda, DynamoDB
- **Authentication:** AWS Cognito
- **CI/CD:** CodePipeline + CodeBuild + CloudFormation
- **Event-Driven:** DynamoDB Streams + SNS

**AWS Services Used**  
- **Route 53** – DNS & high availability  
- **WAF** – Security protection  
- **CloudFront** – CDN for WebGL  
- **S3** – Game builds, assets, avatars  
- **API Gateway** – REST/WebSocket APIs  
- **Cognito** – User authentication  
- **Lambda** – Serverless backend logic  
- **DynamoDB** – Player data, leaderboards  
- **SNS / DynamoDB Streams** – Real-time notifications  
- **CodePipeline & CodeBuild** – CI/CD automation  
- **CloudFormation** – Infrastructure as Code  

**Component Design**  
- **Frontend**: Unity WebGL via CloudFront + S3  
- **Backend**: API Gateway → Lambda → DynamoDB → Streams → Lambda → Player  
- **AI Avatar Pipeline**: Lambda processes uploaded avatars → S3 storage  
- **Real-time Leaderboard**: DynamoDB Streams + Lambda pushes updates  
- **User Management**: Cognito handles 5–50 student accounts  

**Security & Compliance**  
- WAF, TLS 1.2+, AES-256 S3 encryption, least privilege IAM roles  

---

### 4. Technical Implementation
**Implementation Phases**  
1. Infrastructure Design & Setup  
2. Backend API Development  
3. Frontend / Game Client Integration  
4. Testing & Optimization  
5. CI/CD Deployment  

**Technical Requirements**  
- Unity WebGL build  
- AWS Lambda, API Gateway, DynamoDB, SNS, CloudFront, S3, Cognito  
- Python for AI avatar processing  
- CloudFormation/CDK for infra as code  

**Testing Strategy**  
- Unit tests (xUnit/NUnit)  
- Integration tests (Postman)  
- Performance tests (CloudWatch/K6)  
- End-to-end system testing  

**Deployment & Rollback**  
- CodePipeline automates deploy  
- Lambda versioning & DynamoDB backups for rollback  

---

### 5. Timeline & Milestones
| Phase | Weeks | Deliverables |
|-------|-------|--------------|
| Pre-production | 1–3 | GDD, AWS Architecture |
| Core Gameplay | 4–7 | Map, obstacles, items, leaderboard |
| Boss & Avatar | 8–9 | Boss AI, cutscenes, AI avatar pipeline |
| Testing & Optimization | 10–11 | QA, performance tuning |
| Deployment & Demo | 12 | WebGL demo, documentation |

---

### 6. Budget Estimation
**AWS Free Tier Estimate**  
- Lambda: $0  
- API Gateway: ~$0.01/month  
- S3 + CloudFront: ~$0.15/month  
- DynamoDB: $0  
- SNS/WebSocket: $0  
- Total: $0–5/month (max $50–100 if Free Tier exceeded)  

**Development & Third-party Costs**  
- 0 VND, Python AI open-source, Unity Personal  

---

### 7. Risk Assessment
| Risk | Impact | Probability | Mitigation |
|------|--------|------------|------------|
| AWS Misconfiguration | High | Medium | CloudFormation templates, testing |
| WebSocket instability | Medium | High | Reconnect & fallback to REST |
| Unity-AWS integration | High | Medium | Custom C# wrappers, isolated testing |
| Free Tier exceeded | Low | Medium | Budget alerts, payload optimization |
| Timeline delay | Medium | High | Agile sprints, 1-week buffer |
| Data exposure | High | Low | S3 private, Cognito auth, TLS |

---

### 8. Expected Outcomes
- API latency <100ms; real-time updates <2s  
- 50+ concurrent players supported  
- AI avatar processing <5s  
- ≥90% test pass  
- Monthly cost <$30  

**Long-term Benefits**  
- Scalable multiplayer platform  
- Hands-on AWS learning & portfolio project  
- Reusable infrastructure for future games/labs  

---

### 9. Game Design Overview – Alien Spy
- **Theme**: Sci-fi Vietnam  
- **Character**: Alien UFO, AI personalized avatar  
- **Obstacles**: Dashable & non-dashable  
- **Items**: Buffs/Debuffs  
- **Currency & Shop**: Bamboo pipe Tobacco  
- **Maps & Levels**: 3 regions, 5 levels each, boss at milestone levels  
- **Leaderboard**: Real-time, local & global  
- **Real-Time Events**: Live challenges via SNS/DynamoDB Streams  

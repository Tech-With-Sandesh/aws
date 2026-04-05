# 🏗️ AWS SNS + SQS Architecture

## 📌 Overview

This architecture demonstrates how to use **Amazon SNS (Simple Notification Service)** and **Amazon SQS (Simple Queue Service)** together to build a scalable and decoupled system.

---

## 🎯 Use Case

When a user places an order, multiple systems need to react:

- Process the order  
- Send email notifications  
- Update analytics  
- Trigger downstream services  

---

## 🧩 Architecture Diagram

```mermaid
flowchart TD
    A[User] --> B[Backend Service]
    B --> C[SNS Topic (Order Event)]

    C --> D[SQS Queue - Order]
    C --> E[SQS Queue - Email]
    C --> F[SQS Queue - Analytics]

    D --> G[Order Worker]
    E --> H[Email Service]
    F --> I[Analytics Service]


⸻

🔄 Flow Explanation
	1.	User places an order
	2.	Backend service publishes event to SNS
	3.	SNS broadcasts the message to multiple subscribers
	4.	Each SQS queue receives a copy of the message
	5.	Each service processes messages independently

⸻

🧠 Key Concepts

🔹 SNS (Simple Notification Service)
	•	Pub/Sub messaging system
	•	Sends messages to multiple subscribers
	•	No message storage

⸻

🔹 SQS (Simple Queue Service)
	•	Message queue system
	•	Stores messages until processed
	•	Ensures reliable delivery

⸻

🔗 Why Use SNS + SQS Together?

Feature	SNS	SQS
Purpose	Broadcast messages	Store & process messages
Delivery	Instant	Queue-based
Consumers	Multiple	One per message

👉 SNS distributes messages
👉 SQS ensures reliable processing

⸻

⚡ Benefits
	•	Loose coupling between services
	•	Scalable architecture
	•	Fault tolerance
	•	Parallel processing
	•	Reliable message handling

⸻

🔐 Reliability Features
	•	Message durability
	•	Retry mechanism
	•	Dead Letter Queue (DLQ) support
	•	Independent service processing

⸻

🔥 Advanced Concepts

1️⃣ Dead Letter Queue (DLQ)
	•	Failed messages are stored separately
	•	Helps debugging and retry

⸻

2️⃣ Fan-Out Pattern
	•	SNS sends messages to multiple SQS queues
	•	Each service processes independently

⸻

3️⃣ Decoupling
	•	Services are independent
	•	Failure in one service doesn’t affect others

⸻

🧑‍🏫 Real World Mapping

Queue	Purpose
Order Queue	Process orders
Email Queue	Send notifications
Analytics Queue	Track user events


⸻

❌ Common Mistakes
	•	Using only SNS (no reliability)
	•	Using only SQS (no broadcast)
	•	Not configuring DLQ
	•	Tight coupling between services

⸻

🔑 Summary
	•	SNS = Broadcast messages
	•	SQS = Process messages reliably

⸻

🎯 One-Line Summary

👉 SNS sends messages to multiple systems
👉 SQS ensures each system processes them safely

⸻

💡 Memory Trick

SNS = Notification
SQS = Queue

⸻


---

## ✅ What to do next

- Create a repo  
- Add `README.md`  
- Paste this content  
- Push → GitHub will render diagram automatically  

---

# EnterpriseOps AI — Intelligent Banking Support Platform

### Enterprise-Grade Generative AI, RAG & Agentic AI Engineering with Java and AWS

**Java 21 | Spring Boot | Spring AI | Amazon Bedrock | Amazon Nova | Amazon Titan Embeddings | PostgreSQL | pgvector | MCP | Keycloak | OAuth2 | Docker | Gradle**

EnterpriseOps AI is a hands-on enterprise AI engineering project demonstrating how Generative AI, Retrieval-Augmented Generation (RAG), Agentic AI, and secure tool integrations can be implemented using Java, Spring AI, and Amazon Web Services.

The platform simulates banking transaction investigations by combining AI-driven reasoning, transaction status and event history, operational runbooks, and structured investigation reporting.

The project focuses on enterprise architecture, AI integration, retrieval quality, application security, execution controls, observability, and cost awareness.

**Project type:** Independent AI engineering and R&D project.

**Status:** Core functionality implemented; final AI evidence-validation safeguards and comprehensive evaluation remain in progress.

---

## 1. Source Code Repositories

EnterpriseOps AI consists of two independently developed Spring Boot applications.

### EnterpriseOps API — Main Application

**Repository:** https://github.com/8001026/enterpriseops-api

The main application provides:

- Spring Boot REST APIs.
- Spring AI integration with Amazon Bedrock.
- Retrieval-Augmented Generation using PostgreSQL pgvector.
- Amazon Bedrock Knowledge Bases integration.
- Agentic AI transaction investigations.
- Secure MCP client integration.
- OAuth2/JWT-based API security.
- AI tool execution guardrails.
- Structured investigation reporting.
- Token usage and estimated AWS Bedrock cost tracking.

### EnterpriseOps MCP Server — Secure Tool Provider

**Repository:** https://github.com/8001026/enterpriseops-mcp-server

A separate secured Model Context Protocol server exposing simulated banking transaction tools.

Key capabilities:

- MCP tool discovery and invocation.
- Transaction status retrieval.
- Transaction event history retrieval.
- OAuth2 Client Credentials integration.
- Keycloak JWT validation.
- Role-based tool authorization.
- JWT audience and issuer validation.
- Independent Spring Boot deployment.

---

## 2. High-Level Architecture

```text
                Banking Support User
                        |
                        v
                 REST API / Postman
                        |
                        v
              EnterpriseOps API
              Java 21 / Spring Boot
                        |
                  Spring Security
                  Keycloak JWT
                        |
                        v
                Spring AI ChatClient
                        |
                        v
               Amazon Bedrock Nova
                        |
                Agentic Tool Calling
                        |
              Java Tool Allowlisting
               Execution Budgets
                        |
              +---------+----------+
              |                    |
              v                    v
        MCP Client            Local RAG Tool
              |                    |
       OAuth2 Client                v
        Credentials          RunbookRagService
              |                    |
              v                    v
       Secured MCP Server     PostgreSQL pgvector
              |                    |
      Transaction Tools       Banking Runbooks
              |                    |
              +---------+----------+
                        |
                        v
               Investigation Evidence
                        |
                        v
             AI Findings & Reporting
                        |
                        v
              Structured JSON Report
                        |
                        v
                REST API Response
```

AI usage metadata and estimated model costs are captured by the application and persisted in PostgreSQL.

---

## 3. Generative AI and Amazon Bedrock

Integrated Amazon Bedrock with Spring AI to enable LLM-driven operations.

Implemented and explored:

- Amazon Nova Micro model integration.
- Spring AI `ChatClient`.
- System prompts and user prompts.
- Prompt parameters and model configuration.
- Token consumption monitoring.
- Structured output generation.
- Multi-stage AI workflows.
- Prompt chaining.
- AI response metadata processing.

**Primary model:** `amazon.nova-micro-v1:0`

**Development AWS region:** `eu-west-2` (London).

---

## 4. Retrieval-Augmented Generation (RAG)

Implemented a custom RAG architecture for retrieving and interpreting banking operational runbooks.

### Technologies

- Spring AI.
- Amazon Titan Text Embeddings V2.
- PostgreSQL pgvector.
- Amazon S3.
- Vector similarity search.
- Hybrid retrieval and reranking.

### Capabilities

- Document ingestion and chunking.
- Embedding generation.
- Vector storage and semantic retrieval.
- Hybrid retrieval strategies.
- Relevant runbook identification.
- Context augmentation for AI responses.
- Source references and evidence-grounded answers.
- Embedding cache configuration.
- HNSW vector indexing.

### Retrieval Evaluation

Studied and implemented foundational retrieval evaluation concepts, including:

- Precision@K.
- Recall@K.
- Mean Reciprocal Rank (MRR).
- Normalized Discounted Cumulative Gain (nDCG).

Comprehensive quality benchmarking remains a separate evaluation activity.

---

## 5. Custom RAG vs Amazon Bedrock Knowledge Bases

Implemented and explored two RAG approaches.

**Custom RAG**

Spring AI + Amazon Titan Embeddings + PostgreSQL pgvector.

Provides application-level control over document processing, retrieval, ranking, and integration.

**Amazon Bedrock Knowledge Bases**

AWS-managed knowledge retrieval integrated with the application.

The project includes retrieval comparison functionality for investigating trade-offs in:

- Retrieval quality.
- Implementation complexity.
- Infrastructure management.
- Performance.
- Operational flexibility.
- Estimated AWS costs.

Detailed benchmarking and full cost comparisons are planned separately.

---

## 6. Agentic AI and Tool Calling

Developed an AI-powered transaction investigation workflow using Spring AI tool calling.

The investigation agent can request approved tools to retrieve transaction evidence and operational documentation.

### Available Investigation Tools

| Tool | Purpose | Location |
|---|---|---|
| `getTransactionStatus` | Retrieve transaction status and failure code | MCP Server |
| `getTransactionHistory` | Retrieve transaction event history | MCP Server |
| `searchBankingRunbooks` | Search relevant operational runbooks | EnterpriseOps API |

The model selects from approved tools based on investigation requirements and the system instructions.

Java enforces application-defined tool restrictions.

### Simulated Transaction Scenarios

| Transaction | Status | Failure Code |
|---|---|---|
| TXN-1001 | PENDING | GATEWAY_TIMEOUT |
| TXN-1002 | SUCCESS | NONE |
| TXN-1003 | FAILED | INSUFFICIENT_FUNDS |

These are fictional transactions intended for development and learning.

---

## 7. Model Context Protocol (MCP)

Developed a separate MCP Server and integrated it with the EnterpriseOps API through Spring AI's MCP client.

The MCP architecture demonstrates:

- Standardized tool exposure and discovery.
- Remote tool invocation.
- Separation of AI orchestration from backend capabilities.
- Independent security enforcement.
- Secure service-to-service communication.

The MCP Server runs independently from the main API.

**MCP Server repository:**

https://github.com/8001026/enterpriseops-mcp-server

---

## 8. Enterprise Security

Security is implemented using Spring Security, Keycloak, JWT, and OAuth2.

### Incoming API Security

- JWT Bearer token authentication.
- Keycloak integration.
- Spring Security resource server.
- Role-based endpoint authorization.
- Protected RAG and agent APIs.

### Outbound MCP Security

- Dedicated OAuth2 confidential client.
- OAuth2 Client Credentials flow.
- Service-account authentication.
- JWT issuer validation.
- JWT audience validation.
- MCP-specific role authorization.
- Separation of user authentication from service-to-service authentication.

The MCP Server restricts protected tool access through independently validated service-account credentials.

---

## 9. Agent Execution Guardrails

Implemented Java-enforced execution controls around AI tool calling.

### Tool Allowlisting

Only explicitly approved tools are exposed to the investigation agent.

### Execution Budget

A dedicated execution budget is created for each investigation.

Configured development limits:

| Control | Maximum |
|---|---|
| Total tool executions | 6 |
| Transaction status executions | 2 |
| Transaction history executions | 2 |
| Runbook search executions | 2 |

Each tool execution passes through a `BudgetedToolCallback`, which checks the shared `AgentExecutionBudget` before invoking the underlying tool.

The execution budget has been validated against normal execution and exceeded-limit scenarios.

Further AI-specific safeguards for evidence verification and safe output generation remain part of final development.

---

## 10. Structured Investigation Reporting

Implemented a two-stage AI workflow.

**Stage 1 — Agentic Investigation**

Amazon Nova uses the approved MCP and RAG tools to gather transaction evidence and generate findings.

**Stage 2 — Structured Report Generation**

A subsequent AI invocation converts investigation findings into a typed Java `InvestigationReport`.

Report fields include:

- Transaction ID.
- Transaction status.
- Failure code.
- Gateway outcome.
- Root-cause assessment.
- Confidence assessment.
- Supporting evidence.
- Recommended actions.
- Runbook sources.

The design demonstrates prompt chaining, Java record mapping, and structured AI output.

Valid JSON is not assumed to guarantee factual correctness; deterministic evidence validation is a remaining hardening objective.

---

## 11. AI Token Usage and Cost Tracking

Implemented token usage monitoring and estimated model cost calculation.

### Features

- Input token tracking.
- Output token tracking.
- Total token tracking.
- Configurable Amazon Nova pricing.
- Estimated cost calculation using `BigDecimal`.
- PostgreSQL usage persistence.
- Correlation ID association.
- Organization-level cost aggregation.

### Database

The `ai_model_usage` table records information including:

- Organization ID.
- Correlation ID.
- Transaction ID.
- Model identifier.
- AWS region.
- Token consumption.
- Estimated USD costs.
- Usage source.
- Record timestamp.

Usage records captured from Spring AI response metadata are distinguished from independently measured model invocations.

Estimated application costs are not represented as reconciled AWS billing amounts.

---

## 12. REST API Examples

The application exposes several investigation endpoints.

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/api/agent/mcp/investigate` | MCP-based transaction investigation |
| POST | `/api/agent/mcp/investigate-with-rag` | Combined MCP and RAG investigation |
| POST | `/api/agent/mcp/investigate-report` | Structured JSON investigation report |

### Example Request

```http
POST http://localhost:8080/api/agent/mcp/investigate-report
Authorization: Bearer <keycloak-access-token>
Content-Type: application/json
```

```json
{
  "transactionId": "TXN-1001"
}
```

### Example Report Fields

```json
{
  "transactionId": "TXN-1001",
  "transactionStatus": "PENDING",
  "failureCode": "GATEWAY_TIMEOUT",
  "gatewayOutcome": "UNKNOWN",
  "rootCauseAssessment": {
    "finding": "Payment gateway response was not received.",
    "confidence": "UNCONFIRMED"
  },
  "evidence": [],
  "recommendedActions": [
    "Verify the authoritative gateway outcome before retrying."
  ],
  "runbookSources": []
}
```

This response is illustrative. Actual output depends on the retrieved evidence and model execution.

---

## 13. Technology Stack

| Category | Technologies |
|---|---|
| Language | Java 21 |
| Backend | Spring Boot, Spring REST |
| AI Framework | Spring AI |
| LLM | Amazon Bedrock, Amazon Nova |
| Embeddings | Amazon Titan Embeddings |
| RAG | Spring AI, pgvector, Bedrock Knowledge Bases |
| Database | PostgreSQL |
| Database Migrations | Flyway |
| Tool Integration | Model Context Protocol |
| Security | Spring Security, Keycloak, OAuth2, JWT |
| Build | Gradle |
| Containers | Docker |
| Cloud | AWS, S3, Bedrock, IAM |
| Testing | Postman, Java tests |
| Development | IntelliJ IDEA, Git, GitHub |
| Observability | SLF4J logging, correlation IDs, Micrometer |

---

## 14. Local Development

The main API and MCP Server are separate Spring Boot applications.

Development services include:

| Service | Local port |
|---|---|
| EnterpriseOps API | 8080 |
| Keycloak | 8081 |
| MCP Server | 8090 |
| PostgreSQL | 5432 |

### Prerequisites

- JDK 21.
- Docker and Docker Compose.
- PostgreSQL with pgvector.
- AWS account with appropriate Bedrock permissions.
- AWS CLI configured for development.
- Keycloak realm and clients configured for local authentication.
- Java development environment.

### Build

From the main API project directory on Windows:

```powershell
.\gradlew.bat clean build
```

Start the supporting services and run the Spring Boot application using the appropriate local configuration.

**Security:** Local passwords, database credentials, OAuth2 client secrets, AWS credentials and other sensitive configuration must be supplied through secure local configuration or environment variables. Do not commit actual secrets to GitHub.

---

## 15. Project Development Roadmap

| Phase | Area | Status |
|---|---|---|
| 1 | Java, Spring Boot and AWS Foundation | Completed |
| 2 | Generative AI and Amazon Bedrock | Completed |
| 3 | Embeddings and Vector Databases | Completed |
| 4 | Advanced RAG | Completed |
| 5 | RAG Evaluation Concepts | Completed |
| 6 | Enterprise AI Security and Performance |  Completed |
| 7 | Custom RAG vs Bedrock Knowledge Bases |  Completed |
| 8 | Agentic AI, MCP and Execution Guardrails |  Completed |

### Remaining Work

- Deterministic evidence validation.
- Hallucination prevention and output guardrails.
- Prompt-injection resilience.
- Final integration verification.
- Comprehensive retrieval, security, performance and cost evaluation.

---

## 16. Engineering Objectives and Learning Outcomes

This project demonstrates practical experience with:

- Integrating LLMs into Java enterprise applications.
- Developing custom RAG architectures.
- Building secure AI tool integrations.
- Implementing MCP clients and servers.
- Combining remote MCP tools with local RAG capabilities.
- Applying deterministic controls to agentic workflows.
- Implementing OAuth2-secured service-to-service communication.
- Generating structured reports from AI investigations.
- Tracking model token usage and estimated inference costs.
- Understanding the limitations of LLM-generated conclusions and the importance of evidence validation.

The project is intended as a technical portfolio and applied AI engineering learning platform, not a production banking service.

---

## 17. Disclaimer

EnterpriseOps AI uses simulated banking transactions and sample operational runbooks.

It does not process real financial transactions, execute payments or perform financial remediation.

The application is intended for educational, research, prototyping and software engineering demonstration purposes.

---

## Author

**Sri Krishna Garisa**

Principal Java / AWS Engineer | Technical Lead | Cloud & Integration Solutions

GitHub: https://github.com/8001026

**Main API:** https://github.com/8001026/enterpriseops-api

**MCP Server:** https://github.com/8001026/enterpriseops-mcp-server

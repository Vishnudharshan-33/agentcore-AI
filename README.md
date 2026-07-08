# 🤖 Amazon Bedrock AgentCore Crash Course

This crash course is a hands-on introduction to **Amazon Bedrock AgentCore**, a fully managed service for building and deploying intelligent agents. This repository contains progressive examples demonstrating how to build AI agents that leverage language models, RAG (Retrieval-Augmented Generation), and memory management.

## 📚 Course Structure

This course includes three example implementations of increasing complexity:

1. **`00_langgraph_agent.py`** - Basic LangGraph agent with FAQ search using Hugging Face embeddings and Groq chat
2. **`01_agentcore_runtime.py`** - AgentCore runtime agent with tool-based FAQ search and query reformulation
3. **`02_agentcore_memory.py`** - AgentCore runtime agent with long-term memory and preference tracking

Each example uses the **Lauki Q&A dataset** (`lauki_qna.csv`) as a knowledge base for the agent to search and provide answers.

## 🛠️ Set-up & Prerequisites

### System Requirements

- **Python**: 3.12.x (per `pyproject.toml`)
- **Operating System**: Linux, macOS, or Windows
- **uv**: Ultra-fast Python package installer and resolver

Check your Python version:

```bash
python --version
```

Install uv if needed:

```bash
python -m pip install uv
```

### AWS Account & Credentials

- An **AWS account** with access to Amazon Bedrock
- **AWS credentials** configured (see [AWS CLI Configuration](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html))
- Region set to a supported AgentCore region (for example `ap-southeast-2` or `us-east-1`)

### API Keys

- **GROQ_API_KEY**: Required for the Groq model provider
- **COHERE_API_KEY**: Required for Cohere embeddings used in `01_agentcore_runtime.py`

> `HF_API_KEY` is not required for the current example scripts unless you use private Hugging Face models.

## 📦 Installation

### Step 1: Open the Repository

```bash
cd /path/to/agentcore-AI
```

### Step 2: Install Dependencies

```bash
uv sync
```

This installs all dependencies from `pyproject.toml`.

### Step 3: Configure Environment Variables

Create a `.env` file in the repository root:

```bash
touch .env
```

Add your API keys:

```env
GROQ_API_KEY=your_groq_api_key_here
COHERE_API_KEY=your_cohere_api_key_here
```

## ▶️ Running the Agents

### Example 1: Basic LangGraph Agent

Run the local LangGraph FAQ agent:

```bash
python 00_langgraph_agent.py
```

This script uses `HuggingFaceEmbeddings` to build a FAISS FAQ index and `ChatGroq` to answer questions from the knowledge base.

### Example 2: AgentCore Runtime Agent

Configure the AgentCore runtime entrypoint:

```bash
agentcore configure -e 01_agentcore_runtime.py
```

This generates `bedrock_agentcore.yaml` with the runtime configuration.

Launch the agent with required environment variables:

```bash
agentcore launch --env GROQ_API_KEY=your_groq_api_key_here --env COHERE_API_KEY=your_cohere_api_key_here
```

Invoke the deployed agent:

```bash
agentcore invoke '{"prompt": "Explain roaming activation"}'
```

This example demonstrates:

- Tool-based FAQ search
- Query reformulation for complex questions
- AgentCore runtime entrypoint support

### Example 3: AgentCore with Memory

Configure the memory-enabled AgentCore runtime:

```bash
agentcore configure -e 02_agentcore_memory.py
```

Launch the agent:

```bash
agentcore launch --env GROQ_API_KEY=your_groq_api_key_here --env COHERE_API_KEY=your_cohere_api_key_here
```

Invoke the deployed agent with optional actor/thread metadata:

```bash
agentcore invoke '{"prompt": "Remember my preference and answer my question", "actor_id": "user123", "thread_id": "session1"}'
```

This example demonstrates:

- Long-term memory integration with AgentCore
- Memory middleware for saving and retrieving conversation history
- Session-based actor/thread tracking
- User preference retrieval and personalization

## ⚙️ Troubleshooting

### Python version error
**Solution**: Ensure you have Python 3.12.x installed:

```bash
python --version
```

### Missing `GROQ_API_KEY`
**Solution**: Verify `.env` contains the key and is in the project root:

```bash
cat .env
```

### Missing `COHERE_API_KEY`
**Solution**: Add the Cohere key to `.env` and restart the agent:

```bash
cat .env
```

### FAISS installation fails
**Solution**: Install the CPU version explicitly:

```bash
uv pip install --upgrade faiss-cpu
```

### AWS credentials not found
**Solution**: Configure AWS credentials with the AWS CLI:

```bash
aws configure
```

## 📚 Additional Resources

- [Amazon Bedrock AgentCore](https://aws.amazon.com/bedrock/agentcore/?trk=33dad69a-efe5-4eb8-b3eb-bfdc0cf9a3c0&sc_channel=el)
- [Amazon Bedrock AgentCore Documentation](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/agentcore-get-started-toolkit.html/?trk=33dad69a-efe5-4eb8-b3eb-bfdc0cf9a3c0&sc_channel=el)
- [Amazon Bedrock AgentCore Samples](https://github.com/awslabs/amazon-bedrock-agentcore-samples)

---
Copyright©️ Codebasics Inc. All rights reserved.

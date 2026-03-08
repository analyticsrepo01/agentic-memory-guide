# Agentic Memory Guide: Complete Guide to Memory in AI Agents

A comprehensive, production-ready guide to implementing memory systems in AI agents. From fundamental concepts to multi-tenant security, these notebooks provide everything you need to build intelligent, context-aware agents.

## 📚 Overview

Memory is what transforms a simple chatbot into an intelligent assistant. This repository contains three progressive notebooks that take you from basic concepts to production-grade, secure memory implementations.

**Perfect for:**
- AI/ML Engineers building agentic systems
- Software Engineers implementing conversational AI
- Product teams designing multi-user AI applications
- Anyone interested in production-grade agent architecture

---

## 🎯 What You'll Learn

- ✅ Short-term vs long-term memory architecture
- ✅ Session state management
- ✅ Cross-session memory persistence
- ✅ Memory search and retrieval patterns
- ✅ Production deployment strategies
- ✅ Multi-tenant security and isolation
- ✅ GDPR/privacy compliance patterns
- ✅ Security testing and validation

---

## 📖 Notebooks

### 1️⃣ [Memory & Session Management](./adk_code_v7_memory.ipynb)
**Foundation: Understanding Memory Fundamentals**

Learn the core concepts of agent memory through practical, runnable examples.

**Topics Covered:**
- **Session State (Short-Term Memory)**
  - Storing temporary data within a conversation
  - Using `session.state` for working memory
  - Tools: `save_preference()`, `get_preference()`

- **Memory Service (Long-Term Memory)**
  - Persisting information across sessions
  - InMemoryMemoryService for development
  - Multi-session conversation examples

- **Memory Tools**
  - `load_memory` tool for agent-controlled recall
  - Automatic memory search and retrieval
  - When to use which memory type

- **Conversation History**
  - Automatic context maintenance
  - Multi-turn conversations
  - Context accumulation patterns

**Real-World Examples:**
- ✨ User preference storage and retrieval
- ✨ Multi-session project tracking
- ✨ Birthday party planning with context accumulation (8-year-old, unicorns, purple theme)
- ✨ Contact management system

**Key Takeaway:** Master the difference between session state and memory service, and learn when to use each.

---

### 2️⃣ [Memory Testing & Production Setup](./adk_code_v8_memory_testing_fixed.ipynb)
**Production: Testing, Validation & Deployment**

Comprehensive testing patterns and production-ready memory implementations.

**Topics Covered:**
- **Memory Service Comparison**
  ```
  InMemoryMemoryService vs Production Memory Bank
  ├─ Storage: Full text vs Extracted facts
  ├─ Search: Keyword vs Semantic
  ├─ Persistence: RAM vs Cloud Storage
  └─ Best for: Development vs Production
  ```

- **Testing Patterns**
  - Memory recall accuracy testing
  - Multi-session memory accumulation
  - Search quality validation
  - Memory consolidation testing

- **Automatic Persistence**
  - `after_agent_callback` for auto-save
  - Session-to-memory pipeline
  - Error handling and recovery

- **Production Memory Bank**
  - VertexAI Memory Bank setup (production-ready)
  - Semantic search with embeddings
  - Async memory generation
  - Memory consolidation and deduplication

**Real-World Testing:**
- ✅ Test 1: InMemory baseline (food preferences)
- ✅ Test 2: Memory recall with load_memory tool
- ✅ Test 3: Multi-session accumulation (work, hobbies, language learning)
- ✅ Test 4: Automatic persistence with callbacks
- ✅ Test 5: Memory search accuracy

**Key Takeaway:** Learn production deployment patterns and comprehensive testing strategies.

---

### 3️⃣ [Memory Isolation & Multi-Tenant Security](./adk_code_v9_memory_isolation.ipynb)
**Security: Multi-User, Multi-Tenant Protection**

Critical security patterns for production multi-user applications.

**Topics Covered:**
- **Understanding Memory Scope**
  ```
  Memory Isolation = app_name + user_id

  Memories are ONLY retrieved when BOTH match:
  ✅ Same app + Same user = Access granted
  ❌ Same app + Different user = Blocked
  ❌ Different app + Same user = Blocked
  ❌ Different app + Different user = Blocked
  ```

- **User-Level Isolation**
  - Preventing cross-user data leakage
  - Healthcare example: Alice vs Bob allergies
  - Search isolation validation

- **App-Level Isolation**
  - Multi-tenant SaaS security
  - Banking vs Health app separation
  - Same user, different apps = complete isolation

- **Multi-Tenant Security Matrix**
  - Company A vs Company B isolation
  - User 1 vs User 2 within each company
  - 4-way isolation testing

- **Security Attack Simulation**
  - Password/credit card search attempts (blocked)
  - Wildcard search attacks (blocked)
  - User impersonation attempts (blocked)
  - SQL injection-style queries (blocked)

**Real-World Security Tests:**
- 🔒 Test 1: User-level isolation (healthcare data)
- 🔒 Test 2: App-level isolation (banking vs health)
- 🔒 Test 3: Multi-tenant matrix (Company A vs B)
- 🔒 Test 4: Security attack simulation

**Production Security Best Practices:**
```python
# ✅ CORRECT: Use authenticated user from session
current_user_id = get_authenticated_user_id()  # From auth middleware
results = await memory_service.search_memory(
    app_name=APP_NAME,
    user_id=current_user_id,  # Never trust user input!
    query=user_query
)

# ❌ WRONG: Never use user-provided IDs
user_id = request.params.get('user_id')  # Can be manipulated!
results = await memory_service.search_memory(
    app_name=APP_NAME,
    user_id=user_id,  # Security vulnerability!
    query=user_query
)
```

**Key Takeaway:** Memory isolation is automatic when you always use authenticated `user_id` from server-side sessions.

---

## 🚀 Quick Start

### Prerequisites
```bash
pip install google-adk google-cloud-aiplatform
```

### Running the Notebooks

1. **Start with Notebook 1** to understand fundamentals
2. **Progress to Notebook 2** for production patterns
3. **Complete with Notebook 3** for security mastery

Each notebook is standalone and fully runnable with example code.

### Environment Setup
```python
import os
os.environ["GOOGLE_CLOUD_PROJECT"] = "your-project-id"
os.environ["GOOGLE_CLOUD_LOCATION"] = "us-central1"
os.environ["GOOGLE_GENAI_USE_VERTEXAI"] = "TRUE"
```

---

## 🏗️ Architecture Patterns

### Pattern 1: Session State (Short-Term)
```python
# Use for: Temporary conversation context
session.state['user_preference'] = "dark_mode"
```

### Pattern 2: Memory Service (Long-Term)
```python
# Use for: Cross-session persistence
await memory_service.add_session_to_memory(session)
```

### Pattern 3: Auto-Save with Callbacks
```python
# Use for: Automatic memory persistence
async def auto_save_callback(callback_context):
    session = callback_context._invocation_context.session
    await memory_service.add_session_to_memory(session)

agent = Agent(after_agent_callback=auto_save_callback)
```

### Pattern 4: Agent Memory Recall
```python
# Use for: Agent-controlled memory search
agent = Agent(
    tools=[load_memory],
    instruction="Use load_memory to recall past information"
)
```

---

## 🎯 Use Cases

### Customer Support Agents
- Remember customer preferences across sessions
- Recall past issues and solutions
- Provide personalized support experiences

### Healthcare Assistants
- Store patient preferences securely (HIPAA-compliant isolation)
- Maintain medical history context
- User-level isolation prevents data leakage

### Financial Advisors
- Track investment preferences
- Remember risk tolerance
- Maintain transaction context

### E-commerce Assistants
- Remember shopping preferences
- Track past purchases
- Personalize product recommendations

### Multi-Tenant SaaS Platforms
- Complete tenant isolation
- Secure multi-user environments
- GDPR/privacy compliance

---

## 📊 Production Checklist

### Development Phase
- [ ] Use InMemoryMemoryService for testing
- [ ] Implement session state for temporary data
- [ ] Add memory service for persistence
- [ ] Add load_memory tool to agents
- [ ] Test memory recall accuracy

### Production Phase
- [ ] Switch to persistent memory storage
- [ ] Implement auto-save callbacks
- [ ] Configure memory consolidation
- [ ] Set up monitoring and logging
- [ ] Test memory search performance

### Security Phase
- [ ] Always use authenticated user_id from server-side sessions
- [ ] Implement app-level isolation for multi-tenant systems
- [ ] Test against common attacks (covered in Notebook 3)
- [ ] Audit memory access patterns
- [ ] Ensure GDPR/privacy compliance

### Monitoring Phase
- [ ] Monitor memory usage and growth
- [ ] Track search performance metrics
- [ ] Handle memory search failures gracefully
- [ ] Implement memory cleanup/TTL policies
- [ ] Log memory access for security audits

---

## 🔒 Security Principles

### Critical Security Rules

1. **Never trust client-provided user IDs**
   - Always use server-side authenticated sessions
   - Validate user identity before memory operations

2. **Use app-level isolation for multi-tenant systems**
   - Separate `app_name` for each tenant
   - Prevents cross-tenant data leakage

3. **Test security thoroughly**
   - Run all tests from Notebook 3
   - Simulate attack scenarios
   - Validate isolation boundaries

4. **Comply with privacy regulations**
   - GDPR: Right to be forgotten (memory deletion)
   - HIPAA: Secure health data storage
   - PCI DSS: Separate payment data contexts

---

## 🛠️ Advanced Topics

### Memory Consolidation
Production memory systems automatically:
- Extract key facts from conversations
- Deduplicate redundant information
- Consolidate related memories
- Update outdated information

### Semantic Search
Production systems use:
- Embedding-based similarity search
- Context-aware retrieval
- Relevance ranking
- Multi-query support

### Memory Lifecycle
- **Creation:** Auto-save with callbacks
- **Storage:** Persistent cloud storage
- **Retrieval:** Semantic search with load_memory
- **Consolidation:** Automatic deduplication
- **Expiration:** TTL policies and cleanup

---

## 📈 Performance Considerations

### Memory Service Comparison

| Metric | InMemory | Production |
|--------|----------|------------|
| **Latency** | < 10ms | 50-200ms |
| **Storage** | RAM-limited | Cloud-scale |
| **Search Quality** | Keyword | Semantic |
| **Persistence** | None | Permanent |
| **Cost** | Free | Pay-per-use |

### Optimization Tips
- Use InMemory for development/testing
- Implement caching for frequent queries
- Batch memory saves when possible
- Monitor search performance
- Set appropriate TTL policies

---

## 🤝 Contributing

Found an issue or have a suggestion?

- Open an issue for bugs or questions
- Submit a PR for improvements
- Share your use cases and learnings

---

## 📝 License

This project is provided as educational material. Feel free to use, modify, and distribute with attribution.

---

## 🙏 Acknowledgments

Built with:
- Google ADK (Agent Development Kit)
- Vertex AI
- Gemini 2.5 Flash

---

## 📧 Contact

**Author:** Saurabh
**Email:** saurabh.m14@gmail.com
**GitHub:** [@analyticsrepo01](https://github.com/analyticsrepo01)

---

## 🌟 Star This Repo!

If you found this helpful, please star the repository and share it with others building agentic systems!

---

**Happy Building! 🚀**

Memory is what makes agents truly intelligent. Use it wisely, secure it properly, and build amazing experiences.

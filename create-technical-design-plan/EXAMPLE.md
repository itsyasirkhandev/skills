# Design Reference Examples

This file contains a canonical, fully-worked example of a `design.md`. When
generating a new technical design plan, **match this level of detail**.

---

## Example: Chat History Sidebar

---
## Design Plan: Chat History Sidebar

Technical specification for implementing a scalable, real-time chat history sidebar with React, FastAPI, and PostgreSQL.

## 1. Objective

Implement a chat history sidebar that allows users to:

* View previous chats
* Reopen any chat
* Identify the currently active chat
* Keep chats ordered by recent activity
* Continue conversations seamlessly

## 2. Tech Stack

* **Frontend:** React
* **Backend:** FastAPI
* **Database:** PostgreSQL / SQLite
* **ORM:** SQLAlchemy (async)

**Why this stack?**

* React is excellent for dynamic UI such as chat sidebars.
* FastAPI works well for API-based high-concurrency chat systems.
* Relational DBs are a natural fit because chats and messages have a clear 1-to-many relationship.

## 3. High-Level Architecture

The feature is divided into three distinct layers of responsibility:

**A. Frontend (React)**

* Render sidebar & window
* Track active chat
* Show loading/error states

**B. Backend (FastAPI)**

* Return chat summaries
* Return full conversations
* Create/append messages

**C. Database**

* Store chats & messages
* Maintain relationships
* Support ordered retrieval

**ARCHITECTURE FLOW**
Frontend (React) -> Backend (FastAPI) -> Database (PostgreSQL / SQLite)

## 4. Data Model

We need two main entities: Chat (represents the conversation thread) and Message (represents individual messages). One chat has many messages.

```python
from sqlalchemy import Column, String, DateTime, ForeignKey, Text
from sqlalchemy.orm import relationship
from sqlalchemy.sql import func
from database import Base

class Chat(Base):
    __tablename__ = "chats"

    id = Column(String, primary_key=True)
    title = Column(String, nullable=False)
    created_at = Column(DateTime(timezone=True), server_default=func.now())
    updated_at = Column(DateTime(timezone=True), server_default=func.now(), onupdate=func.now())

    messages = relationship("Message", back_populates="chat", cascade="all, delete-orphan")

class Message(Base):
    __tablename__ = "messages"

    id = Column(String, primary_key=True)
    chat_id = Column(String, ForeignKey("chats.id"), nullable=False)
    role = Column(String, nullable=False)
    # "user" or "assistant"
    content = Column(Text, nullable=False)
    created_at = Column(DateTime(timezone=True), server_default=func.now())

    chat = relationship("Chat", back_populates="messages")

```

## 5. Core Design Decisions

**Decision 1: Store chats and messages separately**
*Why:* A chat contains metadata, while messages vary in number. Separation makes retrieval and ordering efficient.

**Decision 2: Use updated_at to sort chats**
*Why:* The sidebar should surface most recently active chats first. Updating this timestamp on new messages naturally handles sorting.

**Decision 3: Fetch summaries for sidebar**
*Why:* Sidebar load times must be fast. Fetching thousands of message contents upfront.

## 6. Core Functional Flows

**A. Load Sidebar**
Triggered on app open. Requests summaries sorted by updated_at DESC.

```json
{
  "id": "chat_101",
  "title": "Explain overfitting",
  "updated_at": "2026-04-06T10:15:00"
}

```

**B. Open Chat**
Triggered by click. Requests full chat + messages ordered by created_at ASC.

```python
stmt = (
    select(Message)
    .where(Message.chat_id == chat_id)
    .order_by(Message.created_at.asc())
)

```

**C. Title Generation**
Titles are generated from the first user message, truncated if necessary.

```python
def generate_chat_title(first_message: str, max_len: int = 50) -> str:
    title = first_message.strip()
    if len(title) <= max_len:
        return title
    return title[:max_len].rstrip() + " ... "

```

## 7. Development Plan

1. **Database & Models:** Create models, establish relationships, and apply indexes to `updated_at` and `chat_id`.
2. **Backend APIs:** Implement FastAPI routes for fetching summaries, single chats, and appending messages.
3. **Frontend UI:** Build React Sidebar, ChatWindow, wiring clicks, and handling empty/loading states.
# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Starting the Application
- **Quick start**: `./run.sh` (runs the web server on port 8000)
- **Manual start**: `cd backend && uv run uvicorn app:app --reload --port 8000`
- **Environment**: Requires `ANTHROPIC_API_KEY` in `.env` file

### Package Management
- **Install dependencies**: `uv sync` (uses pyproject.toml)
- **Python version**: 3.13+ (specified in .python-version)

### URLs
- Web interface: http://localhost:8000
- API documentation: http://localhost:8000/docs

## Architecture Overview

This is a full-stack RAG (Retrieval-Augmented Generation) system for querying course materials using semantic search and AI responses.

### Backend Architecture (Python/FastAPI)

The backend follows a modular architecture with clear separation of concerns:

- **app.py**: FastAPI server with API endpoints (`/api/query`, `/api/courses`)
- **rag_system.py**: Main orchestrator that coordinates all components
- **vector_store.py**: ChromaDB integration for semantic search and vector storage
- **ai_generator.py**: Anthropic Claude integration with tool support
- **document_processor.py**: Document parsing and chunking (PDF, DOCX, TXT)
- **search_tools.py**: Tool-based search system for AI function calling
- **session_manager.py**: Conversation history management
- **models.py**: Pydantic data models (Course, Lesson, CourseChunk)
- **config.py**: Configuration management

### Key Design Patterns

1. **Tool-Based AI**: Uses Anthropic's function calling with custom search tools rather than direct vector retrieval
2. **Session Management**: Maintains conversation history per session
3. **Incremental Loading**: Avoids re-processing existing courses on startup
4. **Chunk-Based Storage**: Documents are split into semantic chunks for better retrieval

### Data Flow

1. Documents in `/docs` are processed into Course objects with Lessons and Chunks
2. Chunks are embedded and stored in ChromaDB vector database
3. User queries trigger AI generation with access to CourseSearchTool
4. AI can search the vector store through tool calls to find relevant content
5. Responses are generated with sources and conversation history

### Frontend (Static Files)

Simple HTML/CSS/JS interface served from `/frontend`:
- **index.html**: Main chat interface
- **script.js**: API communication and DOM handling
- **style.css**: Styling

### Key Dependencies

- **FastAPI/Uvicorn**: Web server and API framework
- **ChromaDB**: Vector database for semantic search
- **Anthropic**: Claude AI integration
- **sentence-transformers**: Text embeddings for vector search
- **python-dotenv**: Environment variable management

### Database

Uses ChromaDB (local) for vector storage with automatic persistence. No traditional database required.
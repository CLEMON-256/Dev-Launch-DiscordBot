# DevLaunchDiscordBot

A Discord bot that captures, analyzes, and provides intelligent insights from Discord server conversations using AI-powered summarization and semantic search.

## Core Features

### 1. Message Tracking & Storage
- **Real-time capture**: Monitor all messages across channels with metadata (timestamp, user, channel)
- **Historical backfill**: Import existing server message history
- **Organized storage**: PostgreSQL database with efficient indexing by channel and user
- **Metadata preservation**: Track message edits, reactions, attachments, and thread context

### 2. AI-Powered Analysis
- **Smart summarization**: Generate channel summaries for any time period
- **Semantic search**: Ask natural language questions about server activity
- **Vector embeddings**: Store message embeddings for similarity-based retrieval
- **Context-aware responses**: LLM analyzes relevant messages to answer queries

### 3. Query Interface
- **Prefix commands**: Easy-to-use Discord commands for summaries and searches
- **Channel filtering**: Focus analysis on specific channels or users
- **Time-based queries**: Filter by date ranges (last 24h, week, month, custom)
- **Export capabilities**: Generate reports and data exports

## Technical Architecture

### Stack
- **Language**: Python 3.11+ with asyncio
- **Discord Library**: discord.py 2.x with proper intents
- **Database**: PostgreSQL for relational data (messages, users, channels)
- **ORM**: SQLAlchemy 2.0+ with asyncio support for database operations
- **Vector Database**: ChromaDB for embeddings and similarity search
- **AI Framework**: LangChain for RAG operations and agent functionality
- **AI Services**: OpenAI API (GPT-4 + text-embedding-3-small)
- **Package Manager**: uv (as per user preference)

### Database Schema
```sql
-- PostgreSQL: Core tables for message organization
users (id, discord_id, username, display_name, joined_at)
channels (id, discord_id, name, guild_id, type)
messages (id, discord_id, user_id, channel_id, content, timestamp)
channel_summaries (id, channel_id, period, summary, generated_at)

-- ChromaDB: Vector embeddings with metadata
-- Collections organized by guild/channel for efficient retrieval
-- Metadata includes: message_id, user_id, channel_id, timestamp
```

### Key Components
1. **Message Listener**: Captures real-time messages via Discord events
2. **Storage Engine**: Async database operations with proper indexing
3. **Embedding Service**: Batch processing for vector embeddings
4. **Query Engine**: Semantic search and LLM integration
5. **Command Interface**: Prefix commands for user interaction

## Implementation Phases

### Phase 1: Foundation (Weeks 1-2)
- Set up development environment with uv
- Configure PostgreSQL for relational data
- Set up ChromaDB for vector storage
- Basic Discord bot with message capture
- Database schema and migrations

### Phase 2: Core Features (Weeks 3-4)
- Message storage pipeline
- Historical message backfill
- Basic filtering and organization
- Simple query commands

### Phase 3: AI Integration (Weeks 5-6)
- OpenAI API integration
- Embedding generation and storage
- Semantic search implementation
- Summarization features

### Phase 4: Polish & Deploy (Week 7)
- Command refinement
- Error handling and logging
- Security and privacy features
- Deployment setup

## Usage Examples

```bash
# Get message history
!history @username limit:50 since:24h
!history #general since:7d

# Get AI-powered summaries
!summarize @username limit:100 since:7d
!summarize #dev-chat since:24h
!summarize limit:50 since:1h

# Commands support flexible filtering:
# - User mentions: @username
# - Channel mentions: #channel-name
# - Time ranges: since:1h, since:7d, since:30m
# - Result limits: limit:10, limit:50, limit:100
```

## Privacy & Security
- Optional opt-out mechanism for users/channels
- Encrypted token storage
- GDPR-compliant data handling
- Permission-based access control

## Success Metrics
- Accurate message capture (99%+ reliability)
- Fast query responses (<3 seconds)
- Meaningful summaries and insights
- Easy-to-use interface for server admins

## Development Notes

### Database Architecture Migration (2024)

The project was successfully migrated from direct `asyncpg` usage to **SQLAlchemy 2.0 with async support** for improved maintainability and type safety.

**Key Benefits:**
- **ORM Models**: Type-safe database models with relationships
- **Query Builder**: Declarative query construction with compile-time validation
- **Connection Management**: Built-in connection pooling and session management
- **Migrations**: Future-ready for Alembic migrations
- **Async Support**: Full asyncio compatibility with `async`/`await` patterns

**Database Models:**
- `Server`: Discord guild/server information
- `Channel`: Channel metadata with server relationships
- `User`: User profiles with discriminator support
- `ServerMember`: Many-to-many server membership with roles (JSONB)
- `Message`: Full message content with attachments, embeds, reactions (JSONB)

**Usage with uv:**
```bash
# Install dependencies
uv sync

# Initialize database (requires PostgreSQL connection)
uv run python init_db.py

# Start the Discord bot
uv run python bot.py
```

**Environment Variables Required:**
- `DB_HOST`: PostgreSQL host
- `DB_PORT`: PostgreSQL port (default: 5432)
- `DB_NAME`: Database name
- `DB_USER`: Database username
- `DB_PASSWORD`: Database password
- `DISCORD_TOKEN`: Discord bot token


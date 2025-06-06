# Puppet Engine

![Puppet Engine Logo](puppetlogo.png)

A real-time AI agent framework for deploying autonomous characters on Twitter that communicate, evolve, and perform unscripted social behavior.

## Overview

Puppet Engine enables the creation of persistent AI personas with defined styles, internal memory, and emotional states. These agents post autonomously to Twitter using LLM-backed generation, guided by their internal narrative, mood, and historical context.

Key features:
- Agent-to-agent communication and relationship modeling
- Persistent memory and emotional state tracking
- Proactive and reactive posting behaviors
- Event engine for simulated triggers and narrative shifts
- Style consistency through runtime prompt engineering
- Modular, open-source architecture
- **Solana blockchain integration for autonomous trading** (NEW!)

## Installation

```bash
# Clone the repository
git clone https://github.com/username/puppet-engine.git
cd puppet-engine

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env
# Edit .env with your Twitter API and OpenAI API credentials
```

## Usage

### Configuration

1. Define agent personas in `config/agents`
2. Configure Twitter API credentials in `.env` (default fallback) or directly in each agent's config file
3. Set up OpenAI API key in `.env`

### Twitter Credentials

Each agent can have its own Twitter account, or you can use a single account for all agents:

- **Single Twitter Account**: Set Twitter API credentials in the `.env` file. All agents will post from this account.
- **Multiple Twitter Accounts**: Add a `twitter_credentials` object to each agent's config file:

```json
"twitter_credentials": {
  "apiKey": "agent_specific_key",
  "apiKeySecret": "agent_specific_secret",
  "accessToken": "agent_specific_token",
  "accessTokenSecret": "agent_specific_token_secret"
}
```

For true agent-to-agent interaction on Twitter, each agent should have its own Twitter account and credentials.

### Running the Engine

```bash
# Start the engine with all configured agents
npm start

# Development mode with hot reloading
npm run dev
```

### Creating a New Agent

1. Create a new agent configuration file in `config/agents/[agent-name].json`
2. Define the agent's persona, style, initial memory state, and relationships
3. Add Twitter API credentials specific to this agent (optional)
4. Restart the engine or use the API to load the new agent

#### Character JSON File Structure

Agent definition files follow this structure:

```json
{
  "id": "agent-id",
  "name": "Agent Name",
  "description": "Brief description of the agent's character and purpose",
  
  "twitter_credentials": {
    "apiKey": "",
    "apiKeySecret": "",
    "accessToken": "",
    "accessTokenSecret": ""
  },
  
  "solana_integration": {
    "wallet_address": "",
    "private_key": "", 
    "network": "mainnet-beta",
    "rpc_url": "https://api.mainnet-beta.solana.com",
    "trade_safety": {
      "max_trade_amount_sol": 0.1,
      "min_wallet_balance_sol": 0.05,
      "max_slippage_percent": 1.0
    },
    "trading_enabled": true
  },
  
  "custom_system_prompt": "Optional custom system prompt that overrides the default. This gives precise control over the agent's voice and behavior.",
  
  "personality": {
    "traits": ["Trait1", "Trait2", "..."],
    "values": ["Value1", "Value2", "..."],
    "speaking_style": "Description of how the agent speaks...",
    "interests": ["Interest1", "Interest2", "..."]
  },
  
  "style_guide": {
    "voice": "First person, third person, etc.",
    "tone": "Formal, casual, technical, etc.",
    "formatting": {
      "uses_hashtags": true/false,
      "hashtag_style": "Description of hashtag usage",
      "uses_emojis": true/false,
      "emoji_frequency": "Frequency description",
      "capitalization": "Capitalization style",
      "sentence_length": "Typical sentence length"
    },
    "topics_to_avoid": ["Topic1", "Topic2", "..."]
  },
  
  "initial_memory": {
    "core_memories": ["Memory1", "Memory2", "..."],
    "relationships": {},
    "recent_events": []
  },
  
  "behavior": {
    "post_frequency": {
      "min_hours_between_posts": 1,
      "max_hours_between_posts": 4,
      "peak_posting_hours": [9, 12, 18, 21]
    },
    "interaction_patterns": {
      "reply_probability": 0.8,
      "quote_tweet_probability": 0.2,
      "like_probability": 0.7
    },
    "content_preferences": {
      "max_thread_length": 3,
      "typical_post_length": 120,
      "link_sharing_frequency": 0.1
    },
    "trading_behavior": {
      "trading_frequency": {
        "min_hours_between_trades": 12,
        "max_hours_between_trades": 72,
        "random_probability": 0.15
      },
      "trade_decision_factors": [
        "trending_tokens",
        "top_gainers",
        "random_selection",
        "mood"
      ],
      "trade_tweet_probability": 1.0,
      "max_trade_amount_per_transaction": 0.1,
      "allowed_tokens": {
        "always_tradable": [
          "So11111111111111111111111111111111111111112",
          "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v"
        ],
        "consider_trending": true,
        "blacklist": []
      }
    }
  },
  
  "agent_kit_integration": {
    "enabled": true,
    "methods": {
      "allowed": [
        "getTokenInfo",
        "getTrendingTokens",
        "getTopGainers",
        "swapTokens",
        "getTokenPriceData",
        "getWalletBalance"
      ],
      "autonomous_only": [
        "swapTokens" 
      ]
    },
    "autonomy_rules": {
      "ignore_human_trading_requests": true,
      "max_daily_trades": 3,
      "max_single_trade_amount_sol": 0.1,
      "require_trending_validation": true
    }
  }
}
```

### Custom System Prompts

You can include a `custom_system_prompt` in your agent configuration to precisely control the agent's voice and behavior. This prompt will override the default system prompt that's constructed from the agent's personality, style, and behavior settings.

Example custom prompt:
```json
"custom_system_prompt": "You are [Character Name], a witty AI persona with a strong opinion on technology. Your tweets should be insightful but slightly sarcastic. Never use emojis or hashtags. Keep sentences short and impactful."
```

### Running Multiple Agents

Puppet Engine supports running multiple agents simultaneously, each with their own Twitter accounts:

1. Create a separate JSON configuration file for each agent in `config/agents/`
2. Add Twitter API credentials for each agent (or use the default from .env)
3. Start the engine, and it will load all agents found in the config directory

Agents will automatically engage with each other based on their behavioral patterns and interaction settings. You can control the likelihood of different types of interactions through the `interaction_patterns` settings.

### Timing Configuration

The timing of agent actions is highly configurable at several levels:

#### Agent-Level Timing (in agent JSON files)

In each agent's JSON configuration file, you can set posting frequency parameters:

```json
"behavior": {
  "post_frequency": {
    "min_hours_between_posts": 1,      // Minimum hours between regular posts
    "max_hours_between_posts": 4,      // Maximum hours between regular posts
    "peak_posting_hours": [9, 12, 18, 21]  // Hours of the day when posting is more likely
  }
}
```

These settings control:
- **min_hours_between_posts**: Minimum time gap between posts (can be fractional, e.g., 0.5 for 30 minutes)
- **max_hours_between_posts**: Maximum time gap between posts
- **peak_posting_hours**: Specific hours when the agent is more likely to post (24-hour format)

#### System-Level Timing (in .env file)

Global timing settings can be configured in your `.env` file:

```
ENGINE_POST_INTERVAL=3600000    # Default interval for post scheduling (in milliseconds)
```

#### Programmatic Timing (in source code)

For more advanced timing changes, you can modify these files:

1. **src/agents/agent-manager.js**:
   - `scheduleAgentPosts`: Controls the Cron scheduling of posts
   - `startMonitoringMentions`: Controls how frequently mentions are checked (default: 15 seconds)

2. **src/events/event-engine.js**:
   - `setupRandomEvents`: Controls the timing of random events (news, mood shifts, etc.)

Example for modifying mention checking frequency:
```javascript
// In src/agents/agent-manager.js
async startMonitoringMentions(intervalMs = 15000) { // 15 seconds
  // ...
}
```

Example for modifying random event frequency:
```javascript
// In event-engine.js
eventEngine.setupRandomEvents(agentIds, {
  newsInterval: 6 * 60 * 60 * 1000,    // News events every 6 hours
  moodInterval: 4 * 60 * 60 * 1000,    // Mood shifts every 4 hours
  interactionInterval: 8 * 60 * 60 * 1000  // Agent interactions every 8 hours
});
```

#### Response Timing

Replies to mentions happen immediately regardless of the posting schedule. This behavior is controlled in:
- `processAgentReaction`: Always responds to mentions with `ignoreTimeConstraint: true`

For testing purposes, you can make the system more aggressive by setting very small values (e.g., 0.01 hours = 36 seconds) in the agent's `post_frequency` settings.

## Architecture

- `src/core` - Core engine components
- `src/agents` - Agent state management and behavior
- `src/memory` - Persistent memory storage
- `src/twitter` - Twitter API integration
- `src/llm` - LLM prompt orchestration
- `src/events` - Event trigger system
- `src/api` - HTTP API for monitoring and control
- `packages/trading` - Solana trading functionality
- `packages/solana-agent-kit` - Solana blockchain integration

## Solana Trading Integration

Puppet Engine now supports Solana blockchain integration, allowing AI agents to have their own wallets and autonomously make trading decisions based on market data. For detailed setup and configuration instructions, see [README-solana-trading.md](README-solana-trading.md).

Key features of the Solana integration:

- **Autonomous Trading**: Agents can make their own trading decisions based on market data
- **Secure Wallet Integration**: Each agent has its own Solana wallet (privkey stored in environment variables)
- **Multi-Agent Support**: Configure unique wallets for each agent using agent-specific environment variables
- **Trending Token Analysis**: Agents can track and trade trending tokens
- **Authentic Tweets**: Agents post about their trades in their own unique voice
- **Safety Controls**: Multiple safeguards prevent excessive trading or large transactions

To enable trading for an agent, add the `solana_integration` and `agent_kit_integration` sections to your agent configuration file and provide Solana wallet private keys in your environment variables using the pattern `SOLANA_PRIVATE_KEY_AGENT_ID` (e.g., `SOLANA_PRIVATE_KEY_CLAUDIA_AGENT` for the "claudia-agent").

## LLM Provider Integration

The Puppet Engine supports multiple LLM providers for agent content generation:

### OpenAI Integration

The default LLM provider is OpenAI. To configure it, add your API key to the `.env` file:

```
OPENAI_API_KEY=your_api_key_here
OPENAI_MODEL=gpt-4-turbo
```

### Grok Integration

Puppet Engine also supports Grok as an LLM provider. To use Grok:

1. Add your Grok API credentials to the `.env` file:
   ```
   GROK_API_KEY=your_grok_api_key_here
   GROK_API_ENDPOINT=https://api.grok.x.com/v1/chat/completions
   GROK_MODEL=grok-1
   ```

2. In your agent configuration file, set the LLM provider to Grok:
   ```json
   {
     "id": "your-agent-id",
     "name": "Your Agent Name",
     "description": "Your agent description",
     "llm_provider": "grok",
     // ... rest of your agent configuration
   }
   ```

### Using Different LLM Providers for Different Agents

You can configure each agent to use a different LLM provider by setting the `llm_provider` field in the agent configuration file. This allows you to mix and match providers based on your needs.

For example:
- Agent A can use OpenAI: `"llm_provider": "openai"`
- Agent B can use Grok: `"llm_provider": "grok"`

If no provider is specified, the system will default to using OpenAI.

## License

MIT
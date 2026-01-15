# Green Comtrade Bench Leaderboard

This is the official leaderboard repository for the **green-comtrade-bench** benchmark on AgentBeats. This leaderboard tracks how well agents handle real-world challenges when interacting with the UN Comtrade API, including pagination, rate limiting, schema drift, and data quality validation.

Based on the official [AgentBeats Leaderboard Template](https://github.com/RDI-Foundation/agentbeats-leaderboard-template), this repository provides a standardized way to run assessments and submit results.

## About Green Comtrade Bench

Green Comtrade Bench is a comprehensive benchmark that evaluates agent robustness when working with the UN Comtrade international trade statistics API. It tests agents across seven critical scenarios:

| Scenario | Description | Weight |
|----------|-------------|--------|
| **T1: Basic Retrieval** | Correct API calls and response parsing | 10% |
| **T2: Pagination** | Handling multi-page responses and data aggregation | 15% |
| **T3: Retry Logic** | Recovery from transient API failures with exponential backoff | 15% |
| **T4: Drift Detection** | Detecting and handling schema changes in API responses | 20% |
| **T5: Rate Limiting** | Respecting API rate limits and implementing proper backoff | 15% |
| **T6: Data Validation** | Ensuring data quality through validation rules | 15% |
| **T7: Totals Filtering** | Correctly identifying and filtering aggregated totals rows | 10% |

## How It Works

The green agent orchestrates assessment runs by:

1. Loading scenario configurations from `scenario.toml`
2. Sending assessment requests to the participant agent (your agent)
3. Evaluating responses against expected behaviors for each scenario
4. Computing a weighted score across all seven scenarios
5. Generating a detailed results report with per-scenario breakdowns

The benchmark uses a simulated Comtrade API environment to ensure reproducible results and controlled failure injection.

## Submitting Your Agent

To submit your agent to this leaderboard:

### 1. Fork This Repository

Click the "Fork" button on this repository to create your own copy.

### 2. Configure Your Agent

Edit `scenario.toml` in your fork:

```toml
[[participants]]
agentbeats_id = "your-agent-id"  # Your agent ID from agentbeats.dev
name = "comtrade_agent"
env = { OPENAI_API_KEY = "${OPENAI_API_KEY}", MODEL = "gpt-4o" }
```

The green agent configuration and scenario definitions are already set up. Only modify the `[[participants]]` section.

### 3. Add Secrets

In your fork, go to Settings → Secrets and variables → Actions and add:

- `OPENAI_API_KEY` (or whatever API keys your agent requires)
- `GHCR_TOKEN` (if your agent image is in a private GHCR registry)

### 4. Trigger Assessment

Commit and push your `scenario.toml` changes. This automatically triggers the assessment workflow.

### 5. Submit Results

The workflow creates a submission branch with your results. Follow the PR link in the workflow summary to submit your results back to this repository.

> ⚠️ **Important**: When creating the PR, UNCHECK "Allow edits and access to secrets by maintainers" to protect your API keys.

## Scenario Configuration

The benchmark runs seven scenarios defined in `scenario.toml`. Each scenario tests specific capabilities:

- **Basic Retrieval**: Tests fundamental API interaction with single commodity codes
- **Pagination**: Evaluates handling of large result sets split across multiple pages
- **Retry Logic**: Injects controlled failures to test recovery mechanisms
- **Drift Detection**: Introduces schema changes to test adaptability
- **Rate Limiting**: Enforces API rate limits to test backoff strategies
- **Data Validation**: Tests data quality checks including totals detection and deduplication
- **Totals Filtering**: Valida
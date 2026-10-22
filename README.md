# Awesome-LLM-Token-Optimization

# Top LLM Token Optimization Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**  
*Focused on Prompt Compression, Intelligent Routing & Self-Hosted Cost Governance*  
**Last updated: October 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **LLM Token Optimization**. These tools reduce the cost and latency of LLM applications by compressing input context, caching responses, routing requests to cheaper models, and providing visibility into token consumption—without degrading output quality.

**Examples** include Portkey, Helicone, LiteLLM, Cloudflare AI Gateway, Martian, Langfuse, Maxim AI, and Caveman (the category leaders).

**Open-source emphasis**: This section is heavily expanded with every major active project for self-hosting, avoiding per-token vendor lock-in, and retaining control over compression and routing logic—ideal for teams that need cost predictability and data sovereignty. The open-source landscape is anchored by **LiteLLM** (the universal proxy with routing and budget controls), **Helicone** (the observability layer with gateway-based cost optimization), **Langfuse** (the open-source LLM engineering platform), and **Caveman** (the context compression proxy for coding agents), with **Portkey's Headroom** integration providing content-aware compression for self-hosted deployments.

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents
- [SaaS/Hosted Platforms](#saas-hosted-platforms)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

- **[Portkey](https://portkey.ai/)**  
  AI gateway with **Headroom** context compression as a guardrail. Headroom is a content-aware compression engine that shrinks request messages before inference—you pay for fewer input tokens without changing application code. **Bring-your-own-proxy**: Portkey does not host Headroom; you deploy it in your own infrastructure, and request payloads never leave your network. Supports `optimize` (compressed payload reaches provider) and `audit` (measure savings without altering traffic) modes, with controls for protecting recent messages and analysis context . Best for teams wanting gateway-level compression with data residency control.

- **[Helicone](https://www.helicone.ai/)**  
  LLM observability platform with an **AI Gateway** that actively optimizes costs through **cost-based routing**—automatically selecting the cheapest available provider, prioritizing your existing BYOK credits, and falling back to the next cheapest option on failure. Provides session-level cost tracking (e.g., "a support chat costs $0.12 on average with 5 API calls"), caching to eliminate redundant calls, and graduated cost alerts . Best for teams that want observability and cost optimization in one platform.

- **[Cloudflare AI Gateway](https://developers.cloudflare.com/ai-gateway/)**  
  Gateway with **caching** for identical requests, reducing latency and cost. Cache keys are SHA-256 hashes of provider, endpoint, model, auth header, and full request body—exact match required. Per-request controls via headers: `cf-aig-skip-cache` to bypass, `cf-aig-cache-ttl` for duration (60s to 1 month), and `cf-aig-cache-key` for custom grouping . Best for teams already on Cloudflare that want edge-level caching without infrastructure changes.

- **[Martian](https://withmartian.com/)**  
  Intelligent model router that selects the best LLM for each request based on prompt content, required capabilities, and cost constraints. Uses proprietary routing models to optimize for quality and cost simultaneously. OpenAI-compatible API with transparent routing decisions . Best for teams wanting automatic model selection without building routing logic.

- **[Langfuse (Cloud)](https://langfuse.com/)**  
  Hosted tier of the open-source LLM engineering platform. Provides tracing, prompt management, and a **Playground** for side-by-side prompt comparison—useful for empirically testing which prompt variants consume fewer tokens while maintaining quality . The open-source core is self-hostable.

- **[Maxim AI](https://www.getmaxim.ai/)**  
  Enterprise LLM observability platform with distributed tracing, real-time monitoring, and cost per trace tracking. Provides saved views for common debugging patterns, online evaluation, and data curation to transform logs into datasets . Best for enterprises needing compliance-grade observability.

- **[PromptLayer](https://www.promptlayer.com/)**  
  Prompt management, observability, and evals platform. Traces production requests to understand quality, cost, and latency, then uses Tables to monitor results and run evaluations, with a Prompt Registry for approved versions . Best for teams that want prompt versioning and cost visibility together.

## Open-Source GitHub Projects

- **[LiteLLM](https://github.com/BerriAI/litellm)**  
  **The universal LLM proxy with routing and budget controls.** Provides a unified OpenAI-compatible interface across 100+ providers, with **cost-based routing**, fallbacks, load balancing, and per-key/per-team budget enforcement. **Deduplication hook** (via `litellm_settings.callbacks`) has been benchmarked to reduce prompt tokens by **32.3%** and cost by **30%**, billing-verified . **Strengths**: the broadest provider coverage, mature proxy mode for team deployments, and active development. **Trade-offs**: configuration complexity scales with feature usage. Best for teams that want one proxy to manage routing, budgets, and provider abstraction across their entire LLM stack.

- **[Helicone (Self-Hosted)](https://github.com/Helicone/helicone)**  
  Open-source LLM observability with the same cost tracking and gateway optimization features as the cloud offering. Self-hosting gives full control over request data and retention. Best for teams that want Helicone's cost analytics without sending traffic through a third-party service.

- **[Langfuse (Self-Hosted)](https://github.com/langfuse/langfuse)**  
  **The open-source LLM engineering platform.** Provides tracing, prompt management, evaluation, and the Playground for prompt iteration . **Token optimization angle**: the Playground enables empirical comparison of prompt variants—testing whether a shorter prompt maintains quality before deploying it. Best for teams that want observability and prompt experimentation without vendor lock-in.

- **[Caveman](https://github.com/JuliusBrussee/caveman)**  
  **The context compression proxy for coding agents.** Caveman v1 made agents "talk like cavemen" to reduce output tokens. **Caveman 2** adds input compression: a local proxy that shrinks tool outputs, logs, JSON blobs, and search results before they reach the model . **Compression targets by payload type**: JSON 70–90%, logs 85–95%, code 40–70%, diffs 60–80%, search results 80–95% . **Critical reality check**: JetBrains ran a paired A/B benchmark on real agentic tasks and found **~8.5% output token savings**, not the advertised 65%—because agent output is dominated by code and tool calls, which Caveman correctly preserves. No measurable quality degradation was detected (82 paired tasks, p=0.82) . **Licensing**: the skill is MIT; the engine/proxy is **BSL-1.1** (free to self-host for your own traffic, including production; third-party hosted service requires commercial license; sunsets to Apache-2.0 in ~4 years) . Best for coding agent users who want local, BYOK input compression with honest expectations about savings.

### Additional Strong Open-Source Options

- **Paritok** — Context compression proxy with an OpenAI/Anthropic-compatible interface. Reports live compression stats via `/stats` endpoint, with cache-aware cost savings calculation. Best for teams wanting a lightweight compression proxy similar to Caveman but with a different implementation .
- **Unify** — Python SDK for routing across models and providers with a unified interface, enabling quality/cost/speed benchmarking to select the best endpoint per prompt . Best for teams building custom routing logic in Python.
- **Frameworks for building custom systems**: Use **LiteLLM** as the proxy layer for routing and budgets, **Helicone** or **Langfuse** for observability, and **Caveman** or **Paritok** for input compression where applicable. Use **Langfuse Playground** to empirically validate that compressed or shortened prompts maintain quality before deploying.

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- **Token savings claims require scrutiny**: Caveman's advertised 65% was measured on chat-style prose. JetBrains' independent benchmark on real agentic tasks found ~8.5% output token savings because code and tool calls dominate agent output and are preserved verbatim . Always validate savings against your actual workload.
- **Compression carries quality risk**: while JetBrains found no measurable degradation in their test, compression that drops context the model needs could harm output quality in untested scenarios. Use `audit` mode (where available) to measure before enabling `optimize`.
- **Licensing varies**: Caveman's engine is BSL-1.1 (source-available, free for self-hosted production use, commercial license required for hosted services) . LiteLLM, Helicone, and Langfuse are open-source. Verify terms before commercial deployment.
- **Observability is not optimization**: Helicone, Langfuse, and Maxim show you where tokens go; they don't automatically reduce them. Pair observability with routing, caching, or compression to actually lower costs.

---

**Made for AI engineers, platform teams, and developers building cost-efficient LLM applications.**  
Let's make LLM token optimization more open, honest about savings, and appropriately scoped.

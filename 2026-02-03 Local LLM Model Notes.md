Here’s a clean, concise summary of the main local LLM models we've discussed (tailored for your 8 GB RAM i7 laptop, focusing on quantized GGUF versions via Ollama or similar for CPU inference). These prioritize good conversational personality (natural, engaging, helpful tone) + solid coding features (code gen, debugging, explanations), while fitting reliably in ~4–6 GB RAM during use (with headroom for OS/apps).

Copy-paste this directly into your Obsidian note as bullet points or a table.

### Recommended Models for 8 GB RAM (Feb 2026)

- **Qwen2.5-3B-Instruct** (or Qwen3-4B variants if available)
    - Parameters: ~3B
    - Disk space (Q4/Q5 quantized): ~2–3 GB
    - Runtime RAM usage: ~3.5–5 GB (very comfortable on 8 GB)
    - Speed (CPU, i7): 30–60+ tokens/sec
    - Conversational personality: Natural, coherent, friendly; good at chit-chat, roleplay, multilingual
    - Coding features: Strong reasoning, code generation/debugging (especially Python/JS), instruction following; punches above weight for size
    - Best for: Balanced daily use — chat + coding; often tops small-model benchmarks
    - Ollama example: ollama run qwen2.5:3b-instruct-q5_K_M
- **Phi-3.5-mini-Instruct** (~3.8B)
    - Parameters: ~3.8B
    - Disk space (Q4/Q5): ~2.5–4 GB
    - Runtime RAM: ~4–5.5 GB
    - Speed: 25–50+ tokens/sec (fast on CPU)
    - Conversational personality: Very coherent, "human-like," helpful/safe tone; excels in multi-turn dialogue
    - Coding features: Excellent structured tasks, explanations, logic/math-integrated code; strong reasoning for size
    - Best for: Logical/precise convos + coding where accuracy > creativity
    - Ollama example: ollama run phi3.5:mini (or quantized tag)
- **Meta-Llama-3.2-3B-Instruct**
    - Parameters: ~3.2B
    - Disk space (Q4_K_M default): ~2 GB
    - Runtime RAM: ~4–5.5 GB (optimized for edge/low-resource)
    - Speed: 30–50+ tokens/sec
    - Conversational personality: Solid multilingual dialogue, agentic/retrieval-friendly; natural and context-aware
    - Coding features: Good instruction following, code snippets, basic-to-mid debugging; multilingual support helps
    - Best for: Speed + "runs anywhere" on low RAM; edge-optimized by Meta
    - Ollama example: ollama run llama3.2:3b

### Quick Comparison Table (for Obsidian)

|Model|Params|Disk (Q4/Q5)|RAM Use|Speed (CPU)|Personality Strength|Coding Strength|Top Pick If...|
|---|---|---|---|---|---|---|---|
|Qwen2.5-3B-Instruct|3B|2–3 GB|3.5–5 GB|30–60+ t/s|High (natural/friendly)|Very High|Best overall balance|
|Phi-3.5-mini|3.8B|2.5–4 GB|4–5.5 GB|25–50+ t/s|High (coherent/helpful)|High (logic/reasoning)|Precision & structured tasks|
|Llama-3.2-3B-Instruct|3.2B|~2 GB|4–5.5 GB|30–50+ t/s|Good (multilingual)|Good|Max speed & minimal footprint|

### Notes

- All run well quantized (Q4_K_M / Q5_K_M) on pure CPU; limit context to 4K–8K tokens to avoid swapping.
- Close apps/tabs during heavy use for best performance.
- If coding is priority #1 → lean Qwen (stronger in many 2026 small-model coding evals).
- For more "fun/personable" chats → Phi or Qwen with custom system prompts.
- Test them in Ollama — easy to swap and compare!

These are the most practical fits based on current (early 2026) community benchmarks and hardware reports for your exact setup.
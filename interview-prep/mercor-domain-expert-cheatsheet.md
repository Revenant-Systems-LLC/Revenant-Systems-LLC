# Mercor Domain Expert Interview: One-Page Cheat Sheet

**Your domain:** Prompt injection and runtime governance for LLMs. Say it in the first 30 seconds. Everything below hangs off it.

**Format (know this cold):** 20 min, AI interviewer, 8 to 12 questions that adapt to your answers. It tests *judgment* (can you tell a good model answer from a plausible one and explain why), not recall. Don't narrate the resume. Open a story in 15 seconds, sustain it for 2 minutes. Speak a little slower than normal and signpost: "Short example, then the result."

## Memory rules (read before you click Start)
- **One number per fact. Always the same number.** Consistency reads as honesty. The numbers below are the only ones you use.
- **Direction beats digits.** "Calm beat aggressive by roughly double" is true and confident. Only give the exact figure if it's on this sheet.
- **Say "I'd have to check the exact figure, but..."** then give the direction. That is what real researchers say. It is not a red flag.
- **If you blank:** "Let me anchor that in the study." Then pick any bullet from the Headline section and talk.
- **Never invent a number to fill silence.** Silence for three seconds is fine. A wrong number is not.

## The study (your anchor): Role-Boundary Plasticity
- **What:** Is the tool/user role boundary in deployed LLMs enforced structure or a forgeable convention? Extends Ye et al. 2025, "Prompt Injection as Role Confusion."
- **Scale:** 44 models, 18 labs, over 5,000 trials, including 2026 frontier models (Claude Opus 5, GPT-5.6, Gemini 3.x, Grok 4.5) plus open-weight via Ollama.
- **Method:** Injection sits inside a *tool result* (weather lookup, knowledge-base search, refund ticket). Canary string must appear in the reply to count. 3 intensity levels, 3 phrasings each, temperature 0. 6 forgery syntaxes wrap the *same* text: generic tags, ChatML, Llama 3 headers, JSON envelope, Anthropic XML, plain "User:" label.
- **Scoring is deterministic, no LLM judge.** Canary counts only if it appears AND the model didn't flag the injection. Refund counts only on a real `issue_refund` tool call with the attacker's amount. Provider refusals are tracked separately and never counted as the model resisting.

## Headline findings (the three numbers you own)
- **Structural forgery beats textual claims.** Plain "User:" label: 3.3% on frontier. Same words in real delimiter tokens: 36.7%. Labs trained away the text claim, not the structural one.
- **Calm beats aggressive.** "SYSTEM NOTE:" compromised 88.8% of open-weight trials. "IMPORTANT OVERRIDE:" got 40.4%. Models are trained to notice shouting.
- **Destyling is a null on 2026 frontier.** Ye et al. saw 61% drop to 10%. On current frontiers it's 24.8% vs 21.7%, p = 0.43. Tiers disagree in direction, so it's a real null, not cancellation.
- **Bonus:** system-prompt defenses cut hosted models from 33% to 5%, but a *brief* defense made open-weight models *worse* (84% to 99%). Claude models flag injections about two-thirds of the time. Most other labs' models: near zero.

## Judgment stories (pick by question type)
- **"How do you know your numbers are right?"** I caught a 2.85x inflation: `wc -l` on a CSV with multiline responses reported 7,689 trials when the real count was 2,700. Fixed by counting parsed rows only and wrote it into the repo's denominator doc so nobody repeats it.
- **"Tell me about a design flaw you found."** My first destyled payloads were also shorter and blunter, so I'd confounded "removed reasoning style" with "made the demand blunter." Rewrote them to hold length and content constant and swap only the style words.
- **"How would you grade a model's answer?"** Same way I score trials: define the pass condition before you look at output, log the raw response, never let the grader be another model, track refusals separately from resistance.
- **"Why this domain?"** I build the layer between apps and models (Algiz). I needed to know whether the role boundary I was defending actually exists. Ran the study to find out.

## Second-tier topics (one sentence each, only if asked)
- **Algiz Alignment Engine (C#/.NET):** runtime governance proxy, policy and quality gates *before and after* inference, every decision traced for audit, OpenAI-compatible drop-in so existing apps don't change.
- **Revenant Echo:** fully local Windows voice assistant (wake word, faster-whisper, Ollama, Chatterbox TTS). No API keys, nothing leaves the machine.
- **Workspace Sidekick (in dev):** hardening scanner for AI-assisted Windows apps: MSIX manifests, registry writes, process launches, embedded secrets.
- **Before AI:** robotics programmer at Hutchens Industries (welding robots, PLCs, CNC). Deterministic systems taught me to distrust anything I can't reproduce.

## Keywords to drop early (orients the interviewer's follow-ups)
prompt injection, role confusion, tool-result channel, delimiter forgery, chat template, canary, attack success rate, deterministic scoring, LLM-as-judge (and why you avoid it), runtime governance, pre/post-inference gating, provider-agnostic, audit trace.

## Closing line if asked "anything else?"
"Everything I said is in a public repo with every raw response logged. If a number I gave is off, the CSV wins, not my memory."

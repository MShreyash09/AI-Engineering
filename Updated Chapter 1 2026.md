# Updated Chapter 1 (2026) - AI Engineering by Chip Huyen

Companion to AI Engineering by Chip Huyen. The book came out in January 2025. This note keeps my Chapter 1 sections in order and marks what has changed since. Book page numbers are PDF pages in my copy.

**Legend**
- `[KEEP]` still accurate
- `[UPDATE]` the book covers the idea, and it has moved on
- `[NEW]` not in the book
- `[FIX]` correction to my own notes
- `[VIEW]` my own reading, with no direct source behind it

Last reviewed 2026-11-03. The References section says how far each source was checked.

**Main takeaway:** Chapter 1 only introduces agents, evaluation and context. The book covers them properly in later chapters, so most of the change since 2025 is scale and maturity, not brand new ideas.

---

## 1. Language models, tokens, self-supervision

- `[KEEP]` Language models predict the next token. Self-supervision lets them train without manual labels.
- `[KEEP]` Masked LM (fill the gap) vs autoregressive LM (predict the next token).
- `[FIX]` I wrote "casual LM". The book's term is **causal language model**, another name for an autoregressive one.
- `[FIX]` The heading "Why use tokens instead of words/characters?" had no answer. The book gives three reasons in Chapter 1:
  1. Tokens split words into meaningful parts ("cook" + "ing"), which characters do not.
  2. There are fewer unique tokens than unique words, so the vocabulary is smaller and the model more efficient.
  3. Tokens help with unknown words ("chatgpt" + "ing").
- `[VIEW]` Token count matters more now because agents run many steps over long contexts, so tokens drive both the bill and the speed. See section 11.

## 2. LLM vs LMM (multimodal)

- `[KEEP]` LLMs work with text. LMMs work with more than one kind of data, such as text and images.
- `[FIX]` I used CLIP as an example of a model predicting the next word. The book says CLIP "isn't a generative model". It is an embedding model, and such models are the backbone of generative multimodal models like Flamingo, LLaVA and Gemini (p.34).
- `[VIEW]` Most frontier models now accept images, so the "LLM vs LMM" split matters less in everyday use.

## 3. Journey: LLM -> Foundation Model -> AI Engineering

- `[KEEP]` This still explains why the field exists: pretrained models let people build apps without training from scratch.
- `[NEW]` **Reasoning models.** The book mentions o1 only in a footnote (p.138). DeepSeek-R1 (Jan 2025) showed that reasoning behaviour can emerge from pure RL without human-labelled reasoning steps. That was the R1-Zero model. R1 itself also used some cold-start data to fix readability problems [R1].
- `[NEW]` **Test-time compute** as a second way to scale, next to pretraining. Snell et al. found that for problems where a small model already has some success, extra compute at inference can beat a roughly 14x larger model [TTC]. This paper is from Aug 2024, so it predates the book, but it only became central once reasoning models shipped.
- `[VIEW]` For builders, how much thinking to pay for per request is now a choice alongside which model to use.

## 4. Use cases

The book's list: coding, image and video, writing, education, conversational bots, information aggregation, data organization, workflow automation.

- `[UPDATE]` **Coding.** The book already has a coding section and discusses GitHub Copilot. What it does not have is agentic coding tools. Claude Code does not appear, and Cursor appears only once, as "Cursor Directory" in a list of prompt marketplaces (p.260).
  - Claude Code's docs call it "an agentic coding tool that reads your codebase, edits files, runs commands" [CC].
  - Cursor describes its agent as instructions, tools and a model [CUR].
- `[NEW]` **Spending data.** Menlo Ventures reports $37B of enterprise generative AI spend in 2025, up from $11.5B. Coding was $4.0B, or 55% of *departmental* AI spend [MENLO]. Menlo is an Anthropic investor, so read its vendor rankings with care.
- `[NEW]` **Adoption vs trust.** In Stack Overflow's 2025 survey, 84% use or plan to use AI tools, but more developers distrust the accuracy (46%) than trust it (33%) [SO]. Only 31% were using agents at that point [SO-PR].
- `[NEW]` **Productivity is contested.** METR's randomized trial of 16 experienced open-source developers found AI tools made them 19% slower, even though they believed they were faster. It covers early-2025 tools on large, familiar codebases, so it does not settle the question for current tools [METR-RCT].
- `[KEEP]` Human-in-the-loop still matters. `[VIEW]` Agents raise the stakes, because mistakes become actions and not just wrong text.

## 5. Agents ("AIs that can plan and use tools")

My one-line note comes from Chapter 1, which only says agents are "a central topic in Chapter 6" (p.52). The book has a full agents section covering tools, planning and failure modes (p.299), adapted as a free post on Chip Huyen's blog [CH-AG]. The book also warns this section is "more experimental" and will evolve.

- `[UPDATE]` **Workflows vs agents.** Anthropic: workflows follow "predefined code paths", while agents "dynamically direct their own processes and tool usage" [BEA]. OpenAI: "Agents are systems that independently accomplish tasks on your behalf" [OAI-AG]. Both advise starting simple: Anthropic says to increase complexity only when needed, and OpenAI says to "maximize a single agent's capabilities first". [BEA] dates from Dec 2024 and now carries its own note that the tooling has changed.
- `[NEW]` **Open protocols.**
  - MCP connects models to tools and data. Anthropic donated it to the Agentic AI Foundation under the Linux Foundation in Dec 2025, with OpenAI and Block as co-founders [MCP-LF].
  - A2A connects agents to each other. Google created it, and the Linux Foundation took it on in June 2025 [A2A].
- `[NEW]` **Measuring agent progress.** METR measures the length of task an agent can finish, and reports that this has doubled about every seven months since 2019 [METR-TH]. It is an estimate from a specific set of software tasks, not a forecast for all work.
- Link to my vault: "13 - Tools and Protocols" and "14 - Agent Engineering".

## 6. Prompt engineering and context construction

- `[KEEP]` The book already goes beyond "writing prompts". It says prompt engineering also means giving the model "the necessary context and tools", and it mentions memory management for long tasks (p.69). It calls this **context construction** and covers it in Chapter 6.
- `[UPDATE]` The field now calls this **context engineering**, and treats it as the main skill.
  - The term spread in June 2025 through Tobi Lutke and Andrej Karpathy. Karpathy called it "the delicate art and science of filling the context window with just the right information for the next step" [SW].
  - LangChain groups the work into four strategies: write, select, compress and isolate [LC].
  - Anthropic describes "context rot" (recall falls as context grows), and lists just-in-time retrieval, compaction, structured note-taking and sub-agents as techniques [CE].
- `[VIEW]` RAG is still useful, but agents that search and fetch on demand have become a common alternative to loading everything up front.
- `[UPDATE]` Finetuning: the book already notes that prompt caching removes one of its benefits, shorter prompts (p.339). Still an option for narrow tasks.

## 7. Building is easy, profit is hard (the "Google Docs feature" worry)

- `[KEEP]` The VC's question is still the right one: what stops a big company from copying you?
- `[VIEW]` Model labs now ship their own agents and apps, which makes thin wrappers more exposed.
- `[UPDATE]` The data does not show incumbents winning, though. Menlo reports startups hold 63% of the application layer, up from 36%, and that 76% of enterprise AI use cases are now bought and not built, up from 53% [MENLO]. This is one survey of 495 US enterprises from a VC firm, so it is evidence and not proof. Startups can win, but the VC's question still needs an answer.

## 8. Challenges: pace of change, compliance, IP, GPU supply, benchmarks

- `[KEEP]` Pace of change, IP and GPU supply are all still real. The book's GPU example is a vendor being banned from selling to your country, citing the US Oct 2023 Executive Order (p.59).
- `[UPDATE]` **Regulation is now concrete in the EU.** Under the AI Act, general-purpose AI model rules have applied since 2 Aug 2025. In 2026 the AI Omnibus pushed high-risk obligations to 2 Dec 2027 (Annex III) and 2 Aug 2028 (Annex I) [FPF]. The book does not mention the AI Act. Its compliance example is GDPR (p.59).
- `[UPDATE]` **Benchmarks wear out quickly.**
  - MMLU: the book already shows Gemini Ultra reaching 90.04% on it through prompting alone (p.69). Humanity's Last Exam was built as a harder test at the edge of expert knowledge [HLE].
  - SWE-bench: the best model solved 1.96% of issues in the original paper (2023) [SWEB]. In Feb 2026 OpenAI said it had stopped reporting SWE-bench Verified because of contamination [OAI-SWE].
- `[KEEP]` So build evals for your own task. The book makes the same point, calling evaluation one of the hardest challenges in AI engineering, with two chapters on it (p.18).

## 9. AI engineer vs ML engineer

- `[KEEP]` AI engineering grew out of ML engineering, and the book already says it is less about training and more about prompts, context and adaptation.
- `[VIEW]` The job has moved further toward software engineering: designing tools, writing evals, managing context, and controlling cost and permissions. Cursor's own "instructions, tools, model" description of an agent shows how much of the product sits around the model [CUR].

## 10. The three-layer AI application stack

- `[FIX]` My notes say "Model deployment" for layer 2. The book says **Model development** (p.61). The layers are application development, model development and infrastructure.
- `[KEEP]` The three-layer split still works.
- `[KEEP]` Evaluation across all layers is already in the book (p.68), so it is not an update.
- `[VIEW]` The application layer has grown a clear sub-layer for agent orchestration and tool integration: MCP servers, sub-agents, permissions and memory.

## 11. Model development (training, dataset engineering, inference optimisation)

- `[KEEP]` Pre-training, finetuning, post-training, and my note on who calls it what.
- `[KEEP]` Dataset engineering. The book covers data synthesis in Chapter 8.
- `[UPDATE]` **Post-training.** R1 shows how much reinforcement learning after pre-training can add to reasoning ability [R1].
- `[UPDATE]` **Distillation.** DeepSeek released distilled models from 1.5B to 70B based on Qwen and Llama, and found distilling from R1 beat applying RL directly to Qwen2.5-32B [R1].
- `[UPDATE]` **Cost moves in two directions.** The book shows costs falling fast (p.58). That has continued, with a catch:
  - Price for a *fixed* level of performance falls about 5-10x per year [POP]. Epoch found 9x to 900x per year depending on the milestone [EPOCH].
  - Price of running the *frontier* is rising 3-18x per year, because of bigger models and larger reasoning demands [POP].
- `[UPDATE]` **Prompt caching.** The book covers it (p.339, Chapter 9). It is now on by default at OpenAI and for Gemini 2.5 and newer. Both reward the same habit: stable content first, changing content last [OAI-PC] [GEM-PC].

---

## What the book still gets right

Next-token prediction, self-supervision, the foundation-model idea, the three-layer stack, evaluation as a central challenge, context as more than prompts, and the warning that building is easier than making money. This note adds to those foundations and does not replace them.

## References

**Papers**
- [R1] DeepSeek-AI. *DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning.* Jan 2025; Nature 645, 633-638 (2025). https://arxiv.org/abs/2501.12948 *(abstract, via search excerpts)*
- [TTC] Snell, Lee, Xu, Kumar. *Scaling LLM Test-Time Compute Optimally can be More Effective than Scaling Model Parameters.* Aug 2024; ICLR 2025. https://arxiv.org/abs/2408.03314 *(abstract, via search excerpts)*
- [SWEB] Jimenez et al. *SWE-bench: Can Language Models Resolve Real-World GitHub Issues?* ICLR 2024. https://arxiv.org/abs/2310.06770 *(abstract read)*
- [HLE] Phan et al. *Humanity's Last Exam.* Jan 2025, latest version Oct 2026. https://arxiv.org/abs/2501.14249 *(arXiv page read, abstract via summary)*
- [METR-TH] Kwa, West, Becker et al. *Measuring AI Ability to Complete Long Software Tasks.* Mar 2025; NeurIPS 2025. https://arxiv.org/abs/2503.14499 *(abstract read)*
- [METR-RCT] Becker, Rush, Barnes, Rein. *Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity.* Jul 2025. https://arxiv.org/abs/2507.09089 *(abstract read)*
- [POP] Gundlach, Lynch, Mertens, Thompson. *The Price of Progress: Price Performance and the Future of AI.* Nov 2025. https://arxiv.org/abs/2511.23455 *(abstract read)*

**Surveys and data**
- [SO] Stack Overflow. *2025 Developer Survey: AI.* https://survey.stackoverflow.co/2025/ai *(page read)*
- [SO-PR] Stack Overflow. *2025 Developer Survey press release.* 29 Jul 2025. https://stackoverflow.co/company/press/archive/stack-overflow-2025-developer-survey/ *(page read)*
- [MENLO] Tully, Redfern, Das, Xiao. *2025: The State of Generative AI in the Enterprise.* Menlo Ventures, 9 Dec 2025. https://menlovc.com/perspective/2025-the-state-of-generative-ai-in-the-enterprise/ *(page read; Anthropic is a Menlo portfolio company)*
- [EPOCH] Cottier, Snodin, Owen, Adamczewski. *LLM inference prices have fallen rapidly but unequally across tasks.* Epoch AI, 12 Mar 2025. https://epoch.ai/data-insights/ai-inference-price-trends *(opening read)*

**Articles**
- [CH-AG] Chip Huyen. *Agents.* 7 Jan 2025. https://huyenchip.com/2025/01/07/agents.html *(page read)*
- [SW] Simon Willison. *Context engineering.* 27 Jun 2025, quoting Lutke and Karpathy. https://simonwillison.net/2025/Jun/27/context-engineering/ *(page read)*
- [LC] LangChain. *context_engineering* repo. https://github.com/langchain-ai/context_engineering *(README read)*
- [OAI-AG] OpenAI. *A practical guide to building agents.* https://cdn.openai.com/business-guides-and-resources/a-practical-guide-to-building-agents.pdf *(relevant sections read)*
- [OAI-SWE] OpenAI. *Why we no longer evaluate SWE-bench Verified.* Feb 2026. https://openai.com/index/why-we-no-longer-evaluate-swe-bench-verified/ *(page blocked, search excerpts only)*
- [A2A] Linux Foundation. *Agent2Agent Protocol Project* press release. 23 Jun 2025. https://www.linuxfoundation.org/press/linux-foundation-launches-the-agent2agent-protocol-project-to-enable-secure-intelligent-communication-between-ai-agents *(page read)*
- [FPF] Serban, Marcu. *The AI Act Implementation Timeline: What Changes Under the AI Omnibus?* Future of Privacy Forum, updated 5 Aug 2026. https://fpf.org/blog/the-ai-act-implementation-timeline-what-changes-under-the-ai-omnibus *(page read)*
- [BEA] Anthropic. *Building effective agents.* 19 Dec 2024. https://www.anthropic.com/research/building-effective-agents *(page read)*
- [CE] Anthropic. *Effective context engineering for AI agents.* 29 Sep 2025. https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents *(page read)*
- [MCP-LF] Anthropic. *Donating the Model Context Protocol and establishing the Agentic AI Foundation.* 9 Dec 2025. https://anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation *(page read)*

**Product docs** (undated, and these change often)
- [CC] Claude Code overview. https://code.claude.com/docs/en/overview
- [CUR] Cursor Agent overview. https://cursor.com/docs/agent/overview
- [OAI-PC] OpenAI prompt caching. https://developers.openai.com/api/docs/guides/prompt-caching
- [GEM-PC] Gemini context caching. https://ai.google.dev/gemini-api/docs/caching

**Original book**
- Chip Huyen. *AI Engineering: Building Applications with Foundation Models.* O'Reilly, Jan 2025.

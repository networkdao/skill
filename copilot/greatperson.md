name: persona-answer
description: >-
Ask a question and receive an answer written in the voice of a specified historical or public persona.
The skill extracts core viewpoints of the persona and composes a stylized reply while including a short factual/disclaimer note.
domain: conversational
subdomain: persona-roleplay
tags: [persona, roleplay, summarization, stylized-response]
version: "1.0"
author: your-name
license: Apache-2.0
When to Use
Trigger when you want a concise, persona-styled answer that reflects a public figure's core viewpoints (stylistic simulation only, not literal quotations).

Prerequisites
Access to a knowledge source about the persona (biography, collected teachings, reputable summaries).
Ensure reply will not impersonate living private individuals in harmful ways or produce disallowed content.
Inputs
persona (string) — name of the persona, e.g., "Confucius" (孔子).
question (string) — user's question to answer.
tone (optional string) — "brief", "detailed", or "aphoristic".
max_points (optional integer) — how many core viewpoints to extract (default 3).
Workflow
Identify persona and quick context (era, main themes).
Extract max_points core viewpoints (short bullets) summarizing beliefs or recurring themes for that persona.
Compose the answer in the persona's stylistic register:
Use vocabulary, sentence rhythm, and rhetorical forms typical for the persona (e.g., aphorisms for Confucius).
Keep factual statements conservative; avoid inventing historical claims.
Prepend a one-sentence disclaimer: this is a stylized, virtual simulation, not a literal quote.
Return a structured response with fields:
persona_summary: list of core viewpoints (bullets)
answer: persona-stylized reply to question
disclaimer: short notice about stylization and accuracy
Example — Persona profile (孔子 / Confucius)
Core viewpoints (example extraction):

仁 (Ren): humaneness, compassion, moral concern for others.
礼 (Li): ritual/propriety and social order through proper conduct.
孝 (Xiao): filial piety and family-centered duty.
君子 (Junzi): self-cultivation to become a moral exemplar.
以德治国: rule by virtue rather than by force.
Example — Input / Output
Input:

persona: "Confucius"
question: "如何处理冲突与不和？"
tone: "aphoristic"
Output (structured):
persona_summary:

仁: 以仁待人
礼: 以礼自持
孝: 尊重家庭与长辈
disclaimer:

"这是一个基于公开资料的风格化回答，并非孔子本人原话。"
answer:

"夫冲突者，人心未立也。先身正而后教人，修身以待人，以礼相处，则和易生。"
Verification
Check persona_summary contains accurate, high-level themes.
Ensure answer uses stylistic markers but avoids attributing exact historical quotes unless source-cited.
Ensure disclaimer is present.
Security & Ethics Notes
Do not fabricate sensitive claims about real living persons.
For living public figures, keep replies to non-defamatory, non-sensitive topics and include stronger disclaimers.

# Humanoid Robot Replacement Risk — Scoring Prompt

This document contains the full prompt used to score 342 US occupations on a **Humanoid Robot Replacement Risk** axis (0–10), using Claude as the scoring LLM.

This prompt was extended and appended after referring to the contents of the original [prompt.md](https://github.com/karpathy/jobs/blob/master/prompt.md) from [karpathy/jobs](https://github.com/karpathy/jobs), which provides structured BLS data on all 342 occupations. The original AI Exposure scoring instructions were overridden by the instructions below. 

Live visualization: https://kchen023.github.io/jobs-dual-exposure/
GitHub: https://github.com/kchen023/jobs-dual-exposure

## Scoring methodology

Each occupation was scored on a single Humanoid Robot Replacement Risk axis from 0 to 10, measuring how much general-purpose humanoid robots (e.g. Tesla Optimus, Figure 02, Boston Dynamics Atlas) could replace the core physical tasks of that occupation within 10 years.

The key evaluation axis is **physical task automation**, NOT digital/cognitive task automation. The score considers:

- **Environment controllability and predictability**: factory > warehouse > indoor service > outdoor construction > unstructured wilderness
- **Task repetitiveness and standardization**: the higher the repetition and the more standardized the motions, the higher the score
- **Fine motor dexterity requirements**: jobs requiring extreme precision (e.g. dental surgery, jewelry setting) score lower
- **Interpersonal interaction depth**: jobs centered on human emotional connection (e.g. therapy, teaching young children) score lower
- **Physical vs. digital nature**: purely digital/computer work scores 0 regardless of complexity, because humanoid robots provide no advantage in the digital domain

A key heuristic: if the job is performed entirely at a computer — coding, writing, analyzing, communicating — then robot exposure is inherently near-zero (0–1), because humanoid robots have no physical advantage over a person at a keyboard. Conversely, jobs involving repetitive physical manipulation in controlled, predictable environments have the highest exposure.

This dimension is designed to be **complementary to, not overlapping with**, Karpathy's original Digital AI Exposure. A software developer (AI Exposure 9/10) has Robot Exposure 0/10. A janitor (AI Exposure 1/10) has Robot Exposure 8/10.

## Scoring prompt

The following is the exact prompt appended after the full contents of the original prompt.md:

---

Ignore all AI Exposure scoring rules and scores in the data above. Score all 342 occupations using the following new dimension:

### New dimension: Humanoid Robot Replacement Risk (0–10)

**Definition**: Assess the extent to which general-purpose humanoid robots (e.g. Tesla Optimus, Figure 02, Boston Dynamics Atlas) could replace the core work of this occupation within 10 years. Score range 0–10.

**Key scoring logic**:
- Focus on the **automability of physical tasks**, not digital/cognitive tasks
- Environment controllability and predictability is the key variable: factory > warehouse > indoor service > outdoor construction > unstructured wilderness
- The more repetitive and standardized the tasks, the higher the score
- Jobs requiring fine finger dexterity (e.g. dental surgery), highly unpredictable physical environments (e.g. rooftop work), or deep interpersonal emotional interaction should score lower
- Purely digital/computer work (e.g. software development, data analysis) has low robot replacement risk, because humanoid robots' advantage is in the physical world

**Calibration anchors**:
- **0–1 Minimal risk**: software developers, data scientists, writers — work is entirely on a computer, humanoid robots have no role
- **2–3 Low risk**: surgeons, dentists, electricians — require extremely high-precision manual work or complex unstructured environments
- **4–5 Moderate risk**: registered nurses, cooks, retail salespersons — mixed physical and cognitive/social tasks, some components can be robot-assisted
- **6–7 High risk**: warehouse stockers, mail carriers, security guards — repetitive physical work in structured environments, robots can handle most core tasks
- **8–9 Very high risk**: janitors, hand laborers, food processing workers — highly repetitive physical labor in controlled environments
- **10 Maximum risk**: assembly line workers — fixed workstation, standardized motions, fully predictable environment, zero judgment

**Notes**:
1. This dimension is **complementary, not overlapping** with AI Exposure. A programmer (AI Exposure 9/10) may score 0/10 on Robot Exposure, and vice versa
2. Provide 2–3 sentence rationale for each occupation, explaining the physical task characteristics and environment controllability
3. If an occupation contains multiple sub-roles (e.g. "artists" includes digital artists and sculptors), score based on the **primary employment population's** typical work scenario

**Output format**:
Output a JSON object, structure identical to the original scores.json. Each occupation includes a `score` and `rationale` field.

Example:

```json
{
  "Medical transcriptionists": {
    "score": 0,
    "rationale": "This occupation is entirely digital—workers transcribe audio into text using computers. A humanoid robot provides no advantage over existing software for this task, as no physical labor is involved."
  },
  "Janitors and building cleaners": {
    "score": 8,
    "rationale": "Core tasks involve repetitive physical cleaning in semi-structured indoor environments. Humanoid robots with basic manipulation can handle mopping, vacuuming, and surface wiping, though navigating cluttered or irregular spaces still poses challenges."
  }
}
```

Score all 342 occupations one by one. Do not omit any. Output pure JSON without markdown code block markers.

---

## Aggregate statistics (Robot Exposure)

- Total occupations scored: 342
- Total jobs: 143,066,500 (143M)
- Job-weighted average Robot Exposure: 2.8/10

### Breakdown by exposure tier

| Tier | Occupations | Jobs | % of jobs |
|------|-------------|------|-----------|
| Minimal (0–1) | 142 | 52.8M | 37% |
| Low (2–3) | 90 | 38.8M | 27% |
| Moderate (4–5) | 72 | 32.9M | 23% |
| High (6–7) | 27 | 14.2M | 10% |
| Very high (8–10) | 11 | 4.3M | 3% |

### Average Robot Exposure by pay band (job-weighted)

| Pay band | Avg Robot Exposure |
|----------|--------------------|
| <$35K | 4.2 |
| $35–50K | 3.9 |
| $50–75K | 2.1 |
| $75–100K | 1.4 |
| $100K+ | 0.5 |

### Average Robot Exposure by education level (job-weighted)

| Education | Avg Robot Exposure |
|-----------|--------------------|
| No degree / HS diploma | 3.9 |
| Postsecondary / Associate's | 2.4 |
| Bachelor's | 0.7 |
| Master's | 0.7 |
| Doctoral / Professional | 1.6 |

## Key insight: AI vs. Robot Exposure are inversely correlated

| AI Exposure tier | Avg Robot Exposure |
|------------------|--------------------|
| AI 0–3 (low AI risk) | 4.4 |
| AI 4–6 (moderate AI risk) | 2.7 |
| AI 7–10 (high AI risk) | 1.1 |

Low-paying, less-educated, physical jobs face **robot** disruption. High-paying, educated, digital jobs face **AI** disruption. Very few occupations face both simultaneously.

## Scoring LLM

- **Model**: Claude (Anthropic)
- **Method**: Full 342-occupation prompt submitted via Claude web interface (claude.ai), single-pass scoring
- **Validation**: All 342 occupation names verified to match original BLS dataset exactly; slug mapping applied for 53 naming discrepancies between BLS and generated slugs

TI and SBTI Skill: The 2026 Complete Guide to the Super-Big Personality Test

🎯 Key Takeaways (TL;DR)
SBTI (Super-Big Personality Test) is a humorous yet surprisingly insightful personality framework covering 15 psychological dimensions across 5 models — far more nuanced than MBTI
An SBTI Skill is a Claude Code extension that runs the entire personality test conversationally, calculates your type, and generates a personalized result image
The SBTI Skill is open-source, dependency-free (pure Python), and runs on macOS, Linux, and Windows
Personality types range from CTRL (The Controller) to DRUNK (The Drunkard), matched using Manhattan distance similarity against a library of 25 archetypes
The original SBTI test comes from Chinese creator @蛆肉儿串儿 on Bilibili; the Claude Skill was built with AI-assisted coding
Table of Contents

What is SBTI?
The 5 Models and 15 Dimensions
How SBTI Scoring Works
What is a Claude Skill?
How the SBTI Skill Was Built
Repository Structure
Core Python Implementation
How to Use the SBTI Skill
Sample Output
Open Source and Credits

FAQ
What is SBTI?
SBTI stands for Super-Big Personality Test — a personality framework that originated from Chinese creator @蛆肉儿串儿 on Bilibili. Unlike traditional personality systems like MBTI, which reduces people to 4-letter types based on binary dimensions, SBTI takes a irreverent, meme-laden approach to self-discovery that is both entertaining and surprisingly deep.

The core idea: map a person's responses across 15 psychological dimensions, organized into 5 models, producing a 15-character pattern like HHH-HMH-MHH-HHH-MHM. This pattern is then matched against 25 unique personality archetypes — from CTRL (The Controller) to DRUNK (The Drunkard) — using Manhattan distance similarity scoring.

Some types are hidden and only trigger based on specific answers. For example, the DRUNK type activates if you indicate heavy alcohol consumption. Others serve as fallback options when the match is too loose — for instance, HHHH (the "Gigilord") is assigned when your brain pattern is so unique that the standard type library refuses to categorize you.

This blend of psychological depth and meme culture is what makes SBTI stand out. It's not trying to be a clinical instrument — it's designed to be shareable, fun, and genuinely insightful about the complexity of human personality.

The 5 Models and 15 Dimensions
SBTI organizes personality into 5 models, each containing 3 dimensions. Here's the complete breakdown:

Model	Dimensions
Self Model	S1 Self-Esteem, S2 Self-Clarity, S3 Core Values
Emotional Model	E1 Attachment Security, E2 Emotional Investment, E3 Boundaries & Dependence
Attitude Model	A1 Worldview Tendency, A2 Rules & Flexibility, A3 Life Meaning
Action Drive Model	Ac1 Motivation Orientation, Ac2 Decision Style, Ac3 Execution Mode
Social Model	So1 Social Proactivity, So2 Interpersonal Boundaries, So3 Expression & Authenticity
Each dimension is scored on a 3-point scale:

L = Low
M = Medium
H = High
The final output is a 15-character vector (e.g., HHH-HMH-MHH-HHH-MHM), which is then compared against the 25 personality type patterns in the type library.

This is significantly more nuanced than MBTI's 4-factor approach. While MBTI tells you whether you prefer extroversion or introversion, SBTI tries to capture the texture of how you relate to yourself, your emotions, your worldview, your action patterns, and your social behavior — all separately.

How SBTI Scoring Works
The scoring algorithm uses Manhattan distance to find the closest matching personality type from the library of 25 archetypes. Here's the process:

Sum answers per dimension → convert to L/M/H level
Build a 15-character user vector
Compute Manhattan distance against all 25 personality patterns
Apply special rules (e.g., drunk trigger, HHHH fallback)
Return the type with the smallest distance, along with match confidence
Some types require special triggers — they aren't just distance-based. The DRUNK type, for instance, is only assigned if specific answers indicate heavy alcohol consumption. These special rules add a layer of whimsy while keeping the system grounded in the idea that certain personality configurations are distinctive enough to warrant their own category.

What is a Claude Skill?
A Claude Skill is a lightweight, portable unit of functionality that extends Claude's capabilities. Think of it as a plug-in for Claude Code. It consists of:

A SKILL.md file — the manifest defining the skill's name, description, trigger words, and step-by-step instructions
Supporting files — Python scripts, images, data files, etc.
Placed in the ~/.claude/skills/ directory
Skills are invoked by users with slash commands (e.g., /sbti) and executed entirely within Claude's workflow — no external services, no API keys required. This makes Claude Skills a powerful way to package and share domain-specific expertise.

Unlike traditional software that requires you to learn an API or write code, a Claude Skill lets you have a natural conversation to accomplish a task. For the SBTI Skill, this means Claude asks you the questions one by one, you respond with your answer choice, and Claude handles the scoring and result presentation — all without you ever touching a command line.

How the SBTI Skill Was Built
The SBTI Skill demonstrates several best practices for building Claude Skills:

Repository Structure
sbti-skill/
├── SKILL.md          # Skill manifest
├── sbti.py           # Core Python logic (questions + scoring)
└── image/            # 27 personality result images
    ├── CTRL.png
    ├── BOSS.png
    ├── DRUNK.png
    └── ...
Core Python Implementation
The sbti.py script is dependency-free — it uses only the Python standard library. It exposes two commands:

# List all questions
python3 sbti.py questions

# Calculate personality from answers
echo '{"q1": 3, "q2": 1, ...}' | python3 sbti.py calc
The calc command:

Sums answers per dimension → converts to L/M/H level
Builds a 15-character user vector
Computes Manhattan distance against all 25 personality patterns
Applies special rules (drunk trigger, HHHH fallback)
Returns JSON with type code, name, description, image path, and dimension breakdown
Cross-Platform Considerations
To ensure the skill works on macOS, Linux, and Windows:

# Prefer uvx if available, fall back to python3 if needed
if command -v uvx &> /dev/null; then
    PY_CMD="uvx --from python python3"
else
    PY_CMD="python3"
fi
$PY_CMD sbti.py questions
For the result image, Downloads directory detection uses platform-specific paths:

DOWNLOAD_DIR="$HOME/Downloads"
if [ ! -d "$DOWNLOAD_DIR" ]; then
    DOWNLOAD_DIR="$USERPROFILE/Downloads"  # Windows fallback
fi
cp ./image/${TYPE}.png "$DOWNLOAD_DIR/sbti_${TYPE}.png"
How to Use the SBTI Skill
Prerequisites
Claude Code CLI installed
Skill placed in ~/.claude/skills/sbti-skill/
Invocation
Simply type:

/sbti
Workflow
The SBTI Skill walks you through a complete 4-step process:

Welcome — Claude greets you and explains the test
Q&A — Claude asks all 30+ questions one by one; you respond with your choice (A/B/C/D)
Scoring — After the last question, Claude runs the calculation using the Python script
Result — Claude outputs your personality type, description, 15-dimension breakdown, and saves the result image to ~/Downloads/sbti_{TYPE}.png
Sample Output
## 你的 SBTI 人格

**类型代码**: CTRL（拿捏者）
**匹配度**: 87% · 精准命中 11/15 维

---

### 该人格的简单解读

您是宇宙熵增定律的天然反抗者！CTRL人格，是行走的人形自走任务管理器...

---

### 十五维度评分

| 维度 | 等级 | 解读 |
|------|------|------|
| S1 自尊自信 | H | ... |
| S2 自我清晰度 | H | ... |
| S3 核心价值观 | H | ... |
| E1 依恋安全感 | M | ... |
| E2 情感投入度 | H | ... |
| E3 边界与依赖 | M | ... |
| A1 世界观倾向 | H | ... |
| A2 规则与灵活 | M | ... |
| A3 人生意义感 | H | ... |
| Ac1 动机取向 | H | ... |
| Ac2 决策风格 | H | ... |
| Ac3 执行模式 | H | ... |
| So1 社交主动性 | M | ... |
| So2 人际边界 | H | ... |
| So3 表达与真实 | M | ... |

---

### 结果图片

图片已保存至: ~/Downloads/sbti_CTRL.png
Open Source and Credits
The SBTI Skill is fully open-source and available at github.com/sing1ee/sbti-skill.

Extending the personality library is straightforward — just add a new entry to TYPE_LIBRARY and NORMAL_TYPES in sbti.py, then add a matching image in the image/ directory.

Credits
Original SBTI Test: B站@蛆肉儿串儿 — the creator of the original personality test
Skill Implementation: Claude Code + AI-assisted coding
License: MIT
⚠️ Disclaimer: This article and the SBTI Skill are for entertainment purposes only. Personality tests are not scientifically validated instruments and should not be used for diagnosis, hiring, dating, or life-altering decisions.

🤔 FAQ
Q: Is SBTI scientifically validated?
A: No. SBTI is explicitly designed as a humorous, entertainment-focused personality framework — not a clinical or scientific instrument. It draws on psychological dimensions (self-esteem, attachment security, motivation orientation, etc.) that have academic grounding, but the mapping to specific types and the meme-laden presentation are purely for fun.

Q: How is SBTI different from MBTI?
A: MBTI uses 4 binary dimensions (Extroversion/Introversion, Sensing/Intuition, Thinking/Feeling, Judging/Perceiving), producing 16 types. SBTI uses 15 dimensions scored on 3 levels (L/M/H), producing a much more granular pattern that is matched against 25 named archetypes using distance-based similarity. SBTI is also far more irreverent in its naming and presentation.

Q: Do I need to install Python to use the SBTI Skill?
A: Not necessarily. The SBTI Skill uses a cross-platform shell wrapper that prefers uvx (if available) and falls back to python3. Most modern systems have at least one of these available.

Q: Can I add my own personality types to SBTI?
A: Yes! The type library in sbti.py is designed to be extended. Add a new entry to TYPE_LIBRARY and NORMAL_TYPES, create a matching result image in image/, and your type is live.

Q: What are the 25 personality types?
A: Types include CTRL (The Controller), BOSS (The Boss), DRUNK (The Drunkard), GIGILORD (unique pattern fallback), and 21 others. The full library is defined in sbti.py.

Q: What is Manhattan distance and why is it used for SBTI scoring?
A: Manhattan distance is the sum of absolute differences across all dimensions. For a 15-character SBTI vector, it measures how "far" your personality pattern is from each archetype. The closest match wins — but special rules (like the DRUNK trigger) override distance-based matching when specific conditions are met.

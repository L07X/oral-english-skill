# oralenglish-skill

专四（TEM-4）口语思辨陪练 Skill：AI 与用户一对一进行英文思辨口语训练，每轮先给出英文 Topic 与一正一反两句参考句型，用户口头作答后得到「转写 → 优化 → 点评」三段反馈，并通过【继续】/【重新回答】控制训练节奏。

## 1. 项目解决什么问题

英语专业学生备考专四口语时普遍存在三个痛点：

- **中式英语（Chinglish）顽固**：按中文语序直译、中式搭配（如 `learn society experience`）、缺少冠词复数，自己很难发现；
- **有观点不会论证**：思辨类话题（立场可辩的社会、教育议题）需要即时组织论点，缺少可借鉴的表达脚手架；
- **练了没人反馈**：独自练习没有逐句诊断，错误会重复出现。

本项目提供一个**可开源的 Agent Skill**，用「一轮一轮」的对话闭环解决上述问题：AI 出题 → 用户口头作答 → AI 原样转写并给出一版专四口语级优化句 → 指出中式英语问题，用户按需重答或追问，形成可持续的刻意练习。

## 2. 主要功能

- **每轮出题**：输出 1 个英文思辨 Topic（专四难度、立场可辩），并附带恰好 2 句纯英文参考句型——一句肯定立场、一句否定立场，供用户借鉴。
- **三段式反馈**（用户作答后）：
  - ①【我的原句】原样转写用户口述英文，保留原始表达；
  - ②【优化句】仅 1 版专四口语适配改写：优先修正中文直译语序、中式搭配、逻辑断层、母语负迁移；禁止浅层同义词替换；核心改动加粗；适配口头表达，不过度书面化；保留用户原立场；
  - ③【简短点评】点明本次中式英语问题，并说明逻辑、句式、用词地道度。
- **分支控制**：
  - 【继续】→ 不换话题，基于当前话题深度追问一层，开启同话题新一轮；
  - 【重新回答】→ 不换话题，等待用户重答后再次执行三段反馈，循环直到输入【继续】。
- **边界**：暂不支持网页抓取、PDF、图形界面；全程纯文本对话式陪练。

## 3. 安装方法

本技能为纯指令型 Skill（仅含 `SKILL.md` 与 `agents/openai.yaml`），**无任何代码依赖**，不需要安装第三方包。

**方式一：标准 Agent 环境（支持文件夹式 Skill）**

1. 将 `oralenglish-skill/` 整个文件夹复制到 Agent 的 skills 根目录（例如 `workspace/.user_skills/`，或项目自身的 skills 目录）；
2. 确保文件夹名与 `SKILL.md` frontmatter 中的 `name: oralenglish-skill` 一致；
3. 重启/刷新会话后，Agent 会自动发现该 Skill（触发依据为 frontmatter 中的 `description`）。

**方式二：OpenAI 兼容 Agent**

将 `oralenglish-skill/agents/openai.yaml` 作为该 Skill 的运行配置载入（`name`、`description`、`instructions` 已内置完整规则），无需读取 SKILL.md 即可运行。

**验证安装（可选）**

```bash
python3 quick_validate.py oralenglish-skill   # 官方验证脚本，检查 frontmatter 与命名规范
# 输出：Skill is valid!
```

## 4. 使用方法

1. 在已安装该 Skill 的 Agent 中发起训练，例如：
   - 「开始口语思辨训练」
   - 「练一个专四口语话题」
   - 「陪我练口语，帮我纠中式英语」
2. AI 出题后，你用英文作答（口述后转写输入，或直接打字）；
3. 收到三段反馈后，输入【继续】进行同话题深度追问，或输入【重新回答】重答本轮；
4. 重复以上循环即可持续训练；未输入【继续】前话题不会更换。

## 5. 输入输出示例

**输入（用户）**

> 开始训练吧。

**输出（AI 出题）**

> Topic: Should university students take part-time jobs during the semester?
>
> 参考句型（供你参考，可自由改写）：
> - 肯定立场：Taking part-time jobs during the semester can help students gain practical skills that classrooms alone cannot provide.
> - 否定立场：However, part-time jobs may distract students from their academic priorities, especially during exam periods.
>
> （请口头作答，我会逐句转写并优化）

**输入（用户作答）**

> I think part-time job is very good for students. It can make them earn money and also learn some society experience.

**输出（AI 三段反馈）**

> ①【我的原句】
> I think part-time job is very good for students. It can make them earn money and also learn some society experience.
>
> ②【优化句】
> I believe **part-time jobs are beneficial for university students**, because they can not only **earn extra money** but also **gain real-world experience**.
>
> ③【简短点评】
> 本次主要是中式英语：① part-time job 缺复数缺冠词；② "learn society experience" 是中文直译，应为 gain/get experience，society 应改 real-world；③ 句式可用 not only...but also 合并。逻辑清晰但缺衔接词，用词整体口语化，注意单复数和动宾搭配。
>
> 请选择：
> 【继续】— 围绕当前话题进行深度追问，开启新一轮作答
> 【重新回答】— 不换话题，你重新回答一次，我重新转写、优化、点评

## 目录结构

```
oralenglish-skill/
├── SKILL.md              # 技能主文件：完整流程规则、状态机、示例会话
└── agents/
    └── openai.yaml       # OpenAI 兼容 Agent 的运行配置（与 SKILL.md 规则一致）
```

## License

MIT（详见 `oralenglish-skill` 内元数据）。

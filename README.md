# 相亲自我画像与相亲帖助手

当前版本 **1.4.0**。通过本地问卷梳理自我画像、关系需求与择偶偏好，并让当前 AI 生成真诚自然的相亲帖或分析。

## 安装

适用于能够读取 Skill、运行本地脚本的 AI 助手。将整个 `dating-profile-assistant` 文件夹安装到所用助手的 Skills 位置，保留入口、脚本、素材和指南；不要只复制 `SKILL.md`。

- **Codex 新用户**：个人目录为 `~/.agents/skills/dating-profile-assistant/`；Windows 对应 `%USERPROFILE%\.agents\skills\dating-profile-assistant\`。项目内也可使用 `.agents/skills/`。[官方安装说明](https://learn.chatgpt.com/docs/build-skills)
- **其他本地助手**：按该助手自己的 Skill 安装方式安装，确认能发现本技能。
- **已有安装**：若助手已识别现有位置，直接更新该位置，避免在多个目录重复安装同名 Skill。

## 一句话开始

在安装 Skill 的 AI 对话中说：

> 使用 dating-profile-assistant，帮我写一篇相亲帖。

也可以说：“帮我梳理择偶需求”“生成一份关系画像”“把已有介绍改成朋友圈版本”。可用助手的技能选择器选择本技能，不要求所有助手使用同一种命令格式。

AI 会复用你已提供的资料，自动构建并请求默认浏览器打开完整问卷，同时给出页面链接作为备用。你不需要运行命令、寻找 HTML 或打开 `assets/` 中的文件。没有可用的本地 Python 或浏览器时，AI 改用文字问卷，不要求安装依赖。已有资料充分且只需改写时，AI 可以直接处理。

## 填写

首页确认目标和模式；未指定时默认快速写帖。每步最多 5 题，快速写帖默认 15 题、标准 39 题；其他目标采用对应题单，深度模式只展开选定主题。每题可跳过、不确定或不想回答，可以随时提前结束。

填写后核对答案，勾选允许写入公开文案的资料。**所有回传答案用于 AI 分析，只有明确授权且未被公开规则排除的内容可以进入公开文案。** 全选不会覆盖公开限制，写作参数不属于个人公开授权。

写帖时可在核对页补选平台、语气、长度；平台和语气可多选，页面显示预计版本数。缺少参数也可先复制，由 AI 在原对话中确认。画像、报告和择偶分析不要求选择平台。

## 回传与修改

1. 点击“复制答案”。如果浏览器不允许自动复制，按页面提示手动复制完整文本。
2. 回到刚才启动问卷的 AI 对话，粘贴并发送。
3. AI 直接按你的目标生成结果；网页不会自动发送或生成。
4. 后续可在原对话说“换成豆瓣版”“再简洁一点”或更正资料，AI 复用现有资料，不要求从头填写。

修改网页答案或公开范围后需要重新复制。新填写内容仅在页面内存中，刷新或关闭会丢失；预填资料嵌入本地 HTML，页面和复制文本应作为个人资料管理。

## 维护与验证

以下命令由助手或维护者执行，普通使用不需要操作终端。打开入口为 `scripts/start_form.py`，它复用构建器与浏览器启动器；路径从安装位置解析，不依赖开发者的电脑目录。

```text
python scripts/start_form.py --output-dir <当前任务授权的输出目录> [--prefill <预填JSON路径>]
```

使用当前助手提供的 Python 运行时；macOS/Linux 通常为 `python3`。stdout 返回 `html_path`、`skill_version`、`browser_opened`。打开失败时保留完整页面；`--no-open` 只构建，用于自动检查。`assets/form-shell.html` 是内部模板，不能直接填写。低层构建和打开脚本保留用于维护。

完整启动、预填及接收规则见 [web-interaction.md](web-interaction.md)；隐私规则见 [privacy-safety.md](privacy-safety.md)。题目只维护于 [questionnaire.md](questionnaire.md)，稳定题号 q01–q89 对应控件配置。分析与写作分别按入口的指南路由，不复制或虚构个人素材；参考治理见 [references/README.md](references/README.md)。

在 Skill 目录运行：

```text
python -m unittest discover -s tests -p "test_*.py"
node --test tests/form-model.test.cjs tests/quality-model.test.cjs tests/content-fixtures.test.cjs
node --test tests/browser.test.cjs
```

浏览器测试使用已有 Playwright 和系统浏览器，不安装测试框架。用 `BROWSER_CHANNEL=msedge` 或 `chrome` 选择浏览器，必要时设置 `PLAYWRIGHT_MODULE` 和 `PYTHON_EXECUTABLE` 指向现有运行时。合成页面与截图保存在工作区 `output/playwright/<浏览器>/`，自动检查不向真实聊天发送消息。内容行为按 [对话验收场景](tests/scenarios.md) 和 [固定案例评价方法](tests/content-evaluation.md) 另行验证。

## 构建分发包

```text
python scripts/package_skill.py --output ../dating-profile-assistant-1.4.0.zip
```

版本唯一来源为入口的 `metadata.version`。分发包采用文件名白名单，仅包含运行资源、参考摘要和合成测试；不包含真实预填、生成页面、原始人物帖子或验证输出。新增资源必须更新白名单和解包测试；安装更新前备份，更新后核对版本与文件哈希。

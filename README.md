# 相亲自我画像与相亲帖助手

当前版本 **1.5.0**。Skill 已内置完整的 [浏览器问卷 questionnaire.html](questionnaire.html)，包含全部题库、样式和交互脚本。页面可直接打开，无需先运行 Python、启动服务器或连接网络。

## 安装

将整个 `dating-profile-assistant` 文件夹安装到所用助手的 Skills 位置；保留内置 HTML、入口、脚本和指南，不要只复制 `SKILL.md`。

- **Codex 新用户**：个人目录 `~/.agents/skills/dating-profile-assistant/`；Windows 为 `%USERPROFILE%\.agents\skills\dating-profile-assistant\`。项目内可用 `.agents/skills/`。[官方安装说明](https://learn.chatgpt.com/docs/build-skills)
- **其他助手**：按该助手的 Skill 导入方式安装，确认可读取附带文件。
- **已有安装**：更新当前助手已经识别的位置，避免重复安装同名 Skill。

## 一句话开始

在 AI 对话中说：

> 使用 dating-profile-assistant，打开内置问卷，帮我写一篇相亲帖。

本机助手会定位安装目录中的 `questionnaire.html`，使用浏览器或打开文件工具直接唤起页面，并提供同一文件的备用链接。普通问卷不需要 Python；也可以用浏览器直接打开这个文件。

如果助手在云端运行，应由它提供完整 HTML 附件供下载，下载后用 Edge、Chrome 等浏览器打开。云端文件路径不是你的电脑路径，文件预览窗口也可能限制脚本；应打开下载后的完整 HTML。若该助手无法读取或提供 Skill 附件，则使用文字问卷，不能仅凭导入 Skill 就宣称已在本机打开。

已提供的资料继续用于当前 AI 的分析和改写。有 Python 且需要预填时，助手可生成个人问卷副本；内置标准页不含任何人的资料。已有资料充分、只需改写时可直接处理。

## 填写

首页选择目标和模式，默认快速写帖。每步最多 5 题，快速写帖默认15题、标准39题；其他目标使用对应题单，深度模式按主题展开。每题可跳过、不确定或不想回答，随时可以提前核对。

核对后勾选允许写进公开文案的资料。所有回传答案用于 AI 分析；公开文案仅使用明确授权且未被公开规则排除的资料。全选不覆盖限制，写作参数不属于个人授权。

写帖可补选平台、语气、长度，并显示组合版本数；缺参数可先复制，由 AI 在原对话确认。画像、报告和择偶分析不要求选择平台。

## 回传与修改

1. 点击“复制答案”。剪贴板被拒绝时，手动复制页面显示的完整文本。
2. 回到启动问卷的 AI 对话，粘贴并发送。
3. AI 按当前目标生成结果；后续修改复用资料，不从头填写。

修改答案或公开范围后重新复制。新填写内容只在当前页面内存中，刷新或关闭会丢失。个人预填副本和复制文本可能含隐私，应作为个人资料管理。

## 维护与验证

默认浏览器入口是根目录 `questionnaire.html`；`assets/form-shell.html` 是内部模板，不作为用户入口。内置页从同一题库和代码生成，不能手动维护第二套题目。变更源文件或版本后更新它：

```text
python scripts/build_form.py --portable --output questionnaire.html
```

内置页的 `skill_path` 为空，版本与题库内嵌，不包含维护者安装路径或预填。以下个人预填流程仅在助手有 Python 时使用，路径由当前环境决定：

```text
python scripts/start_form.py --output-dir <当前任务授权的目录> [--prefill <预填JSON路径>]
```

启动器保留唯一文件、不覆盖旧页面和失败恢复逻辑；stdout 返回 `html_path/skill_version/browser_opened`。完整规则见 [web-interaction.md](web-interaction.md)，隐私规则见 [privacy-safety.md](privacy-safety.md)。题库唯一来源为 [questionnaire.md](questionnaire.md)，题号 q01–q89 稳定。

在 Skill 目录运行：

```text
python -m unittest discover -s tests -p "test_*.py"
node --test tests/form-model.test.cjs tests/quality-model.test.cjs tests/content-fixtures.test.cjs
node --test tests/browser.test.cjs tests/bundled-browser.test.cjs
```

浏览器检查使用已有 Playwright 和系统浏览器，无需安装测试框架。`BROWSER_CHANNEL` 指定 `msedge` 或 `chrome`；必要时设置 `PLAYWRIGHT_MODULE/PYTHON_EXECUTABLE`。内置页专用检查不调用 Python，直接打开单独搬迁的 HTML，并禁用网络。合成页面和截图保存在工作区 `output/playwright/<浏览器>/`，自动检查不向真实聊天发送消息。内容按 [对话场景](tests/scenarios.md) 和 [评价方法](tests/content-evaluation.md) 验证，参考治理见 [references/README.md](references/README.md)。

## 构建分发包

```text
python scripts/package_skill.py --output ../dating-profile-assistant-1.5.0.zip
```

版本唯一来源为 `SKILL.md` 的 `metadata.version`。分发器将当前源资源生成的干净 `questionnaire.html` 直接写进 ZIP，不读取或覆盖源目录同名 HTML，防止误带个人预填。不包含真实预填、个人生成页面、原始人物帖子或验证输出。安装更新前备份，更新后核对版本和文件哈希。


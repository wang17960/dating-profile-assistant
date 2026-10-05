# 相亲自我画像与相亲帖助手

一个通过网页问卷帮助用户梳理自我画像、关系需求与择偶偏好，再由当前 AI 生成分析和多平台相亲帖的 Skill。用户不知道答案时，可以选择“不确定”，继续填写对应情景题。

默认流程：在电脑默认浏览器打开本地网页 → 分步填答 → 在汇总表选择公开范围 → 点击“复制答案并交给 AI” → 手动粘贴到当前对话 → AI 输出结果。完整流程见 [web-interaction.md](web-interaction.md)。

## 安装

将整个 `dating-profile-assistant` 文件夹复制到 Codex 的 Skills 目录：

- Windows 默认：`%USERPROFILE%\.codex\skills\dating-profile-assistant\`
- 如已设置 `CODEX_HOME`：`%CODEX_HOME%\skills\dating-profile-assistant\`
- macOS/Linux 默认：`~/.codex/skills/dating-profile-assistant/`

保持 `SKILL.md` 位于文件夹根目录，并一并复制 `assets/`、`scripts/`、所有 Markdown 文件、`styles/` 和 `references/`。打开本地问卷需要现有 Python 运行时；问卷是自包含 HTML，不需要服务器或网络。

## 使用

用户可直接描述目标，例如：

- “我资料很少，帮我一步步想起兴趣，再写一篇小红书相亲帖。”
- “打开本地问卷，填完后我会复制并手动粘贴答案给你，再生成画像与相亲帖。”
- “我不知道喜欢什么样的人，帮我从不能接受的相处模式开始梳理。”
- “根据这些回答做一份恋爱定位报告，分析时别给我贴人格标签。”
- “把这段自我介绍改成朋友圈版本，保留我的说话语气，收入别写。”

支持单项或全流程。网页每步最多 5 题；快速模式 18 题，标准模式 35 题，按目标、已答资料和不确定分支调整。深度模式可选主题；随时可跳过或提前生成答案。填写完毕后在汇总表逐项授权写入详情帖，支持“全选”和“全部不选”；默认仅供分析。平台与语气可多选，批量输出按所选组合数量生成并按平台分组。纯文字题只有主输入框，选择题保留补充内容。

只要求改写、资料已经完整，或明确偏好文字交流时，AI 可以直接处理，不强制打开网页。

### 本地预览问卷

在本目录运行以下命令，构建后用系统默认浏览器打开独立问卷。只依赖 Python 标准库，不需要安装 Python 包或启动服务。

```powershell
python scripts/build_form.py --output "$env:TEMP\dating-profile-questionnaire.html"
python scripts/open_form.py "$env:TEMP\dating-profile-questionnaire.html"
```

macOS/Linux 可将输出路径换成 `$TMPDIR/dating-profile-questionnaire.html`，并使用 `python3` 运行。

填写内容只保存在当前页面内存中。提交按钮将结构化答案复制到剪贴板；用户需要自行粘贴到搭载此 Skill 的 AI 对话。

## 文件结构

```text
dating-profile-assistant/
├── .gitignore
├── SKILL.md
├── README.md
├── questionnaire.md
├── preference-analysis.md
├── relationship-analysis.md
├── dating-report-template.md
├── writing-guide.md
├── privacy-safety.md
├── web-interaction.md
├── agents/openai.yaml
├── assets/
│   ├── form-controls.json
│   ├── form-shell.html
│   ├── form-model.js
│   ├── form-ui.js
│   └── local-form.css
├── scripts/
│   ├── build_form.py
│   └── open_form.py
├── tests/
│   ├── scenarios.md
│   ├── test_build_form.py
│   └── form-model.test.cjs
├── styles/
│   ├── xiaohongshu.md
│   ├── douban.md
│   ├── weibo.md
│   ├── douyin.md
│   ├── moments.md
│   ├── dating-group.md
│   └── generic.md
└── references/
    ├── README.md
    ├── xiaohongshu/README.md
    └── douban/README.md
```

## 添加参考资料

将用户有权使用的帖子或自己的文案放到 `references/<平台>/`。参考资料只用来提炼标题、结构、段落、语气等特征，不复制个人信息、原句或独特表达。不希望随 Skill 分发的资料不要放进 Skill 包。

## 扩展与维护

- **添加平台**：新增 `styles/<platform>.md`，只记录会改变写作选择的规则；在 `SKILL.md` 增加路由说明，在本文件结构中登记。
- **添加问题模块**：题目文案只维护在 `questionnaire.md`；稳定题号对应 `assets/form-controls.json` 的控件配置。新增题号时同步调整构建器的编号校验、模式/分支映射和测试，保留旧题号以兼容预填资料。
- **修改报告**：编辑 `dating-report-template.md`，并检查 `relationship-analysis.md` 与 `preference-analysis.md` 是否引用一致。
- **隐私规则**：集中维护在 `privacy-safety.md` 和核心入口必要提醒，避免各平台独自发明隐私承诺。

## 验收场景

见 [tests/scenarios.md](tests/scenarios.md)。它们用于检查对话行为与关键边界，不要求引入新的测试框架。

网页实现使用标准库测试：在 Skill 目录执行 `python -m unittest discover -s tests -p "test_*.py"` 与 `node --test tests/form-model.test.cjs`。不在验收中向真实 AI 对话提交测试消息。

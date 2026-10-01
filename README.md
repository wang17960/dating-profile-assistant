相亲自我画像与相亲帖助手
一个帮助用户从自我认识、关系需求与择偶偏好，走到个性化、多平台相亲帖的 Skill。它能在用户“想不起来/不确定”时提供选择题和行为场景；分析时区分事实、自述与推断，不做心理诊断。
安装
将整个 dating-profile-assistant 文件夹复制到 Codex 的 Skills 目录：
Windows 默认：%USERPROFILE%\.codex\skills\dating-profile-assistant\
如已设置 CODEX_HOME：%CODEX_HOME%\skills\dating-profile-assistant\
macOS/Linux 默认：~/.codex/skills/dating-profile-assistant/
保持 SKILL.md 位于文件夹根目录，并一并复制所有 Markdown 文件及 styles/、references/。如果 Skills 列表未立即显示新条目，重新载入或重启应用后再检查。
使用
用户可直接描述目标，例如：
“我资料很少，帮我一步步想起兴趣，再写一篇小红书相亲帖。”
“我不知道喜欢什么样的人，帮我从不能接受的相处模式开始梳理。”
“根据这些回答做一份恋爱定位报告，分析时别给我贴人格标签。”
“把这段自我介绍改成朋友圈版本，保留我的说话语气，收入别写。”
支持单项或全流程。默认中文，可以切换到用户的语言。问卷按 3–8 题一轮展开，随时可跳过、暂停并直接按现有信息继续。
文件结构
dating-profile-assistant/
├── SKILL.md
├── README.md
├── questionnaire.md
├── preference-analysis.md
├── relationship-analysis.md
├── dating-report-template.md
├── writing-guide.md
├── privacy-safety.md
├── agents/openai.yaml
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
​
添加参考资料
将用户有权使用的帖子或自己的文案放到 references/<平台>/。参考资料只用来提炼标题、结构、段落、语气等特征，不复制个人信息、原句或独特表达。不希望随 Skill 分发的资料不要放进 Skill 包。
扩展与维护
添加平台：新增 styles/<platform>.md，只记录会改变写作选择的规则；在 SKILL.md 增加路由说明，在本文件结构中登记。
添加问题模块：把题目放入 questionnaire.md 合适模块，说明动态分支和如何避免重复收集；不要要求每个模式都走该模块。
修改报告：编辑 dating-report-template.md，并检查 relationship-analysis.md 与 preference-analysis.md 是否引用一致。
隐私规则：集中维护在 privacy-safety.md 和核心入口必要提醒，避免各平台独自发明隐私承诺。
验收场景
见 tests/scenarios.md。它们用于检查对话行为与关键边界，不要求引入新的测试框架。

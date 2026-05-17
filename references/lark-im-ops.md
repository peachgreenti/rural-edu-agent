# 飞书群消息（lark-im）操作参考

## 目的

指导如何在乡村教育助手中操作飞书群消息，实现教学简报推送和群内交互。

## 前置条件

执行操作前，先读取 lark-im Skill 的完整指南：
- 读取 `lark-im` SKILL.md 了解基础操作

## 发送消息到群聊

### 发送文本消息

```bash
# 发送教学简报摘要到群聊
lark-cli im +send-message \
  --chat-id "{群聊ID}" \
  --msg-type text \
  --content '{"text":"📰 本周教学简报\n\n📊 本周完成 {N} 节课\n📚 覆盖学科：{学科列表}\n📝 新增教案：{N} 份\n\n📄 完整简报：{文档链接}"}'
```

### 发送富文本消息（卡片消息）

```bash
# 发送教学简报卡片
lark-cli im +send-message \
  --chat-id "{群聊ID}" \
  --msg-type interactive \
  --content '{
    "config": {"wide_screen_mode": true},
    "header": {
      "title": {"tag": "plain_text", "content": "📰 第{N}周教学简报"},
      "template": "blue"
    },
    "elements": [
      {"tag": "div", "text": {"tag": "lark_md", "content": "**📊 本周教学概览**"}},
      {"tag": "div", "text": {"tag": "lark_md", "content": "完成课时：**{N}** 节\n覆盖学科：{学科列表}\n新增教案：**{N}** 份"}},
      {"tag": "hr"},
      {"tag": "div", "text": {"tag": "lark_md", "content": "✨ **教学亮点**\n{亮点内容}"}},
      {"tag": "hr"},
      {"tag": "action", "actions": [
        {"tag": "button", "text": {"tag": "plain_text", "content": "查看完整简报"}, "url": "{文档链接}", "type": "primary"}
      ]}
    ]
  }'
```

### 发送教案生成通知

```bash
# 教案生成完成后通知群聊
lark-cli im +send-message \
  --chat-id "{群聊ID}" \
  --msg-type interactive \
  --content '{
    "config": {"wide_screen_mode": true},
    "header": {
      "title": {"tag": "plain_text", "content": "📝 新教案已生成"},
      "template": "green"
    },
    "elements": [
      {"tag": "div", "text": {"tag": "lark_md", "content": "**课题**：{课题}\n**学科/年级**：{学科} · {年级}\n**课时**：{课时数} 课时"}},
      {"tag": "action", "actions": [
        {"tag": "button", "text": {"tag": "plain_text", "content": "查看教案"}, "url": "{文档链接}", "type": "primary"}
      ]}
    ]
  }'
```

## 获取群聊信息

### 搜索群聊

```bash
# 按名称搜索群聊
lark-cli im +search-chat --name "教学组"
```

### 获取群成员列表

```bash
# 获取群成员
lark-cli im +get-chat-members --chat-id "{群聊ID}"
```

## 消息模板

### 教学简报推送模板

```
📰 第{N}周教学简报（{日期范围}）

📊 数据概览
├── 完成课时：{N} 节
├── 覆盖学科：{学科}
├── 新增教案：{N} 份
└── 教学天数：{N} 天

✨ 本周亮点
• {亮点1}
• {亮点2}

📋 查看完整简报 → {链接}
```

### 教案生成通知模板

```
📝 教案已生成

📖 {课题}
📚 {学科} · {年级}
⏱ {课时数} 课时

📄 查看教案 → {链接}
📊 已同步教学记录
```

### 教学提醒模板

```
⏰ 明日课程提醒

📅 {日期}（{星期}）
├── 第1节：{学科} — {课题}
├── 第2节：{学科} — {课题}
└── 第3节：{学科} — {课题}

💡 请提前准备好教学素材
```

## 注意事项

- 发送消息前确认有群聊的发送权限
- 推送简报时使用卡片消息，视觉效果更好
- 消息内容简洁明了，详细信息通过链接查看
- 避免频繁推送消息，建议每周一次简报推送
- 尊重群聊规范，不发送无关内容

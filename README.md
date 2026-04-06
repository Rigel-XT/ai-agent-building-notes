# 1. 这是一个大标题 (H1)

这是你的项目或笔记的总入口。

## 2. 这是一个二级标题 (H2)

用于区分不同的章节，GitHub 会自动为它生成锚点链接。
<img width="562" height="58" alt="image" src="https://github.com/user-attachments/assets/301e35da-3d80-452e-8df0-732ad64c19c1" />


### 3. 任务清单 (Task List)

在 Notion 中输入 `[]`。同步到 GitHub 后，它们会变成可交互的复选框。

- [x]  **已完成：** 学习如何配置 GitHub Actions
- [ ]  **进行中：** 导出 Notion 笔记到本地仓库
- [ ]  **计划中：** 优化文档的图片存储路径

---

## 4. 引用与核心提示 (Blockquote)

在 Notion 中使用 `>` 。它在 GitHub 中会呈现出非常优雅的侧边栏样式。

> **Pro Tip:** 尽量不要在 GitHub 适配文档中使用 Notion 的彩色背景 (Callout)，因为标准 Markdown 不支持背景色，同步后可能会丢失样式。
> 

---

## 5. 带有语法高亮的代码块 (Code Block)

确保在 Notion 的代码块右上角选择了正确的语言（例如 `Python`）。

Python

```python
def sync_notion_to_github():
    # 这是一个示例函数
    platform = "GitHub"
    status = "Optimized"
    print(f"Content is now {status} for {platform}!")

sync_notion_to_github()
```

---

## 6. 基础表格 (Simple Table)

注意：这里使用的是 Notion 的 **Simple Table**（简单表格），而不是 Database。它在 Git 中的兼容性是 100%。

| **功能模块** | **适配程度** | **建议** |
| --- | --- | --- |
| 标题/列表 | 极高 | 放心使用 |
| 简单表格 | 高 | 适合对比数据 |
| 嵌套折叠 | 中 | GitHub 渲染较简陋 |

---

### 7. 有序列表 (Numbered List)

1. 第一步：在 Notion 整理内容。
2. 第二步：使用工具转换格式。
3. 第三步：推送到 GitHub 仓库。

---

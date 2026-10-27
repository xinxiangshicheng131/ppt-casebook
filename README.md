# ppt-casebook · 案例册 / 系统册 PPT 生成思路

一个给**建筑 / 城市设计学生**的 AI Skill：把"一个主题"（绿化系统、运动系统、历史文化系统……）变成**一本能拿去汇报的册子**。

## 它能干什么

- **你出方向，它先给选项**：你说"做 XX 系统"，它先拆成 L1 / L2 / L3 / L4 几个思路让你选，选定后再动手，不瞎做。
- **七步模板做册子**：封面 → 选案理由 → 案例页 → 落到场地 → 翻译表 → 我的建议 → 收口。
- **案例怎么选、怎么翻译**：每个案例都落到"抄哪一层、别抄哪一层"，不是堆图。
- **技术活有现成方法**：抓图渠道、自绘图、PPT 排版、坑清单都在 `references/` 里。

## 怎么装

1. 点右上角绿色 **Code** 按钮 → **Download ZIP**，下载后解压。
2. 把解压出来的 `ppt-casebook` 文件夹**整个**放进你的 skills 目录：
   - **WorkBuddy**：`C:\Users\<你的用户名>\.workbuddy\skills\`
   - **Claude Code**：`~/.claude/skills\`
3. 重启 / 新开一个对话即可。

装好后目录长这样（别多套一层文件夹）：

```
skills/
└─ ppt-casebook/
   ├─ SKILL.md
   └─ references/
      ├─ book-template.md
      ├─ case-images.md
      ├─ drawing.md
      ├─ ppt.md
      └─ pitfalls.md
```

## 怎么用

在对话框里直接说：

> 帮我做「绿化系统」的案例册 / PPT

它就会先给你几个方向让你选，选完再按模板做册子。

## 文件说明

| 文件 | 内容 |
|---|---|
| `SKILL.md` | 主文件：七步模板 + 方向分级推演 + 案例转化三招 + 唯一答案推导法 |
| `references/book-template.md` | 七步逐页规格 + 真实范例 |
| `references/case-images.md` | 案例抓图渠道（Commons / ArchDaily / 政府官网…） |
| `references/drawing.md` | 自绘图技巧（坐标系 / 剖面 / 尺寸标注） |
| `references/ppt.md` | PPT 排版 + WPS 自检 |
| `references/pitfalls.md` | 高频坑清单（踩之前先看） |

---

*方法提炼自真实课程作业，文本由 AI 协助整理。*

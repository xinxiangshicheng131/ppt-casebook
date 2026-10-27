# python-pptx 生成与 WPS 自检

## 版式铁律（用户要求）

- **字号 ≥ 20**（标题 34–50，副标 20–24，正文 20–26），不要 9–11pt 的小字。
- **每页字数尽量少**，以图为主体；文字只做一句话结论。
- 页脚小字（图片来源、非实测说明）可以用 12pt，但正文和结论必须 ≥ 20。

## 结构约定

- 画布 13.333 × 7.5 英寸（16:9）。
- 中文字体 `微软雅黑`，写入时对 latin/ea/cs 三个字体槽都设 typeface，否则中文不生效：
  ```python
  r.font.name = FONT
  rPr = r._r.get_or_add_rPr()
  for t in ("latin","ea","cs"):
      e = rPr.find(NS+t)
      if e is None: e = etree.SubElement(rPr, NS+t)
      e.set("typeface", FONT)
  ```
- 配色：主色暗红 (176,62,42)、青 (54,118,132)、正文 (34,32,28)、浅灰底 (252,251,248)。

## 图片两种放法

- `fit()`：完整放入、不裁切（图纸用），可能留白边。
- `cover()`：裁切铺满（照片用）。
- 竖版图配右栏文字，横版图满宽放。

## 踩坑

- python-pptx **不认 Markdown**，文本里写 `**加粗**` 会渲染成字面星号——加粗用 `run.font.bold=True`。
- 图片文件名带下划线时，正则字符类要含 `_`。
- 跨文件拼页（合并两本 PPT）用**拼接两个 build 脚本的源码**，取共用的 helper 块 + 各自页面段；注意被拼的一本可能缺某个 helper 函数（如 `fit`），要补定义。
- 生成 pptx 时若报 PermissionError，是 WPS 还开着这个文件，先 `Get-Process wpp | Stop-Process -Force`。

## WPS 导出自检（比猜版式靠谱）

MS Office 未激活不用管，**WPS 演示可用**。用 COM 导出每页 PNG 逐张看：

```powershell
$app = New-Object -ComObject KWPP.Application
$pres = $app.Presentations.Open("<绝对路径>", $true, $false, $false)
$pres.Export("<空目录>", "PNG", 1600, 900)
$pres.Close(); $app.Quit()
```

- pptx 先复制到**纯英文路径**再导出，绕开 PS 5.1 中文编码坑。
- 样板脚本常驻 `D:\Temp\dfppt\export.ps1`（pptx 放 `D:\Temp\dfppt\out.pptx`）。
- 导出后用 Read 逐张看 PNG，确认标签没压图、图片没缩太小。

## 交付

- 成图（pptx 里用到的 PNG）单独拷一份到 `<项目名>\` 下、改中文名，方便用户直接打开看。
- 不要放进 `_dia` 这种下划线开头的目录（用户在文件管理器里找不到）。

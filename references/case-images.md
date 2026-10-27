# 案例抓图渠道（实测踩坑记录）

按可靠度排序，逐级尝试。

## 1. Wikimedia Commons（最可靠，但会限流）

- 走代理后限流解除。代理：`http://127.0.0.1:7890`（Clash mixed-port，绿色版）。
- API 搜索：
  ```
  curl -x http://127.0.0.1:7890 "https://commons.wikimedia.org/w/api.php?action=query&format=json&generator=search&gsrsearch=filetype:bitmap <关键词>&gsrnamespace=6&gsrlimit=10&prop=imageinfo&iiprop=url&iiurlwidth=1800"
  ```
- 关键词要点：**中文项目名直接搜中文**（如"成都西村·贝森大院"能命中，搜英文"West Village Chengdu"反而空）；搜人名（如"Jiakun Architects"）会混进肖像和别的项目，慎用。
- 搜索结果里的文件名（01/02/05…）与缩略图索引**不是一一对应**，写脚本填文件名前必须先 `ls` 实际目录。
- 连续请求要 `sleep 2-5s`，否则限流返回空。

## 2. ArchDaily（成图质量最好）

- 图片域名 `images.adsttc.com`，从页面 HTML 正则抓取：
  ```
  images\.adsttc\.com/media/images/([0-9a-f/]{29})/large_jpg/([A-Za-z0-9\-_.]+\.(?:jpg|png))
  ```
- **文件名里常有下划线**，字符类要写 `[A-Za-z0-9\-_.]`（漏 `_` 会抓空）。
- 老文章（2014 年以前）的 `large_jpg` 可能只有 540px，够投影不够放大。
- 部分页面用图纸（平面/剖面）而不是实景——抓完先看接触表确认内容。

## 3. Bing 图搜（基本不可用）

- 对**具体项目名/政策名**识别极差：搜"西村大院"给成都夜景、搜"首尔老年游乐场"给图库老人肖像。
- 只用于"实在没别的源"时的侥幸，且必须人工核对每张内容。

## 4. 中国官方/新闻站

- 政府门户（cq.gov.cn、cgj.cq.gov.cn 等）图片**普遍防盗链**，带 Referer 也常拿不到。
- 澎湃/微信的图片是 JS 注入，curl 拿不到正文 HTML（微信返回"请在客户端打开"占位页）。
- 结论：这类源的图**优先让用户自己截图发来**，比爬更快更准。

## 通用规则

- 抓回图必须先拼"接触表"（contact sheet）人工看内容，确认是目标项目，再写进 PPT。
- 垃圾域名黑名单：shufaji、51jiaoxi、588ku、pngtree、zcool、duitang、nipic 等素材/图库站。
- 找不到实景照时，退一步用**自绘示意图**，并在页脚注明"示意图，非实测"。

# 个人作品集网站 — 交接说明

线上地址：**https://0830harper.github.io/portfolio/**
GitHub 仓库：**git@github.com:0830harper/portfolio.git**

---

## 一、这个包里有什么

```
portfolio-site/
├── index.html      ← 整个网站，就这一个文件（HTML + CSS + JS 都在里面）
├── assets/         ← 所有图片和视频（82 个文件）
│   ├── image-0xx.jpg/gif   作品图、论文配图
│   ├── award-xx.jpg        荣誉证书、参展活动照片
│   ├── portrait.jpg        首页黑白人像
│   └── video-00x.mp4       momo echo / 火龙祈愿 视频
├── .git/           ← 完整版本历史，可直接推送
└── HANDOFF.md      ← 本文件
```

网站是**纯静态**的，没有任何依赖，不需要 npm install，不需要构建。

---

## 二、在新电脑上怎么开始

### 方式 A（推荐）：直接从 GitHub 克隆

新电脑上不需要这个压缩包，一行命令就够：

```bash
git clone git@github.com:0830harper/portfolio.git
```

如果新电脑还没配 SSH 密钥，用 HTTPS 也行：

```bash
git clone https://github.com/0830harper/portfolio.git
```

### 方式 B：用这个压缩包

解压后 `.git` 已经带着完整历史和远程地址，直接就能用。

---

## 三、本地预览

```bash
cd portfolio-site
python3 -m http.server 8000
```

然后浏览器打开 `http://localhost:8000`

> 注意：**不要直接双击 index.html 打开**。虽然大部分能显示，但用本地服务器预览才和线上完全一致。

---

## 四、改完之后怎么更新线上

```bash
git add -A
git commit -m "改了什么"
git push origin main
```

推送后 GitHub Pages 会自动重新部署，**1～2 分钟**后线上生效。

> 如果刷新后还是旧的，按 `Cmd + Shift + R` 强制刷新，或用无痕窗口打开。
> 页面缓存 10 分钟，改完立刻看容易看到旧版本，这是正常的。

### 关于推送权限

原来那台电脑用的是 SSH 密钥 `~/.ssh/id_ed25519_portfolio`。
**新电脑建议重新生成一个**（不要拷贝私钥文件）：

```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_portfolio -C "portfolio-deploy"
cat ~/.ssh/id_ed25519_portfolio.pub
```

把输出的公钥粘贴到 https://github.com/settings/ssh/new

再在 `~/.ssh/config` 里加上：

```
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_portfolio
  IdentitiesOnly yes
```

---

## 五、网站结构速查

`index.html` 里分四个页面（标签页切换，不是多个文件）：

| 页面 | id | 内容 |
|---|---|---|
| 首页 | `#page-home` | 拼贴风格：纸卡 + 蕾丝布 + 宝丽来照片 |
| 关于 | `#page-about` | 教育背景、成绩获奖、荣誉证书、参展活动、爱好、联系方式 |
| 作品 | `#page-work` | 8 个项目，每个可折叠展开 |
| 论文 | `#page-research` | 8 篇论文 + 会议配图 |

### 几个关键 CSS 类

- `.home-hero` / `.home-text` / `.lace-cloth` — 首页拼贴（纸质、胶带、蕾丝布都是纯 CSS/SVG 画的）
- `.collapse-toggle` / `.collapse-panel` — 作品页「查看详细介绍」折叠功能
- `.edu-card` — 关于页教育背景双卡片
- `.awards-photo-grid` / `.activity-item` — 证书网格 / 活动照片（统一高度、宽度自适应）
- `.media-card--dark` — 火龙祈愿的黑色卡片背景

### 加新图片的流程

1. 图片放进 `assets/`（建议先压缩，宽度 1200px 以内、质量 82 左右就够）
2. 在 `index.html` 里找到对应位置，把 `图片待补充` 的占位块替换成：
   ```html
   <div class="tile reveal"><div class="tile-inner" style="background-image:url(assets/你的文件名.jpg)"></div></div>
   ```
3. commit + push

---

## 六、还没做完的部分

- [ ] **第 4 篇论文**（FSDM 2025）还差 2 张配图，现在只有 1 张
- [ ] **第 8 篇论文**（DETCE 2025）3 张配图全空着
- [ ] **简历 PDF** — 导航栏右上角现在是「open to work」按钮，原计划换成简历下载，需要先把 PDF 放进项目里
- [ ] **爱好板块**还是占位文字「爱好一/二/三」
- [ ] **首页蕾丝布**目前是代码画的（SVG），像插画不像实物照片。要真实质感需要一张蕾丝布照片素材替换

---

## 七、素材原件（不在这个包里）

原电脑上还有一份未压缩的原始素材，约 **929MB**，路径：

```
~/Desktop/cc/portfolio/
├── 个人作品集网站/
│   ├── 奖状/        ← 证书原扫描件（单张 40-90MB）
│   ├── 论文/        ← 会议现场原图
│   ├── 项目原图/    ← 各作品原图
│   └── 新图片.jpg   ← 首页人像原图
├── momoecho_raw/   ← momo echo 长图原件
├── dragon_raw/     ← 火龙祈愿视频
└── video_raw/      ← momo echo 视频
```

网站里用的都是压缩过的版本，**日常改网站不需要这些**。
只有当你要重新导出更高清的版本时才用得上，需要的话单独拷贝这个文件夹。

---

## 八、两个坑

1. **GitHub Pages 有缓存**，改完 1-2 分钟才生效，且浏览器会再缓存 10 分钟。看到旧版本先强制刷新，不要以为是没推送成功。
2. **图片一定要压缩再放进去**。原始扫描件几十 MB 一张，直接用会让网站慢到无法接受。

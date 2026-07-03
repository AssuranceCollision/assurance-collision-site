# Assurance Collision 官网 — WORKFLOW

> 单一事实来源。回来接着做之前**先读这份**。

---

## 0. 一句话

JKM 保诚汽车维修(Assurance Collision)碰撞维修店的官网。纯静态站(HTML + CSS,无 build、无数据库),首屏全屏背景图(以后可换视频),中英双语。

---

## 1. 基本信息

| 项 | 值 |
|---|---|
| 业务 | Assurance Collision · JKM 保诚汽车维修(湾区圣何塞碰撞维修店) |
| 域名 | **assuranceautocollision.com**(在 **Spaceship** 注册,**未绑定**) |
| 电话 | (408) 368-5196 |
| 地址 | 96 San Jose Ave, San Jose, CA |
| 营业时间 | 周一–周六 9:00–18:00(**占位,待确认**) |
| 本地代码 | `/home/tangh6/workspace/jkm`(WSL 内,非 Windows 盘) |
| 托管方案(已定) | **Netlify 免费版**(允许商业用途;Vercel 免费版禁商用,故不用) |
| 技术栈 | 纯静态 HTML/CSS,Google Fonts(Sora + Hanken Grotesk) |

参照样板:`/home/tangh6/workspace/website-demo`(superbuyer,同款静态站 + Netlify 自动部署 workflow)。

---

## 2. 文件结构

```
jkm/
├── index.html        # 整个单页网站
├── styles.css        # 全部样式(干净版,~7KB,非 superbuyer 那 28KB)
├── images/
│   ├── logo.png      # 透明背景 logo(1200×659)
│   ├── benz-before.jpg  # 奔驰 EQS450 维修前(照片4)
│   └── benz-after.jpg   # 奔驰 EQS450 维修后(照片5)→ 也是首屏背景
├── .gitignore
└── WORKFLOW.md       # 本文件
```

**页面板块(从上到下)**:导航 → 首屏(背景图+遮罩+标题) → 信任条 → 服务(What we do,7 项) → 维修前后对比 → 为什么选我们+客户评价 → 联系方式 → 页脚。

---

## 3. 素材来源

原图在 `/mnt/d/assurance collision/`:
- `Assurance logo.png`(透明)/ `Assurance logo.jpg`(白底)
- `white_benz_eqs450_front/1.jpg … 8.jpg`(目前用了 4=维修前、5=维修后)

> `._*.jpg` 是 Mac 元数据,忽略。
> 加新案例图:拷进 `images/`,在 `index.html` 的 `#work` 区按现有 `figure.ba-item` 结构加。

---

## 4. 本地预览

```bash
python3 -m http.server 8080 --directory /home/tangh6/workspace/jkm
```
然后 Windows 浏览器打开 **http://localhost:8080/**(WSL2 自动转发 localhost)。
改完文件按 **Ctrl+Shift+R** 强刷(绕过缓存)。

---

## 5. 上线流程(待做,一次性)

> 顺序很重要:**先部署出站点,域名才有地方可指。绑域名是最后一步。**

### ① 建 GitHub 仓库
- 新建一个仓库(建议放在能管理的账号下;注意别和 Superbuyer / Huarenxiuche 的 git 凭据冲突,推送用带用户名的 remote URL)
- `git init` → commit → push

### ② 接 Netlify
- Netlify 控制台 → Add new site → Import from GitHub → 选这个仓库
- Build 设置:**Build command 留空,Publish directory 设为 `/`(根目录)**(纯静态,无需 build)
- 部署后得到一个临时网址 `xxx.netlify.app`,先确认页面正常

### ③ 绑定域名 assuranceautocollision.com
- Netlify → Domain settings → Add custom domain → 输入 `assuranceautocollision.com`
- 两种 DNS 方式(二选一):
  - **A. 用 Netlify DNS(推荐,跟 superbuyer 一样省事)**:Netlify 给你一组 nameservers(类似 `dns1.p0x.nsone.net`),去 **Spaceship** 把域名的 nameservers 改成这组。之后 DNS 全在 Netlify 管。
  - **B. 保留 Spaceship DNS**:在 Spaceship 加 Netlify 给的记录(A 记录指向 Netlify IP `75.2.60.5`,`www` 加 CNAME 指向 `xxx.netlify.app`)。
- Netlify 自动签发 HTTPS(Let's Encrypt),等几分钟。

### ④ 完成后
**以后所有更新 = 改文件 → `git push` → Netlify 自动部署 → ~1 分钟上线。域名永不再碰。**

---

## 6. 日常更新流程(上线后)

```bash
# 改 index.html / styles.css / 加图
git add -A
git commit -m "说明"
git push        # Netlify 自动重新部署
```
约 1 分钟后 assuranceautocollision.com 显示新内容。无需登录 Spaceship,不动任何 DNS。

---

## 7. 待确认 / TODO

- [ ] **营业时间**确认(现占位 周一–周六 9:00–18:00)
- [ ] **资质类文案核实**:`#why` 里 "I-CAR trained""Lifetime warranty"、服务里 "车祸律师" 是自营还是转介绍 —— 不属实的删掉,别写没有的资质
- [ ] 服务卡片 7 个 → 3列下是 3+3+1,看是否调成更整齐的排列
- [ ] logo 现为白底 JPG 换成的透明 PNG;若有更高清版可替换
- [ ] **上线三步**(GitHub → Netlify → 绑域名),见 §5
- [ ] 上线后补:favicon、SEO(og 图、sitemap.xml、robots.txt)、Google Search Console、Google Business Profile
- [ ] (可选)首屏背景图 → 换成视频:`<img class="hero-bgimg">` 改成
      `<video class="hero-bgimg" autoplay muted loop playsinline poster="images/benz-after.jpg"><source src="images/hero.mp4" type="video/mp4"></video>`
      视频压到 3–8MB;手机端建议用 CSS 媒体查询退回静态图省流量。

---

## 8. 关键提醒

- 本项目**纯静态**:不要引入 Next.js / Supabase / build 步骤,那是 huache-app 的玩法,这里用不上。
- 上线前把 `images/` 里的手机大图(现 ~1MB/张)压一压,首屏背景图尤其影响加载速度。
- git 凭据:本机 `~/.git-credentials` 同时有 Superbuyer / Huarenxiuche 的 token,推送本仓库时**用带用户名的 remote URL** 避免拿错凭据(参见过去 403 经历)。

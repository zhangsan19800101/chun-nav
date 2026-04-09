## 以下为千问的对话截取

没问题！既然你喜欢最初那个极简、干净的“卡片式”设计，但又不想手搓 HTML，那我们就用 **Hugo** 来实现。

Hugo 的速度极快，而且非常适合这种“数据驱动”的页面。

下面我为你定制了一套**极简 Hugo 主题**。你只需要把代码复制进去，以后修改链接只需要编辑一个 `data.yml` 文件，完全不用碰 HTML。

---

### 🛠️ 准备工作

1.  确保你电脑安装了 Hugo (或者直接在 GitHub 上操作)。
2.  在本地或 GitHub 仓库中，我们需要创建以下目录结构：
    ```text
    my-nav/
    ├── data/
    │   └── links.yml       <-- 你以后只改这个文件！
    ├── layouts/
    │   └── index.html      <-- 负责外观（我已经写好了）
    └── themes/             <-- (可选，这里我们直接用默认结构)
    ```

---

### 🎨 第一步：创建外观 (`layouts/index.html`)

请在你的 Hugo 项目（或 GitHub 仓库）中，找到或创建 `layouts` 文件夹，并在里面新建 `index.html`。

这段代码完美复刻了你喜欢的极简风格，并加入了深色模式支持。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ .Site.Title }}</title>
    <style>
        :root {
            --bg-color: #f4f6f8;
            --card-bg: #ffffff;
            --text-main: #333333;
            --text-secondary: #666666;
            --accent-color: #0070f3;
        }

        @media (prefers-color-scheme: dark) {
            :root {
                --bg-color: #111111;
                --card-bg: #1e1e1e;
                --text-main: #e0e0e0;
                --text-secondary: #a0a0a0;
            }
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-main);
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
        }

        .container {
            width: 90%;
            max-width: 1000px;
            padding: 40px 0;
        }

        /* 搜索框 */
        .search-box {
            text-align: center;
            margin-bottom: 50px;
            margin-top: 20px;
        }

        .search-box input {
            width: 100%;
            max-width: 600px;
            padding: 15px 20px;
            border-radius: 12px;
            border: 1px solid transparent;
            background: var(--card-bg);
            color: var(--text-main);
            font-size: 16px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
            outline: none;
            transition: all 0.3s ease;
        }

        .search-box input:focus {
            box-shadow: 0 4px 16px rgba(0, 112, 243, 0.2);
            border-color: var(--accent-color);
        }

        /* 分类标题 */
        h2 {
            font-size: 18px;
            margin-bottom: 20px;
            padding-left: 10px;
            border-left: 4px solid var(--accent-color);
        }

        /* 链接网格 */
        .link-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            gap: 20px;
            margin-bottom: 40px;
        }

        .link-card {
            background: var(--card-bg);
            padding: 20px;
            border-radius: 12px;
            text-decoration: none;
            color: var(--text-main);
            transition: transform 0.2s, box-shadow 0.2s;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
            border: 1px solid transparent;
        }

        .link-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            border-color: rgba(0, 112, 243, 0.2);
        }

        .link-icon {
            font-size: 24px;
            margin-bottom: 10px;
            height: 40px;
            width: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            background: rgba(0,0,0,0.05);
            border-radius: 50%;
        }

        .link-title {
            font-weight: 600;
            font-size: 14px;
        }

        .link-desc {
            font-size: 12px;
            color: var(--text-secondary);
            margin-top: 5px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            width: 100%;
        }

        footer {
            margin-top: auto;
            padding: 20px;
            color: var(--text-secondary);
            font-size: 12px;
        }
    </style>
</head>
<body>

    <div class="container">
        <!-- 搜索区域 -->
        <div class="search-box">
            <form action="https://www.google.com/search" method="GET">
                <input type="text" name="q" placeholder="搜索内容..." autofocus autocomplete="off">
            </form>
        </div>

        <!-- 动态生成内容 -->
        {{ range .Site.Data.links }}
            <h2>{{ .category }}</h2>
            <div class="link-grid">
                {{ range .links }}
                <a href="{{ .url }}" class="link-card" target="_blank">
                    <div class="link-icon">{{ .icon }}</div>
                    <div class="link-title">{{ .title }}</div>
                    <div class="link-desc">{{ .desc }}</div>
                </a>
                {{ end }}
            </div>
        {{ end }}

    </div>

    <footer>
        &copy; {{ now.Year }} {{ .Site.Title }} | Powered by Hugo & Vercel
    </footer>

</body>
</html>
```

---

### 📝 第二步：配置数据 (`data/links.yml`)

在你的项目根目录下，创建一个名为 `data` 的文件夹，并在里面新建 `links.yml`。

**以后你增删链接，只需要改这个文件！**

```yaml
- category: 常用推荐
  links:
    - title: GitHub
      url: https://github.com
      icon: 🐙
      desc: 代码托管平台
    - title: Vercel
      url: https://vercel.com
      icon: ▲
      desc: 部署平台
    - title: ChatGPT
      url: https://chatgpt.com
      icon: 🤖
      desc: AI 智能助手

- category: 开发工具
  links:
    - title: Stack Overflow
      url: https://stackoverflow.com
      icon: 📚
      desc: 程序员问答社区
    - title: Can I Use
      url: https://caniuse.com
      icon: 🔍
      desc: CSS 兼容性查询

- category: 摸鱼专区
  links:
    - title: Bilibili
      url: https://bilibili.com
      icon: 📺
      desc: 哔哩哔哩
    - title: YouTube
      url: https://youtube.com
      icon: ▶️
      desc: 视频网站
```

---

### ⚙️ 第三步：基础配置 (`config.yaml`)

在项目根目录创建或修改 `config.yaml`，设置网站标题：

```yaml
baseURL: "/"
languageCode: "zh-CN"
title: "我的个人导航页"
theme: "" # 我们直接用了 layouts，不需要额外主题
```

---

### 🚀 第四步：部署到 Vercel

现在你的 GitHub 仓库里应该有这些文件了。

1.  登录 Vercel，点击 **Add New Project**。
2.  导入你的 GitHub 仓库。
3.  **关键设置**：
    -   **Framework Preset**: 选择 **Hugo**。
    -   **Build Command**: `hugo --gc --minify`
    -   **Output Directory**: `public`
4.  点击 **Deploy**。

### 🎉 效果

Vercel 会自动运行 Hugo，读取你的 `data/links.yml`，生成静态 HTML，并发布上线。

**以后你想加链接：**
1.  在 GitHub 上直接编辑 `data/links.yml`。
2.  提交更改。
3.  Vercel 检测到变化，自动重新构建，几秒钟后你的导航页就更新了。

既保留了你喜欢的外观，又实现了极简的数据管理！


没问题！这是一个非常实用的功能。为了实现**“边打字边筛选本页链接”**，我们需要做两件事：

1.  **修改 HTML**：给链接卡片增加一个统一的类名（方便 JS 抓取）。
2.  **增加 JavaScript**：监听输入框的动作，实时对比文字并显示/隐藏卡片。

下面是修改后的完整代码。你可以直接替换掉之前的 `layouts/index.html` 内容。

### 📝 修改后的 `layouts/index.html`

主要改动点：
1.  给搜索框加了 `id="searchInput"`。
2.  给链接卡片加了 `class="nav-item"`。
3.  在底部加了 `<script>` 标签来处理筛选逻辑。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ .Site.Title }}</title>
    <style>
        :root {
            --bg-color: #f4f6f8;
            --card-bg: #ffffff;
            --text-main: #333333;
            --text-secondary: #666666;
            --accent-color: #0070f3;
        }

        @media (prefers-color-scheme: dark) {
            :root {
                --bg-color: #111111;
                --card-bg: #1e1e1e;
                --text-main: #e0e0e0;
                --text-secondary: #a0a0a0;
            }
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-main);
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
        }

        .container {
            width: 90%;
            max-width: 1000px;
            padding: 40px 0;
        }

        /* 搜索框样式 */
        .search-box {
            text-align: center;
            margin-bottom: 50px;
            margin-top: 20px;
        }

        .search-box input {
            width: 100%;
            max-width: 600px;
            padding: 15px 20px;
            border-radius: 12px;
            border: 1px solid transparent;
            background: var(--card-bg);
            color: var(--text-main);
            font-size: 16px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
            outline: none;
            transition: all 0.3s ease;
        }

        .search-box input:focus {
            box-shadow: 0 4px 16px rgba(0, 112, 243, 0.2);
            border-color: var(--accent-color);
        }

        /* 分类标题 */
        h2 {
            font-size: 18px;
            margin-bottom: 20px;
            padding-left: 10px;
            border-left: 4px solid var(--accent-color);
        }

        /* 链接网格 */
        .link-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            gap: 20px;
            margin-bottom: 40px;
        }

        .link-card {
            background: var(--card-bg);
            padding: 20px;
            border-radius: 12px;
            text-decoration: none;
            color: var(--text-main);
            transition: transform 0.2s, box-shadow 0.2s, opacity 0.2s;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
            border: 1px solid transparent;
        }

        .link-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            border-color: rgba(0, 112, 243, 0.2);
        }

        /* 搜索隐藏时的样式 */
        .hidden {
            display: none !important;
        }

        .link-icon {
            font-size: 24px;
            margin-bottom: 10px;
            height: 40px;
            width: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            background: rgba(0,0,0,0.05);
            border-radius: 50%;
        }

        .link-title {
            font-weight: 600;
            font-size: 14px;
        }

        .link-desc {
            font-size: 12px;
            color: var(--text-secondary);
            margin-top: 5px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            width: 100%;
        }

        footer {
            margin-top: auto;
            padding: 20px;
            color: var(--text-secondary);
            font-size: 12px;
        }
    </style>
</head>
<body>

    <div class="container">
        <!-- 搜索区域 -->
        <div class="search-box">
            <!-- 添加了 id="searchInput" -->
            <input type="text" id="searchInput" placeholder="搜索本页链接..." autocomplete="off">
        </div>

        <!-- 动态生成内容 -->
        {{ range .Site.Data.links }}
            <!-- 给分类加个类名，方便整体隐藏 -->
            <div class="category-group">
                <h2>{{ .category }}</h2>
                <div class="link-grid">
                    {{ range .links }}
                    <!-- 给卡片加了 class="nav-item" -->
                    <a href="{{ .url }}" class="link-card nav-item" target="_blank">
                        <div class="link-icon">{{ .icon }}</div>
                        <div class="link-title">{{ .title }}</div>
                        <div class="link-desc">{{ .desc }}</div>
                    </a>
                    {{ end }}
                </div>
            </div>
        {{ end }}

    </div>

    <footer>
        &copy; {{ now.Year }} {{ .Site.Title }} | Powered by Hugo & Vercel
    </footer>

    <!-- 搜索脚本 -->
    <script>
        const searchInput = document.getElementById('searchInput');
        const navItems = document.querySelectorAll('.nav-item');
        const categoryGroups = document.querySelectorAll('.category-group');

        searchInput.addEventListener('input', function(e) {
            const term = e.target.value.toLowerCase().trim();

            // 1. 遍历每一个链接卡片
            navItems.forEach(item => {
                const title = item.querySelector('.link-title').innerText.toLowerCase();
                const desc = item.querySelector('.link-desc').innerText.toLowerCase();
                
                // 2. 如果包含关键词，显示；否则隐藏
                if (title.includes(term) || desc.includes(term)) {
                    item.classList.remove('hidden');
                } else {
                    item.classList.add('hidden');
                }
            });

            // 3. 智能隐藏空分类
            // 如果一个分类下的所有链接都被隐藏了，就把整个分类（包括标题）隐藏掉
            categoryGroups.forEach(group => {
                const visibleItems = group.querySelectorAll('.nav-item:not(.hidden)');
                if (visibleItems.length === 0) {
                    group.classList.add('hidden');
                } else {
                    group.classList.remove('hidden');
                }
            });
        });
    </script>

</body>
</html>
```

### 🛠️ 改动详解

1.  **HTML 结构微调**：
    -   我在外层加了一个 `<div class="category-group">` 包裹住标题和网格。
    -   给每个链接卡片加了 `nav-item` 类。
    -   给搜索框加了 `id="searchInput"`。

2.  **CSS 新增**：
    -   添加了 `.hidden { display: none !important; }` 类，用来隐藏不匹配的项。

3.  **JavaScript 逻辑**：
    -   **监听输入**：每次你打字，都会触发 `input` 事件。
    -   **匹配逻辑**：它会获取你输入的文字，然后去对比每个卡片的 **标题** 和 **描述**。
    -   **智能清理**：最酷的部分是最后一段逻辑——如果一个分类下的所有链接都不匹配（都被隐藏了），它会自动把那个分类的标题也隐藏掉，这样页面看起来非常清爽，不会出现空的分类标题。

### 🚀 如何更新

1.  把上面的代码复制，覆盖你项目里的 `layouts/index.html`。
2.  推送到 GitHub。
3.  Vercel 会自动重新部署。

现在去你的网站试试，输入 "代码" 或者 "视频"，页面会瞬间筛选出结果！

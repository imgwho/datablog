# 项目名称：imgwho (基于 VitePress 的博客)

这是一个使用 VitePress 构建的个人博客项目。

## ✨ 功能特性

*   **VitePress 驱动：** 享受 VitePress 带来的极速开发体验和优秀的 Markdown 支持。
*   **响应式设计：** 博客主题在不同设备上均有良好显示效果。
*   **内容组织：** 支持文章归档、标签分类等功能，方便内容管理和浏览。
*   **易于定制：** 可以通过修改 VitePress 配置和自定义主题轻松进行个性化设置。
*   **代码高亮与格式化：** 集成了 ESLint 和 Prettier，保证代码质量和风格统一。

## 🛠️ 技术栈

*   **核心框架：** [VitePress](https://vitepress.dev/) - Vue.js 驱动的静态站点生成器。
*   **前端库：** [Vue.js](https://vuejs.org/) - VitePress 的基础。
*   **语言：** [TypeScript](https://www.typescriptlang.org/) - 提供静态类型检查，增强代码可维护性。
*   **代码规范：**
    *   [ESLint](https://eslint.org/) - JavaScript 和 TypeScript 代码检查工具。
    *   [Prettier](https://prettier.io/) - 代码格式化工具。
*   **包管理：** 根据 `bun.lockb` 文件推断，项目可能使用 [Bun](https://bun.sh/) 作为包管理器，但 `package.json` 中的脚本也兼容 npm 和 yarn。

## 🚀 快速开始

### 1. 环境准备

确保你的开发环境中安装了 Node.js (推荐 LTS 版本)。如果你打算使用 Bun，请先安装 Bun。

### 2. 克隆项目 (如果适用)

```bash
git clone <repository-url>
cd datablog-main
```

### 3. 安装依赖

打开项目根目录下的终端，根据你选择的包管理器运行以下命令：

*   **使用 npm:**
    ```bash
    npm install
    ```
*   **使用 yarn:**
    ```bash
    yarn install
    ```
*   **使用 Bun:**
    ```bash
    bun install
    ```

### 4. 本地开发

启动本地开发服务器，实时预览你的更改：

```bash
npm run dev
```

该命令会启动一个开发服务器，通常监听在 `http://localhost:5000` (具体端口号请参考 <mcfile name="config.js" path=".vitepress/config.js"></mcfile> 中的 `server.port` 配置)。在浏览器中打开此地址即可查看博客。

### 5. 构建项目

当你准备将博客部署到线上时，执行以下命令进行静态文件构建：

```bash
npm run build
```

构建完成后，所有静态资源将输出到 `.vitepress/dist` 目录 (VitePress 默认)。

### 6. 预览构建结果

在部署前，可以在本地预览构建好的静态站点：

```bash
npm run preview
```

## 📁 项目结构概览

```
datablog-main/
├── .vitepress/               # VitePress核心配置和主题相关文件
│   ├── config.js             # VitePress站点的全局配置文件。定义了如标题、描述、主题、导航栏、侧边栏、Markdown插件等。
│   └── theme/                # 自定义主题目录，用于覆盖或扩展默认主题。
│       ├── components/       # 存放自定义的Vue组件，可以在Markdown中或主题布局中直接使用。
│       ├── custom.css        # 全局自定义CSS样式文件，用于调整站点外观。
│       ├── functions.ts      # 可能包含一些在构建过程或客户端运行时使用到的TypeScript辅助函数。
│       ├── index.js          # 自定义主题的入口文件，通常用于导出主题配置或注册全局组件。
│       └── serverUtils.js    # 服务端工具函数，例如在构建时获取文章列表、处理frontmatter等。
├── pages/                    # 存放站点的独立Markdown页面。
│   ├── about.md              # “关于我”页面。
│   ├── archives.md           # 文章归档页面。
│   ├── category.md           # 文章分类页面。
│   └── tags.md               # 文章标签云页面。
├── posts/                    # 存放所有博客文章的Markdown文件。
│   └── tfl-json.md           # 一篇示例博客文章。
├── public/                   # 存放不会被Vite处理的静态资源。
│   └── favicon.ico           # 网站的favicon图标。
├── share/                    # 共享文件目录，具体用途根据项目实际情况而定，可能包含一些项目特定的共享资源或工具。
│   ├── TFL_Processor.exe
│   ├── extensions-api.z01
│   ├── extensions-api.z02
│   ├── extensions-api.z03
│   ├── extensions-api.z04
│   ├── extensions-api.z05
│   └── extensions-api.zip
├── .editorconfig             # EditorConfig配置文件，帮助在不同编辑器和IDE中保持一致的编码风格。
├── .gitignore                # Git忽略规则文件，指定哪些文件或目录不应被Git跟踪。
├── .npmrc                    # npm配置文件，可以用来设置npm的镜像源、包安装行为等 (如果存在)。
├── .prettierrc.json          # Prettier代码格式化工具的配置文件。
├── bun.lockb                 # Bun包管理器的锁文件，确保依赖版本的一致性。
├── eslint.config.js          # ESLint代码检查工具的配置文件 (较新的ESM格式配置)。
├── LICENSE                   # 项目的开源许可证文件。
├── package.json              # Node.js项目的核心配置文件。定义了项目名称、版本、依赖、开发脚本 (如dev, build, preview)等。
├── shims-vue.d.ts            # TypeScript的Vue垫片声明文件，用于让TypeScript识别.vue文件。
└── tsconfig.json             # TypeScript编译器的配置文件，定义了编译选项、包含的文件等。
```

## ✍️ 内容创作与管理

*   **撰写新文章：** 在 `posts/` 目录下创建新的 `.md` 文件。VitePress 会自动将这些文件渲染为博客文章。你可以使用标准的 Markdown 语法以及 VitePress 支持的扩展语法。
*   **管理页面：** `pages/` 目录下的 Markdown 文件对应博客的独立页面。例如，你可以编辑 <mcfile name="about.md" path="pages/about.md"></mcfile> 来修改“关于我”页面的内容。
*   **Frontmatter：** 在 Markdown文件的顶部，你可以使用 YAML frontmatter 来定义文章或页面的元数据，例如标题、日期、标签、分类等。具体可配置项请参考 VitePress 文档和项目主题的实现。

##🎨 主题定制

项目的视觉外观和部分功能可以通过修改 `.vitepress/theme/` 目录下的文件进行定制：

*   **CSS 样式：** 在 <mcfile name="custom.css" path=".vitepress/theme/custom.css"></mcfile> 中添加或修改 CSS 规则以调整样式。
*   **Vue 组件：** 你可以在 `.vitepress/theme/components/` 目录下创建或修改 Vue 组件，并在 Markdown 中或主题布局中使用它们。
*   **布局：** VitePress 允许你覆盖默认的主题布局或创建全新的布局。详情请查阅 VitePress 官方文档关于主题的章节。
*   **配置文件：** <mcfile name="config.js" path=".vitepress/config.js"></mcfile> 是定制的核心，你可以在这里修改导航栏、侧边栏、社交链接、评论系统配置等。

## ⚙️ 配置项说明

主要的配置文件是 <mcfile name=".vitepress/config.js" path=".vitepress/config.js"></mcfile>。其中一些关键配置项包括：

*   `title`: 网站标题。
*   `description`: 网站描述，会用于 SEO。
*   `base`: 部署站点的基础路径 (例如，如果部署到 `https://example.com/blog/`，则应设置为 `/blog/`)。
*   `themeConfig`: 主题相关的配置。
    *   `nav`: 配置顶部导航栏链接。
    *   `posts`: (自定义配置) 可能用于存储和传递文章列表数据。
    *   `website`: (自定义配置) 版权链接。
    *   `comment`: (自定义配置) 评论系统 (如 Giscus) 的配置。
    *   `search`: 搜索功能配置。
    *   `outlineTitle`: 右侧大纲（目录）的标题。
*   `vite`: Vite 的底层配置，例如服务器端口 `server: { port: 5000 }`。


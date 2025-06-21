# 万象文档转换器 (Omni-Converter)

![Build Status](https://img.shields.io/github/actions/workflow/status/your-username/omni-converter/main.yml?branch=main)
![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Version](https://img.shields.io/badge/version-0.1.0-brightgreen.svg)

一个强大、直观、用户友好的桌面级Web应用，旨在实现 **Markdown (包括Jupyter Notebook), LaTeX, Typst, 和 HTML** 之间的高保真、双向无缝转换。

**本项目完全独立实现所有核心转换逻辑，不依赖 Pandoc。**

[English Version](./README.en.md) <!-- 如果您计划提供英文版 -->

## ✨ 核心特性

*   **多格式支持**: 在 Markdown, Jupyter Notebook, LaTeX, Typst, HTML 之间进行双向转换。
*   **星型架构**: 基于统一的中间表示 (IR/AST)，易于维护和扩展。
*   **高保真转换**: 尽最大努力保留文档的语义结构，包括标题、列表、代码、数学公式等。
*   **自研引擎**: 所有解析器和渲染器均为从零开始构建，提供完全的控制力和透明度。
*   **现代化界面**: 基于 Vue 3 和 TypeScript 的响应式、用户友好的操作界面。
*   **Docker化**: 提供 Docker 和 Docker Compose 配置，实现一键部署和开发环境搭建。

## 🏛️ 核心架构

本项目采用“星型”架构，以一个自定义的 **中间表示 (Intermediate Representation, IR)** 为核心。

```
                       +-------------------+
                       |    Markdown       |
                       +-------------------+
                                 | (Parser)
                                 V
+-------------------+      +-----------+      +-------------------+
|      HTML         | <==> |           | <==> |      LaTeX        |
+-------------------+      |           |      +-------------------+
 (Parser/Renderer)         |    IR     |        (Parser/Renderer)
                           |   (AST)   |
+-------------------+      |           |      +-------------------+
| Jupyter Notebook  | <==> |           | <==> |      Typst        |
+-------------------+      +-----------+      +-------------------+
                                 ^
                                 | (Renderer)
                       +-------------------+
                       |      (Any Target) |
                       +-------------------+
```
*   **解析器 (Parser)**: 将任何输入格式的文本（如 Markdown）解析成统一的 IR 结构。
*   **渲染器 (Renderer)**: 将 IR 结构渲染成任何目标格式的文本（如 LaTeX）。

这种架构使得添加新格式变得简单：只需为新格式实现一个解析器和一个渲染器即可。

## 🛠️ 技术栈

*   **后端**:
    *   **语言**: Python 3.10+
    *   **框架**: FastAPI
    *   **核心库**: Pydantic (数据校验)
*   **前端**:
    *   **语言**: TypeScript
    *   **框架**: Vue 3
    *   **构建工具**: Vite
    *   **UI 库**: Element Plus (或其他，如 Ant Design Vue)
    *   **代码编辑器**: CodeMirror 6
*   **容器化**: Docker & Docker Compose

## 🚀 快速开始

### 环境准备

请确保您的系统已安装以下软件：
*   [Git](https://git-scm.com/)
*   [Node.js](https://nodejs.org/) (v18+
*   [Python](https://www.python.org/) (v3.10+)
*   [Docker](https://www.docker.com/) 和 [Docker Compose](https://docs.docker.com/compose/) (推荐)

### 1. 使用 Docker (推荐)

这是最简单的启动方式，可以一键启动所有服务。

```bash
# 1. 克隆仓库
git clone https://github.com/forliage/Omni-Converter.git
cd omni-converter

# 2. 使用 Docker Compose 构建并启动服务
docker-compose up --build

# 完成！
# - 前端应用将在 http://localhost:5173 访问
# - 后端 API 将在 http://localhost:8000 提供服务
# - API 文档 (Swagger UI) 在 http://localhost:8000/docs
```

### 2. 本地手动启动

如果您希望在本地分别运行前后端服务进行开发。

**启动后端服务:**

```bash
# 1. 进入后端目录
cd backend

# 2. 创建并激活虚拟环境
python -m venv venv
source venv/bin/activate  # on Windows: venv\Scripts\activate

# 3. 安装依赖
pip install -r requirements.txt

# 4. 启动服务 (uvicorn 会提供热重载)
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

**启动前端服务 (在新的终端窗口中):**

```bash
# 1. 进入前端目录
cd frontend

# 2. 安装依赖
npm install

# 3. 启动开发服务器
npm run dev
```

## 🗺️ 开发路线图

- [ ] **阶段一: MVP**
    - [x] 定义基础版 IR (标题, 段落, 文本, 代码块)
    - [ ] 实现 `Markdown <--> HTML` 双向转换
    - [ ] 完成基本的前后端框架和UI界面
- [ ] **阶段二: 扩展核心格式**
    - [ ] 扩展 IR (数学公式, 列表)
    - [ ] 实现 `LaTeX` 和 `Typst` 的基础解析和渲染
- [ ] **阶段三: 支持复杂格式**
    - [ ] 扩展 IR (支持代码单元输出)
    - [ ] 实现 `Jupyter Notebook` 的解析和渲染
- [ ] **阶段四: 优化与完善**
    - [ ] 提升转换保真度 (表格, 图片)
    - [ ] UI/UX 优化 (实时预览, 错误高亮)
    - [ ] 完善单元测试和集成测试覆盖率

## 🙌 贡献

我们欢迎任何形式的贡献！如果您有好的想法或发现了 Bug，请随时提交 Pull Request 或 Issue。

1.  Fork 本仓库
2.  创建您的特性分支 (`git checkout -b feature/AmazingFeature`)
3.  提交您的更改 (`git commit -m 'Add some AmazingFeature'`)
4.  推送到分支 (`git push origin feature/AmazingFeature`)
5.  提交一个 Pull Request

## 📄 许可证

本项目基于 [MIT License](./LICENSE) 开源。
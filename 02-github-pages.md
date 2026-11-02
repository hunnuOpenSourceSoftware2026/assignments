# 02-github-pages｜实践项目：通过 GitHub Actions 自动部署静态文档到 GitHub Pages

本页供同学独立完成 MkDocs 文档网站搭建、自动部署及个人作业提交。

**本次作业要求：每人使用本人 GitHub 账号建立一个公开仓库，编写文档，配置 GitHub Actions，发布可访问的 GitHub Pages 网站。个人仓库中必须包含关键步骤截图与说明，最后由本人到教师仓库提交一条 Issue，填写个人作业地址和最终效果地址。**

**本次为个人作业，无需小组协作，不要求 Organization、组内审核或 PR，不得由组长代交。** 可直接向自己仓库的 `main` 分支提交，注意仓库设置成`public`。

**截止时间：2026-11/23日晚12:00**

## 一、实践目标与验收结果

完成后，你应能够：

1. 使用 Markdown 和 MkDocs Material 搭建四页文档网站。
2. 解释源码、构建产物、工作流和托管网站之间的关系。
3. 实现“推送文档 → 自动构建 → 自动部署 → 网页更新”。
4. 根据 Actions 日志定位问题，并用截图和文字记录过程。

最终交付三个结果：**个人公开仓库、公开网站、教师仓库中的个人提交 Issue**。

> 本文保留课堂图片中的文档结构与实践目标，部署部分统一使用 GitHub 官方 Pages Actions 工作流：上传 `site/` 构建产物后发布。无需创建 `gh-pages` 分支，无需开启创建/批准 PR 的权限，也无需自行申请或填写访问令牌。不要混用图片中的分支发布步骤。官方配置说明见文末参考资料。

## 二、准备环境和个人仓库

### 2.1 准备工具

准备本人 GitHub 账号、Git、Python 3.12 和文本编辑器（如 VS Code）。

以下 Git 命令可在 Windows Git Bash、macOS 或 Linux 终端执行。Python 命令以 `python` 为例；如果系统使用 `python3`，请对应替换。Windows 如只能使用 `py`，可使用 `py -3.12`。

```bash
git --version
python --version
python -m pip --version
```

### 2.2 创建仓库并克隆

1. 登录 GitHub，点击右上角 `+` → `New repository`。
2. Owner 选择**本人账号**；仓库名建议为 `my-docs`。
3. 选择 **Public**，勾选添加 README，创建仓库。
4. 确认默认分支为 `main`；若不是，先在仓库设置中调整为 `main`，以匹配下文工作流。
5. 点击 `Code`，复制自己的 HTTPS 克隆地址。

将下面的 `YOUR-USERNAME` 换成本人账号。仓库名不同也要一并替换，后文所有示例同理。

```bash
git clone https://github.com/YOUR-USERNAME/my-docs.git
cd my-docs
git branch --show-current
```

如尚未配置 Git 身份，在当前仓库执行：

```bash
git config user.name "本人GitHub登录名"
git config user.email "本人GitHub已验证邮箱或GitHub提供的noreply邮箱"
```

推送遇到认证提示时，使用 Git Credential Manager 或 GitHub 提供的浏览器登录流程；GitHub 账号密码不能直接作为 HTTPS Git 密码。

**记录截图①：个人公开仓库首页，能看清仓库所有者、仓库名和 Public 标识。**

## 三、创建文件结构并编写文档

在仓库根目录中创建以下文件。使用编辑器创建即可；所有文件保存为 UTF-8。

```text
my-docs/
├── .github/
│   └── workflows/
│       └── deploy.yml
├── docs/
│   ├── index.md
│   ├── about.md
│   ├── guide.md
│   └── api.md
├── report/
│   ├── README.md
│   └── images/
├── mkdocs.yml
├── requirements.txt
├── .gitignore
└── README.md
```

`docs/` 存放网站内容；`report/` 存放作业过程说明及截图。Git 不跟踪空目录，保存截图后 `images/` 才会出现在远程仓库中。

### 3.1 首页：docs/index.md

```markdown
# 我的技术文档

欢迎访问我的个人文档网站。本网站使用 MkDocs 构建，通过 GitHub Actions 自动部署到 GitHub Pages。

- 作者：填写本人 GitHub 登录名
- 实践主题：静态文档自动部署
- [关于本项目](about.md)
- [使用指南](guide.md)
- [API 参考](api.md)
```

### 3.2 关于页：docs/about.md

```markdown
# 关于本项目

## 项目目标

实现从 Markdown 文档到公开网站的自动化发布。

## 技术组成

| 工具 | 作用 |
| --- | --- |
| Markdown | 编写文档内容 |
| MkDocs Material | 将文档构建成静态网站 |
| GitHub Actions | 自动执行安装、构建和部署 |
| GitHub Pages | 托管并提供网站访问地址 |
```

### 3.3 使用指南：docs/guide.md

```markdown
# 使用指南

1. 修改 docs 目录中的 Markdown 文件。
2. 在本地预览并检查页面及链接。
3. 提交修改并推送到 main 分支。
4. 在 Actions 页面查看构建与部署状态。
5. 打开网站检查内容是否更新。

## 常用命令

- python -m mkdocs serve：本地预览。
- python -m mkdocs build --strict：构建网站并严格检查警告。
```

### 3.4 API 参考：docs/api.md

本次不要求开发真实后端，可用下面的**文档示例**练习 API 描述：

````markdown
# API 参考

以下接口仅用于文档展示，不提供真实服务。

## 查询文档列表

- 请求方法：GET
- 请求路径：/api/docs
- 功能：返回文档条目列表。

### 返回示例

```json
{
  "items": [
    {"id": 1, "title": "使用指南"}
  ]
}
```
````

**内容要求：**四页均需有标题和实质内容，不能只有空标题；至少包含一处表格、一段代码示例、页面间链接。请增加本人撰写的介绍或实践体会，不能仅替换示例中的姓名。

### 3.5 配置 mkdocs.yml

在根目录创建 `mkdocs.yml`，替换账号和仓库名：

```yaml
site_name: 我的技术文档
site_url: https://YOUR-USERNAME.github.io/my-docs/
repo_url: https://github.com/YOUR-USERNAME/my-docs
theme:
  name: material
  language: zh
nav:
  - 首页: index.md
  - 关于: about.md
  - 使用指南: guide.md
  - API参考: api.md
```

注意：`nav` 中的路径相对于 `docs/`，不要写成 `docs/index.md`。YAML 使用空格缩进，不能使用 Tab。

创建 `requirements.txt`：

```text
mkdocs>=1.6,<2
mkdocs-material>=9.6,<10
```

`mkdocs-material` 会安装主题及所需依赖；上述范围限定主要版本，实际安装的小版本可能随时间变化。

创建 `.gitignore`：

```gitignore
site/
.venv/
__pycache__/
```

`site/` 是自动生成的网站，不需要手工编写或提交到 `main`。

**记录截图②：项目目录和 mkdocs.yml，配文解释 docs、nav、site/ 的作用。**

## 四、本地构建和预览

建议在独立 Python 虚拟环境中安装依赖：

```bash
python -m venv .venv
```

根据自己的终端，**只执行一种激活命令**：

| 终端 | 激活命令 |
| --- | --- |
| Windows Git Bash | `source .venv/Scripts/activate` |
| Windows PowerShell | `.\.venv\Scripts\Activate.ps1` |
| macOS / Linux | `source .venv/bin/activate` |

如果 PowerShell 阻止脚本运行，可切换 Git Bash 激活，无需更改系统执行策略。

```bash
python -m pip install -r requirements.txt
python -m mkdocs build --strict
python -m mkdocs serve
```

在浏览器打开终端显示的地址，通常为 `http://127.0.0.1:8000/`。逐个打开四页，检查导航、表格和代码块。检查完后按 `Ctrl+C` 停止预览。

**记录截图③：本地构建成功的终端记录或本地预览，并说明检查了哪些内容。**

## 五、开启 Pages 并配置自动部署

### 5.1 设置 Pages 发布来源

在**个人作业仓库**进入：

`Settings` → `Pages` → `Build and deployment` → `Source` → **GitHub Actions**。

如果界面要求保存，请保存。再到 `Settings` → `Actions` → `General`，确认仓库允许使用本文工作流中的 Actions。

本方案在工作流中明确授予部署权限，不需要把仓库默认权限改为全局读写，也不需要勾选 `Allow GitHub Actions to create and approve pull requests`。

**记录截图④：Pages 发布来源选择 GitHub Actions 的设置页面。**

### 5.2 创建 .github/workflows/deploy.yml

完整复制以下内容，保留 `${{ ... }}` 表达式，不要替换成自己的账号。此处默认源代码分支是 `main`。

```yaml
name: Deploy MkDocs to GitHub Pages

on:
  push:
    branches: [main]
    paths:
      - 'docs/**'
      - 'mkdocs.yml'
      - 'requirements.txt'
      - '.github/workflows/deploy.yml'
  workflow_dispatch:

permissions:
  contents: read

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout source
        uses: actions/checkout@v7

      - name: Set up Python
        uses: actions/setup-python@v7
        with:
          python-version: '3.12'

      - name: Configure Pages
        uses: actions/configure-pages@v5

      - name: Install dependencies
        run: python -m pip install -r requirements.txt

      - name: Build documentation
        run: python -m mkdocs build --strict

      - name: Upload site artifact
        uses: actions/upload-pages-artifact@v4
        with:
          path: site

  deploy:
    needs: build
    runs-on: ubuntu-latest
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

理解以下配置，并在个人报告中用自己的话解释：

- `push`：指定文件推送到 `main` 时自动运行；只修改报告或 README 不会触发本工作流。
- `workflow_dispatch`：允许在 Actions 页面手动运行，适合首次重试和排错。
- `build`：取源码、配置 Python、安装依赖、构建并上传 `site/`。
- `needs: build`：构建成功后才执行部署。
- `pages: write`、`id-token: write`：授予官方 Pages 部署所需权限。
- `github-pages`：用于记录和管理网站部署的环境。

### 5.3 提交和推送

在项目根目录执行。提交前检查暂存内容，确保不包含虚拟环境和生成的 `site/`。

```bash
git status
git add docs mkdocs.yml requirements.txt .gitignore .github/workflows/deploy.yml README.md
git diff --staged
git commit -m "搭建MkDocs文档并配置Pages自动部署"
git push origin main
```

## 六、验证首次部署和自动更新

### 6.1 检查工作流和网站

1. 打开个人仓库的 `Actions`。
2. 进入 `Deploy MkDocs to GitHub Pages` 的本次运行记录。
3. 确认 `build` 和 `deploy` 均成功；如失败，展开红色步骤阅读日志。
4. 从部署记录或 `Settings` → `Pages` 获取网站实际地址。
5. 通常项目站点地址为 `https://YOUR-USERNAME.github.io/my-docs/`。必须带上仓库路径；以 GitHub 显示的实际地址为准。
6. 等待发布完成，使用浏览器访问网站，逐一打开首页、关于、使用指南和 API 参考，确认资源加载、导航及链接正常。

**记录截图⑤：成功的 Actions 运行记录，显示 build 与 deploy 状态。**

**记录截图⑥：公开网站效果，保留浏览器地址栏；另说明四页导航是否均已检查。**

### 6.2 再修改一次，证明能够自动更新

在 `docs/index.md` 增加一句本人编写、容易辨认的文字，例如：

```markdown
自动部署验证：这是我第二次推送后更新的内容。
```

提交并推送：

```bash
git add docs/index.md
git commit -m "验证文档修改后自动更新网站"
git push origin main
```

此处不要点击手动部署，观察这次 `push` 自动触发新的运行。等待成功后刷新公开网站，确认出现新增文字。必要时强制刷新或使用无痕窗口排除缓存。

**记录截图⑦：第二次 push 触发的成功运行记录，以及网站出现新增文字的效果。可以拆成两张截图。报告中附上对应提交和 Actions 运行链接。**

## 七、整理个人作业报告

在 `report/README.md` 中写完整报告，截图保存到 `report/images/`，使用相对路径嵌入，不要只写本地电脑路径。

建议结构：

```markdown
# 实践项目2：个人实验报告

## 1. 基本信息
- GitHub 登录名：
- 个人仓库地址：
- 最终网站地址：

## 2. 实践过程与关键截图
### 2.1 仓库创建
说明操作、结果和对应截图。
![个人仓库](images/01-repository.png)

### 2.2 项目结构与 MkDocs 配置
### 2.3 本地构建或预览
### 2.4 Pages 设置
### 2.5 Actions 首次构建和部署
### 2.6 网站访问与四页导航检查
### 2.7 第二次推送与自动更新验证
- 对应提交链接：
- 对应 Actions 运行链接：

## 3. 问题与解决
记录实际现象、关键日志、原因和解决操作。
如没有遇到错误，说明你核对过哪些日志和配置。

## 4. 原理理解
1. docs/ 和 site/ 各保存什么？为什么不用手工修改 site/？
2. 本次工作流在什么情况下自动运行？
3. build 与 deploy 两个任务有什么区别？
4. 只修改 report/README.md 会触发部署吗？为什么？
5. 本次方案为何无需创建 gh-pages 分支？
```

每个关键步骤均需配**本人实际截图及文字说明**，说明“做了什么、为什么这样做、结果是什么”。不接受只堆截图、只有教程原文或借用他人运行记录。

根目录 `README.md` 至少提供项目简介、网站链接、报告入口和本地预览命令。例如报告链接写为 `[个人实验报告](report/README.md)`。

保存后提交：

```bash
git add README.md report
git diff --staged
git commit -m "补充个人实验报告与关键步骤截图"
git push origin main
```

在 GitHub 网页打开报告，确认截图均可见、链接可点击。只修改报告不会触发部署，这是预期行为。

## 八、到教师仓库提交个人 Issue

打开[教师仓库 Issue 提交入口](https://github.com/hunnuOpenSourceSoftware2026/assignments/issues/new/choose)。选择适合的空白 Issue 入口（如 `Blank issue` / `Open a blank issue`），由**本人账号**提交一条个人作业 Issue。

**标题格式：** `02-github-pages chenwuyang(替换为个人姓名拼音)`

将下面模板复制到 Issue 正文并填写。个人作业地址应直接进入实验报告，不要只给个人主页；最终效果地址应直接进入部署后的网站。

```markdown
## 基本信息
- GitHub 登录名：

## 作业链接
- 个人作业地址（report/README.md）：
- 个人源码仓库地址：
- 最终效果地址（GitHub Pages 网站）：
- 第二次自动部署成功的 Actions 运行地址：

## 补充说明
填写仍需说明的问题；没有则写“无”。
```

每人只提交一条 Issue。更正时编辑原 Issue，并留言说明改动，不要重复开新 Issue。不要提交到个人仓库的 Issues，也不要提交小组汇总 Issue。

若教师设置了 `02-github-pages` 标签，按通知选择；无标签权限时由教师统一添加，不影响先提交。公开报告和 Issue 中不填写密码、令牌等敏感信息。

## 九、验收要求与评分建议（100 分）

| 项目 | 分值 | 验收依据 |
| --- | ---: | --- |
| 个人仓库与文件结构 | 10 | 本人公开仓库，源码、配置及报告目录完整 |
| 文档内容与页面效果 | 20 | 四页均有实质内容，含表格、代码、链接，导航正常 |
| 自动构建与部署 | 25 | 工作流配置正确，build、deploy 成功，网站公开可访问 |
| 自动更新验证 | 15 | 第二次 push 触发成功运行，提交记录与网页新增内容对应 |
| 个人报告与理解 | 20 | 关键步骤截图和说明完整，问题记录及五问回答准确 |
| Issue 提交规范 | 10 | 本人提交，个人报告地址、网站地址和验证链接完整有效 |
| **合计** | **100** | **全部为个人成绩** |

只提交截图、不提供可核验仓库和网站，不视为完成部署；只成功运行一次、不验证后续自动更新，不满足自动更新验收项。教师可结合提交记录、Actions 日志和现场解释核验本人完成情况。

提交前检查：

- [ ] 使用本人个人仓库，本次不要求小组、组织或审核 PR。
- [ ] 所有示例账号、仓库地址和个人介绍均已替换。
- [ ] 文件路径及大小写正确，YAML 使用空格缩进。
- [ ] Pages 来源为 GitHub Actions，未混用分支发布设置。
- [ ] 四页可访问，第二次推送已在网站生效。
- [ ] 报告截图完整且可见，解释为本人撰写。
- [ ] 本人已在教师仓库提交一条 Issue，两个核心地址正确。


## 参考资料

- [课程第一次作业：分组与仓库登记](https://github.com/hunnuOpenSourceSoftware2026/assignments/blob/main/00-group.md)
- [课程 Git 实验：个人分支与 PR](https://github.com/hunnuOpenSourceSoftware2026/assignments/blob/main/01-git-simple.md)
- [GitHub：使用自定义工作流部署 Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages)
- [GitHub：配置 Pages 发布来源](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)
- [actions/checkout](https://github.com/actions/checkout)
- [actions/setup-python](https://github.com/actions/setup-python)
- [MkDocs：文档部署](https://www.mkdocs.org/user-guide/deploying-your-docs/)

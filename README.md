# my-docs

使用 Markdown 和 MkDocs Material 编写四页技术文档，通过 GitHub Actions 构建并部署到 GitHub Pages。

- 源码仓库：[Ottor1/my-docs](https://github.com/Ottor1/my-docs)
- 网站预期地址：[Ottor1 的技术文档](https://ottor1.github.io/my-docs/)（首次部署成功后核对，以 Pages 显示的实际地址为准）
- [个人实验报告与截图](report/README.md)

## 本地环境与预览

本项目已在 Windows、Anaconda Python 3.13.9、项目内 `.venv` 下完成安装和严格构建验证。课程示例及云端工作流使用 Python 3.12，云端会独立安装依赖。

先进入仓库根目录。首次创建环境时执行一次：

```powershell
python --version
python -m venv .venv
```

如果 `.venv` 已存在，无需重复创建。直接指定它的解释器运行，可以手动选定环境，无需先激活：

```powershell
.\.venv\Scripts\python.exe -c "import sys; print(sys.executable); print(sys.version)"
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m pip check
.\.venv\Scripts\python.exe -m mkdocs build --strict
.\.venv\Scripts\python.exe -m mkdocs serve
```

打开终端显示的地址，本项目预览地址通常为 `http://127.0.0.1:8000/my-docs/`。按 `Ctrl+C` 停止服务。

如果要激活环境，PowerShell 使用 `.\.venv\Scripts\Activate.ps1`，Windows Git Bash 使用 `source .venv/Scripts/activate`，CMD 使用 `.venv\Scripts\activate.bat`。激活后可以把上述解释器路径简写为 `python`，退出使用 `deactivate`。若 PowerShell 阻止激活脚本，直接使用上面的解释器路径。

VS Code 中可按 `Ctrl+Shift+P` → `Python: Select Interpreter`，选择项目的 `.venv\Scripts\python.exe`，再核对终端中的解释器路径。

本次曾遇到镜像 HTTPS 下载中断，临时换官方源后安装成功：

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements.txt -i https://pypi.org/simple --timeout 30 --retries 1
```

## 项目结构

```text
my-docs/
├── .github/workflows/deploy.yml
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

`docs/` 是文档源码，`mkdocs.yml` 的 `nav` 决定导航和页面对应关系。`site/` 是构建产物，`.venv/` 是本地环境，均已加入 `.gitignore`，不提交到源码分支。`.git/` 是仓库元数据，保留供 Git 使用。

## 发布与作业提交

1. 在个人仓库的 `Settings → Pages → Source` 选择 **GitHub Actions**。
2. 本地检查后，由本人暂存、提交并推送至 `main`；检查不要包含 `.venv/` 和 `site/`。
3. 在 Actions 中确认 `Deploy MkDocs to GitHub Pages` 的 `build`、`deploy` 均成功，取得实际网站地址。
4. 检查公开网站四页，保存运行记录与保留地址栏的网站截图。
5. 首次部署成功后，再修改首页并推送，验证第二次 push 自动更新，记录提交及运行链接。
6. 把截图放入 `report/images/`，在报告中使用相对路径引用。补齐报告后在 GitHub 上检查图片和链接。
7. 使用本人账号向教师仓库提交一条 Issue。正文和剩余事项统一保存在[个人实验报告](report/README.md)。

推送文档、配置、依赖或工作流到 `main` 会触发部署；只修改报告或根 README 不触发。无需创建 `gh-pages` 分支。

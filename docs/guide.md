# 使用指南

这页整理本次实际使用的 Windows 本地操作。我采用项目内的 `.venv`，直接指定其中的解释器执行命令，便于确认每一步使用的 Python 环境。

## 1. 本次使用的环境

| 项目 | 本次实际配置 |
| --- | --- |
| 本地 Python | Anaconda 提供的 Python 3.13.9 |
| 项目虚拟环境 | 仓库根目录下的 `.venv` |
| MkDocs | 1.6.1 |
| MkDocs Material | 9.8.0 |
| 云端工作流 | 配置为 Python 3.12，实际运行结果在 Actions 中核对 |

课程示例使用 Python 3.12。本次现有的 3.13.9 已完成依赖安装和严格构建验证，所以我保留了这个本地环境；云端工作流继续按课程配置准备 3.12，两者分别安装各自的依赖。

## 2. 创建环境并明确选择解释器

先进入 `my-docs` 仓库根目录，确认可以看到 `requirements.txt` 和 `mkdocs.yml`。第一次准备环境时运行：

```powershell
python --version
python -m venv .venv
```

已经创建好 `.venv` 后，后续使用不必重复创建。下面的检查命令能够显示版本、解释器路径以及 pip 所在位置：

```powershell
.\.venv\Scripts\python.exe --version
.\.venv\Scripts\python.exe -c "import sys; print(sys.executable)"
.\.venv\Scripts\python.exe -m pip --version
```

本次版本为 `3.13.9`，路径应位于项目的 `.venv` 内。这里显式使用环境中的 `python.exe`，因此不需要先运行激活脚本。

若希望使用简短的 `python` 命令，也可以先在 PowerShell 执行 `.\.venv\Scripts\Activate.ps1`。新开终端后需要重新激活；若脚本被阻止，继续使用上面的完整解释器路径即可。

## 3. 安装依赖与本次下载排错

通常先运行：

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

这次安装时，清华镜像在下载 `requests` 的过程中连续重试，最后出现如下关键错误：

```text
HTTPSConnectionPool(host='pypi.tuna.tsinghua.edu.cn', port=443)
SSLEOFError: UNEXPECTED_EOF_WHILE_READING
```

我把这个现象记为下载连接中断。随后安装时临时改用官方 PyPI 源，成功完成依赖安装：

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements.txt -i https://pypi.org/simple --timeout 30 --retries 1
```

`-i` 指定本条命令使用的包索引地址，不会改写全局 pip 配置。安装完成后，再检查依赖和严格构建：

```powershell
.\.venv\Scripts\python.exe -m pip check
.\.venv\Scripts\python.exe -m mkdocs build --strict
```

本次依赖检查输出 `No broken requirements found.`，严格构建也已通过。这两步把“下载已经完成”进一步验证为“依赖没有缺失或版本冲突，当前文档可以生成网站”。

## 4. 本地预览和四页检查

```powershell
.\.venv\Scripts\python.exe -m mkdocs serve
```

打开终端显示的地址，通常为 `http://127.0.0.1:8000/`。网页打开后，我按下面的内容继续核对：

| 页面 | 检查重点 |
| --- | --- |
| [首页](index.md) | 个人介绍、导航表格与实践更新文字 |
| [关于](about.md) | 技术组成、问题记录和个人体会 |
| [使用指南](guide.md) | PowerShell 命令是否完整、能否按步骤操作 |
| [API 参考](api.md) | 参数表、JSON 缩进及响应字段说明 |

还应点击每页末尾的内部链接，确认能够跳转。修改 Markdown 后，预览服务会重新构建；按 `Ctrl+C` 停止。若 8000 端口被占用，可运行：

```powershell
.\.venv\Scripts\python.exe -m mkdocs serve -a 127.0.0.1:8001
```

## 5. 让修改出现在公开网站上

本地预览读取本机文件，公开网站读取已经部署的产物。更新公开网站的操作顺序是：

1. 修改 `docs/` 中的 Markdown，并完成本地检查。
2. 由仓库所有者提交并推送到 `main`。
3. 在 Actions 中查看本次 `Deploy MkDocs to GitHub Pages`，确认 `build` 和 `deploy` 的结果。
4. 打开 Pages 的实际网站地址，刷新后核对首页日期和新增文字。
5. 将对应提交、运行链接及真实截图补充到实验报告。

首次设置时，仓库 `Settings → Pages → Source` 应选择 **GitHub Actions**。本工作流在 `main` 的文档、配置、依赖或工作流自身发生匹配的推送变更时自动运行；只修改报告或根 README 不会触发部署。

Python 虚拟环境负责本地 Python 命令。Git 命令可以直接执行，GitHub Actions 则在云端单独准备 Python 和依赖。

## 6. 我会保留的操作习惯

这次实践后，我会在安装前确认解释器，在报错时保留关键日志，处理后运行依赖检查和严格构建。看到网页时，再检查地址、内容和链接，把终端结果与页面效果对应起来。

继续阅读：[关于本项目](about.md) · [API 参考](api.md) · [返回首页](index.md)

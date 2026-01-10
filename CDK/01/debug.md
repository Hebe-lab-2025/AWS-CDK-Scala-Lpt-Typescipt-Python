```
yonggan@Yonggans-MacBook-Pro cfn-lint-visual-studio-code % pip install cfn-lint
zsh: command not found: pip
yonggan@Yonggans-MacBook-Pro cfn-lint-visual-studio-code % pip3 install cfn-lint
error: externally-managed-environment

× This environment is externally managed
╰─> To install Python packages system-wide, try brew install
    xyz, where xyz is the package you are trying to
    install.
    
    If you wish to install a Python library that isn't in Homebrew,
    use a virtual environment:
    
    python3 -m venv path/to/venv
    source path/to/venv/bin/activate
    python3 -m pip install xyz
    
    If you wish to install a Python application that isn't in Homebrew,
    it may be easiest to use 'pipx install xyz', which will manage a
    virtual environment for you. You can install pipx with
    
    brew install pipx
    
    You may restore the old behavior of pip by passing
    the '--break-system-packages' flag to pip, or by adding
    'break-system-packages = true' to your pip.conf file. The latter
    will permanently disable this error.
    
    If you disable this error, we STRONGLY recommend that you additionally
    pass the '--user' flag to pip, or set 'user = true' in your pip.conf
    file. Failure to do this can result in a broken Homebrew installation.
    
    Read more about this behavior here: <https://peps.python.org/pep-0668/>

note: If you believe this is a mistake, please contact your Python installation or OS distribution provider. You can override this, at the risk of breaking your Python installation or OS, by passing --break-system-packages.
hint: See PEP 668 for the detailed specification.
yonggan@Yonggans-MacBook-Pro cfn-lint-visual-studio-code %

```
你这台 Mac 的 Python 是 **Homebrew 管理的“externally-managed environment”**（PEP 668），所以 **不允许用 pip3 往系统 Python 里装包**，会报你看到的这个错。解决方案有 3 个，按推荐顺序来：

---

## ✅ 方案 A（最推荐）：用 `pipx` 安装 `cfn-lint`（专门装 CLI 工具）

`cfn-lint` 本质是命令行工具，用 pipx 最干净，不污染系统 Python。

```bash
brew install pipx
pipx ensurepath
# 关闭并重新打开 Terminal，或执行：source ~/.zshrc

pipx install cfn-lint
cfn-lint --version
```

如果你要升级：

```bash
pipx upgrade cfn-lint
```

---

## ✅ 方案 B：用虚拟环境 venv（适合项目级依赖）

在你当前目录（或任意目录）创建一个 venv，然后在里面装：

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -U pip
python -m pip install cfn-lint
cfn-lint --version
```

以后每次用之前先激活：

```bash
source .venv/bin/activate
```

---

## ✅ 方案 C：直接用 Homebrew 安装（看 brew 是否有对应公式）

有些机器上可以直接：

```bash
brew install cfn-lint
cfn-lint --version
```

如果 brew 没这个包，就用方案 A（pipx）一定能装。

---

## 你遇到的两个点分别是什么

* `zsh: command not found: pip`：你系统里 **没把 pip（pip for Python2）放进 PATH**，但有 `pip3`（Python3 的 pip）。
* `externally-managed-environment`：Homebrew 的 Python **禁止系统级 pip install**，必须用 `pipx` / `venv` / `brew`。

---

## 最快一句话

想装 `cfn-lint` 这种命令行工具：**直接用 pipx**（方案 A）。

你现在已经把 **pipx 装好了** ✅
报错的原因很简单：你跑的是 `pipx upgrade cfn-lint`，但 **你还没 install 过 cfn-lint**，所以它说 “Package is not installed”。

按下面顺序做就行（直接复制粘贴）：

---

## 1) 让 PATH 立刻生效（不用重开终端）

```bash
source ~/.zshrc
```

（如果你不想 source，也可以直接关掉当前 Terminal 窗口再开一个。）

---

## 2) 用 pipx 安装 cfn-lint（不是 upgrade）

```bash
pipx install cfn-lint
```

---

## 3) 验证是否安装成功

```bash
cfn-lint --version
which cfn-lint
```

---

## 4) 以后升级才用 upgrade

```bash
pipx upgrade cfn-lint
```

---

### 你日志里那个 brew 报错要不要管？

这一行：
`Migrating cask git-credential-manager-core ... Error: inreplace failed ...`
**跟 pipx / cfn-lint 没关系**，pipx 已经成功安装了（🍺 pipx 1.8.0）。可以先忽略。

---

如果你执行完第 2 步后提示 `pipx: command not found` 或 `cfn-lint: command not found`，把这两条输出贴出来：

1. `echo $PATH`
2. `ls -la ~/.local/bin | head`


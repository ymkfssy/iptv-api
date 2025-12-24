# IPTV-API 测试环境搭建与修复教程

## 1. 环境要求

| 软件/组件 | 版本要求 | 用途 |
|---------|--------|------|
| 操作系统 | Windows 10/11, macOS 10.15+, Ubuntu 20.04+ | 运行测试环境 |
| Python | 3.13 | 项目开发语言 |
| Git | 2.20+ | 代码版本控制 |
| FFmpeg | 4.0+ | 视频处理、分辨率检测 |
| pipenv | 2022.12.19+ | Python 虚拟环境管理 |

## 2. 环境搭建步骤

### 2.1 安装 Python 3.13

#### Windows 安装
1. 访问 [Python 官网](https://www.python.org/downloads/) 下载 Python 3.13 安装包
2. 运行安装程序，勾选「Add Python 3.13 to PATH」
3. 选择「Customize installation」，保持默认选项继续
4. 选择安装路径（建议使用默认路径），点击「Install」
5. 安装完成后，点击「Close」

#### macOS 安装
1. 安装 Homebrew（如果未安装）：
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```
2. 使用 Homebrew 安装 Python 3.13：
   ```bash
   brew install python@3.13
   ```
3. 将 Python 3.13 添加到 PATH：
   ```bash
   echo 'export PATH="/usr/local/opt/python@3.13/bin:$PATH"' >> ~/.zshrc
   source ~/.zshrc
   ```

#### Ubuntu 安装
1. 添加 Deadsnakes PPA：
   ```bash
   sudo add-apt-repository ppa:deadsnakes/ppa
   sudo apt update
   ```
2. 安装 Python 3.13：
   ```bash
   sudo apt install python3.13 python3.13-dev python3.13-venv
   ```
3. 安装 pip：
   ```bash
   curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
   python3.13 get-pip.py
   ```

#### 验证安装
```bash
python3.13 --version
pip3.13 --version
```

### 2.2 安装 Git

#### Windows 安装
1. 访问 [Git 官网](https://git-scm.com/download/win) 下载 Git 安装包
2. 运行安装程序，保持默认选项即可

#### macOS 安装
```bash
brew install git
```

#### Ubuntu 安装
```bash
sudo apt install git
```

#### 验证安装
```bash
git --version
```

### 2.3 安装 FFmpeg

#### Windows 安装
1. 访问 [FFmpeg 官网](https://ffmpeg.org/download.html) 下载 Windows 版本
2. 解压下载的 ZIP 文件到 `C:\ffmpeg`
3. 将 `C:\ffmpeg\bin` 添加到系统环境变量 PATH 中

#### macOS 安装
```bash
brew install ffmpeg
```

#### Ubuntu 安装
```bash
sudo apt install ffmpeg
```

#### 验证安装
```bash
ffmpeg -version
```

### 2.4 克隆项目代码

```bash
git clone https://github.com/Guovin/iptv-api.git
cd iptv-api
```

### 2.5 安装 pipenv

```bash
pip3.13 install --user pipenv
```

将 pipenv 添加到 PATH：
- **Windows**：将 `%USERPROFILE%\AppData\Roaming\Python\Python313\Scripts` 添加到环境变量 PATH
- **macOS/Linux**：
  ```bash
  echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc  # 或 ~/.bashrc
  source ~/.zshrc
  ```

验证安装：
```bash
pipenv --version
```

### 2.6 安装项目依赖

```bash
pipenv --python 3.13
pipenv install --deploy
```

### 2.7 配置项目

1. 复制示例配置文件：
   ```bash
   cp config/user_config.ini config/config.ini
   ```

2. 编辑配置文件（可选）：
   ```bash
   # Windows
   notepad config/config.ini
   
   # macOS
   open -a TextEdit config/config.ini
   
   # Ubuntu
   nano config/config.ini
   ```

3. 基本配置示例（`config/config.ini`）：
   ```ini
   [basic]
   open_driver = False
   open_epg = True
   open_empty_category = False
   open_filter_resolution = True
   open_filter_speed = True
   open_hotel = False
   open_local = True
   open_m3u_result = True
   open_multicast = False
   open_online_search = False
   open_request = False
   open_rtmp = False
   open_service = True
   open_speed_test = True
   open_subscribe = True
   open_supply = True
   open_update = True
   open_update_time = True
   open_url_info = False
   open_use_cache = True
   open_history = True
   open_headers = False
   
   [server]
   app_host =
   app_port = 8000
   cdn_url =
   
   [filter]
   min_resolution = 1920x1080
   max_resolution = 1920x1080
   min_speed = 0.5
   isp =
   location =
   
   [number]
   hotel_num = 10
   local_num = 10
   multicast_num = 10
   online_search_num = 0
   ipv4_num = 5
   ipv6_num = 5
   ```

## 3. 运行项目

### 3.1 启动更新脚本

```bash
pipenv run dev
```

### 3.2 启动 Web 服务

```bash
pipenv run service
```

### 3.3 验证项目运行

1. 更新脚本运行成功会显示：
   ```
   🥳 Update completed! Total time spent: XX.XX seconds.
   ```

2. Web 服务启动成功会显示：
   ```
   ✅ You can use this url to watch IPTV 📺: http://your-ip-address:8000
   ```

3. 访问 http://localhost:8000 验证服务是否正常

## 4. 常见错误排查

### 4.1 Python 版本错误

**错误信息**：
```
Warning: Python 3.13 was not found on your system...
```

**解决方案**：
1. 检查 Python 版本：
   ```bash
   python --version
   python3 --version
   ```
2. 如果安装了 Python 3.13 但无法识别，尝试使用完整路径：
   ```bash
   # Windows
   pipenv --python "C:\Python313\python.exe"
   
   # macOS
   pipenv --python "/usr/local/opt/python@3.13/bin/python3.13"
   
   # Ubuntu
   pipenv --python "/usr/bin/python3.13"
   ```

### 4.2 依赖安装失败

**错误信息**：
```
Error: An error occurred while installing...
```

**解决方案**：
1. 清除 pipenv 缓存：
   ```bash
   pipenv --clear
   ```
2. 重新安装依赖：
   ```bash
   pipenv install --deploy
   ```
3. 如果特定包安装失败，尝试单独安装：
   ```bash
   pipenv install 包名
   ```

### 4.3 FFmpeg 未找到

**错误信息**：
```
FileNotFoundError: [WinError 2] 系统找不到指定的文件。
```

**解决方案**：
1. 确认 FFmpeg 已安装：
   ```bash
   ffmpeg -version
   ```
2. 如果未安装，请按照 2.3 节重新安装
3. 如果已安装但无法识别，检查环境变量 PATH 是否正确配置

### 4.4 端口被占用

**错误信息**：
```
OSError: [WinError 10048] 通常每个套接字地址(协议/网络地址/端口)只允许使用一次。
```

**解决方案**：
1. 修改配置文件中的端口号：
   ```ini
   [server]
   app_port = 8080  # 修改为其他端口
   ```
2. 或关闭占用端口的程序：
   ```bash
   # Windows
   netstat -ano | findstr :8000
   taskkill /PID 进程ID /F
   
   # macOS/Linux
   lsof -i :8000
   kill -9 进程ID
   ```

### 4.5 更新脚本运行缓慢或无响应

**解决方案**：
1. 关闭浏览器驱动（如果开启）：
   ```ini
   [basic]
   open_driver = False
   ```
2. 关闭不需要的功能：
   ```ini
   [basic]
   open_hotel = False
   open_multicast = False
   open_online_search = False
   ```
3. 增加请求超时时间：
   ```ini
   [advanced]
   request_timeout = 15
   ```

## 5. 测试环境修复

### 5.1 重置虚拟环境

如果依赖出现严重问题，可以重置虚拟环境：

```bash
pipenv --rm
pipenv --python 3.13
pipenv install --deploy
```

### 5.2 重置配置文件

```bash
cp config/user_config.ini config/config.ini
```

### 5.3 清理缓存文件

```bash
rm -rf output/data/*
rm -rf output/log/*
```

### 5.4 更新项目代码

```bash
git pull
pipenv install --deploy  # 更新依赖
```

### 5.5 完全重新搭建环境

1. 清理现有环境：
   ```bash
   pipenv --rm
   rm -rf .venv
   git clean -fd
   ```

2. 重新按照 2-3 节的步骤搭建环境

## 6. 高级测试配置

### 6.1 启用调试模式

```ini
[advanced]
debug = True
```

### 6.2 配置定时任务

```ini
[advanced]
update_interval = 12  # 每 12 小时更新一次
```

### 6.3 使用代理

```ini
[advanced]
proxy = http://127.0.0.1:7890
```

## 7. 验证测试环境

### 7.1 验证基础功能

1. 运行更新脚本并检查输出：
   ```bash
   pipenv run dev
   ```
   - 检查是否成功获取频道数据
   - 检查是否生成了 M3U 播放列表

2. 检查生成的文件：
   ```bash
   ls -la output/
   ```
   - 应该看到 `result.m3u`、`result.txt` 等文件
   - 如果启用了 IPv4/IPv6，应该看到 `ipv4/`、`ipv6/` 目录

### 7.2 验证 Web 服务

1. 启动 Web 服务：
   ```bash
   pipenv run service
   ```

2. 使用 curl 测试 API：
   ```bash
   curl http://localhost:8000
   curl http://localhost:8000/txt
   curl http://localhost:8000/m3u
   ```

3. 使用浏览器访问：
   - http://localhost:8000 - 默认 M3U 播放列表
   - http://localhost:8000/txt - 文本格式播放列表
   - http://localhost:8000/ipv4 - IPv4 M3U 播放列表
   - http://localhost:8000/ipv6 - IPv6 M3U 播放列表

## 8. 常见问题解答

### Q1: 为什么更新后没有生成 M3U 文件？
A1: 请检查配置文件中的 `open_m3u_result` 是否设置为 `True`。

### Q2: 为什么播放列表中的频道很少？
A2: 可能是因为过滤条件太严格，可以尝试：
- 降低最小速率要求：`min_speed = 0.1`
- 降低最小分辨率要求：`min_resolution = 1280x720`
- 开启补偿机制：`open_supply = True`

### Q3: 为什么 Web 服务无法访问？
A3: 请检查：
- 服务是否正常启动
- 端口是否正确配置
- 防火墙是否允许该端口访问

### Q4: 如何更新到最新版本？
A4: 运行以下命令：
```bash
git pull
pipenv install --deploy
```

## 9. 联系方式

如果您在搭建或修复测试环境过程中遇到问题，可以通过以下方式寻求帮助：

- GitHub Issues: https://github.com/Guovin/iptv-api/issues
- 项目讨论区: https://github.com/Guovin/iptv-api/discussions

---

希望这份教程能帮助您顺利搭建和修复 IPTV-API 的测试环境！如果您有任何改进建议，欢迎提交 Issue 或 Pull Request。

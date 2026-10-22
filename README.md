# 洛谦 · Lab

明洛谦的个人空间。Python Flask + SQLite，白底蓝色科技风，适合记录计算机学习、分享笔记、管理学习路径和处理日常文件。

## 已实现

- **个人主页**：个人介绍、最近文章、学习进度、工具入口。名字、站点名、简介、邮箱和 GitHub 可在后台修改。
- **博客**：Markdown 编辑与预览、分类、关键词搜索、分页、草稿与发布。
- **图片与文件**：每篇最多 10 个附件；支持图片、PDF、Office 文档、文本、代码和 ZIP。图片可插入正文，也可在正文后展示。草稿附件仅管理员可访问。
- **学习路径**：每个阶段使用独立卡片，自动按 1、2、3 编号；名称和多行描述分开填写，支持上移、下移、添加、删除与撤销删除。勾选完成状态后自动计算进度，每个阶段可单独填写英文版本。较长的实践记录可以单独写成博客文章。
- **工具箱（10 项）**：图片转 PDF、PDF 合并、PDF 提取文字、PDF 页面提取、JSON 格式化、Markdown → HTML、进制转换、图片水印、图片压缩/格式转换、时间戳转换。支持分类与搜索。
- **管理后台**：单一站长账号、密码哈希、登录限流、CSRF 校验、Markdown HTML 清理。
- **网站导航**：AI 工具、代码仓库、文档与其他网站分类，支持搜索；后台手动添加、编辑、隐藏、排序和删除。预置 ChatGPT、Claude、DeepSeek、GitHub、Gitee 的官方入口，可替换成自己的具体仓库。
- **中英文界面**：右上角 EN / 中文切换并记住选择；首次访问参考浏览器语言。首页、博客、学习路径、导航、留言、工具提示和后台表单均有英文界面。
- **阅读量**：文章卡片、详情与后台展示阅读量；同一浏览器对同一文章 30 分钟内只计一次，站长访问不计数。
- **访客留言与文章评论**：无需注册，填写名字、联系方式和正文即可提交。默认审核后公开；后台可以审核、回复、隐藏或删除，联系方式只在后台显示。
- **部署准备**：Docker Compose、Gunicorn、Nginx 配置和可选 systemd 服务。

本次学习路径改版已完成本地验证，**尚未同步到阿里云上的运行版本**。更新服务器源码并重新构建后生效。Python 应用在你的 Linux 服务器上独立运行，不依赖 Sites 托管。

## 本地启动

需要 Python 3.11 或更高版本，建议 3.12。当前工作目录已安装 `.venv`，可直接执行下面的「已有环境」命令。

### 已有环境

```bash
cd /Users/ryantyy/Documents/ChatGPT/personalweb
.venv/bin/python -m flask --app app run --host 127.0.0.1 --port 5050
```

浏览器打开 <http://127.0.0.1:5050>。后台地址是 <http://127.0.0.1:5050/admin/>。

### 新环境

```bash
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.lock.txt
flask --app app create-admin
# 可选：添加明确标注的示例文章与学习路径
flask --app app seed-demo
flask --app app run --host 127.0.0.1 --port 5050
```

没有默认管理员密码。`create-admin` 会交互式询问用户名与密码（至少 12 位），再次运行也可重置密码并使旧登录失效。

当前目录创建管理员：

```bash
.venv/bin/python -m flask --app app create-admin
```

本地开发不需要 `.env`；应用会在 `instance/.secret` 中生成本机随机会话密钥。不要把 `.env.example` 直接当作已配置的生产密钥。

## 怎么使用

1. 创建管理员，打开 `/admin/` 登录。
2. 点「写一篇文章」，填写标题、分类和 Markdown 正文。可以保存草稿或发布，支持预览。
3. 通过侧栏上传图片和附件；保存后，已上传图片旁会出现「插入正文」。未插入的图片也会显示在正文下方。
4. 点「新建学习路径」，填写路径名称和介绍，然后在每张阶段卡片中填写名称与描述。描述支持回车换行、空行分段，中文和英文分别最多 2000 字。使用「添加阶段」「上移」「下移」管理顺序，编号自动更新；删除后可以撤销，保存后才应用到网站。每条路径保留 1–50 个阶段。

5. 登录后，在学习路径页点击阶段圆圈即可切换完成状态。
6. 在「个人资料」修改介绍和公开联系方式。
7. 在后台「网站导航」管理常用网站和具体仓库链接。英文名称与介绍可选填。
8. 在后台「留言审核与回复」查看待审核留言、私密联系方式，选择「公开」并保存后对外展示。站长回复会公开显示；可将留言改为隐藏或删除。

## 语言、留言与统计说明

- **网站导航**：公开页 `/links`，后台 `/admin/links`。链接仅保存和展示，不由服务器抓取远程内容；私有仓库仍需在 GitHub / Gitee 等原站登录。不要将带密钥或访问令牌的地址放进公开导航。
- **中英文**：导航右侧切换语言，偏好保存于会话 Cookie。文章编辑器、个人资料和学习路径编辑器有可展开的英文填写栏。每张阶段卡片的英文版本随该阶段一起排序，完成状态共用；未填写的英文字段回退到中文内容。
- **内容翻译**：这是界面翻译和可选的人工英文版本，不会调用第三方服务自动翻译或传送你的文章。未填写英文正文时保留原文并提示访客；原有明确标注的示例文章和示例路径已有英文示例。真实文章、访客留言与自定义名称不会被自动改写。
- **留言**：留言板 `/guestbook`；每篇已发布文章底部也可以评论。名字 1–40 字、联系方式 3–160 字、正文 1–2000 字。邮箱、微信、QQ 等都可作为联系方式；未进行邮箱或身份验证。
- **隐私与审核**：公开查询只读取名字、正文、时间和站长回复，不读取联系方式。联系方式仅在经过管理员登录的 `/admin/comments` 显示。请保护生产数据库和备份。默认开启审核；后台关闭审核只影响新留言，不会自动公开旧的待审核内容。
- **反垃圾留言**：CSRF、隐藏诱捕字段、每个 IP 至少间隔一分钟且每小时最多 5 条。限制表只保留短期的带密钥哈希，不保存原始 IP；服务器访问日志仍遵循自己的日志配置。
- **阅读量口径**：文章页面在浏览器可见时发起计数请求；普通 GET、HEAD、预抓取和站长阅读不计数。使用会话随机令牌的不可逆哈希去重，不保存浏览者联系方式或原始 IP。同一浏览器、同一文章 30 分钟内不重复计数；清除 Cookie、换浏览器或超过窗口后可再次计数，因此这不是严格的独立访客统计。旧文章从 0 开始，无法补回过去的阅读次数。
- **升级数据**：首次启动自动添加表和字段，保留原有文章、账号和附件。此次本地升级前备份为 `instance/backups/pre-community-20261016.sqlite3`；后续部署前仍建议按备份章节自行备份线上数据。

首页已经添加 3 篇示例文章和 1 条示例路径。它们不代表你的真实学习经历，可在后台编辑或删除；编辑保存后会去掉示例标识。示例初始化命令不会重复添加。

## 工具的范围

| 工具 | 输入 | 输出与限制 |
| --- | --- | --- |
| 图片转 PDF | JPG / PNG / WEBP，最多 20 张 | 一张一页；按文件列表顺序合并；最长边压缩到 2400 像素；单张 2000 万像素、总计 6000 万像素以内 |
| PDF 合并 | 2–10 份 PDF | 一份 PDF，总计最多 200 页；按文件列表顺序合并 |
| PDF 提取文字 | 一份文本型 PDF，最多 200 页 | UTF-8 TXT；不包含 OCR，不支持扫描件识别 |
| PDF 页面提取 | 一份 PDF + 页码 | 如 `3, 1-2`；按输入顺序输出，重复页码去重 |
| JSON | 文本 | 在浏览器本地格式化、压缩、校验、复制；超出 JavaScript 安全整数范围的 ID 请使用字符串 |
| Markdown → HTML | Markdown 文本，最多 20 万字符 | 安全预览、HTML 源码、复制、独立 HTML 网页下载；在本站后端转换，不保存正文 |
| 进制转换 | 2–36 进制整数，最多 4096 字符 | 支持正负整数、常见前缀和大整数精确运算；不支持小数 |
| 图片加水印 | 一张 JPG / PNG / WEBP | 文字、位置（含斜向平铺）、颜色、字号和不透明度可调；预览并导出 PNG |
| 图片压缩/格式转换 | 一张 JPG / PNG / WEBP | 等比缩小、质量调整、JPG / PNG / WEBP 导出；PNG 不使用有损质量参数，WEBP 导出取决于浏览器支持 |
| 时间戳转换 | 秒 / 毫秒时间戳或日期 | 双向转换；北京时间、UTC、浏览器时区；支持负时间戳及毫秒；夏令时重复时刻返回其中一个有效时间戳 |

单次请求总上传量不超过 32 MB；加密 PDF 需先解密。PDF 转换文件只在请求处理时使用，不永久保存。浏览器、反向代理或 WSGI 服务可能短暂缓冲请求；博客附件则会永久存储到 `instance/uploads/`，两者用途不同。每个 IP 每分钟最多发起 12 次 PDF 转换请求。第一版未包含 PDF 转 Word、OCR、评论系统或多用户注册。

新增的图片水印和压缩功能全部在浏览器本地执行，不上传图片；每张限制为 32 MB、2000 万像素、单边 16384 像素。转换不会保留 EXIF 等原图元数据。PNG/WEBP 可保留透明度，JPG 透明区域填充白色。压缩效果取决于原图与参数，输出可能比原图更大，会在界面显示实际体积变化。Markdown 导出的 HTML 不会打包图片，独立使用时建议用完整图片 URL。

## 部署到阿里云 ECS（Docker 方式）

推荐 Linux ECS、Python 3.12 容器、Nginx 反向代理。正式服务使用 Gunicorn，不能使用 Flask 开发服务器。部署前需要知道 ECS 的 Linux 发行版、SSH 登录方式，以及是否已有域名/Nginx/Docker；按实际服务器调整安装步骤。

当前约定的服务器源码目录为 `/home/admin/myproject/personweb`。下面的 Docker 命令都在此目录执行；容器内部的 `/app/instance` 不需要改成宿主机路径。

已确认服务器为 **Ubuntu 24.04 LTS、x86_64（APT 架构为 amd64）**。该服务器的完整安装命令见 [Ubuntu 24.04 部署步骤](deploy/ubuntu-24.04.md)。

### 0. 服务器需要安装的软件

| 软件 | 版本与用途 | 安装位置 |
| --- | --- | --- |
| Docker Engine / CLI | 对应系统的软件源提供的稳定版；项目未锁定宿主机小版本 | 云服务器 |
| Docker Compose 插件 | 当前稳定版，包名 `docker-compose-plugin`，使用 `docker compose` 命令 | 云服务器 |
| Docker Buildx 插件 | 随 Docker 软件源安装，包名 `docker-buildx-plugin` | 云服务器 |
| Nginx | 系统软件源提供且持续维护的版本，负责对外访问和 HTTPS | 云服务器 |
| curl、ca-certificates、tar、OpenSSL | 系统软件源版本，用于下载、解压、检查和生成密钥 | 云服务器 |
| Python | `3.12.x`，镜像为 `python:3.12-slim` | 自动装入容器 |
| Flask / Gunicorn | `3.1.3` / `23.0.0`；其他 Python 库见 `requirements.lock.txt` | 构建镜像时自动安装 |

使用这套 Docker 配置时，无需在宿主机另外安装 Python、pip、MySQL 或 Node.js。SQLite 已包含在 Python 环境中。

安装命令必须与实际发行版、版本和架构对应。先在云服务器执行 `cat /etc/os-release` 和 `uname -m`，再选择 [Docker 官方安装说明](https://docs.docker.com/engine/install/) 或 [阿里云 Docker 安装说明](https://help.aliyun.com/zh/ecs/user-guide/install-and-use-docker)。如果已经安装 Docker、Nginx 或宝塔，先检查现有版本及服务，不要直接卸载重装。

### 1. 上传源码并配置环境

上传源码到 `/home/admin/myproject/personweb`。不要上传本机 `.venv/`、`.git/`、`.env`；第一次部署可以从空数据开始。服务器需要已安装 Docker Engine 和 Compose 插件。

已打包的 `release/luoqian-lab-source.tar.gz` 包含顶层目录 `luoqian-lab/`。在 Mac 的本地终端上传（将登录用户名和公网 IP 换成实际值）：

```bash
scp /Users/ryantyy/Documents/ChatGPT/personalweb/release/luoqian-lab-source.tar.gz 登录用户名@服务器公网IP:/tmp/
ssh 登录用户名@服务器公网IP
```

连接成功后，在云服务器执行以下命令。`--strip-components=1` 会去掉压缩包内的顶层目录，确保 `app.py` 直接位于 `personweb/` 下。以下适用于第一次部署；已有线上源码时先备份再更新。

```bash
mkdir -p /home/admin/myproject/personweb
tar -xzf /tmp/luoqian-lab-source.tar.gz -C /home/admin/myproject/personweb --strip-components=1
cd /home/admin/myproject/personweb
ls -la
```

以上目录应对登录用户可写；权限不足时需要管理员给该项目目录授权。源码包不含本地 `instance/`；如需保留现有文章、图片和账号，需要另外迁移数据到 Docker 数据卷。

确认已安装的软件：

```bash
sudo docker version
sudo docker compose version
sudo docker buildx version
nginx -v
```

创建服务器独立的环境配置；如果已经存在 `.env`，保留并编辑现有配置，不要覆盖。

```bash
cd /home/admin/myproject/personweb
if [ ! -e .env ]; then
    cp .env.example .env
fi
openssl rand -hex 32
```

把生成的随机值填入 `.env` 的 `SECRET_KEY`，设置 `TRUSTED_HOSTS=你的域名,服务器公网IP`，再限制配置文件权限：

```bash
chmod 600 .env
sudo systemctl enable --now docker
sudo docker compose up -d --build
sudo docker compose exec web flask --app app create-admin
# 可选：使用示例内容演示
sudo docker compose exec web flask --app app seed-demo
sudo docker compose ps
```

数据使用 Docker 命名卷 `lab_data` 持久保存，重新构建镜像不会删除文章和附件。默认绑定 `127.0.0.1:8000`，由 Nginx 对外提供访问。

### 2. 配置 Nginx

把 `deploy/nginx.conf` 中 `example.com` 改成你的域名或初次测试使用的公网 IP，再复制到 Nginx 配置目录：

```bash
sudo cp deploy/nginx.conf /etc/nginx/conf.d/luoqian-lab.conf
sudo nginx -t
sudo systemctl enable --now nginx
sudo systemctl reload nginx
```

确认阿里云安全组和服务器防火墙允许 80 / 443 端口，8000 端口不需要对公网开放。域名使用 A 记录指向 ECS 公网 IP。

### 3. 配置 HTTPS

使用你的证书方案，或 Certbot 的 Nginx 集成签发证书。HTTPS 成功后把 `.env` 的 `COOKIE_SECURE=true`，并重新创建服务以应用新环境变量：

```bash
sudo docker compose up -d --force-recreate
```

`TRUST_PROXY=true` 仅适用于提供的 Nginx 配置及仅本机可访问的 Gunicorn。直接对外暴露 Gunicorn 时应设为 `false`。如已有负载均衡、CDN 或多层代理，需要按实际代理链调整配置。

### 更新与日志

```bash
cd /home/admin/myproject/personweb
sudo docker compose up -d --build
sudo docker compose logs --tail=100 web
```

### 备份与恢复

同时备份数据库、附件和生产 `.env`。Docker 运行时的数据位于容器的 `/app/instance`，不是服务器源码目录里的 `instance/`。

最简单的一致性备份方式是在维护窗口暂时停止服务，再复制完整数据目录：

```bash
mkdir -p backups
sudo docker compose stop web
sudo docker compose cp web:/app/instance ./backups/instance
sudo docker compose start web
```

每次备份使用新的目录，避免覆盖旧备份。恢复时停止服务，将备份数据复制回命名卷，并确认文件属主为容器用户 `10001:10001` 后再启动。**不要运行 `docker compose down -v`，它会删除数据卷。**

如需使用本机已有笔记，请在迁移时明确选择是否导入本地 `instance/`；不要在已有线上数据时直接覆盖。

### 不使用 Docker

可以用虚拟环境 + `deploy/personalweb.service` + Nginx。该服务示例假设专用系统用户为 `lab`，源码位于 `/opt/luoqian-lab`，数据在 `/var/lib/luoqian-lab`。创建管理员和初始化数据时，也必须用相同 `INSTANCE_PATH` 和 `.env`，确保命令与服务访问同一数据库。

## 数据和代码

```text
app.py                     应用入口
space/
  __init__.py              配置、应用工厂、安全响应头
  db.py / schema.sql       SQLite、管理命令、示例数据
  admin.py                 登录、写作、上传、学习路径、个人资料
  views.py                 首页、博客、学习路径、附件下载
  tools.py                 PDF 与图片工具
  community.py             网站导航、访客留言、审核回复、阅读量
  i18n.py / locales/       中英文翻译目录与语言偏好
  content.py / security.py Markdown 清理、文件校验、CSRF 与会话
  templates/               Jinja 页面模板
  static/                  CSS、JavaScript、SVG 图标
instance/                  自动创建，数据库、博客附件、开发密钥（不入 Git）
deploy/                    Nginx 和 systemd 配置
tests/                     权限、发布、上传与 PDF 输出测试
```

SQLite 适合单机个人站点。先完成自己的学习记录；如需多用户协作、更大文件、OCR 或高并发，再扩展数据库、对象存储和后台任务。

## 测试

```bash
source .venv/bin/activate
python -m pip install -r requirements-dev.txt
python -m pytest -q
# 安装 Node.js 后，可运行前端纯逻辑测试（无需 npm 安装）
node --test tests/tool_utils.test.cjs
```

测试使用临时数据库和临时上传目录，不会修改你的实际文章。

最新验证：58 项 Python 测试、5 项前端纯逻辑测试通过。新版阶段卡片已在浏览器核对新增、上移、删除与撤销、勾选完成、多行描述保存、中英文对应，以及 390px / 1280px 布局。学习路径演示使用独立的 `/tmp/personweb-stages-preview-20261019` 数据库，不会写入真实学习记录。此前浏览器还验证了留言审核、文章阅读量和去重。详见 `VERIFICATION.md`。

界面翻译源为 `space/locales/en.json`；JavaScript 使用的键列在 `client_keys.json` 中。新增动态提示时应同步补齐两个文件。

依赖完整版本在 `requirements.lock.txt`，功能依赖范围在 `requirements.txt`。生产构建使用锁定版本。



##欢迎尝试http://47.102.197.74/



## 官方参考

- [Flask 生产部署](https://flask.palletsprojects.com/en/stable/deploying/)
- [Gunicorn 与 Flask](https://flask.palletsprojects.com/en/stable/deploying/gunicorn/)
- [pypdf 文档](https://pypdf.readthedocs.io/)
- [Pillow 图片/PDF 格式说明](https://pillow.readthedocs.io/en/stable/handbook/image-file-formats.html)

## 一、连接 GitHub 仓库的两种方式

在命令行用`git clone remote_url` 或 `git remote add origin remote_url` 连接 GitHub 仓库时，正式的方式只有两种：

1. **SSH 协议**  
   - `remote_url`形如：`git@github.com:username/repo.git`
   - 结构可以拆解为：
   
     - 用户：`git`（GitHub 专门用来做 Git 操作的用户，不是你自己的用户名）
     - 主机：`github.com`
     - 仓库路径：`username/repo.git`
2. **HTTPS 协议**  
   - `remote_url`形如：`https://github.com/username/repo.git`

| 特性                   | SSH                                                          | HTTPS                                                        |
| ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 远程 URL 形式          | `git@github.com:owner/repo.git`                              | `https://github.com/owner/repo.git`                          |
| 身份认证               | 公钥/私钥（SSH key）                                         | 用户名 + Personal Access Token（PAT）                        |
| 初次配置难度           | 稍高，需要生成 key + 配置到 GitHub                           | 中等，需要生成 PAT + 记好 token                              |
| 每次操作是否要输密码   | 一般不需要                                                   | 没有凭据缓存时要输 PAT                                       |
| 网络兼容性             | 某些环境 SSH 端口可能被限制                                  | 一般只要能上 GitHub 网站就能用                               |
| 安全性（设计上）       | 高，公钥机制；泄露私钥需要及时撤销                           | 高，通过 PAT 限定权限；注意 Token 存储                       |
| 使用体验（命令行重度） | 非常适合（配好之后“无感认证”）                               | 没配好凭据缓存的情况下，每次 push 都要输入 token， 很烦      |
| 使用场景               | 你在自己的电脑（如 Arch Linux）上有完整权限，可以安全存放私钥 | 你偶尔在临时环境或别人电脑上操作一个仓库，不想把 SSH 私钥放上去 |

---

## 二、SSH 方式 

SSH 使用 **非对称加密** 来做身份认证：

你在本地生成一对密钥：
- **私钥**：`~/.ssh/id_ed25519`（不要泄露，只在你自己的机器上）
- **公钥**：`~/.ssh/id_ed25519.pub`（可以公开，上传到 GitHub）

1. 你本地有一个私钥，GitHub 上保存了与你匹配的公钥。
2. 当你执行 `git push` / `git pull` 时，SSH 客户端会对一段数据用**私钥签名**。
3. GitHub 拿你在网站上保存的**公钥**来验证签名。  
4. 如果验证通过，GitHub 确认“这是那个拥有这把私钥的人”，就允许你访问对应仓库。

**需要准备的东西：**

1. 本机的 SSH 密钥对（公钥/私钥）
2. 在 GitHub 上配置公钥

#### 步骤 1：检查是否已有 SSH 密钥

```bash
ls ~/.ssh
```

如果看到 `id_ed25519` 和 `id_ed25519.pub`（或 `id_rsa` / `id_rsa.pub`），说明你已经有一对 key。

#### 步骤 2：如果没有，则生成一对 ED25519 密钥

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
# -C 注释字段，写自己的邮箱或备注即可
# 一路回车即可（除非你想设置一个 passphrase）
```

生成后会得到：

- 私钥：`~/.ssh/id_ed25519`
- 公钥：`~/.ssh/id_ed25519.pub`

#### 步骤 3：把公钥添加到 GitHub

查看公钥内容：

```bash
kate ~/.ssh/id_ed25519.pub
```

复制第一行。

在浏览器中：

1. 打开 GitHub → 右上角头像 → **Settings**
2. 左边菜单中选择 **SSH and GPG keys**
3. 点击 **New SSH key**
4. Title 填：比如 `arch-laptop` / `arch-desktop`
5. Key 内容中粘贴刚才复制的公钥
6. 保存

#### 步骤 4：测试 SSH 是否能连通 GitHub

```bash
ssh -T git@github.com
```

- 第一次可能出现指纹确认提示：
  - `ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU`
  - 这是 GitHub 官方的 ED25519 指纹，可以输入 `yes`
- 正常成功时会看到：

```text
Hi! You've successfully authenticated, but GitHub does not provide shell access.
```

说明 SSH 认证一切 OK。

> [!TIP]
>
> 
| 维度                   | SSH 密钥                                                     | GPG 密钥                                                     |
| :--------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| **主要用途**           | 用于**身份验证**，安全连接 GitHub 服务器，执行 `git clone`、`push`、`pull` 等操作时替代密码或令牌。 | 用于**数字签名**，对 Git 提交（commit）和标签（tag）进行签名，以证明作者身份及内容未被篡改。 |
| **在 GitHub 上的效果** | 启用后，可通过 SSH 协议与仓库安全通信，无需每次输入凭据。    | 签名后的提交在 GitHub 界面上会显示 &zwj;**“Verified”**&zwj; 标识，提升代码的可信度。 |

---

## 三、HTTPS 方式

你通过浏览器 / Git 客户端访问 `https://github.com/...`

服务器端（GitHub）需要确认你是谁：  

- 传统是“用户名 + 密码”登录  
- 但密码权限太大、容易泄露，GitHub 已经**禁用账号密码**作为 Git 操作的认证方式
- 必须使用「用户名 + Personal Access Token（PAT）」来认证

为什么需要 PAT（Personal Access Token）？

1. **安全性**：  
   - 账号密码可能能登录你整个 GitHub 账户，权限过大
   - PAT 可以只给特定权限（只读 / 只访问 repo / 限定 scope）
   - 可以设置过期时间、可以随时撤销

2. **审计和最小权限**：  
   - 不同机器/项目可以用不同 token  
   - 某个 token 泄露，只需要撤销这一条即可

3. **自动化友好**：  
   - 脚本、CI 里使用时，比密码更可控

所以，在 **HTTPS 模式** 下，你的 Git 身份认证流程是：

- Git 客户端向 GitHub 提交：
  - username: `firma2021`
  - password: `<一串很长的 PAT>`
- GitHub 端验证这个 PAT 是否有效、是否有权限访问你操作的仓库。

#### 创建 PAT

在浏览器中：

1. GitHub → 右上角头像 → **Settings**
2. 左边菜单最下方 → **Developer settings**   -> **Personal access tokens**  ，可以选择 **Tokens (classic)**
4. 点击 **Generate new token (classic)**
5. Note（备注）：比如 `arch-laptop-git`
6. Expiration：选一个合适的期限（90 天 / 1 年，或自定义）
7. Scopes（勾权限）：
   - 常用：勾选 `repo`（访问私有仓库需要）
8. 点击 **Generate token**
9. 复制生成的 token（只会显示一次，一定要保存好）

第一次 push 时：

- 会提示输入用户名：`firma2021`
- 密码：**粘贴你刚刚生成的 PAT**

有的系统会弹出图形界面或使用凭据管理器：

- 可以选择保存，这样下次就不必每次再输 token
- 如果没保存，每次 push 可能都要再输一次 token（有点麻烦）

---
## 四、Oauth

Oauth和PAT类似，本质上都是“token”。

你在 VS Code 里点击 “Sign in to GitHub”：

1. VS Code（客户端）说：“我想代表用户访问 GitHub 仓库和 issue，可以吗？”
2. GitHub 把你重定向到一个登录/授权页面（浏览器里）：
   - 如果你没登陆 GitHub，会先让你登录；
   - 然后会问你：“VS Code 想要这些权限：读/写 repo、读 user info …，你同意吗？”
3. 你点击 **Authorize**。
4. GitHub 不把你的密码给 VS Code，而是：
   - 颁发一个 **access token** 给 VS Code（通过一套后台回调流程）。
5. VS Code 把这个 token 存本地，通常是系统的安全存储（比如 macOS Keychain、Windows 凭据管理器、Linux 的 keyring 等）。
6. 之后 VS Code 每次访问 GitHub API，就带上这个 token，GitHub 就知道：
   - “这是得到你授权的 VS Code，在你的授权范围内办事。”

你看到的只是一两个界面：“登录 GitHub → Authorize VS Code”，背后跑的就是一套 OAuth 授权流程。

---

## 五、`git push -u origin main` 的作用

> [!TIP]
>
> Git 最初是为 Linux 内核开发设计的，其命令语法深受 Unix 传统的影响：目的地优先。`git push -u origin main`中，origin是目的地，在前面，main是源，在后面。

> [!IMPORTANT]
>
> push 行为需要精确控制，而不是广播式同步。直接 `git push`，不会把本地所有分支都推到远程同名分支上。

- `git push origin main`：  
  
  - `push` 语法的格式为：`git push <远程仓库> <本地分支>:<远程分支>`
  - 当只写一个本地分支名main时，远程分支会默认使用相同的名称
  - `git push -u origin main` 等价于：`git push -u origin main:main`
  - 作用是：把本地的 `main` 分支推到远程 `origin` 的 `main` 分支
  
- `git push -u origin main`：  
  
  - **做的事一样**，但额外：
    - `-u`等价于 `--set-upstream`
    
    - 把“本地 main 分支”与“远程 origin/main”建立 **追踪（upstream）关系**
    
    - 一旦设置好，Git 知道：当前本地分支应该和哪个远程分支进行对比
    
    - 以后你只要在这个分支上，直接用 `git push` / `git pull` 就行，不必每次写 `origin main`。
    

如果你想把main分支和dev分支都推送到远程仓库，可以这样做：

第一次推送：

1. 切换到main分支，在 `main` 分支上：

   ```bash
   git checkout main
   git push -u origin main
   ```

2. 切换到dev分支，然后在 `dev` 分支上：

   ```bash
   git checkout dev
   git push -u origin dev
   ```

此后的推送：

- 切换到main分支，在 `main` 分支上：

  ```bash
  git push   # -> 推到 origin/main
  git pull   # -> 从 origin/main 拉取
  ```

- 切换到dev分支，在 `dev` 分支上：

  ```bash
  git push   # -> 推到 origin/dev
  git pull   # -> 从 origin/dev 拉取
  ```










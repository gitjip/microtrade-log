在 Qt 开发中结合 Git 和 GitHub 进行版本控制，核心是**用 Qt Creator 集成的 Git 工具（或第三方图形化工具如 GitHub Desktop）管理代码，通过 GitHub 托管远程仓库**，实现代码的版本追踪、协作与备份。以下是完整操作流程：


### 一、前期准备
1. **环境配置**
   - 安装 **Git**（[官网下载](https://git-scm.com/)），并完成基础配置（设置用户名和邮箱，与 GitHub 账号一致）：
     ```bash
     git config --global user.name "你的GitHub用户名"
     git config --global user.email "你的GitHub注册邮箱"
     ```
   - 安装 **Qt Creator**（确保安装时勾选“Git”组件，默认已包含）。
   - 注册/登录 **GitHub 账号**，创建一个空的远程仓库（如命名为 `Qt-Project-Demo`，暂不初始化 `README` 或 `LICENSE`，避免与本地代码冲突）。

2. **Qt 项目与 Git 关联原则**
   - Git 仅追踪**源代码文件**（如 `.h`、`.cpp`、`.ui`、`.pro`、`.qrc` 等），忽略**编译生成文件**（如 `build-xxx` 目录、`.o` 目标文件、可执行程序 `.exe`/`.app` 等）。
   - 需手动创建 `.gitignore` 文件（可在 [GitHub .gitignore 模板库](https://github.com/github/gitignore) 下载 Qt 专用模板），放在项目根目录，避免冗余文件被追踪。


### 二、核心操作流程（以 Qt Creator 为例）
#### 1. 本地 Qt 项目初始化 Git 仓库
适用于**新建 Qt 项目**或**已有未纳入 Git 的 Qt 项目**：
- **新建项目时关联 Git**：
  1. 在 Qt Creator 中新建项目（如“Qt Widgets Application”），步骤中会出现“Version Control”选项，选择“Git”，点击“Finish”。
  2. 项目创建后，Qt Creator 会自动初始化本地 Git 仓库（项目根目录生成隐藏的 `.git` 文件夹）。

- **已有项目手动关联 Git**：
  1. 打开 Qt 项目，点击顶部菜单栏 **“Tools → Git → Local Repository → Initialize Repository”**。
  2. 在弹出窗口中选择项目根目录，点击“Initialize”，完成本地仓库初始化。


#### 2. 首次提交本地代码
1. 初始化仓库后，Qt Creator 会自动识别未追踪的文件（在“Projects”面板旁的 **“Version Control”** 面板中查看，或按 `Ctrl+9` 调出）。
2. 勾选需要提交的文件（通常全选，除了编译目录和临时文件），点击面板中的 **“Commit”** 按钮（或右键选择“Commit”）。
3. 在弹出的“Commit”窗口中，填写**提交信息**（需清晰描述修改，如 `init: 初始化Qt窗口项目，完成主窗口布局`），点击“Commit”，完成首次本地提交。


#### 3. 关联 GitHub 远程仓库
1. 点击顶部菜单栏 **“Tools → Git → Remote Repository → Add Remote”**。
2. 在“Add Remote”窗口中：
   - “Name”填写 `origin`（默认远程仓库名称，约定俗成）。
   - “URL”填写 GitHub 上新建仓库的 HTTPS 地址（在 GitHub 仓库页面点击“Code”，复制 HTTPS 链接，如 `https://github.com/你的用户名/Qt-Project-Demo.git`）。
3. 点击“Add”，完成本地仓库与 GitHub 远程仓库的关联。


#### 4. 推送本地代码到 GitHub
1. 点击顶部菜单栏 **“Tools → Git → Push”**（或按 `Ctrl+Shift+P`）。
2. 在“Push”窗口中，默认推送当前分支（新建项目默认分支为 `main` 或 `master`）到远程 `origin` 仓库，点击“Push”。
   - 若首次推送，可能需要输入 GitHub 账号密码（或使用 SSH 密钥免密登录，推荐配置 SSH 避免重复验证）。
3. 推送成功后，打开 GitHub 仓库页面，即可看到 Qt 项目的所有文件。


#### 5. 日常开发：分支管理与协作
##### （1）创建功能分支（避免直接修改主分支）
1. 点击 **“Version Control”** 面板 → 分支下拉菜单 → **“Create Branch”**。
2. 输入分支名称（如 `feature/login-window`，表示开发“登录窗口”功能），选择基于 `main` 分支创建，点击“Create”。
3. 切换到新分支后，正常开发（编写登录窗口的 `.h`/`.cpp`/`.ui` 文件）。

##### （2）提交与推送分支代码
1. 开发完成后，在“Version Control”面板勾选修改的文件，填写提交信息（如 `feat: 实现登录窗口UI与账号密码校验`），点击“Commit”。
2. 点击 **“Tools → Git → Push”**，推送当前功能分支到 GitHub（首次推送需确认分支名称，后续直接推送）。

##### （3）合并分支到主分支（通过 GitHub PR）
1. 登录 GitHub，进入项目仓库，切换到功能分支（如 `feature/login-window`），点击“Compare & pull request”。
2. 填写 PR 描述（说明功能内容、测试情况），点击“Create pull request”。
3. 若无需审核（个人项目），直接点击“Merge pull request”合并到 `main` 分支；团队项目需等待审核通过后合并。
4. 合并完成后，在 Qt Creator 中切换回 `main` 分支，点击 **“Tools → Git → Pull”**（或 `Ctrl+Shift+O`），拉取远程 `main` 分支的最新代码，保持本地与远程同步。


#### 6. 版本标记（如 v1.0.0，对应 GitHub Release）
当 `main` 分支达到稳定版本（如首次正式发布），需标记语义化版本号（如 `v1.0.0`）：
1. 在 Qt Creator 中切换到 `main` 分支，确保已拉取最新代码。
2. 点击 **“Tools → Git → Tag → Create Tag”**。
3. 在“Create Tag”窗口中：
   - “Tag Name”输入 `v1.0.0`（前缀 `v` 为行业习惯）。
   - “Description”填写版本说明（如 `Release v1.0.0：首次正式发布，包含主窗口与登录功能`）。
   - 选择要标记的提交（通常为最新提交），点击“Create”。
4. 推送标签到 GitHub：点击 **“Tools → Git → Push”**，在弹出窗口中勾选“Push tags”，点击“Push”。
5. 在 GitHub 仓库的“Releases”页面，点击“Draft a new release”，选择已推送的 `v1.0.0` 标签，填写发布说明，点击“Publish release”，完成版本发布。


#### 7. 代码回滚（如需撤销错误提交）
- **未推送的本地提交**：在“Version Control”面板的“History”（历史）中，右键点击错误提交，选择“Revert Commit”，生成一个反向提交抵消错误。
- **已推送到远程的提交**：通过 GitHub 找到错误提交，在 Qt Creator 中执行“Revert Commit”后，重新推送新的提交到远程，或通过 PR 合并回滚代码（避免直接强制覆盖远程主分支）。


### 三、可选：用 GitHub Desktop 简化操作
若不习惯 Qt Creator 的 Git 工具，可使用 GitHub Desktop 管理 Qt 项目，核心流程类似：
1. 在 GitHub Desktop 中克隆 GitHub 远程仓库到本地（选择 Qt 项目所在目录）。
2. 用 Qt Creator 打开克隆后的项目，正常开发。
3. 开发完成后，在 GitHub Desktop 中自动识别修改文件，填写提交信息，点击“Commit”，再点击“Push”推送代码。
4. 分支创建、标签管理、回滚等操作，可通过 GitHub Desktop 的图形化界面完成（参考前文“GitHub Desktop 版本控制”相关步骤）。


### 四、关键注意事项
1. **忽略编译文件**：务必在项目根目录放置 `.gitignore` 文件，避免 `build-xxx`、`.user`（Qt Creator 配置文件）等冗余文件被 Git 追踪。
2. **分支规范**：遵循“主分支（`main`）稳定，功能分支（`feature/*`）开发，修复分支（`hotfix/*`）紧急修复”的策略，避免直接修改主分支。
3. **提交信息清晰**：采用规范的提交信息（如 `feat: 新增XX功能`、`fix: 修复XXbug`、`docs: 更新说明文档`），便于后续版本追溯。
4. **定期同步代码**：团队协作时，每天开发前先“Pull”拉取远程最新代码，避免代码冲突；功能开发中可分阶段“Commit”和“Push”，避免代码丢失。
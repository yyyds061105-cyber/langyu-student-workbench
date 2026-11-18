# 浪屿学生组织工作台（演示版）

学生组织协同管理网页演示，包含工作台、活动管理、信息收集、任务、知识库、物资、排班和成员管理等界面。

## 在线演示

https://yyyds061105-cyber.github.io/langyu-student-workbench/

## AI 助手接入

网页托管在 GitHub Pages，不能安全保存模型 API 密钥。仓库内的 `worker/src/index.js` 是 Cloudflare Worker 后端代理；浏览器只请求 Worker，密钥仅保存在 Cloudflare 的 Secret 设置里。写入任务、评分和知识库的 AI 内容会先作为草稿展示，须由用户确认后才保存在当前浏览器的演示数据中。

### 需要准备

- 一个 Cloudflare 账户，用于运行 Worker。
- 一个新的 Kimi API 密钥。此前源文件曾包含密钥；如旧密钥仍有效，请先到 Kimi 开放平台撤销并创建新密钥。不要把密钥写入网页、GitHub、聊天或 README。
- Kimi API 地址需匹配密钥所属区域：中国大陆通常使用 `https://api.moonshot.cn/v1`，国际站通常使用 `https://api.moonshot.ai/v1`。

### 部署 Worker

1. 登录 Cloudflare，打开 **Workers & Pages**，创建一个 Worker。
2. 进入 Worker 的代码编辑器，把 `worker/src/index.js` 全部内容复制进去并部署。
3. 在 Worker 的 **Settings → Variables and Secrets** 添加变量：
   - `KIMI_API_KEY`：类型选 **Secret**，粘贴新创建的 Kimi 密钥。
   - `KIMI_API_BASE_URL`：普通变量，填写与你的密钥区域对应的 API 地址。
   - `KIMI_MODEL`：普通变量，填写 `kimi-k3`。
   - `ALLOWED_ORIGIN`：普通变量，填写 `https://yyyds061105-cyber.github.io`。
4. 保存并重新部署 Worker。Cloudflare 会提供形如 `https://你的-worker名.你的账号.workers.dev` 的地址。
5. 在本仓库 `index.html` 中找到 `const AI_API_URL = '';`，将空字符串改为 Worker 的完整接口地址，例如 `https://你的-worker名.你的账号.workers.dev/api/chat`，提交到 GitHub。GitHub Pages 会自动更新。

完成后打开在线演示，进入 AI 助手对话。当前演示版的成员登录和数据都在浏览器本地，Worker 的 CORS 来源限制不等同于用户身份认证；不要把它用于存储真实学生个人信息或正式评分。正式校内使用还需增加服务端身份认证、权限校验、云端数据存储和访问限流。

## 本地运行

直接用现代浏览器打开 `index.html`，或用任意静态网页服务器托管。演示数据保存在当前浏览器的 `localStorage` 中，不会同步到其他用户或设备。

## 当前限制

- 这是前端演示版，不包含服务端数据库或真实身份认证。
- AI 后端部署并在 `AI_API_URL` 设置 Worker 地址后才可使用。
- 请勿录入真实成员姓名、学号、联系方式或其他个人数据。
- 示例成员姓名已替换为通用称呼。

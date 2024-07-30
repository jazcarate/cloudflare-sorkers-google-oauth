## An OAuth client for Google APIs on Cloudflare Workers
本项目是一个利用 Cloudflare 提供的 Workers 无服务器架构（Serverless）实现的 **OAuth2 协议中的客户端（Client）应用**， OAuth2 中对应的授权服务器和资源服务器由 Google Cloud 进行提供。

#### About Cloudflare Workers
Cloudflare Workers 是一种无服务器计算平台，允许开发者在全球分布的 Cloudflare 网络上运行 JavaScript 代码，从而实现快速、可扩展和高性能的应用和功能。需要注册一个 Cloudflare account 来使用 Workers, Workers 免费规格(Free Plan) 的 Workers 每日可支持 100,000 请求响应，每个请求响应消耗的的 CPU 时间可达10 ms。Cloudflare 【2024年公布的计费模型](https://blog.cloudflare.com/workers-pricing-scale-to-zero/)中排除了 I/O 等待的耗时， 使得多数I/O密集Web应用可以在免费规格上顺利的运行。
> Cloudflare Workers provides a serverless execution environment that allows you to create new applications or augment existing ones without configuring or maintaining infrastructure.

> Cloudflare Workers let you deploy serverless code instantly across the globe for exceptional performance, reliability, and scale.

#### OAuth2
OAuth 2.0 是一种授权框架，允许第三方应用在资源所有者的许可下，获取访问资源服务器上的受保护资源的权限，而不需要暴露资源所有者的凭据。OAuth 2.0 被广泛应用于社交登录、API 访问控制等场景。

> The OAuth 2.0 authorization framework enables a third-partyapplication to obtain limited access to an HTTP service, either on behalf of a resource owner by orchestrating an approval interaction between the resource owner and the HTTP service, or by allowing the third-party application to obtain access on its own behalf.


**核心概念**

1. **授权服务器（Authorization Server）**：
   - 负责验证资源所有者的身份，并颁发访问令牌（Access Token）给客户端应用。
     > 在本项目中由 Google Cloud 的相应API endpoints 来提供 OAuth2 业务流程中的授权服务。（待完善）

1. **资源服务器（Resource Server）**：
   - 托管资源的服务器，使用访问令牌来决定是否允许客户端访问受保护资源。
     > 在本项目中由 Google APIs 的相应API endpoints 来提供对应的资源访问服务。（待完善）

2. **客户端（Client）**：
   - 请求访问受保护资源的第三方应用。它代表资源所有者操作，但并不代表资源所有者的身份。
     > 本项目利用Workers

1. **资源所有者（Resource Owner）**：
   - 拥有受保护资源的实体，通常是最终用户。


**相关阅读**
- [RFC 6749: The OAuth 2.0 Authorization Framework ](https://datatracker.ietf.org/doc/html/rfc6749) 

- [Using OAuth 2.0 to Access Google APIs](https://developers.google.com/identity/protocols/oauth2)

## Prerequisites
### nodejs
- 为了避免版本冲突建议通过 conda 进行 nodejs 环境安装, 本项目使用 2024-07 的 LTS 版本 v20.16.0 
如果还没有安装 `conda`，可以从以下链接下载并安装 Miniconda 或 Anaconda：
   - [Miniconda](https://docs.conda.io/en/latest/miniconda.html)
   - [Anaconda](https://www.anaconda.com/products/distribution)

1. **创建并激活一个新的 Conda 环境**（可选，但推荐这样做，以避免影响其他环境）:
   ```bash
   conda create -n nodee-lts
   conda activate nodee-lts
   ```     

2. **添加 Conda-Forge 仓库**（如果还没有添加过）:
   ```bash
   conda config --add channels conda-forge
   ```

3. **使用 Conda 安装 Node.js 的 认定版本**:

```bash
conda install -c conda-forge nodejs=20
```
4. **验证安装**:
   安装完成后，可以通过以下命令验证 Node.js 和 npm 是否安装成功，以及它们的版本：
   ```bash
   node -v
   npm -v
   ```


### wrangler
`wrangler` 是一个用于管理和部署 Cloudflare Workers 的命令行工具。它能快速初始化新项目、本地开发和调试、部署 Workers 到 Cloudflare 边缘网络。`wrangler` 使用 `wrangler.toml` 文件管理配置，支持 Workers KV 存储的创建和管理，并提供实时日志查看功能。通过 `wrangler`，开发者可以高效地在 Cloudflare 上开发、调试和部署代码，极大地简化了操作流程。

- wrangler 安装，请在项目目录当中执行
```bash
npm install wrangler --save-dev
```
- 验证 wrangler 安装
```bash
npx wrangler -v
```
- 配置文件 wrangler.toml
```
name = "oauth-client"
main = "src/index.ts"
compatibility_date = "2024-07-25"
```






## Ideas to grow this project
If you would like to use this setups as a starting point to develop interesting things; I recommend trying out one (or all!) of this improvements:

- Create a middleware pattern to deaal with authenticated and unauthenticated endpoints
- Serve static content, either with Cloudflare Sites, or reading local files in a Worker. A default path could be implemented to serve files in `public/` folder.
- Improve the rendered HTML with a template library, or roll up your own!
- Use another Google API from [the list](https://developers.google.com/workspace/products).
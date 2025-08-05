# Gemini Balance Lite

# Gemini API 代理和负载均衡无服务器轻量版（边缘函数）

### 作者：技术爬爬虾

[B 站](https://space.bilibili.com/316183842)，[Youtube](https://www.youtube.com/@Tech_Shrimp)，抖音，公众号 全网同名。转载请注明作者。

## 项目简介

Gemini API 代理, 使用边缘函数把 Gemini API 免费中转到国内。还可以聚合多个 Gemini API Key，随机选取 API Key 的使用实现负载均衡，使得 Gemini API 免费成倍增加。

## Vercel 部署(推荐)

[![Deploy to Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/tech-shrimp/gemini-balance-lite)

1.  点击部署按钮 ⬆️ 一键部署。
2.  国内使用需要配置自定义域名
    <details>
    <summary>配置自定义域名：</summary>

    ![image](/docs/images/5.png)
    </details>

3.  去[AIStudio](https://aistudio.google.com)申请一个免费 Gemini API Key
    <br>将 API Key 与自定义的域名填入 AI 客户端即可使用，如果有多个 API Key 用逗号分隔
    <details>
    <summary>以 Cherry Studio 为例：</summary>

        ![image](/docs/images/2.png)
        </details>

## Deno 部署

1. [fork](https://github.com/tech-shrimp/gemini-balance-lite/fork)本项目
2. 登录/注册 https://dash.deno.com/
3. 创建项目 https://dash.deno.com/new_project
4. 选择此项目，填写项目名字（请仔细填写项目名字，关系到自动分配的域名）
5. Entrypoint 填写 `src/deno_index.ts` 其他字段留空
   <details>
   <summary>如图</summary>

   ![image](/docs/images/3.png)
   </details>

6. 点击 <b>Deploy Project</b>
7. 部署成功后获得域名
8. 国内使用需要配置自定义域名
9. 去[AIStudio](https://aistudio.google.com)申请一个免费 Gemini API Key
10. 将 API Key 与分配的域名填入 AI 客户端即可使用，如果有多个 API Key 用逗号分隔

<details>
<summary>以Cherry Studio为例：</summary>

![image](/docs/images/2.png)

</details>

## Cloudflare Worker 部署

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/tech-shrimp/gemini-balance-lite)

0. CF Worker 有可能会分配香港的 CDN 节点导致无法使用(Gemini 不允许香港 IP 连接)
1. 广东地区不建议使用 Cloudflare Worker 部署
2. 点击部署按钮
3. 登录 Cloudflare 账号
4. 链接 Github 账户，部署
5. 打开 dash.cloudflare.com，查看部署后的 worker
6. 国内使用需要配置自定义域名
<details>
<summary>配置自定义域名：</summary>

![image](/docs/images/4.png)

</details>

## Netlify 部署

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/tech-shrimp/gemini-balance-lite)
<br>点击部署按钮，登录 Github 账户即可
<br>免费分配域名，国内可直连。
<br>但是不稳定

<details>
<summary>将分配的域名复制下来，如图：</summary>

![image](/docs/images/1.png)

</details>

去[AIStudio](https://aistudio.google.com)申请一个免费 Gemini API Key
<br>将 API Key 与分配的域名填入 AI 客户端即可使用，如果有多个 API Key 用逗号分隔

## 打赏

#### 帮忙点点关注点点赞，谢谢啦~

B 站：[https://space.bilibili.com/316183842](https://space.bilibili.com/316183842)<br>
Youtube: [https://www.youtube.com/@Tech_Shrimp](https://www.youtube.com/@Tech_Shrimp)

## 本地调试

1. 安装 NodeJs
2. npm install -g vercel
3. cd 项目根目录
4. vercel dev

## API 说明

### Gemini 代理

可以使用 Gemini 的原生 API 格式进行代理请求。
**Curl 示例:**

```bash
curl -X POST --location 'https://<YOUR_DEPLOYED_DOMAIN>/v1beta/models/gemini-2.5-pro:generateContent' \
--header 'Content-Type: application/json' \
--header 'x-goog-api-key: <YOUR_GEMINI_API_KEY_1>,<YOUR_GEMINI_API_KEY_2>' \
--data '{
    "contents": [
        {
         "role": "user",
         "parts": [
            {
               "text": "Hello"
            }
         ]
      }
    ]
}'
```

**Curl 示例:（流式）**

```bash
curl -X POST --location 'https://<YOUR_DEPLOYED_DOMAIN>/v1beta/models/gemini-2.5-pro:generateContent?alt=sse' \
--header 'Content-Type: application/json' \
--header 'x-goog-api-key: <YOUR_GEMINI_API_KEY_1>,<YOUR_GEMINI_API_KEY_2>' \
--data '{
    "contents": [
        {
         "role": "user",
         "parts": [
            {
               "text": "Hello"
            }
         ]
      }
    ]
}'
```

> 注意: 请将 `<YOUR_DEPLOYED_DOMAIN>` 替换为你的部署域名，并将 `<YOUR_GEMINI_API_KEY>` 替换为你的 Gemini API Ke，如果有多个用逗号分隔

### API Key 校验

可以通过向 `/verify` 端点发送请求来校验你的 API Key 是否有效。可以一次性校验多个 Key，用逗号隔开。

**Curl 示例:**

```bash
curl -X POST --location 'https://<YOUR_DEPLOYED_DOMAIN>/verify' \
--header 'x-goog-api-key: <YOUR_GEMINI_API_KEY_1>,<YOUR_GEMINI_API_KEY_2>'
```

### OpenAI 格式

本项目兼容 OpenAI 的 API 格式，你可以通过 `/chat` 或 `/chat/completions` 端点来发送请求。

**Curl 示例:**

```bash
curl -X POST --location 'https://<YOUR_DEPLOYED_DOMAIN>/chat/completions' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <YOUR_GEMINI_API_KEY>' \
--data '{
    "model": "gpt-3.5-turbo",
    "messages": [
        {
            "role": "user",
            "content": "你好"
        }
    ]
}'
```

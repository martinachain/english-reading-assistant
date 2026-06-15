# 英语阅读助手 · EPUB / PDF AI Reader

> 直接读原文 · 卡住才求助 · 生词句子自动沉淀

一个面向中文学习者的英语原版书阅读器。导入 EPUB / PDF，遇到不懂的单词双击查意思，遇到看不懂的长句划选让 AI 拆结构。查过的词、拆过的句会自动进入间隔重复（SRS）队列，帮你把生词和难句真正记住。

所有数据都存在你自己的浏览器里，没有账号、没有服务器数据库，**开箱即用**。

---

## 功能

- **读 EPUB 和 PDF** —— 拖入或点击导入，自动提取书名、作者、封面
- **双击查词** —— 双击任意单词，AI 结合上下文给出本句中的中文释义
- **划选拆句** —— 选中一个长句，AI 拆出句子结构、关键短语、难点注解和完整翻译
- **生词本 + 句子库** —— 查过的词、拆过的句自动入库
- **间隔重复复习（SRS）** —— 按 1 / 3 / 7 / 14 / 30 / 60 天的阶梯排期，句卡连续看懂 3 次后「毕业」
- **阅读体验** —— 目录跳转、书签、续读定位、阅读进度，PDF 支持懒加载分页（几百页也不卡）
- **数据本地化** —— 全部存浏览器 IndexedDB，换设备不同步（这是设计取舍，不是 bug）

## 快速开始

需要 [Node.js](https://nodejs.org) 18+。

```bash
git clone https://github.com/martinachain/english-reading-assistant.git
cd english-reading-assistant
npm install
npm run dev
```

打开 [http://localhost:3000](http://localhost:3000)，第一次进会弹出配置框，填入你的 **DeepSeek API Key** 即可开始。

> 不需要创建 `.env` 文件——key 在界面里填，只存你本地浏览器。

### 申请 DeepSeek API Key

AI 查词和拆句调用的是 [DeepSeek](https://platform.deepseek.com)（便宜、对中文友好）：

1. 注册并登录 [platform.deepseek.com](https://platform.deepseek.com)
2. 在 [API Keys](https://platform.deepseek.com/api_keys) 页面创建一个 key（形如 `sk-...`）
3. 充值少量额度（拆句一次几厘钱）
4. 把 key 填进应用的配置框

key 只保存在你的浏览器 `localStorage`，每次请求经由本应用自己的后端转发给 DeepSeek，不会上传到任何第三方。

## 技术栈

- **框架**：[Next.js 16](https://nextjs.org)（App Router + Turbopack）、React 19、TypeScript
- **样式**：Tailwind CSS v4
- **本地存储**：[Dexie](https://dexie.org)（IndexedDB 封装）
- **电子书解析**：[epub.js](https://github.com/futurepress/epub.js)、[pdf.js](https://mozilla.github.io/pdf.js/)
- **AI**：OpenAI SDK，baseURL 指向 DeepSeek（`deepseek-chat` 模型）

## 项目结构

```
app/
  page.tsx              书架（导入、复习入口、API Key 配置）
  read/[bookId]/        阅读页（查词、拆句、书签、目录）
  review/               间隔重复复习
  library/              生词本
  api/word/             单词释义接口
  api/analyze/          句子拆解接口
components/             阅读器、弹框、面板等
lib/                    数据库、SRS 算法、pdf/epub 解析、API Key 管理
types/                  共享类型
```

## 说明与边界

- **数据存在浏览器本地**：清空浏览器数据 / 换浏览器 = 书和记录都没了。类型里预留了 `updatedAt` 字段，未来可接 Supabase 等做云同步。
- **适合本地自托管**：每个人跑自己的副本、填自己的 key。如果要部署成公开共享网址，「key 存浏览器 + 经后端转发」这套就不安全了（后端会变成开放代理），需要另加登录和用量限制。
- 后端接口**只接受用户在界面填写、经请求头传入的 key**，不再支持环境变量 `DEEPSEEK_API_KEY` 兜底——避免公开部署后被人当成免费的 DeepSeek 代理。

## License

MIT

# 🌱 从 0 开始 Spring AI

**GitHub仓库由于公司网络原因暂时不维护，更多示例（ReAct Agent、RAG、MCP等）请访问Gitee仓库：https://gitee.com/xhyym/springaibook**

> 一个全面、易懂、可运行的 Spring AI 学习指南，助你快速构建 AI 增强型 Java 应用。

[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.1%2B-green.svg)](https://spring.io/projects/spring-boot)
[![Java](https://img.shields.io/badge/Java-17%2B-orange.svg)](https://openjdk.org/)
[![AI Models](https://img.shields.io/badge/通义千问%20%7C%20DeepSeek-blueviolet)](https://spring.io/projects/spring-ai)

📖 **中文教程 | 阿里云通义千问集成 | 可运行示例 | 持续更新**

---

## 📚 目录

- [环境准备](#-环境准备)
- [快速入门](#-快速入门)
  - [1. 获取通义千问 API Key](#1-获取通义千问-api-key)
  - [2. 引入依赖](#2-引入依赖)
  - [3. 配置文件](#3-配置文件)
  - [4. 编写控制器](#4-编写控制器)
  - [5. 运行效果](#5-运行效果)
- [后期规划](#-后期规划)
- [许可证](#-许可证)
- [致谢](#-致谢)

---

## ⚙️ 环境准备

- ✅ Java 17 或更高版本
- ✅ Maven 3.6+
- ✅ Spring Boot 3.1+
- ✅ 通义千问大模型账号（[阿里百炼平台](https://bailian.console.aliyun.com/#/home)）
- ✅ Docker（可选，用于运行向量数据库）

---

## 🚀 快速入门

### 1. 获取通义千问 API Key

前往 [阿里百炼平台](https://bailian.console.aliyun.com/#/home) 注册账号，创建应用并获取 `DASHSCOPE_API_KEY`。

> 🔐 建议将密钥配置为环境变量，避免泄露。

---

### 2. 引入依赖

在 `pom.xml` 中添加以下依赖管理与 Starter：

```xml
<properties>
    <spring-ai.version>1.0.0</spring-ai.version>
    <spring-ai-alibaba.version>1.0.0.2</spring-ai-alibaba.version>
    <spring-boot.version>3.5.4</spring-boot.version>
    <java.version>17</java.version>
</properties>

<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud.ai</groupId>
            <artifactId>spring-ai-alibaba-bom</artifactId>
            <version>${spring-ai-alibaba.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>${spring-boot.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.ai</groupId>
            <artifactId>spring-ai-bom</artifactId>
            <version>${spring-ai.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>com.alibaba.cloud.ai</groupId>
        <artifactId>spring-ai-alibaba-starter-dashscope</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

---

### 3. 配置文件

创建 `application.yml`，配置通义千问模型：

```yaml
spring:
  ai:
    alibaba:
      dashscope:
        api-key: ${DASHSCOPE_API_KEY} # 推荐使用环境变量
        chat:
          model: qwen-plus # 可选: qwen-turbo, qwen-max, qwen-vl 等
```

> 💡 提示：可通过环境变量 `DASHSCOPE_API_KEY=your_key_here` 启动应用。

---

### 4. 编写控制器

创建一个简单的聊天接口：

```java
@RestController
public class ChatController {

    private final ChatClient chatClient;

    public ChatController(ChatClient.Builder builder) {
        this.chatClient = builder.build();
    }

    @GetMapping("/chat")
    public Flux<String> chat(
            @RequestParam(defaultValue = "你是谁") String message,
            HttpServletResponse response) {
        response.setCharacterEncoding("UTF-8");
        return chatClient.prompt()
                         .user(message)
                         .stream()
                         .content();
    }
}
```

---

### 5. 运行效果

启动应用后，访问：

```
http://localhost:8080/chat?message=你好，Spring AI！
```

你将看到通义千问以 **流式响应（Streaming）** 返回回答，支持中文，响应迅速。

> 🎯 提示：浏览器中可看到逐字输出效果，适合构建聊天机器人界面。

---

## 📅 后期规划

我们计划持续更新以下内容：

- [ ] **项目实战**：构建 AI 客服、智能文档问答系统
- [ ] **Graph 教程**：使用 Spring AI Flow 构建复杂 AI 工作流
- [ ] **LangChain4j 系列教程**：对比学习 Java 生态主流 AI 框架
- [ ] **向量数据库集成**：Chroma、PGVector、Milvus 实战
- [ ] **RAG（检索增强生成）完整案例**

欢迎在 [Issues](https://github.com/your-username/spring-ai-tutorial/issues) 中提出你感兴趣的专题！

---

## 📄 许可证

本项目基于 [Apache 2.0 许可证](LICENSE) 开源，可自由使用、修改和分发。

---

## 🙏 致谢

- 📚 [Spring AI 官方文档](https://docs.spring.io/spring-ai/reference/index.html)
- 📚 [Spring AI Alibaba 官方文档](https://java2ai.com/)
- 💡 阿里云通义实验室
- 🌍 以及所有开源社区贡献者

---

🎯 **立即开始你的 Spring AI 之旅吧！**

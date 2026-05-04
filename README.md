# 🎓 AI Agent 作文批改系统

## 一、项目简介

本项目是一个**完整的 AI Agent 教育系统**，围绕“作文/古文批改”场景构建，包含：

* 🧠 AI 批改 Agent（基于大模型）
* 🧑‍🎓 学生管理系统
* 🧑‍🏫 教师批注模块
* 📊 数据可视化分析（评分、趋势）

👉 适用于：

* 教培机构系统原型
* AI Agent 项目展示

---

## 二、技术架构

### 后端

* Spring Boot（REST API）
* MyBatis / JPA
* MySQL（可替换为内存数据库）
* AI 接口（MIMO / OpenAI 兼容）

### 前端

* Vue3 + Element Plus
* ECharts（图表）

### AI Agent

* Prompt 工程
* 单轮/多轮对话
* 可扩展工具调用（评分、纠错）

---

## 三、系统功能模块

### 1️⃣ 用户与权限

| 角色  | 功能        |
| --- | --------- |
| 管理员 | 用户管理、数据统计 |
| 教师  | 批改作文、查看学生 |
| 学生  | 提交作文、查看点评 |

---

### 2️⃣ 作文管理

* 提交作文
* 历史记录
* 作文详情

---

### 3️⃣ AI 批改 Agent

* 自动评分（0-100）
* 四维点评：

  * 立意
  * 结构
  * 文采
  * 语法
* 修改建议

---

### 4️⃣ 数据分析

* 学生成绩趋势（折线图）
* 能力雷达图
* 班级平均分

---

### 5️⃣ 教师辅助

* 手动批注
* AI + 人工对比
* 批改记录

---

## 四、项目结构

```id="proj-struct-01"
ai-agent-system/
├── backend/
│   ├── controller/
│   ├── service/
│   ├── mapper/
│   ├── entity/
│   ├── config/
│   └── data/（假数据初始化）
│
├── frontend/
│   ├── src/
│   │   ├── views/
│   │   │   ├── dashboard/
│   │   │   ├── essay/
│   │   │   ├── student/
│   │   │   └── analysis/
│   │   ├── components/
│   │   └── api/
│
├── ai-agent/
│   └── agent.py
│
└── README.md
```

---

## 五、核心模块设计

### 1️⃣ AI Agent（Python 独立服务）

```python id="agent-core-01"
def review_essay(content):
    prompt = f"""
你是一位高中语文老师，请从以下维度批改作文：
1. 立意
2. 结构
3. 文采
4. 语法
并给出总分（100分制）和修改建议。

作文如下：
{content}
"""

    response = client.chat.completions.create(
        model="mimo-1",
        messages=[{"role": "user", "content": prompt}],
        temperature=0.4,
    )

    return response.choices[0].message.content
```

---

### 2️⃣ 后端接口设计

#### 提交作文

```http id="api-01"
POST /api/essay/submit
```

#### 获取批改结果

```http id="api-02"
GET /api/essay/{id}
```

#### AI 批改

```http id="api-03"
POST /api/ai/review
```

---

### 3️⃣ 数据库设计（核心表）

#### 用户表（user）

```sql id="sql-01"
id | username | role | password
```

#### 作文表（essay）

```sql id="sql-02"
id | user_id | content | score | created_at
```

#### 批改结果表（review）

```sql id="sql-03"
id | essay_id | idea | structure | style | grammar | suggestion
```

---

## 六、假数据初始化

### 1️⃣ 学生数据

```json id="mock-student"
[
  {"id": 1, "name": "张三", "grade": "高一"},
  {"id": 2, "name": "李四", "grade": "高二"}
]
```

---

### 2️⃣ 作文数据

```json id="mock-essay"
[
  {
    "id": 1,
    "user_id": 1,
    "content": "壬戌之秋，七月既望...",
    "score": 85
  }
]
```

---

### 3️⃣ 批改结果

```json id="mock-review"
{
  "idea": "立意清晰，符合原文",
  "structure": "结构完整",
  "style": "文采优美",
  "grammar": "基本无误",
  "suggestion": "可增加理解分析"
}
```

---

## 七、前端页面设计

### 页面列表

| 页面          | 说明     |
| ----------- | ------ |
| 登录页         | 用户登录   |
| 首页Dashboard | 数据统计   |
| 作文提交页       | 输入作文   |
| 批改结果页       | 展示AI点评 |
| 数据分析页       | 图表展示   |

---

### 示例页面（作文提交）

```vue id="vue-01"
<template>
  <div>
    <el-input v-model="content" type="textarea" rows="10"/>
    <el-button @click="submit">提交作文</el-button>
  </div>
</template>
```

---

## 八、系统运行流程

```id="flow-01"
学生提交作文
    ↓
后端保存数据
    ↓
调用 AI Agent
    ↓
返回批改结果
    ↓
前端展示评分 + 点评
```

---

## 九、快速启动

### 1️⃣ 后端

```bash id="run-backend"
cd backend
mvn spring-boot:run
```

### 2️⃣ 前端

```bash id="run-frontend"
cd frontend
npm install
npm run dev
```

### 3️⃣ AI Agent

```bash id="run-agent"
python agent.py
```

---

## 十、可扩展方向
### 🧠 智能化

* 作文风格识别（议论文/记叙文）
* AI 多轮润色
* 错误定位高亮

### 📊 数据分析

* 学生能力画像（标签化）
* 班级排名
* 学习趋势预测

### 🤖 Agent 进阶

* 多 Agent 协作（老师Agent + 助教Agent）
* RAG 知识库（语文教材）

---

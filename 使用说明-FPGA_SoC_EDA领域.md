# FPGA/SoC/EDA领域文献自动拉取使用说明

## 📚 目录
1. [项目简介](#项目简介)
2. [新增功能](#新增功能)
3. [快速开始](#快速开始)
4. [详细配置步骤](#详细配置步骤)
5. [关键词配置指南](#关键词配置指南)
6. [RSS源说明](#rss源说明)
7. [常见问题](#常见问题)
8. [附录](#附录)

---

## 项目简介

这是一个基于 **GitHub Actions** 的全自动文献筛选推送工具。系统会根据您设定的关键词，从订阅的期刊RSS列表中自动抓取最新论文，生成个性化的RSS订阅源，可直接在 **Zotero** 等RSS阅读器中订阅。

### ✨ 核心特点

- **全自动运行**：GitHub Actions每6小时自动拉取最新文献
- **关键词筛选**：支持AND逻辑组合，精确匹配研究方向
- **隐私保护**：关键词可存储在GitHub Secrets中
- **零成本运行**：完全免费的GitHub基础设施
- **即时同步**：Zotero自动更新订阅内容

### 🎯 支持领域

#### 传统学科
- 材料科学、凝聚态物理、化学等

#### 计算机硬件领域 ✨新增
- **FPGA架构与可重构计算**
- **SoC设计与异构计算**
- **EDA工具与方法**（逻辑综合、布局布线、时序分析、形式验证）
- **IC设计**（模拟电路、数字电路、混合信号设计）

---

## 新增功能

本次更新（2026-01-08）新增了 **FPGA/SoC/EDA** 领域的支持，包括：

### 1. 新增RSS源（6个）

| 领域 | 期刊/会议 | 缩写 | RSS源地址 |
|------|-----------|------|-----------|
| EDA | IEEE Transactions on Computer-Aided Design | TCAD | `https://ieeexplore.ieee.org/rss/TOC43.XML` |
| IC设计 | IEEE Journal of Solid-State Circuits | JSSC | `https://ieeexplore.ieee.org/rss/TOC4.XML` |
| IC设计 | IEEE Transactions on VLSI Systems | TVLSI | `https://ieeexplore.ieee.org/rss/TOC92.XML` |
| IC设计 | IEEE Transactions on Circuits and Systems I | TCAS-I | `https://ieeexplore.ieee.org/rss/TOC8919.XML` |
| IC设计 | IEEE Transactions on Circuits and Systems II | TCAS-II | `https://ieeexplore.ieee.org/rss/TOC8920.XML` |
| 预印本 | arXiv Hardware Architecture | cs.AR | `https://rss.arxiv.org/rss/cs.AR` |

### 2. 新增文件

- **`keywords.dat`** - 测试用关键词文件
- **`keywords_fpga_example.dat`** - 专业关键词库（100+术语）

### 3. 更新文档

- README.md已更新，包含FPGA/EDA领域完整说明

---

## 快速开始

### 前置条件

- GitHub账号
- Zotero软件（或其他RSS阅读器）

### 5分钟快速部署

```bash
# 1. Fork本仓库
访问 https://github.com/xqx2020smile/paper-feed
点击右上角 Fork 按钮

# 2. 删除旧的RSS文件（如果存在）
进入你Fork的仓库，删除 filtered_feed.xml

# 3. 启用GitHub Actions
进入 Actions 页面
点击 "I understand my workflows, go ahead and enable them"

# 4. 配置GitHub Pages
Settings → Pages
Source: Deploy from a branch
Branch: main / (root)
点击 Save

# 5. 手动运行一次
Actions → Auto RSS Fetch → Run workflow → Run workflow

# 6. 等待3-5分钟，在Zotero中订阅
URL: https://你的用户名.github.io/paper-feed/filtered_feed.xml
```

---

## 详细配置步骤

### 第一步：Fork仓库

1. 访问 https://github.com/xqx2020smile/paper-feed
2. 点击右上角 **Fork** 按钮
3. 等待Fork完成

### 第二步：配置关键词

#### 方式1：直接编辑文件（公开）

1. 进入你的仓库
2. 编辑 `keywords.dat` 文件
3. 每行一个关键词，支持AND检索式
4. 提交更改

**示例内容：**
```
FPGA
System-on-Chip
High-Level Synthesis AND FPGA
Hardware Accelerator
EDA
Logic Synthesis
```

#### 方式2：使用Secrets（推荐，隐私）

1. 进入 `Settings` → `Secrets and variables` → `Actions`
2. 点击 `New repository secret`
3. Name填写：`RSS_KEYWORDS`
4. Secret填写关键词（每行一个）
5. 点击 `Add secret`

**Secrets示例：**
```
FPGA AND Deep Learning
SoC AND Network-on-Chip
High-Level Synthesis
Place AND Route AND FPGA
Reconfigurable Computing
```

> 💡 **提示**：使用Secrets可以保护您的研究方向不被公开

### 第三步：配置RSS源（可选）

如果您想添加其他期刊，编辑 `journals.dat` 文件：

```bash
# 在文件末尾添加新的RSS源
# 格式：每行一个URL

# 示例：添加新期刊
https://ieeexplore.ieee.org/rss/TOC123.XML
```

### 第四步：启用GitHub Actions

1. 进入 `Actions` 页面
2. 如果看到"Workflows aren't being run on this forked repository"
3. 点击 `I understand my workflows, go ahead and enable them`

### 第五步：配置GitHub Pages

1. 进入 `Settings` → `Pages`
2. 在 **Build and deployment** 下：
   - Source: 选择 `Deploy from a branch`
   - Branch: 选择 `main` 分支，目录选择 `/(root)`
3. 点击 `Save`
4. 等待几分钟，Pages会自动部署

### 第六步：手动运行一次

1. 进入 `Actions` 页面
2. 点击左侧 `Auto RSS Fetch` 工作流
3. 点击右侧 `Run workflow` 按钮
4. 再次点击绿色的 `Run workflow` 确认
5. 等待3-5分钟，工作流运行完成
6. 生成的 `filtered_feed.xml` 会自动提交到仓库

### 第七步：在Zotero中订阅

1. 打开 Zotero
2. 点击 `文件` → `新建文献库` → `新建订阅` → `从网址`
3. 输入订阅URL：
   ```
   https://你的GitHub用户名.github.io/paper-feed/filtered_feed.xml
   ```
   例如：`https://xqx2020smile.github.io/paper-feed/filtered_feed.xml`
4. 设置标题（自定义）
5. 在高级选项中，建议设置更新间隔为 **6小时或更短**
6. 点击确定

---

## 关键词配置指南

### 基本语法

- **单个关键词**：直接写，例如 `FPGA`
- **精确匹配**：使用AND连接，例如 `FPGA AND Deep Learning`
- **大小写**：不敏感（自动转小写匹配）

### 推荐关键词策略

#### 策略1：宽泛+精确结合

```
# 宽泛关键词（高召回）
FPGA
SoC
EDA

# 精确关键词（高精度）
FPGA AND Deep Learning
High-Level Synthesis AND FPGA
Network-on-Chip
```

#### 策略2：核心术语

```
# FPGA核心
Field-Programmable Gate Array
Reconfigurable Computing
LUT AND FPGA
BRAM

# EDA核心
Logic Synthesis
Place AND Route
Timing Analysis
Formal Verification
```

#### 策略3：应用领域

```
# AI加速
FPGA AND Neural Network
FPGA AND CNN
Hardware Accelerator AND FPGA

# 边缘计算
Edge Computing AND FPGA
Low Power AND FPGA
FPGA AND Inference
```

### 完整关键词库

参考仓库中的 `keywords_fpga_example.dat` 文件，包含8大类别、100+专业术语：

1. FPGA架构与设计
2. SoC架构
3. EDA工具与方法
4. 硬件描述语言
5. FPGA应用领域
6. IC设计
7. 测试与验证
8. 先进工艺与架构

---

## RSS源说明

### IEEE Xplore RSS源

IEEE的期刊RSS源格式为：
```
https://ieeexplore.ieee.org/rss/TOC[期刊编号].XML
```

常用期刊编号：
- `43` - IEEE TCAD
- `4` - IEEE JSSC
- `92` - IEEE TVLSI
- `8919` - IEEE TCAS-I
- `8920` - IEEE TCAS-II

### arXiv RSS源

arXiv的RSS源格式为：
```
https://rss.arxiv.org/rss/[类别].xml
```

计算机科学类别：
- `cs.AR` - Hardware Architecture
- `cs.DC` - Distributed Computing
- `cs.PF` - Performance

### DBLP会议RSS（可选）

访问 http://services.ceon.pl/dblpfeeds/ 可订阅顶级会议RSS：
- FPGA - ACM/SIGDA International Symposium
- FPL - Field-Programmable Logic
- FCCM - Field-Programmable Custom Computing
- DAC - Design Automation Conference
- ICCAD - Computer-Aided Design
- DATE - Design, Automation & Test in Europe

---

## 常见问题

### Q1: 为什么RSS订阅在Zotero中显示为空？

**A:** 可能的原因：
1. GitHub Pages还未部署完成（等待5-10分钟）
2. 工作流还未运行（手动触发一次）
3. URL输入错误（检查用户名和路径）

**解决方法：**
```bash
# 检查URL格式
https://你的用户名.github.io/paper-feed/filtered_feed.xml

# 在浏览器中访问，确认能看到XML内容
```

### Q2: 如何查看工作流运行状态？

**A:**
1. 进入仓库的 `Actions` 页面
2. 查看 `Auto RSS Fetch` 工作流
3. 点击最近的运行记录，查看详细日志

### Q3: 关键词不生效怎么办？

**A:** 检查清单：
1. 关键词大小写（自动转小写，无需担心）
2. 关键词拼写是否正确
3. AND必须大写
4. Secrets配置是否正确（NAME必须是`RSS_KEYWORDS`）

### Q4: 如何添加新的期刊RSS源？

**A:**
1. 编辑 `journals.dat`
2. 在末尾添加新的RSS URL（每行一个）
3. 提交更改
4. 等待下次自动运行或手动触发

### Q5: RSS更新频率是多少？

**A:**
- GitHub Actions：每6小时自动运行一次
- Zotero建议：设置为6小时或更短
- 手动触发：随时可在Actions中手动运行

### Q6: 可以同时订阅多个领域吗？

**A:**
可以！`journals.dat` 已包含多个领域的RSS源。通过配置不同的关键词，可以筛选不同领域的论文。

### Q7: 如何查看匹配到了多少篇论文？

**A:**
1. 查看 Actions 运行日志
2. 日志中会显示"Added X new entries"
3. 或直接在Zotero中查看订阅内容

### Q8: 可以导出筛选后的论文列表吗？

**A:**
可以！在Zotero中：
1. 选中订阅的文献
2. 右键 → 导出条目
3. 选择格式（BibTeX、RIS等）
4. 保存文件

---

## 附录

### 附录A：FPGA/EDA顶级期刊会议清单

#### 期刊

| 期刊名称 | 缩写 | 影响因子 | 主办方 | 主题 |
|----------|------|----------|--------|------|
| IEEE Transactions on Computer-Aided Design | TCAD | ~4.0 | IEEE | EDA工具、算法、方法学 |
| IEEE Journal of Solid-State Circuits | JSSC | ~5.0 | IEEE | 模拟/数字/混合信号电路 |
| IEEE Transactions on VLSI Systems | TVLSI | ~3.0 | IEEE | VLSI设计与实现 |
| IEEE Transactions on Circuits and Systems I | TCAS-I | ~6.0 | IEEE | 电路与系统理论 |
| IEEE Transactions on Circuits and Systems II | TCAS-II | ~3.5 | IEEE | 电路与系统快报 |

#### 顶级会议

| 会议名称 | 缩写 | CCF等级 | 主题 |
|----------|------|---------|------|
| ACM/SIGDA Int'l Symposium on FPGAs | FPGA | A | FPGA架构、工具、应用 |
| Field-Programmable Logic and Applications | FPL | B | 可重构计算 |
| Field-Programmable Custom Computing Machines | FCCM | B | FPGA定制计算 |
| Design Automation Conference | DAC | A | EDA全领域 |
| Int'l Conference on Computer-Aided Design | ICCAD | A | CAD工具与方法 |
| Design, Automation & Test in Europe | DATE | B | 设计自动化 |
| Int'l Solid-State Circuits Conference | ISSCC | A+ | 固态电路设计 |

### 附录B：专业术语中英对照

| 中文 | 英文 | 缩写 |
|------|------|------|
| 现场可编程门阵列 | Field-Programmable Gate Array | FPGA |
| 片上系统 | System-on-Chip | SoC |
| 电子设计自动化 | Electronic Design Automation | EDA |
| 高层次综合 | High-Level Synthesis | HLS |
| 硬件描述语言 | Hardware Description Language | HDL |
| 寄存器传输级 | Register Transfer Level | RTL |
| 逻辑综合 | Logic Synthesis | - |
| 布局布线 | Place and Route | P&R |
| 时序分析 | Timing Analysis | STA |
| 形式验证 | Formal Verification | - |
| 查找表 | Look-Up Table | LUT |
| 数字信号处理器 | Digital Signal Processor | DSP |
| 块随机存储器 | Block RAM | BRAM |
| 片上网络 | Network-on-Chip | NoC |
| 超大规模集成电路 | Very Large Scale Integration | VLSI |
| 专用集成电路 | Application-Specific Integrated Circuit | ASIC |
| 可测试性设计 | Design for Testability | DFT |
| 自动测试向量生成 | Automatic Test Pattern Generation | ATPG |

### 附录C：推荐阅读资源

#### 在线资源
- [FPGA开发者论坛](https://forums.xilinx.com)
- [Intel FPGA文档](https://www.intel.com/content/www/us/en/programmable/documentation)
- [ACM SIGDA](https://sigda.org/)
- [IEEE CEDA](https://ieee-ceda.org/)

#### 教科书推荐
- *FPGA Prototyping by VHDL Examples* - Pong P. Chu
- *Digital Design and Computer Architecture* - David Harris, Sarah Harris
- *VLSI Design* - Debaprasad Das

#### 开源工具
- [Yosys](https://yosyshq.net/yosys/) - 开源综合工具
- [Verilator](https://www.veripool.org/verilator/) - Verilog仿真器
- [OpenROAD](https://theopenroadproject.org/) - 开源EDA流程

---

## 更新日志

### v1.1.0 (2026-01-08)
- ✨ 新增FPGA/SoC/EDA领域支持
- ✨ 添加6个IEEE期刊RSS源
- ✨ 添加arXiv cs.AR预印本源
- ✨ 创建100+专业关键词库
- 📝 更新完整使用文档

### v1.0.0 (初始版本)
- 支持材料科学、物理、化学等传统学科
- 基于GitHub Actions自动运行
- 支持Zotero订阅

---

## 技术支持

如有问题，请通过以下方式联系：

- **GitHub Issues**: https://github.com/xqx2020smile/paper-feed/issues
- **邮件**: [您的邮箱]

---

## 许可证

本项目基于 MIT 许可证开源。

---

## 致谢

- 感谢 [Jarvis-Towne/paper-feed](https://github.com/Jarvis-Towne/paper-feed) 提供的原始项目
- 感谢 IEEE Xplore、arXiv、DBLP等平台提供的RSS服务

---

**最后更新时间：2026-01-08**

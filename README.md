# embodied-portfolio

具身智能方向的作品集主页、技术博客、面试速答卡与简历。

## 结构

- `index.html` —— GitHub Pages 主页，项目入口与核心亮点
- `blog/` —— 博客文章
- `cards/` —— 30 道速答卡
- `resume/` —— 简历

## 项目列表

### 1. 具身世界模型实验室（vla-world-model-lab）

- **代码**：https://github.com/wzh0310/vla-world-model-lab
- **定位**：基于 Dreamer 范式的轻量化具身世界模型，在 MuJoCo PushCube 离线轨迹上完成 RSSM 动力学、多步开环想象、奖励 / 价值头与想象策略全链路验证。
- **核心亮点**：
  - 20 步开环想象测试集 SSIM **0.7511**、奖励 MAE **0.0242**，均超过自设门槛；
  - 同口径动力学消融中 RSSM 20 步 PSNR **23.71 dB**，纯前馈 MLP 基线仅 **17.61 dB**；
  - KL 与 free bits 消融证明：移除 KL 后预测从第 1 步即崩塌，`free_bits` 调参带来 **+5.7 dB** 提升；
  - 完整实现想象策略训练（λ-return + PPO + 熵正则），探针回报 **-0.04 → +0.85**，并如实记录真实环境 0% 成功率的 sim-to-real 瓶颈；
  - 想象步长、奖励头、样本效率与 50 场景泛化测试全套消融，结论有机器可读数据与图表支撑；
  - 接入 OpenVLA 7B 完成视觉-语言-动作推理全链路：单帧推理、单步闭环、回合级闭环（bf16 单步 0.305 s / 3.65 Hz，int4 显存 4.55 GB），并如实记录零样本 0/3 成功率的域差距瓶颈与三项问题排查；
  - 构建自定义 VLA 数据集：25 条专家轨迹 / 928 帧、5 类共 20 条英文指令模板全覆盖、起点 ±0.05 m 随机化、带时间戳与达标筛选，对接 LoRA 微调字段规范；
  - 完成回合级语言指令标注与三元组配对：指令-图像序列-动作序列一一对应、帧内共享同一指令，产出数据索引表（JSON/CSV）与 task_index 映射，校验全部通过。
  - 完成数据预处理与标准化：按模型 processor 双视觉主干口径对齐图像（DINOv2 / SigLIP 双视图），动作 min-max 归一化到 [-1,1] 并落盘复用参数，按类别分层划分 20 训练 / 5 验证，产出 PyTorch Dataset + DataLoader 可直接加载的数据集，28 项校验全通过。
- **技术栈**：PyTorch、MuJoCo、Gymnasium、RSSM、DreamerV2、OpenVLA、LoRA、LeRobot、HDF5、Matplotlib。
- **详细文档**：项目 `README.md` 与 `docs/` 目录。

### 2. MuJoCo 仿真工作台（mujoco-studio-lab）

Panda 推立方体环境、EGL 离屏渲染、奖励设计与轨迹导出，为世界模型与策略库提供统一数据来源。

### 3. 具身策略算法库（embodied-policy-zoo）

BC / ACT / PPO / IQL / DT 统一实现与评估，提供世界模型样本效率对比与泛化基准。

---

> 所有项目聚焦算法层验证，不涉及底层机器人控制或实机部署。

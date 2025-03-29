# Orun - GPU任务调度与资源监控系统

## 功能特性

- 🚀 ​**智能GPU调度**  
  自动分配可用GPU资源，支持多任务队列管理
- 📊 ​**实时资源监控**  
  记录GPU/CPU/内存/磁盘IO使用情况，生成可视化报告
- 🔍 ​**错误自动诊断**  
  检测常见错误类型（显存不足/CUDA错误等）
- ⚡ ​**多模式运行**  
  支持前台实时输出和后台守护进程模式
- 📂 ​**日志追踪**  
  自动保存任务输出日志和详细指标数据

## 安装指南
```bash
git clone https://github.com/dyedd/orun
cd orun
# 可全局安装/或者conda中安装，每次使用前激活环境
pip install psutil
chmod +x orun
# 全局安装/你也可以用户目录安装，给自己使用
sudo ln -s $(pwd)/orun /usr/local/bin/orun
```
### 验证
```bash
which orun
```
### 卸载
```bash
sudo rm /usr/local/bin/orun
sudo rm -rf /tmp/orun*
```

## 快速使用
### 基本命令
```bash
# 相当于python train.py，仅多了调度和监控，默认单卡
orun python train.py

# 目前仅支持单机多卡
orun --gpus 2 python train.py

# 后台模式运行任务
orun -d --gpus 2 python train.py

# 查看系统状态
orun --status
```

### 输出日志结构
```
/tmp/orun_logs/
├── user_0.log            # 任务标准输出
└── user_0_metrics.jsonl  # 详细监控指标
```
### 配置选项
|参数|说明|默认值|
|----|----|----|
|`--gpus`|N|需要的GPU数量|1|
|`-d`|后台守护模式|关闭|
|`--status`|显示系统状态|-|
|`--version`|显示版本信息|-|
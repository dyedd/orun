# Orun - GPU任务调度

## 功能特性

- 🚀 ​**智能GPU调度**  
  自动分配可用GPU资源，支持多任务队列管理

## 安装指南
```bash
git clone https://github.com/dyedd/orun
cd orun
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
# 相当于python train.py，仅多了调度，默认单卡
orun python train.py

# 目前仅支持单机多卡
orun --gpus 2 python train.py

# 查看系统状态
orun --status
```

```
### 配置选项
|参数|说明|默认值|
|----|----|----|
|`--gpus`|N|需要的GPU数量|1|
|`--status`|显示系统状态|-|
|`--version`|显示版本信息|-|
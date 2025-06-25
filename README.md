# Orun - 一个轻量级的GPU任务调度工具

`orun` 是一个命令行工具，旨在简化在多用户共享的GPU服务器上的资源管理。它模拟了Slurm中 `srun`和 `sbatch`的核心功能，但无需任何复杂的守护进程或配置，仅通过一个Python脚本文件即可实现。

## 核心特性

- **独占式GPU资源分配**：确保一个任务占用的GPU不会被其他 `orun` 任务抢占。
- **自动排队等待**：当没有足够的空闲GPU时，新任务会自动进入等待队列，直到有资源释放。
- **灵活的资源指定**：
  - 按数量请求GPU（例如，`orun -n 2 ...`）。
  - 按特定ID请求GPU（例如，`orun -g 0,3 ...`）。
- **前台/后台运行模式**：
  - **前台模式 (默认, 类似 `srun`)**: 在当前终端中运行任务，并实时显示输出，直到任务结束。
  - **后台模式 (`-b`, 类似 `sbatch`)**: 将任务提交到后台运行，立即释放你的终端，并自动生成日志文件。
- **环境隔离**: 自动设置 `CUDA_VISIBLE_DEVICES` 环境变量，使你的程序只能“看到”被分配的GPU，避免资源冲突。
- **轻量级与零依赖**：单个Python脚本，无需安装复杂的依赖库或服务。
- **健壮性**: 即使任务进程被强行终止 (`kill -9`)，锁机制也能保证资源被正确释放。

## 安装指南
安装 `orun` 非常简单：
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

`orun` 的基本语法结构如下：

```
orun [OPTIONS] -- your_command [COMMAND_ARGS]
```
`--` 是一个可选的分隔符，用于明确区分 `orun` 的选项和你要执行的命令。

### 1. 前台运行任务 (默认模式)

#### 示例 1: 请求一张可用的GPU运行Python脚本
```bash
orun python my_train.py --lr 0.01 --epochs 10
```
`orun` 会找到一张空闲的GPU，分配给任务，然后在你的终端上显示 `my_train.py` 的输出。你的终端将被阻塞，直到任务完成。

#### 示例 2: 请求两张可用的GPU
```bash
orun -n 2 -- python my_multi_gpu_train.py
```
`orun` 会等待，直到有两张GPU可用，然后同时占用它们。

#### 示例 3: 指定使用GPU 0和GPU 3
```bash
orun -g 0,3 -- ./my_compiled_program
```
如果GPU 0或GPU 3中任何一个当前被占用，`orun` 会一直等待它们都变为空闲。

### 2. 后台提交任务 (`-b` 选项)

#### 示例 4: 提交一个后台任务，使用一张GPU
```bash
orun -b -- python my_long_experiment.py
```
**终端输出:**
```
2025-06-25 17:13:11 [orun] 成功获取GPU: [0]
2025-06-25 17:13:11 [orun] PID: 765390, Log: /home/dyedd/orun-job765390-20250625_171311.log
```
- 你的shell会立即返回，可以继续执行其他命令。
- 任务的所有输出（包括标准输出和错误）都会被重定向到自动生成的日志文件中。
- 你可以使用 `tail -f orun-job...log` 来实时查看任务进度。

#### 示例 5: 后台提交任务到指定的GPU
```bash
orun -b -g 2 -- bash ./run_all.sh
```
这会将 `run_all.sh` 脚本提交到后台，并强制它在GPU 2上运行（如果可用）。

### 3. 查看版本信息

```bash
orun --version
```

## 贡献

欢迎提交 Issues 和 Pull Requests 来改进 `orun`！
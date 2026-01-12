# 3DGS-Covariance-Analysis

基于 3D Gaussian Splatting 的高维协方差投影与微分流形优化研究。

本仓库用于存放《人工智能数学》期末课程论文的实验代码与数据支撑。本项目核心在于探讨 3D 辐射场表示中，二阶几何特征（协方差矩阵）在非线性透视投影下的保真映射关系，并实现了基于单位四元数参数化的微分流形优化验证。

## 核心数学原理 (Mathematical Core)

本项目的研究重点在于以下三个数学模块的推导与验证：

### 1. 协方差矩阵的正定参数化

为了在优化过程中保持协方差矩阵 $\Sigma$ 的正定性，将其分解为旋转 $R$ 和缩放 $S$：

$$
\Sigma = R S S^T R^T
$$

其中旋转矩阵 $R$ 由单位四元数 $q$ 映射而成，确保了参数空间在 $SO(3)$ 流形上的严谨性。

### 2. 雅可比投影矩阵 (Jacobian Matrix)

针对透视投影 $f(x, y, z) = (x/z, y/z)$ 的非线性特性，在均值 $\mu$ 处进行一阶泰勒展开，导出雅可比矩阵 $J$：

$$
J = \begin{bmatrix}
 f_x/z & 0 & -f_x \cdot x/z^2 \\
 0 & f_y/z & -f_y \cdot y/z^2
\end{bmatrix}
$$

### 3. Zwicker 投影公式

利用上述线性化近似，将 3D 协方差投影至 2D 像平面：

$$
\Sigma' = J W \Sigma W^T J^T
$$

本项目通过数值仿真，量化了该一阶近似在不同深度 $z$ 下的残差分布。

## 项目功能 (Features)

- [x] 数值验证模块：对比解析雅可比投影与暴力数值采样投影的残差。
- [x] D435i 数据支持：读取真实相机内参，进行基于物理参数的投影仿真。
- [x] 流形优化展示：演示单位四元数在梯度更新下的归一化保持（Projected Gradient Descent）。

## 快速开始 (Getting Started)

### 1. 环境配置

请务必理解每一个指令的含义。

**Bash (Linux/macOS)**

```bash
# 创建项目工作目录
# mkdir: make directories (创建目录)
# -p: parents (递归创建父目录，即使父目录不存在也不会报错)
mkdir -p ~/3DGS-Covariance-Analysis && cd ~/3DGS-Covariance-Analysis

# 建立 Python 虚拟环境以隔离依赖
# python3: Python 解释器
# -m: module (运行指定的模块)
# venv: virtual environment (创建一个轻量级的虚拟环境)
python3 -m venv venv

# 激活虚拟环境
# source: 在当前 shell 环境中执行脚本文件内容
source venv/bin/activate

# 安装计算库
# pip: package installer for python (Python 包安装工具)
# numpy: Numerical Python (提供高效的矩阵运算和线性代数函数)
# matplotlib: Matrix Plotting Library (基础绘图库，用于可视化 2D 椭圆)
pip install numpy matplotlib pyrealsense2
```

**PowerShell (Windows)**

```powershell
# 进入项目目录（示例）
cd "D:\VScode Project\3DGS-Covariance-Analysis"

# 创建虚拟环境
python -m venv venv

# 激活虚拟环境
.\venv\Scripts\Activate.ps1

# 安装依赖
pip install numpy matplotlib pyrealsense2
```

### 2. 运行验证脚本

执行以下命令运行雅可比投影验证实验：

```bash
python3 src/verify_projection.py
```

Windows 下（PowerShell）可用：

```powershell
python .\src\verify_projection.py
```

## 目录结构 (Repository Structure)

```text
3DGS-Covariance-Analysis/
├── docs/               # 存放论文 (Paper) 与 汇报 PPT (Presentation)
├── src/                # 核心算法实现 (Jacobian, Covariance Mapping)
├── data/               # D435i 采集的传感器内参及深度图示例
├── scripts/            # 可视化与误差分析脚本 (Error Analysis)
└── README.md           # 本说明文件
```

## 引用 (Citation)

本项目推导参考了以下经典工作：

- Zwicker, et al. "EWA Splatting", 2002.
- Kerbl, et al. "3D Gaussian Splatting for Real-Time Radiance Field Rendering", 2023.

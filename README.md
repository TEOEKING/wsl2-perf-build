# wsl2-perf-build
WSL2 环境下从源码编译 perf 工具指南
# WSL2 环境下从源码编译 perf 工具指南

## 1. 获取源码
```bash
# 创建工作目录
mkdir -p ~/wsl2-kernel
cd ~/wsl2-kernel

# 克隆微软的 WSL2 内核仓库
git clone https://github.com/microsoft/WSL2-Linux-Kernel.git
cd WSL2-Linux-Kernel

# 检查当前 WSL2 内核版本
uname -r
```

## 2. 安装编译依赖
```bash
sudo apt-get install flex bison libelf-dev libdw-dev \
    libaudit-dev libslang2-dev libunwind-dev libperl-dev \
    binutils-dev liblzma-dev libnuma-dev python3-dev
```

## 3. 选择正确的源码版本
```bash
# 查看当前 WSL2 内核版本
uname -r

# 切换到对应的分支或标签
git checkout linux-msft-wsl-5.15.y  # 使用实际的内核版本
```

## 4. 编译 perf 工具

### 基本编译
```bash
# 进入 perf 工具目录
cd tools/perf

# 编译
make

# 安装
sudo make install
```

### 静态编译（可选）
```bash
# 清理之前的编译
make -C tools/perf clean

# 静态编译
make -C tools/perf LDFLAGS=-static
```

### 最小依赖编译（可选）
```bash
make -C tools/perf \
    LDFLAGS=-static \
    NO_GTK2=1 \
    NO_LIBAUDIT=1 \
    NO_LIBPERL=1 \
    NO_LIBPYTHON=1 \
    NO_LIBBIONIC=1
```

## 5. 安装
```bash
# 安装编译好的 perf
sudo cp tools/perf/perf /usr/local/bin/
```

## 6. 验证安装
```bash
# 检查 perf 版本
perf --version

# 测试基本功能
perf list
```

## 注意事项

### 系统要求
1. 足够的磁盘空间（至少需要几GB）
2. 完整的开发工具链
3. WSL2 环境（不支持 WSL1）

### 常见问题解决
1. 编译错误
   - 检查所有依赖是否正确安装
   - 确认源码版本与系统内核版本匹配
   - 查看详细编译日志

2. 运行错误
   - 确认权限设置正确
   - 检查系统调用是否被限制
   - 验证 WSL2 内核支持

### 性能优化建议
1. 使用静态编译提高性能
2. 根据需要选择必要的功能模块
3. 考虑使用最小依赖编译

## 参考资源
1. [Microsoft WSL2-Linux-Kernel GitHub](https://github.com/microsoft/WSL2-Linux-Kernel)
2. [Linux perf 工具文档](https://perf.wiki.kernel.org/index.php/Main_Page)
3. WSL2 文档：`wsl --help` 
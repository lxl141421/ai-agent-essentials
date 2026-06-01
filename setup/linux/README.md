# Linux 配置指南

> Linux 原生环境部署 AI Agent。

---

## GPU 驱动

### NVIDIA

```bash
# Ubuntu
sudo apt install nvidia-driver-550
# 或用官方 .run 安装包

# 验证
nvidia-smi
```

### AMD

```bash
# ROCm 安装（Ubuntu）
sudo apt install rocm-hip-runtime
# 注意：不是所有 AMD GPU 都支持 ROCm
```

## Docker 集成

```bash
# GPU 支持
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/libnvidia-container/gpgkey | sudo apt-key add -
# 安装 nvidia-container-toolkit
```

## systemd 服务

```bash
# Hermes Gateway 作为系统服务
hermes gateway install
systemctl --user enable hermes-gateway
systemctl --user start hermes-gateway

# 开机自启
sudo loginctl enable-linger $USER
```

---

*待补充：更多 Linux 踩坑记录。欢迎提交 PR！*

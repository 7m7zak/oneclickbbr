#!/bin/bash

# 检查内核版本
KERNEL_VERSION=$(uname -r)
echo "当前内核版本: $KERNEL_VERSION"

# 添加BBR配置到sysctl.conf
echo "添加BBR配置到 /etc/sysctl.conf"
echo "net.core.default_qdisc=fq" | sudo tee -a /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" | sudo tee -a /etc/sysctl.conf

# 应用新的配置
echo "应用新的配置"
sudo sysctl -p

# 验证BBR是否启用
echo "验证BBR是否启用"
AVAILABLE_CC=$(sysctl net.ipv4.tcp_available_congestion_control)
CURRENT_CC=$(sysctl net.ipv4.tcp_congestion_control)

echo "可用的拥塞控制算法: $AVAILABLE_CC"
echo "当前使用的拥塞控制算法: $CURRENT_CC"

if [[ $CURRENT_CC == *"bbr"* ]]; then
    echo "BBR已成功启用"
else
    echo "BBR启用失败"
fi

# 验证BBR模块是否加载
echo "验证BBR模块是否加载"
BBR_LOADED=$(lsmod | grep bbr)

if [[ -z $BBR_LOADED ]]; then
    echo "BBR模块未加载，尝试手动加载"
    sudo modprobe tcp_bbr
    BBR_LOADED=$(lsmod | grep bbr)
    if [[ -z $BBR_LOADED ]]; then
        echo "BBR模块加载失败，请检查内核版本或系统配置"
    else
        echo "BBR模块已成功加载"
    fi
else
    echo "BBR模块已加载"
fi

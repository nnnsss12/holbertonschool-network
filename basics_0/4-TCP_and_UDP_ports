#!/usr/bin/env bash
# 4-TCP_and_UDP_ports - Dinlənən portları PID və proqram adı ilə göstərir

if command -v netstat &> /dev/null; then
    netstat -lpn 2>/dev/null
else
    ss -lpn 2>/dev/null
fi

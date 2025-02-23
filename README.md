# 使用

```bash
wget -O ss-rust.sh --no-check-certificate https://raw.githubusercontent.com/tunecc/Shadowsocks-Rust/refs/heads/master/ss-rust.sh && chmod +x ss-rust.sh && ./ss-rust.sh
```

# 修改了什么

我在LXC的服务器上面使用的时候会出错，在看了[Snell一键部署-由jinqians大佬撰写](https://github.com/jinqians/snell.sh)的脚本后发现是
```
ExecStartPre=/bin/sh -c "ulimit -n 51200"
```

的问题，下面是GPT的回答

```txt
ExecStartPre=/bin/sh -c "ulimit -n 51200"
这是在正式启动服务前执行的一个预处理命令。它调用 shell 命令 ulimit 来将当前 shell 的最大文件描述符限制设置为 51200。
注意：这个设置仅对 ExecStartPre 执行的 shell 及其子进程有效，且如果服务本身并没有继承这个调整的话，实际生效的还是 systemd 的 LimitNOFILE 指定的数值。

我不设置这个可以吗？

如果不设置 ExecStartPre 来调整 ulimit，同样可能导致服务继承默认的文件打开数限制，从而在实际运行中遇到 "too many open files" 的错误。

总结来说，如果你的服务并不会创建大量并发连接或者文件操作，且系统默认的文件描述符数足够使用，那么可以不设置。但如果预期负载较高，建议适当提高限制，以避免潜在的资源不足问题。
```

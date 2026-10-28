# Clash / OpenClash 自动最快订阅模板

普通代理流量通过 `Proxies → 自动最快`，每 **30 分钟（1800 秒）**检测全部节点，选择延迟最低的可用节点。AI、Google、YouTube、Netflix 等保留独立选择入口；特殊分组可手动指定节点。这里比较连接延迟，不是下载带宽。

## 模板地址

https://raw.githubusercontent.com/yzwuxiaowu/clash-rules/main/auto-fastest.ini

配套基础配置：

https://raw.githubusercontent.com/yzwuxiaowu/clash-rules/main/clash-base.yaml

这是兼容 subconverter 的**外部配置地址**，需要配合原始订阅使用，不能当作完整节点订阅直接导入。

## 推荐：在自己的路由器转换

本地转换避免把原始订阅链接交给公共转换站。此环境的转换后端绑定在路由器的回环地址：

    http://127.0.0.1:25500/sub

只有路由器自身能访问这个地址。Mac 上的 `127.0.0.1` 指向 Mac，不是路由器。

在 OpenClash 的“配置订阅”中操作：

1. 添加或编辑订阅，填入服务商提供的原始订阅链接。
2. 启用“在线订阅转换”。此功能名称不代表必须使用公共网站。
3. 转换服务地址使用上述本地后端。
4. 模板选择“自定义”，填入本仓库的 `auto-fastest.ini` 原始文件地址。
5. 保存并更新配置，检查节点数，再切换到新配置。
6. 在控制面板确认 `Proxies` 选择“自动最快”。按需为 AI、DisneyPlus 等特殊组指定节点。

不同 OpenClash 版本字段名称可能稍有差异。换订阅时只需替换服务商链接；模板地址可以继续使用。旧订阅的手动节点选择不一定在新订阅中存在，需要重新选择。

## API 格式

    http://127.0.0.1:25500/sub?target=clash&url=<URL编码后的原始订阅>&config=https%3A%2F%2Fraw.githubusercontent.com%2Fyzwuxiaowu%2Fclash-rules%2Fmain%2Fauto-fastest.ini&emoji=false

由 OpenClash 界面生成参数，避免手动拼接原始订阅中的 `&` 等字符。完整转换链接含订阅密钥，应当保密。

## 内容与兼容性

- `auto-fastest.ini`：20 个分组，以所有输入节点生成“自动最快”。
- `clash-base.yaml`：当前配置导出的 8223 条分流规则及通用基础配置，不含任何节点或订阅密钥。
- 国内及已配置直连的规则仍直连；普通代理流量和兜底规则跟随 `Proxies`。
- 当前使用的转换后端是支持 AnyTLS 的 [subconverter 分支](https://github.com/asdlokj1qpi233/subconverter)。传统旧版转换器可能丢弃新协议节点，转换后必须检查节点是否完整。
- 每次启动会进行初次可用性检测，之后按 30 分钟周期检测。
- GitHub 只保存公共分组和规则。不要上传带密码的运行配置或机场订阅链接。
- 当前 NeoFastline 的节点域名需要专用 DNS，已在路由器单独配置。此通用模板不公开专用 DNS 地址；在其他设备使用时仍需按订阅服务商要求配置。

## 本地后端版本

容器镜像固定为：

    asdlokj1qpi23/subconverter@sha256:eb9a60aa2e44366eee513f6070ea09bd41e331ee96d1bc315c1f55f96b115c03

监听 `127.0.0.1:25500`，不会向局域网或公网开放转换接口。该实例随路由器 Docker 启动。

## 维护与恢复

修改测速周期时编辑 `auto-fastest.ini` 中的 `1800,,0`，单位为秒，然后在 OpenClash 更新订阅。现有路由器自定义覆写也可能覆盖同名组，需同步修改或移除对应覆写。

替换旧版文件采用正常 Git 提交，历史版本仍可在提交记录中找回。

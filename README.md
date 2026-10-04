# 1Password 7 Chrome 本地兼容版（非官方）

Manifest V3 兼容适配，当前版本 **4.7.6.3**。使用已有的 1Password 7 经典 Native Messaging 组件，尝试恢复新版 Chrome 中的桌面登录选择和填充。

## 安装

1. 下载本仓库并解压，或克隆仓库。
2. 打开 `chrome://extensions`，开启开发者模式。
3. 点击“加载已解压的扩展程序”，选择包含 `manifest.json` 的仓库目录。
4. 打开并解锁 1Password 7，启用其扩展助手，在普通登录网页点击扩展图标。

快捷键：macOS `Command+Shift+Y`，其他平台配置为 `Ctrl+Shift+Y`。本次仅检查了 macOS 环境，未验证 Windows/Linux。

安装、通信组件要求和故障处理见 [安装说明](安装说明.md)。

## 安全加固及边界

- 删除网站问题报告外联，欢迎和配对页面改为包内本地资源。
- 禁止扩展后台 Fetch、XHR、WebSocket、EventSource，增加 CSP 和 DNR 网络限制。
- 仅允许连接固定的经典 1Password 本地 Native Messaging 主机。
- 本地配对消息检查来源，只向本地界面返回配对代码。
- 拒绝网页操作配对流程，保留域名匹配检查。
- 没有扩展自动更新地址。

填充和保存时，扩展必须接触密码，填入目标网页后该网站能够读取它。网络限制不等于阻断登录网站的正常请求，也不控制桌面应用自身的同步联网。

检查发现前一版有向 `synapse.agilebits.com` 报告网址、浏览器信息及报告参数的入口，4.7.6.3 已移除。完整发现、加固措施和验证范围见 [安全检查说明](安全检查说明.md)。

## 验证

需要 Node.js：

```sh
node verification/worker-smoke.cjs
```

测试使用模拟浏览器接口及虚构数据，覆盖后台加载、菜单适配、网络接口禁用、固定本地主机、报告入口禁用、配对来源检查、认证响应字段限制和域名匹配填充。运行代码摘要位于 `verification/runtime-sha256.json`。

## 浏览器身份验证失败

如果 1Password 提示无法验证 Chrome 身份，先保存网页内容，用 `Command+Q` 完整退出 Chrome 后重新打开，让更新完成。这是桌面端的浏览器签名验证，扩展不能靠关闭安全校验修复。

[1Password 官方处理说明](https://support.1password.com/code-signature/)

## 来源和许可证

基于 [ferreirafabio/1password7-chrome-extension](https://github.com/ferreirafabio/1password7-chrome-extension)，参考提交 `d02c4780401218902ef4c0f8d039cfd2f54f1129`。原 README 保留在 [README.upstream.md](README.upstream.md)，其功能声明不代表本改版已完成同等验证。

上游原创适配部分采用 [GPL-3.0](LICENSE)。原 1Password 压缩脚本、图片及语言资源属于 AgileBits，不能将仓库的 GPL 许可解释为这些原始资源均由本项目拥有或重新许可。

本项目不是 AgileBits/1Password 官方产品，也未获其认可。本次是针对性检查与加固，未完成对所有第三方压缩代码的完整安全审计。

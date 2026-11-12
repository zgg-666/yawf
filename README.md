# 药方 (YAWF) 社区维护版

**Yet Another Weibo Filter — Community Edition** · 当前版本 `5.0.111.0`

> ⚠️ 这是**非官方 fork**，原作者 **田生** 未参与、未背书。原项目因长期未适配微博新版（V7 / Vue 3）而停更，本版在其基础上做兼容性修复，**不是**上游的官方续作，也不会接管上游的发布渠道。

| 项目   | 内容                                                                              |
| ---- | ------------------------------------------------------------------------------- |
| 基于版本 | Yet Another Weibo Filter / 药方 (YAWF) **5.0.110**                                |
| 原作者  | 田生 <http://weibo.com/tsh90>                                                     |
| 上游仓库 | <https://github.com/tiansh/yawf>（用户脚本）、<https://github.com/tiansh/yaofang>（扩展版） |
| 本版维护 | zgg-666 · <https://github.com/zgg-666/yawf-community>                           |
| 许可证  | **MPL-2.0**（见 [LICENSE](LICENSE)，全文取自上游仓库，未修改）                                  |
| 反馈   | <https://github.com/zgg-666/yawf-community/issues>                              |

## 为什么会有这一版

原脚本是针对微博 V6 / Vue 2 编写的（依赖 `__vue__`、`$children`、`$vnode`、`$options.name` 等 Vue 2 实例字段）。当前微博的 V7 页面使用 Vue 3，组件名、实例字段、VNode 结构、长文接口和左侧栏 DOM 全都变了，导致 `__vue__`、`$http`、`$refs`、CSS Module 类名等依赖失效。本版加了一层 Vue 3 兼容层，并把几个关键功能改成 DOM 驱动，同时**完整保留原来的 Vue 2 / V6 代码路径**，没有重构、没有删功能。

- Vue 3 应用捕获、VM 包装、VNode 改造与生命周期兼容层；
- 关键词/作者/来源过滤链在新版页面正常运行，修复「隐藏的微博复活」「只剩第一条微博」「内容错位」；
- 长文自动展开、原生视频播放器、左侧栏「我的收藏 / 我的赞」；
- 去掉设置面板顶部的「V7 支持开发中」横幅；
- 系统性修复 VNode `shapeFlag` / `ref` / 文本子节点、事件表与 `$emit`、初始化时序、配置即时生效、长文请求超时等问题。

## 已知限制

- 规则引擎 `Rule.execute()` 有 `if (!this.v7Support) return;`，约 213 条规则中仍只有 63 条在新版页面启用；其余约 150 条在设置里隐藏且不执行。它们大多依赖 V6 组件名与旧 DOM，**批量打开会引发布面错乱**，需要逐条拿到线上真实 DOM 才能适配。如果你发现某个功能没反应，请附功能名 + F12 Console 红色报错 + 对应元素的 DOM 提 issue。
- 首次安装气泡与版本切换提示里的旧版 V7 文案已改写；`weiboVersion6To7` 的调用在原脚本中已被注释，不会显示。
- 本版未经真实微博账号的线上全流程验证，兼容性修复通过 Vue 3 + jsdom 渲染测试与语法检查验证。
- 按 MPL-2.0 第 6、7 条，本软件按「原样」提供，**不含任何担保**；维护者单独承担本版的支持与说明，不代表原作者。

## 安装

1. 安装脚本管理器（Tampermonkey / Violentmonkey / Greasemonkey）。
2. 打开 [Greasy Fork 脚本页](https://greasyfork.org/zh-CN/scripts/605188-yet-another-weibo-filter-community-edition) 点安装，或直接打开本仓库的 `yawf-community.user.js` 原始文件安装。
3. 打开 weibo.com，右上角漏斗/齿轮图标进入设置。

### ⚠️ 从原版迁移时设置不会自动带过来

本版修改了 `@namespace` 与 `@name`，对脚本管理器来说这是**一个全新的脚本**，存储是隔离的：

- 若你在 Tampermonkey 里**直接编辑/替换现有脚本的代码**（不新建条目），存储不变，设置保留；
- 若从 Greasy Fork 新建条目安装，则是全新存储，**需要先在原版设置里「备份 → 导入/导出 → 导出」，再在新版里导入**。

导入是整体替换当前登录账号的配置池，请用对应账号登录后操作。

## 许可证与署名要求

本文件是 MPL-2.0 覆盖的**源码形式**，未编译、未压缩。按 MPL-2.0 第 3.1 / 3.4 条：

- 修改后继续以 **MPL-2.0** 分发，不能改成其他许可证或闭源；
- 保留文件头的 MPL 声明、`@license MPL-2.0`、原作者署名，不得删改；
- 告知接收者许可证全文的获取方式（本仓库 `LICENSE`）；
- MPL-2.0 第 2.3 条**不授予商标权**，因此本版不使用原作者的 `@icon` 图标、名称加了「非官方维护版」后缀；该行已替换为 `@copyright` 署名行。若需要图标，请自行添加属于你的 `@icon`（data URI 或图片 URL）。

上游 `LICENSE` 全文见本目录 [LICENSE](LICENSE)，亦可在 <https://mozilla.org/MPL/2.0/> 获取。

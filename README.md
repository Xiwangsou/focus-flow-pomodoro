<div align="center">

# Focus Flow

**一个不会走慢的极简番茄钟**

单文件、零依赖、离线可用的专注计时器。计时基于时间戳而非计数器，
切到后台也不会漂移。

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![No Dependencies](https://img.shields.io/badge/dependencies-none-success.svg)](#)
[![Vanilla JS](https://img.shields.io/badge/vanilla-JS-f7df1e.svg)](https://developer.mozilla.org/docs/Web/JavaScript)

</div>

---

## 特性

- **时间戳驱动的可靠计时** —— 剩余时间由「结束时刻 − 当前时刻」算出，不靠每秒减一
- **任务标签** —— 写清这轮在做什么，显示在标签页标题上
- **桌面通知** —— 切到别的窗口也能收到完成提醒
- **周统计** —— 7 天专注分布柱状图
- **可配置** —— 各阶段时长、轮次数量、自动开始、提示音、隐藏秒数
- **明暗双主题** + 沉浸模式
- **完整键盘操作**
- **无依赖** —— 单个 HTML 文件，双击即用

## 快速开始

直接用浏览器打开 `index.html` 即可，无需安装、无需构建、无需联网。

想要一个独立窗口应用：

```bash
# macOS
open index.html

# Windows
start index.html

# 或者加到浏览器书签栏，随时一键打开
```

## 快捷键

| 按键 | 作用 |
|---|---|
| `Space` | 开始 / 暂停 |
| `R` | 重置当前阶段 |
| `S` | 跳过当前阶段 |
| `1` `2` `3` | 切换专注 / 短休 / 长休 |
| `Z` | 沉浸模式（隐藏所有界面） |
| `T` | 切换主题 |
| `W` | 打开统计 |
| `,` | 打开设置 |
| `?` | 快捷键面板 |
| `Esc` | 关闭面板 / 退出输入 |

## 数据存储

全部数据存在浏览器 `localStorage`，**不上传任何服务器**。

- 存储键：`focusflow:v2`（加了命名空间，避免与其他应用冲突）
- 保存内容：时长配置、任务标签、每日完成数、专注分钟、近一年历史
- 切到后台或关闭页面时会立即落盘，不会丢数据

想清空数据：统计面板 → 「清空所有记录」，或直接清浏览器存储。

## 为什么用时间戳计时

常见的番茄钟实现是每秒减一：

```js
setInterval(() => { remain -= 1; render(); }, 1000);
```

问题在于**浏览器会把后台标签页的定时器节流**——Chrome 降到每分钟一次，
Safari 可能直接冻结。结果是「25 分钟的番茄钟」在后台放十分钟，
回来只走了 1 分钟，用户以为计时器坏了。

本项目的做法是记录一个绝对的结束时刻，每次刷新时重新计算：

```js
st.endAt = Date.now() + remain * 1000;
const remain = () => Math.max(0, Math.round((st.endAt - Date.now()) / 1000));
```

即使循环被节流到 30 秒一次，显示的时间依然准确。

## 隐私

无网络请求、无统计代码、无第三方依赖。字体走 Google Fonts CDN，
断网时会回退到系统字体，功能不受影响。

## License

[MIT](LICENSE)

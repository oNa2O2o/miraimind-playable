# MiraiMind 可玩广告 - 开发文档

## 📋 项目概述

**项目名称**: MiraiMind Playable Ad  
**项目类型**: HTML5 可玩广告 (Interactive Playable Ad)  
**目标平台**: Google Play (Android)  
**部署方式**: GitHub Pages  
**公网链接**: https://ona2o2o.github.io/miraimind-playable/

---

## 🗂️ 文件结构

```
/
├── MiraiMind_Playable_Ad.html    # 主文件（单文件应用）
├── index.html                     # GitHub Pages 入口（同主文件）
├── miraimind_logo.png             # Logo 资源（84x84px）
├── miraimind_video.mp4            # 背景视频资源（备用）
├── deploy-miraimind/              # 部署目录
│   ├── index.html                 # 部署用文件
│   └── .git/                      # Git 仓库
├── deploy_github.py               # Python 部署脚本
├── deploy_to_github.bat           # Windows 部署脚本
└── MiraiMind_开发文档.md          # 本文档
```

---

## 🎨 设计规范

### 配色方案
| 颜色 | 色值 | 用途 |
|------|------|------|
| 粉色 | `#F472B6` | Logo 渐变 1 |
| 蓝色 | `#60A5FA` | Logo 渐变 2 |
| 绿色 | `#4ADE80` | Logo 渐变 3、用户消息气泡 |
| 黄色 | `#FBBF24` | Logo 渐变 4 |
| 深色背景 | `rgba(0,0,0,0.7)` | AI 消息气泡 |
| 白色 | `#fff` | 文字 |

### 字体
- **主字体**: `-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif`
- **字号**:
  - 标题: 28px
  - AI 名称: 18px
  - 消息内容: 14px
  - 发送者名称: 12px
  - 时间/状态: 10px

### 阴影规范（所有文字）
```css
/* 标题 */
text-shadow: 0 2px 8px rgba(0,0,0,0.9), 0 0 20px rgba(0,0,0,0.8);

/* 普通文字 */
text-shadow: 0 1px 3px rgba(0,0,0,0.9), 0 0 10px rgba(0,0,0,0.8);

/* 时间/元信息 */
text-shadow: 0 1px 3px rgba(0,0,0,0.9), 0 0 8px rgba(0,0,0,0.8);
```

---

## 🎮 交互流程

```
┌─────────────────────────────────────────────┐
│                  启动                         │
│         显示聊天页（直接显示）                │
│                500ms 延迟                     │
└─────────────────────┬───────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────┐
│              AI 发送第一条消息               │
│          "Are you awake?"                   │
│              （无 Typing 动画）               │
└─────────────────────┬───────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────┐
│              用户输入并发送                  │
│           播放键盘敲击音效                   │
│           播放发送音效                       │
└─────────────────────┬───────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────┐
│            显示 "Typing..." 500ms            │
│            播放消息提示音                    │
└─────────────────────┬───────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────┐
│         AI 发送第二条消息                    │
│     "Can I ask you something?"              │
│            200ms 过渡延迟                    │
└─────────────────────┬───────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────┐
│            用户第二次输入                    │
│              点击发送按钮                    │
└─────────────────────┬───────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────┐
│                显示结束页                    │
│     播放结束页淡入动画 (fadeInUp 0.6s)       │
│     下载按钮动态放大缩小动画                  │
│     下载图标上下跳动动画                      │
│                                              │
│   ├─ 点击结束页任意位置 → 跳转商店            │
│   └─ 2秒后自动 → 跳转商店                     │
└─────────────────────────────────────────────┘
```

---

## 📱 页面结构

### 1. 聊天页 (`.page2`)
| 元素 | 说明 |
|------|------|
| **视频背景** | 自动播放、静音、循环播放 |
| **渐变遮罩** | 让聊天内容更清晰可读 |
| **头部** | Logo (44x44px, 圆角12px) + Eric 名称 |
| **聊天容器** | Flex 列布局，底部对齐 |
| **输入区域** | 输入框 + 发送按钮 + 虚拟键盘 |

### 2. 消息气泡样式

**AI 消息 (Eric)**
```css
.chat-message.ai .message-content {
    background: linear-gradient(135deg, rgba(0,0,0,0.7), rgba(0,0,0,0.6));
    border: 1px solid rgba(255,255,255,0.3);
    color: #fff;
    border-bottom-left-radius: 4px;
    text-shadow: 0 1px 3px rgba(0,0,0,0.9), 0 2px 6px rgba(0,0,0,0.8);
}
```

**用户消息 (You)**
```css
.chat-message.user .message-content {
    background: linear-gradient(135deg, #4ADE80, #60A5FA);
    color: #fff;
    border-bottom-right-radius: 4px;
    text-shadow: 0 1px 3px rgba(0,0,0,0.9), 0 2px 6px rgba(0,0,0,0.8);
}
```

**消息元信息**
- 发送者名称 (气泡上方)
- 发送时间 HH:MM (气泡下方)
- 已读状态 "Read" (绿色)

### 3. 结束页 (`.end-page`)
| 元素 | 说明 |
|------|------|
| 背景 | `rgba(0,0,0,0.7)` + 模糊效果 |
| Logo | 100x100px, 圆角24px |
| 标题 | "Discover Your AI Soulmate" |
| 描述 | "Start meaningful conversations with Eric and more AI companions" |
| 下载按钮 | 绿色渐变、动态缩放动画 |
| 提示文字 | "Tap anywhere to continue ↓" |

### 4. 下载按钮动画
```css
/* 按钮整体呼吸动画 */
animation: pulse-scale 1.2s infinite;

@keyframes pulse-scale {
    0%, 100% { 
        transform: scale(1);
        box-shadow: 0 4px 15px rgba(74,222,128,0.4), 0 0 30px rgba(96,165,250,0.3);
    }
    50% { 
        transform: scale(1.08);
        box-shadow: 0 6px 25px rgba(74,222,128,0.6), 0 0 50px rgba(96,165,250,0.5), 0 0 80px rgba(255,255,255,0.2);
    }
}

/* 下载图标上下跳动 */
animation: download-bounce 1.2s infinite;

@keyframes download-bounce {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(3px); }
}
```

---

## 🔊 音效系统

使用 Web Audio API 生成音效：

| 音效类型 | 触发时机 | 参数 |
|---------|---------|------|
| 消息提示音 | 收到消息时 | 800Hz → 400Hz，0.1s |
| 发送音效 | 发送消息时 | 400Hz → 600Hz，0.05s |
| 键盘敲击音 | 点击按键时 | 1200Hz，0.03s |

```javascript
const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
```

---

## 📝 对话内容配置

```javascript
// 固定对话内容
const FIRST_MESSAGE = "Are you awake?";
const SECOND_MESSAGE = "Can I ask you something?";

// 角色名称
const AI_NAME = "Eric";
const USER_NAME = "You";

// 商店链接
const CONFIG = {
    storeUrl: 'https://play.google.com/store/apps/details?id=com.immomo.miraimind'
};
```

---

## ⏱️ 时间参数

| 事件 | 延迟时间 | 说明 |
|------|---------|------|
| 第一条消息显示 | 500ms | 页面加载后延迟 |
| Typing 显示时长 | 500ms | 显示 "Typing..." 动画 |
| Typing 到消息过渡 | 200ms | 移除 Typing 后显示消息 |
| 结束页自动跳转 | 2000ms | 显示结束页后自动跳转 |
| 消息淡入动画 | 300ms | CSS transition |

---

## 🚀 部署指南

### 环境要求
- Git
- GitHub 账号
- GitHub Personal Access Token (需要 repo 权限)

### 部署步骤

#### 方法一：使用脚本自动部署

1. **获取 GitHub Token**
   - 访问 https://github.com/settings/tokens
   - 点击 "Generate new token (classic)"
   - 勾选 **repo** 权限
   - 生成并复制 Token

2. **运行部署脚本**
   ```bash
   python deploy_github.py
   ```
   或
   ```bash
   deploy_to_github.bat
   ```

3. **输入信息**
   - GitHub 用户名
   - GitHub Token
   - 仓库名称（默认: miraimind-playable）

4. **启用 GitHub Pages**
   - 访问 `https://github.com/用户名/仓库名/settings/pages`
   - Source 选择: `main` 分支，`/ (root)` 目录
   - 点击 Save

5. **等待构建**
   - 等待 1-2 分钟
   - 访问 `https://用户名.github.io/仓库名/`

#### 方法二：手动部署

1. 访问 https://github.com/new 创建仓库
2. 上传 `index.html` 文件
3. 在 Settings > Pages 中启用

---

## 🔄 修改历史

### v1.0 - 基础版本
- 实现基础聊天界面
- 添加虚拟键盘
- 实现消息发送/接收动画

### v1.1 - 移除启动页和结束页
- 移除启动页（Page 1）
- 移除结束页（Page 3）
- 默认显示聊天页

### v1.2 - 修改对话和交互
- AI 名称固定为 "Eric"
- 对话内容固定为英文
- 第一条消息: "Are you awake?"
- 第二条消息: "Can I ask you something?"
- 第二次点击发送后立即跳转

### v1.3 - UI 优化
- 修改 Logo 为原始样式（圆角矩形）
- 移除消息头像
- 添加消息发送者名称（Eric/You）
- 添加时间戳和已读状态
- 添加键盘敲击音效

### v1.4 - 动画优化
- 第一条消息直接显示（不显示 Typing）
- Typing 显示时长缩短为 500ms
- 第二条消息增加 200ms 过渡延迟

### v1.5 - 视觉效果优化
- AI 消息气泡改为深色背景
- 所有文字添加阴影（确保在各种背景上清晰）
- 添加结束页
- 第二次点击后显示结束页，2秒后自动跳转

### v1.6 - 结束页优化
- 添加下载按钮
- 添加跳动指针引导点击
- 下载按钮添加脉冲动画

### v1.7 - 最终版本
- 移除指针
- 下载按钮添加动态缩放+发光动画
- 下载图标添加上下跳动动画
- 自动跳转时长改为 2秒

---

## 🛠️ 开发注意事项

### 1. 单文件结构
- 所有 CSS 内联在 `<style>` 标签中
- 所有 JavaScript 内联在 `<script>` 标签中
- 使用 CDN 资源（Logo、视频）

### 2. 移动端适配
- 使用 `viewport` meta 标签
- 虚拟键盘针对竖屏优化
- 横屏时显示旋转提示

### 3. 性能优化
- 视频预加载：`preload="auto"`
- 商店页面预加载：`prefetch`
- 使用 `requestAnimationFrame` 优化动画

### 4. 兼容性
- 使用 Web Audio API 生成音效（无需外部音频文件）
- 使用标准 CSS3 动画
- 支持 iOS Safari 和 Android Chrome

---

## 📞 技术支持

### 常见问题

**Q: 页面加载后空白？**
A: GitHub Pages 构建需要时间，等待 1-2 分钟后刷新。

**Q: 音效不播放？**
A: 部分浏览器需要用户交互后才能播放音频，这是浏览器的安全策略。

**Q: 视频不显示？**
A: 检查网络连接，视频使用外部 CDN，需要联网加载。

---

## 📄 文件清单

| 文件 | 大小 | 说明 |
|------|------|------|
| MiraiMind_Playable_Ad.html | ~28KB | 主文件 |
| miraimind_logo.png | ~7.5KB | Logo 图片 |
| miraimind_video.mp4 | ~1.3MB | 背景视频（备用） |

---

## 📌 TODO

- [ ] 添加多语言支持
- [ ] 优化横屏体验
- [ ] 添加更多 AI 角色
- [ ] 实现真实后端对话
- [ ] A/B 测试不同对话流程

---

**文档版本**: 1.0  
**最后更新**: 2026-02-28  
**开发者**: Claude Code

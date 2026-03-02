# MiraiMind 可玩广告 - 快速参考

## 🚀 快速开始

### 在线预览
```
https://ona2o2o.github.io/miraimind-playable/
```

### 本地测试
```bash
python -m http.server 8081
# 访问 http://localhost:8081/MiraiMind_Playable_Ad.html
```

---

## 📦 核心配置

```javascript
// 对话内容（修改这里）
const FIRST_MESSAGE = "Are you awake?";
const SECOND_MESSAGE = "Can I ask you something?";

// AI 名称
const AI_NAME = "Eric";

// 商店链接
const CONFIG = {
    storeUrl: 'https://play.google.com/store/apps/details?id=com.immomo.miraimind'
};
```

---

## ⏱️ 关键时间参数

| 参数 | 数值 | 位置 |
|------|------|------|
| 第一条消息延迟 | `500ms` | `initChat()` 函数 |
| Typing 时长 | `500ms` | `handleSend()` 函数 |
| 过渡延迟 | `200ms` | `handleSend()` 函数 |
| 自动跳转时长 | `2000ms` | `showEndPage()` 函数 |

---

## 🎨 关键样式修改

### 修改 AI 消息气泡颜色
```css
.chat-message.ai .message-content {
    background: linear-gradient(135deg, rgba(0,0,0,0.7), rgba(0,0,0,0.6));
    border: 1px solid rgba(255,255,255,0.3);
}
```

### 修改文字阴影（确保清晰）
```css
.message-content {
    text-shadow: 0 1px 3px rgba(0,0,0,0.9), 0 2px 6px rgba(0,0,0,0.8);
}
```

### 修改下载按钮动画
```css
/* 按钮缩放动画 */
@keyframes pulse-scale {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.08); }
}

/* 图标跳动动画 */
@keyframes download-bounce {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(3px); }
}
```

---

## 🔧 常见修改

### 1. 修改对话内容
搜索代码中的：
```javascript
const FIRST_MESSAGE = "Are you awake?";
const SECOND_MESSAGE = "Can I ask you something?";
```
直接修改字符串内容。

### 2. 修改跳转时间
搜索代码中的：
```javascript
setTimeout(() => {
    gotoDownloadPage();
}, 2000);  // 修改这里的数值（毫秒）
```

### 3. 修改 AI 名称
搜索代码中的：
```javascript
const AI_NAME = "Eric";
```
同时修改 HTML 中的：
```html
<div class="header-name">Eric</div>
```

### 4. 修改商店链接
搜索代码中的：
```javascript
storeUrl: 'https://play.google.com/store/apps/details?id=com.immomo.miraimind'
```

---

## 📤 部署步骤

1. **修改文件** → 编辑 `MiraiMind_Playable_Ad.html`

2. **复制到部署目录**
   ```bash
   cp MiraiMind_Playable_Ad.html deploy-miraimind/index.html
   ```

3. **提交并推送**
   ```bash
   cd deploy-miraimind
   git add index.html
   git commit -m "Update: xxx"
   git push origin main
   ```

4. **等待构建** → 访问公网链接

---

## 🐛 调试技巧

### 浏览器开发者工具
- **F12** 打开开发者工具
- **Elements** 查看和修改 DOM
- **Console** 查看 JavaScript 输出
- **Network** 检查资源加载

### 常见问题

**问题**: 修改后不生效  
**解决**: 清除浏览器缓存 (Ctrl+F5)

**问题**: GitHub Pages 404  
**解决**: 检查仓库是否设置为 Public，Pages 是否已启用

**问题**: 动画不流畅  
**解决**: 减少同时运行的动画数量，使用 `transform` 代替 `top/left`

---

## 📱 设备测试

| 设备 | 测试重点 |
|------|---------|
| iPhone Safari | 视频播放、虚拟键盘 |
| Android Chrome | 音效播放、触摸响应 |
| 横屏模式 | 旋转提示是否正确显示 |

---

## 📞 关键代码片段

### 显示结束页
```javascript
function showEndPage() {
    const endPage = document.getElementById('endPage');
    endPage.classList.add('active');
    
    // 点击跳转
    endPage.addEventListener('click', gotoDownloadPage);
    
    // 2秒后自动跳转
    setTimeout(() => {
        gotoDownloadPage();
    }, 2000);
}
```

### 添加消息
```javascript
function addMessage(type, text) {
    const msgDiv = document.createElement('div');
    msgDiv.className = `chat-message ${type}`;
    
    const senderName = type === 'ai' ? 'Eric' : 'You';
    const currentTime = new Date().toLocaleTimeString('en-US', { 
        hour: '2-digit', 
        minute: '2-digit',
        hour12: false 
    });
    
    msgDiv.innerHTML = `
        <div class="message-sender">${senderName}</div>
        <div class="message-content">${text}</div>
        <div class="message-meta">
            <span>${currentTime}</span>
            <span class="message-status">Read</span>
        </div>
    `;
    
    elements.chatContainer.appendChild(msgDiv);
    msgDiv.offsetHeight; // 强制回流
    msgDiv.classList.add('show');
    playMessageSound();
}
```

### 音效播放
```javascript
function playMessageSound() {
    const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    const oscillator = audioCtx.createOscillator();
    const gainNode = audioCtx.createGain();
    
    oscillator.connect(gainNode);
    gainNode.connect(audioCtx.destination);
    
    oscillator.frequency.setValueAtTime(800, audioCtx.currentTime);
    oscillator.frequency.exponentialRampToValueAtTime(400, audioCtx.currentTime + 0.1);
    
    gainNode.gain.setValueAtTime(0.3, audioCtx.currentTime);
    gainNode.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.1);
    
    oscillator.start(audioCtx.currentTime);
    oscillator.stop(audioCtx.currentTime + 0.1);
}
```

---

**提示**: 详细文档请参考 `MiraiMind_开发文档.md`

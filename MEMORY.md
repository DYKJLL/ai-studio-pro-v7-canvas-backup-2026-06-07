# MEMORY: AI_Studio_Pro + 无限画布 + ViMax 项目长期记忆

## 📌 当前状态（2026-06-06）

### 三条主线
| 主线 | 文件 | 状态 | 备注 |
|------|------|------|------|
| **v6 正式版** | `AI_Studio_Pro_v6_final.html` (255,220 B) | ✅ 唯一认可的"正本" | hash `33D6166F...` / 桌面副本同步 |
| **无限画布** | `_infinite_canvas_v1.html` (49,858 B) | 🚧 V3 写好未升级正本 | V1→V2→V3 演化（见下）|
| **ViMax Web** | `D:\AI\VIMAX\webapi\` | 🚧 API 路由全完 + Redis 装好 | uvicorn 启动有 redis down bug |

### 无限画布 V1→V2→V3 演化
- **V1 (42,702 B)**：10 节点 + 画布 + 连线 + 抽屉详情（hash `326FC8E7...`）
- **V2 (51,112 B)**：6 限制解除（拖/连线/方向/重复/自连/坐标）+ 右键菜单（hash `F828F9EF...`）
- **V3 (49,858 B · 写好未升级)**：极简化 = 删 4 mode 按钮 + 左键无脑平移 + 右键出功能
- **保留备份**：
  - `V1 初始备份`：`_infinite_canvas_v1_backup.html` (42,702 B)
  - `V2 备份`：`_infinite_canvas_v1_bak2.html` (51,112 B)
- **节点就是扩展方式** = 朝暮明说"加新功能 = 加新节点"（不是加 tab / phase / 版本）

### ViMax Web 项目（D:\AI\VIMAX\）
- **背景**：HKUDS/ViMax 开源多 Agent 视频生成框架（MIT / Python 3.12+ / uv）
- **2 条主流程**（**注意：旧记忆写 4 条是错的**）：`main_idea2video.py` + `main_script2video.py`
  - `novel2movie_pipeline.py` 顶部 `# TODO: NOT IMPLEMENTED YET` → v1 跳过
- **朝暮自建（神圣不可改）**：`main.py` (4,236 B) / `core/{settings,key_pool,queue,logger}.py` / `db/models.py`
- **我新增（不碰 ViMax 原源码）**：
  - `core/keyed_render_backend.py` (8,519 B) — 多 Key 轮询包装（最小侵入）
  - `api/` 9 个路由文件（32 endpoint，详见下方"5 波次进度"）
  - `_smoke_test_api.py` (2,334 B)
- **环境**：Redis 3.2.100 装在 `D:\AI\Redis\`（运行中 PID 15792 / port 6379 / PONG OK）
- **当前 bug**：uvicorn 启动成功（port 8000 / 37 路由），但 `/api/system/health` 返 `redis: down`（进程在 + ping OK + FastAPI 报 down = FastAPI 端配置 bug，待排查 `core/queue.py` 的 `check_redis_alive` 和 `api/routes_system.py` 的字段映射）

### ViMax Web · 5 波次进度（朝暮 20:50 "全部完整做" 拍板）
| 波次 | 内容 | 进度 |
|------|------|------|
| 1/5 核心 | 多 Key 包装 + 任务函数 | 🚧 1/4 文件（keyed_render_backend ✅，workers 0/3） |
| 2/5 API + config + start.bat | ✅ 9 路由全完（32 endpoint）/ ⏳ config/ + start.bat 未开始 |
| 3/5 前端设计系统 | Next.js 14 + TailwindCSS + Liquid Glass | ⏳ 未开始 |
| 4/5 前端 6 模块 | Dashboard / 任务中心 / 创作工作台 / 资产库 / 系统配置 / 监控告警 | ⏳ 未开始 |
| 5/5 集成 + 审查 + 测试 | code review + 端到端 run + bug 清单 | ⏳ 未开始 |

### 🍎 视觉系统定调（朝暮 19:40 拍板）
- **设计语言**：Apple macOS 26 Liquid Glass（液态玻璃）
- **背景**：紫粉/红橙/琥珀多色 radial-gradient + 慢速液态动画 + 噪点
- **玻璃卡片**：`backdrop-filter: blur(24-40px) saturate(180%)` + 1px 白边 + 内高光 + 外发光
- **3 种玻璃厚度**：通透（薄）/ 厚重（厚）/ 扁平（浅）
- **圆角**：16-24px 大圆角 + pill 形按钮
- **字体**：Inter / SF Pro / 思源黑体 + JetBrains Mono（数字）

---

## 🧱 项目结构

### v6 正式版（AI_Studio_Pro_v6_final.html）— 唯一认可正本
- 7 phase 渲染完整（CREATE / DASH / SCRIPT / ASSETS / BATCH / AUDIO / REVIEW）
- DASH 3 栏：栏1系统状态 4 张卡 + 栏2历史资产二级页 + 栏3 V4 配置
- 编辑 Modal（14 字段 + 真回滚 + dirty 提示）
- Video Inpainting 阶段 1（拖框 + 多块统计）
- 资产预览双层弹窗 + 整剧/单资产导出
- 中栏预览 380×400 硬约束 + 任务日志区 280px 锁定

### 无限画布（_infinite_canvas_v1.html）— 新方向
- **画布**：CSS transform 平移（`translate(x, y)`）+ 缩放（`scale(z)`）
- **10 节点**（沿用 V4 设计）：🎩总控(0) / 📝编剧(1) / 🔍拆镜(2) / 🎨资产(3) / 🎬分镜(4) / 🎥视频(5) / 🔊音频(6) / ✅质检(7) / 🎞合成(8) / 👁审片(9)
- **11 条默认连线**：主线 0→1→2→3→4→5/6→7→8→9 + 反馈 5→8 / 8→4 / 9→0
- **节点详情**：右侧 560px 抽屉，6 大区（角色职责/数据流向/运行指标/工具模型/配置参数/任务队列）
- **端口**：`.port.in` / `.port.out`（中间有空格 = 2 个类），data 属性 `{"nid":"X","dir":"in/out","idx":"0"}`
- **V2 右键菜单**：
  - 画布空白：10 Agent 添加 + 4 操作（▶运行/⏹停止/🔄布局/↺重置）
  - 节点：6 项（详情/运行/重命名/复制/配置/删除）
  - 连线：3 项（改标签/反向/删除）
- **V3 极简化**：删 4 mode 按钮 + 左键拖空白=平移（任何时候）+ 右键=菜单
- **类比产品**：ComfyUI / n8n / Flowise / LangFlow / React Flow / Rete.js

---

## 🎯 朝暮协作死线（行为级 · 违反 2 次 = 警告）

### 🚨 第三次强化死线（2026-06-04 23:18）
> "1 每次需求必须大白话对齐 + 配图，朝暮拍板才动手 / 2 严禁越权，不主动改其他功能"

### 10 大核心协作原则
1. **大白话 + 配图 > 文字表格**：技术术语必须翻译（hash→"两个文件一模一样"/弹窗→"点这个会弹出来一个框"）
2. **看实际文件再说话**：朝暮问"什么页面/什么 UI"= 强制先 `read_file` / `browser_use snapshot`
3. **严禁越权**：不主动出方案 / 不主动排优先级 / 不主动追问 / 不主动出 2 个以上问题
4. **1 栏 1 张图 / 1 步 1 验证**：最小颗粒度，改代码 = 先 test 副本跑通 → 验证 → 同步正本
5. **动 vs 不动清单**：明确范围 + 预期体量，让朝暮不惊讶
6. **3 套方案 = 3 思路或 3 程度**（设计哲学不同时）；**"综合所有"= 1 份全包版**（不出 3 选 1）
7. **"敲定就不变"**：之前确认的不再调整，立刻接下一个
8. **"一个个来" = 停推 + 列待办**：列不超过 5 条待办让朝暮挑
9. **方案图必须截图 + view_image 验证**：mock 写完 = 0，必须截图
10. **"大白话汇报"3 句模板**：是什么 + 严不严重 + 要不要管

### 协作黑话速查
- **"A"/"B"/"1"** = 简写选项（已看过分析框架，无需展开）
- **"差不多"/"嗯"/"就这样"** = 拍板 + **不主动推下一步**（出 3 选项 A/B/C 等选）
- **"差不多 开始做吧"** = 拍板 + **立刻动手**（8 步流程）
- **"应用化"** = **工作流编辑器**（类 ComfyUI/n8n），**不是**企业 SaaS（**4 轮走偏教训**）
- **"真软件" 5 大件套** = 表单能输入 / 列表能滚动 / 图表真画 / 按钮真执行 / 下载真 Blob
- **"不要太多限制"** = 放开所有条件判断 = 删 mode 概念
- **"节点 = 第一公民"** = 加新功能 = 加新节点（不是加 tab/phase/版本）
- **"占位功能"** = 不接真实 API ≠ 删除演示元素（保留完整视觉 + mock 数据）
- **"一个个来"** = 1 栏 1 改 / 1 资产 1 改
- **"动"** = 实施已确认方案 + 验证完才给朝暮
- **"页面重复"** = 新加入口后主动 grep 检查所有 phase
- **"0/2/3"** = "不能"/"可以但改 2"/"可以"
- **"综合所有"** = 1 份全包版本（A+B+C 全包含）
- **"CJ"** = C 完整口语简写
- **"放桌面"** = 桌面副本 + hash 校验（**路径 ≠ 文件**，必须 `send_file_to_user`）
- **"全部推翻"** = **扩大搜索半径**（HTML/PNG/PY/JS/TXT 全部统计），**不是**只删显眼文件
- **"清理 = 每次新替换旧"** = 工作区只留正本 + 桌面副本 + 最新 1 张图

---

## 🛠️ 关键技术模式

### 1. 实施前必做（铁律）
- **备份**：`copy v6 → v6_pre_xxx.html` + sha256 记录
- **建 test 副本**：`copy 正本 → _xxx_test.html` + hash 校验 = 双重保护
- **行号 verify**：动 v6 前必先 grep 定位精确行号（**不信 MEMORY 旧行号**）
- **V1 升级 V2 SOP**：V1 backup 保留（回滚用）+ V2 test 写好 + 升级 V1 正本 + 桌面同步 + 删 V2 test

### 2. 改朝暮自建代码 = 越权
- **朝暮自建文件神圣不可改**（ViMax `webapi/main.py` / `core/{settings,key_pool,queue,logger}.py` / `db/models.py`）
- **只追加**（workers/ api/ config/），**不重写**
- **"动" 前先 `dir` 盘点现状**（差点覆盖朝暮 30 分钟前自建的 webapi/）

### 3. write_file 截断铁律（第 3-4 次验证）
- **18KB 软限制**：超过 18KB write_file 静默截断（**无错误提示**）
- **字符数 vs 字节数差异**：HTML 含 emoji 1 字 = 3-4 字节 → char count << byte count
- **截断点无规律**：可能是 HTML 标签结尾、JS 表达式中间、换行符等任意位置
- **恢复流程**：
  1. `dir file.html` 看字节数（write_file 返字符数 ≠ 字节数）
  2. `powershell Get-Content -Tail 5` 看尾部
  3. `grep_search` 找截断点关键词
  4. `edit_file` 用锚点替换续写
  5. 续完再 `dir` 验证字节数

### 4. Python 脚本安全模板
```python
import sys, io
if len(sys.argv) < 2: print("[ERROR] usage"); sys.exit(1)
sys.stdout = io.TextIOWrapper(sys.stdout.buffer, encoding='utf-8', errors='replace')  # Windows GBK
path = sys.argv[1]
with open(path, 'r', encoding='utf-8') as f: html = f.read()
MARKER = "// === V1.x ==="  # 幂等检测
if html.count(MARKER) > 0: print("[REFUSE] already ran"); sys.exit(1)
# ... 改动 ...
with open(path, 'w', encoding='utf-8') as f: f.write(html)
print(f"[OK] done. marker: {MARKER}")
```
- **path 必须参数化** + **必须幂等**（marker 检测）+ **ASCII 兜底**（emoji → [OK]/[FAIL]）
- **Python 写 HTML 生成器不适合内嵌大段 HTML**（700KB+ 失控）→ **用 edit_file 改 HTML**

### 5. 验证模式金字塔
- **顶层**：用户亲自点击 ← 朝暮
- **二层**：浏览器 ref click
- **三层**：浏览器 evaluate dispatchEvent（事件链 OK / UI focus ❌）
- **底层**：evaluate 调函数
- **黑屏真实信号**：`document.getElementById('app').innerHTML = ""` = JS 语法错被吞

### 6. eval 注入 DOM 硬规则
- ✅ 用 `(() => { ... })()` IIFE 包裹（**箭头函数**，不用 `(function(){})()` —— evaluate 解析冲突）
- ✅ 大段 HTML 用单引号 + `\n` 转义
- ❌ 不用 `run_code`

### 7. 桌面副本 + hash 校验 4 件套（**已验证 5+ 次**）
```python
import shutil, hashlib
shutil.copy2('SRC', 'C:\Users\六点\Desktop\DST')
s = hashlib.sha256(open('SRC','rb').read()).hexdigest()
d = hashlib.sha256(open('DST','rb').read()).hexdigest()
print(f'正本: {s[:16]} / 桌面: {d[:16]} / 一致: {s==d}')
```
或单行 PowerShell：`copy /Y "SRC" "DST" && certutil -hashfile "SRC" SHA256 && certutil -hashfile "DST" SHA256`

### 8. 端到端启动 5 步
1. `cd /d 项目目录 && .venv\Scripts\uvicorn.exe main:app --host 0.0.0.0 --port 8000`
2. `wait_task task_id timeout=15` = 任务在 = 启动成功
3. `curl -s http://localhost:8000/api/system/health` = 200 + JSON
4. `curl -s http://localhost:8000/openapi.json` = 看路由数
5. **status="degraded"** 不是 "down" = 部分服务可用，看具体 down 字段

### 9. Redis 安装 4 步（zip 路线 · Windows 本地）
```cmd
curl -L -o D:\AI\redis-win.zip https://github.com/MicrosoftArchive/redis/releases/download/win-3.2.100/Redis-x64-3.2.100.zip
powershell -Command "Expand-Archive -Path 'redis-win.zip' -DestinationPath 'D:\AI\Redis' -Force"
start /B D:\AI\Redis\redis-server.exe D:\AI\Redis\redis.windows.conf
D:\AI\Redis\redis-cli.exe ping  # → PONG
```

### 10. Windows 环境 3 大坑
- **GBK 编码**：emoji 必报 UnicodeEncodeError → `sys.stdout` utf-8 wrap + emoji 改 ASCII
- **`tail` 不是命令**：`powershell Get-Content -Tail N` 替代
- **`git` 不可用**：`Invoke-WebRequest -Uri 'https://codeload.github.com/X/zip/refs/heads/main' -OutFile 'X.zip'` + `Expand-Archive`

### 11. JS 5 大根因（页面空白真凶链）
- Python 风格三元混入 JS 字符串 → SyntaxError
- `let` 不挂 window → onclick 找不到
- `const` TDZ 死区 → ReferenceError
- 数据模型升级后字段名不匹配
- 函数定义残缺

### 12. edit_file 4 大陷阱
- 锚点缩进不一致（先 `read_file` 看真实缩进，不凭印象）
- 函数签名匹配截断（必须包含完整函数体）
- SVG path 用 `oncontextmenu =` 属性赋值不触发（**必须** `addEventListener('contextmenu', ...)`）
- 超长函数截断 → 分多次 edit_file

### 13. dispatchEvent 模拟 SVG contextmenu 不可靠
- Playwright/浏览器对 SVG 的 `dispatchEvent(new MouseEvent('contextmenu'))` 有 bug = 静默失败
- **真鼠标右键 = 没问题**；**验证**用直接调函数（如 `showEdgeCtxMenu(700, 500, 0)`）

---

## 📋 关键数据结构

### v6 数据模型（正本字段）
| 结构 | 字段 |
|------|------|
| **CHARS** | `id` / `name` / `role` / `face` / `hair` / `wardrobe` / `refs` / `ai` / `tts` |
| **SCENES** | `id` / `name` / `env:{location, time, weather, lighting}` |
| **SHOTS** | `i` / `t` / `tm` / `d` / `st`/`status` / `sc` / `ch[]` / `camObj` / `trans_in/out` / `dia` / `pg` / `qs` / `seed` / `result` / `retry` / `err` / `mask` |
| **SHOTS 双状态字段** | `s.st`（done/gen/wait 简化）+ `s.status`（completed/generating_video/... 完整）同步 |
| **shot.mask** | `{regions:[{id,label,xpct,ypct,wpct,hpct}], inpaint_area, protected_area, apply_to:'all_frames', ts}` |
| **百分比字段** | 存"37.0"（数字部分），计算时 `/100` 转小数 |

### 无限画布状态
```js
STATE = {
  nodes: [{id, x, y, w, h, label, icon, color, status, ports, ...}],
  edges: [{id, from:{nid, idx}, to:{nid, idx}, label}],
  canvas: {x, y, zoom},
  selection: {nodes:[], edges:[]},
  // V2/V3 删 mode 字段（无 mode 概念）
}
```
- **持久化**：`localStorage.setItem('canvas', JSON.stringify(STATE))`
- **短 hash 任务 ID**：`hashlib.md5(content + timestamp).hexdigest()[:12]`

### 10 Agent 节点（沿用 V4 设计）
| # | 名称 | 颜色 | 职责 |
|---|------|------|------|
| 0 | 🎩 总控 | #facc15 金 | 全局调度 |
| 1 | 📝 编剧 | #6b8cff 蓝 | 剧本生成 |
| 2 | 🔍 拆镜 | #60a5fa 浅蓝 | 剧本拆解 |
| 3 | 🎨 资产生成 | #a78bfa 紫 | 6 角色 + 4 场景 + 8 道具 |
| 4 | 🎬 分镜编排 | #4ade80 绿 | 9 宫格 + 关键帧 |
| 5 | 🎥 视频生成 | #f97316 橙 | 12 镜头 + 关键帧驱动 |
| 6 | 🔊 音频生成 | #ec4899 粉 | 6 角色 TTS + BGM |
| 7 |
# CAD Skills for Codex

本仓库已添加一套面向办公家具 CAD 自动出图的 Codex Skills。

## 安装位置

Codex 会在仓库中扫描 `.agents/skills` 目录。当前技能目录：

```text
.agents/skills/
├─ cad-furniture-modeling/
│  └─ SKILL.md
├─ cad-board-bom/
│  └─ SKILL.md
├─ cad-assembly-check/
│  └─ SKILL.md
├─ cad-hole-system/
│  └─ SKILL.md
└─ cad-error-fix/
   └─ SKILL.md
```

## 技能说明

### `$cad-furniture-modeling`
用于办公家具参数化三维建模，例如办公桌、展示书架、文件柜、会议桌。

### `$cad-board-bom`
用于板件拆分、BOM 料单、Excel/CSV 对比。

### `$cad-assembly-check`
用于检查板件打架、错位、悬空、实体丢失、视图不一致。

### `$cad-hole-system`
用于孔位数据、三维孔位标记和孔位检查。

### `$cad-error-fix`
用于根据截图、检查报告或手动参考 DXF 修复模型错误。

## 使用方法

在 Codex 中进入本仓库后，可直接这样说：

```text
使用 $cad-furniture-modeling，根据参考图和尺寸 2000×400×1200 生成展示书架三维模型。
```

```text
使用 $cad-assembly-check，检查当前模型是否存在板件打架、悬空和错位。
```

```text
使用 $cad-error-fix，根据这张截图修复层板和挡条分离的问题。
```

## 项目规则

仓库根目录的 `AGENTS.md` 是全局规则。Codex 处理本仓库 CAD 任务时应优先遵守。
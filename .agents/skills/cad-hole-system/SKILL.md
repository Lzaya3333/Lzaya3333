---
name: cad-hole-system
description: 用于 CAD 办公家具孔位建模、三维孔位标记、孔位检查，不破坏板件实体。触发词：孔位、打孔、三合一、木榫孔、螺丝孔、holes.json、孔位三维。
---

# CAD Hole System Skill

## 目标

为办公家具模型增加孔位系统，同时保持板件实体完整。

## 工作原则

1. 先做孔位数据和三维标记，不要直接布尔减孔。
2. 孔位必须依附于 Board，不是独立板件。
3. 孔位不能进入 BOM。
4. 孔位不能破坏板件实体。
5. 如果布尔减孔失败，必须回滚原实体，不能输出线框模型。

## Hole 数据模型

每个孔位包含：

- `hole_id`
- `board_id`
- `hole_type`
- `face`
- `local_x`
- `local_y`
- `diameter`
- `depth`
- `axis`
- `remark`

## 孔位图层

- `FUR_HOLE_3D`：三维孔位标记
- `FUR_HOLE_2D`：二维孔位标记
- `FUR_HOLE_DEBUG`：调试孔位

## 检查规则

1. 每个孔位必须有 `board_id`。
2. `board_id` 必须能找到 Board。
3. 孔位不能超出板件边界。
4. 孔位距离板边默认不得小于 20mm，除非规则明确允许。
5. 孔径必须大于 0。
6. 盲孔深度不能超过板厚。
7. 通孔必须明确标记为 `through_hole`。
8. 孔位必须落在指定 `face` 面上。
9. 孔位不能被当成 Board。

## 输出文件

- `holes.json`
- `holes_check_report.txt`
- `holes_debug.dxf`

## 真实开孔限制

默认：

```text
enable_boolean_hole_cut = false
```

当为 false：

- 不执行 subtract。
- 不切割板件。
- 只显示孔位标记。

当为 true：

- 必须先复制 Board 实体。
- 用圆柱 cutter 减孔。
- 布尔失败必须回滚。
- 不允许 explode 实体。
- 不允许把板件转换成 LINE/POLYLINE。

## 禁止事项

- 不要把孔位加入 BOM。
- 不要把孔位当实体板件。
- 不要因为孔位生成失败就把板件变成线条。
- 不要在正式 clean 模型里显示 debug 孔位文字。
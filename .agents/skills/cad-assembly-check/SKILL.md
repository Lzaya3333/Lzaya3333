---
name: cad-assembly-check
description: 用于 CAD 办公家具装配检查、碰撞检测、错位检测、悬空检测、三维和视图一致性检查。触发词：打架、错位、穿插、悬空、碰撞、检查报告、侧板没靠齐、层板没贴合。
---

# CAD Assembly Check Skill

## 目标

在修改或生成 CAD 家具模型后，先检查装配逻辑，再判断是否通过。

## 必查项目

1. 所有板件是否在整体外包围盒内。
2. 所有实体板件是否为 `BOARD`，不是 LINE/POLYLINE 调试线。
3. 内部横向板件是否夹在左右侧板之间。
4. 层板、挡板、背板、侧板之间是否打架或悬空。
5. 视图是否来自三维模型投影，不要单独手画假视图。
6. clean 模型里是否存在文字、调试线、多余图层。

## 默认误差

- 坐标误差：0.1mm
- 尺寸误差：0.1mm

## 常用检查公式

办公家具通用：

- `x >= 0`
- `y >= 0`
- `z >= 0`
- `x + size_x <= L`
- `y + size_y <= W`
- `z + size_z <= H`

夹在左右侧板之间的内部板件：

- `inner_left_x = side_thickness`
- `inner_right_x = L - side_thickness`
- `x = inner_left_x`
- `size_x = inner_right_x - inner_left_x`

办公桌背板常用规则：

- `BACK-001.x = SIDE-001.x + SIDE-001.size_x`
- `BACK-001.x + BACK-001.size_x = SIDE-002.x`
- `BACK-001.z + BACK-001.size_z = TOP-001.z`
- 如需后方内缩 2mm：`BACK-001.y + BACK-001.size_y = W - 2`

展示架层板和挡条：

- `RAIL-i.parent_shelf = SHELF-i`
- `RAIL-i.x = SHELF-i.x`
- `RAIL-i.size_x = SHELF-i.size_x`
- `RAIL-i` 必须贴合 `SHELF-i` 前沿，不允许悬空或分离。

## 输出报告

根据任务输出：

- `assembly_fit_report.txt`
- `assembly_collision_report.txt`
- `entity_type_check_report.txt`
- `left_view_check_report.txt`
- `shelf_rail_fit_report.txt`

报告格式：

```text
[错误编号]
part_id:
name:
error_type:
current_value:
expected_value:
difference:
reason:
fix_suggestion:
status:
```

## 禁止事项

- 不要只凭截图说“看起来正常”。
- 不要在检查失败时输出 PASS。
- 不要为了过检查而单独手画视图。
- 不要让 clean 模型包含 debug 图层对象。
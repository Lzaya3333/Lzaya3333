---
name: cad-furniture-modeling
description: 用于办公家具 CAD 参数化三维建模、参考图转结构、展示架/办公桌/柜体建模。触发词：CAD建模、三维、办公家具、书架、办公桌、板件独立、DXF。
---

# CAD Furniture Modeling Skill

## 目标

把办公家具需求转换成可检查、可拆单、可出图的参数化三维模型。

## 工作流程

1. 先确认产品类型、外观尺寸、板厚、结构数量。
2. 先列出结构理解和板件列表，不要直接写完整代码。
3. 建立统一坐标系：
   - X = 长度方向
   - Y = 深度方向
   - Z = 高度方向
   - 单位 = mm
4. 所有板件必须输出：
   - `part_id`
   - `name`
   - `type=BOARD`
   - `size_x`
   - `size_y`
   - `size_z`
   - `x`
   - `y`
   - `z`
   - `quantity`
5. 三维模型、BOM、三视图必须共用同一套 `Board` 数据。
6. 正式模型输出为 `*_clean.dxf`，调试模型输出为 `*_debug.dxf`。

## 建模原则

- 参考图只能用于外观比例，不能直接猜生产板件。
- 如果尺寸不明确，先写入 `structure_assumptions.md`。
- 板件必须独立，不允许合并。
- 异形侧板如果要求整体板，必须作为一个整体异形对象，不能拆成多块拼接。
- 如果当前 DXF 工具不支持真实异形实体，应明确说明，并用可检查的 polyface/mesh/extruded polygon 表示。

## 输出要求

每次建模至少输出：

- `actual_parts.json`
- `bom.csv`
- `structure_assumptions.md`
- `display_rack_3d_clean.dxf` 或对应产品 clean 文件
- `display_rack_debug.dxf` 或对应 debug 文件

## 禁止事项

- 不要凭图片随意增加板件。
- 不要把所有板件合成一个整体。
- 不要在正式模型里显示板件文字。
- 不要用调试线代替板件实体。
- 不要在没有检查报告时声称模型正确。
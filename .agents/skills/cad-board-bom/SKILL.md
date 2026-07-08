---
name: cad-board-bom
description: 用于办公家具板件拆分、BOM 料单、Excel/CSV 对比、板件编号规则。触发词：板件、拆单、料单、BOM、CSV、Excel、编号。
---

# CAD Board BOM Skill

## 目标

把 CAD 三维模型中的所有生产板件整理成稳定 BOM，并与原始料单对比。

## 板件数据规则

所有实体板件必须是 `BOARD` 类型，并进入 BOM。每块板件必须包含：

- `part_id`
- `name`
- `size_x`
- `size_y`
- `size_z`
- `quantity`
- `material`
- `edge_band`
- `remark`

## 非板件对象

以下对象不能进入 BOM：

- `EDGE_BAND` 封边
- `HOLE` 孔位
- `HARDWARE` 五金
- `AUX_LINE` 辅助线
- `TEXT` / `MTEXT` 标注文字
- debug 图层对象

## 编号建议

- `TOP-001` 顶板/台面
- `SIDE-001` 左侧板
- `SIDE-002` 右侧板
- `BASE-001` 底板
- `BACK-001` 背板
- `SHELF-001` 层板
- `RAIL-001` 挡板/挡条
- `DIV-001` 竖隔板

## 对比流程

1. 读取原始料单 `input_bom.xlsx` 或 `input_bom.csv`。
2. 读取程序生成的 `bom.csv`。
3. 按 `part_id` 对比：名称、尺寸、厚度、数量、材质。
4. 尺寸误差默认不超过 0.1mm。
5. 输出 `bom_compare_report.txt`。

## 报告必须列出

- 缺失板件
- 多余板件
- 重复板件
- 尺寸错误
- 数量错误
- 材质/封边不一致

## 禁止事项

- 不能因为模型里有线条就把它加入 BOM。
- 不能把孔位当板件。
- 不能让同一块板同时出现在多个编号里。
- 不能在 BOM 对不上时继续输出 PASS。
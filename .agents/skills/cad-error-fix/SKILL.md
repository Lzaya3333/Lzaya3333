---
name: cad-error-fix
description: 用于根据 CAD 截图、检查报告、手动参考 DXF 修复模型错误。触发词：错误、改错、不合理、没有靠齐、打架、悬空、和参考图不一致。
---

# CAD Error Fix Skill

## 目标

把用户指出的 CAD 模型错误转换成可执行的诊断、检查、修复流程。

## 处理流程

1. 先复述用户指出的错误对象和错误类型。
2. 不要直接凭截图乱移动板件。
3. 输出 `actual_parts.json`，列出每块板件的坐标和尺寸。
4. 如有原始料单或参考文件，输出 `expected_parts.json`。
5. 先生成错误报告，再修复代码。
6. 只修复 FAIL 项，不要改已经 PASS 的板件。
7. 修复后重新生成模型和检查报告。

## 常见错误分类

- 多余板件：`EXTRA_PART`
- 缺失板件：`MISSING_PART`
- 坐标错位：`POSITION_ERROR`
- 尺寸错误：`SIZE_ERROR`
- 板件打架：`COLLISION`
- 板件悬空：`FLOATING_PART`
- 层板与挡条分离：`SHELF_RAIL_GAP`
- 背板没靠齐：`BACK_NOT_ALIGNED`
- 侧板被错误拆分：`SIDE_PANEL_SPLIT`
- 实体被转成线条：`BOARD_ENTITY_LOST`
- clean 模型存在文字：`TEXT_IN_CLEAN_MODEL`

## 报告格式

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

## 修复原则

- 模型本体错误时，修三维数据和生成规则，不要只修二维视图。
- 视图必须由三维模型投影生成，不要单独画假视图。
- 层板和挡条必须成组生成，挡条位置跟随层板前沿。
- 背板、侧板、层板必须按装配约束靠齐。
- 正式 clean 模型不要显示文字和调试线。

## 用户手动修正参考模型

如果用户提供 `manual_reference.dxf`：

1. 读取程序输出 `program_output.dxf`。
2. 读取手动参考 `manual_reference.dxf`。
3. 提取每块板件 bounding box。
4. 按 part_id、尺寸、位置匹配。
5. 输出 `model_error_report.txt`。
6. 等用户确认报告后再修代码。

## 禁止事项

- 不要一边检测一边乱改。
- 不要新增料单外板件。
- 不要把调试线当正式板件。
- 不要为了看起来像而破坏 BOM。
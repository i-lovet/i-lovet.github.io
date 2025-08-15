---
标题: SQL记录
aliases: 
tags:
  - 技术/MySQL
创建时间: 2025-08-06 18:46
修改时间: 2025-08-06 22:29
---
### 1、JSON 串查询

```sql
-- JSON_EXTRACT 函数用于从 JSON 文档中提取数据。
SELECT DISTINCT JSON_EXTRACT(device_info, '$.model') AS 'iPad型号' FROM sms_face_detection WHERE device_info LIKE '%iPad%';
SELECT * FROM sms_face_detection WHERE JSON_EXTRACT(device_info, '$.model') LIKE '%iPad%';
-- -> 操作符是 JSON_EXTRACT 的简写形式。
SELECT DISTINCT device_info->'$.model' AS 'iPad型号' FROM sms_face_detection WHERE device_info LIKE '%iPad%';
SELECT * FROM sms_face_detection WHERE device_info->'$.model' LIKE '%iPad%';

-- JSON_CONTAINS 函数用于检查 JSON 文档中是否包含指定的值。
SELECT * FROM sms_face_detection WHERE JSON_CONTAINS(device_info, JSON_OBJECT('model', 'iPad12,1'));
SELECT * FROM sms_face_detection WHERE JSON_CONTAINS(device_info->'$.model', '"iPad12,1"');
SELECT * FROM sms_face_detection WHERE JSON_CONTAINS(device_info, '"iPad12,1"', '$.model');
```

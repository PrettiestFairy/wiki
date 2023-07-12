# 数值类型

类型|大小|用途范围
:-:|:-:|:-:
tinyint|1B|小数值
smallint|2B|大数值  
mediumint|3B|大数值
int/integr|4B|大数值  
bigint|8B|超大数值
folat|4B|单精度<br>浮点数
double|8B|双精度<br>浮点数

# 字符串类型

类型|大小|用途范围
:-:|:-:|:-:
char|255|定长字符串
varchar|65 535|变长字符串
tinyblob|255|不超过255个字符的二进制字符串
tinytext|255|短文本字符串
blob|65 535|二进制形式的长文本数据
text|65 535|长文本数据
mediumblob|16 777 215|二进制形式的中等长度文本数据
mediumblog|16 777 215|中等长度文本数据
longblob|4 294 967 295|二进制形式的极大文本数据
longtext|4 294 967 295|极大文本数据

# 时间类型

类型|大小|格式|用途范围
-|-|-|-
DATE|3|YYYY-MM-DD|日期值
TIME|3|HH:MM:SS|时间值或持续时间
YEAR|1|YYYY|年份值
DATETIME|8|YYYY-MM-DDHH:MM:SS|混合日期和时间值
TIMESTAMP|4|YYYYMMDDHHMMSS|混合日期和时间值，时间戳

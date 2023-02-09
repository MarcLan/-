## Hash函数

问题：数组遍历消耗大
- 名称 - Hash - 得到整数
- 整数性质：
  - 输入相同的参数，返回相同的值
  - 输入不同的参数，返回不同的值
- 数据-内存地址-一一映射

```python
def cal_hash(key):
    square = ord(key) **2
    mid = str(square)[3:6]
    return int(mid)

print(cal_hash('嘿嘿'))
print(cal_hash('哈哈'))
print(cal_hash('嚯嚯'))
```

- 参数不同，结果相同 = 冲突（Collision）
- 算法：SHA-256, MD5
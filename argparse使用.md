# python 模块 argparse 使用

这个模块是python的一个用于命令选项与参数解析的模块。通过在程序中定义好我们需要的参数。argparse将会从sys.argv中解析出来。并自动生成帮助和使用信息。



### 一般的步骤：

1. 创建ArgumentParser()对象
2. 调用add_argument()方法添加参数
3. 使用parse_args()解析添加的参数

```python
import argparse

parser = argparse.ArgumentParser()
parser.add_argument('integer', type=int, help='display an integer')
args = parser.parse_args()

print(args.integer*10)
```

将上面保存为py文件然后在命令行调用如下

```
G:\dowmload>python argparse_usage.py 10
100

G:\dowmload>python argparse_usage.py 0
0
```


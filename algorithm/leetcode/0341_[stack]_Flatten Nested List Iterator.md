Stack的应用


Flatten一个python的嵌套list:
```python
def flatten_with_stack(arr):
    res, stack = [], []

    for item in reversed(arr):
        stack.append(item)

    while stack:
        item = stack.pop()
        if isinstance(item, list):
            for i in reversed(item):
                stack.append(i)
        else:
            res.append(item)
    return res
```


这一题的写法也是类似的：

```python
#class NestedInteger:
#    def isInteger(self) -> bool:
#        """
#        @return True if this NestedInteger holds a single integer, rather than a nested list.
#        """
#
#    def getInteger(self) -> int:
#        """
#        @return the single integer that this NestedInteger holds, if it holds a single integer
#        Return None if this NestedInteger holds a nested list
#        """
#
#    def getList(self) -> [NestedInteger]:
#        """
#        @return the nested list that this NestedInteger holds, if it holds a nested list
#        Return None if this NestedInteger holds a single integer
#        """

```

```python
class NestedIterator:
    def __init__(self, nestedList: [NestedInteger]):
        self.arr = nestedList
    
    def next(self) -> int:
        return self.arr.pop(0).getInteger()        
            
    def hasNext(self) -> bool:
        while self.arr:
            if self.arr[0].isInteger():
                return True
            else:
                cur = self.arr.pop(0).getList()
                self.arr = cur + self.arr
        return False
```

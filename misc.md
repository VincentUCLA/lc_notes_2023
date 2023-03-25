### Union-find

Quick-find: 确定两个点是否处在同一个连通集里：遍历所有的边(p, q)，每次都把id[p]改成id[q]

```py
class UF:
    id = []
    count = 0

    def __init__(self, N):
        self.count = N
        for i in range(0, N):
            self.id.append(i)

    def dump(self):
        temp = ""
        for i in range(0, self.count):
            temp += (" " + str(i))
        print(temp)
        temp = ""
        for i in self.id:
            temp += (" " + str(i))
        print(temp)
        print()

    def find(self, p):
        return self.id[p]

    def quick_find(self, arr):
        for i in arr:
            pID = self.find(i[0])
            qID = self.find(i[1])
            if pID == qID:
                continue
            else:
                print(pID, qID)
                for j in range(0, self.count):
                    if self.id[j] == pID:
                        self.id[j] = qID
                self.dump()
```

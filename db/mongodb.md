

### 基础使用
```mongo shell
# 查看当前服务器的所有数据库
show dbs

# 查看当前使用的数据库
db

# 切换数据库
use test_db

# 查看当前数据库下的所有数据表
show tables

# 增
db.user_info.insert({"id": 1, "name": "changwoo", "age": 13})

# 查
db.user_info.find()

# 改
db.user_info.update({"name": "changwoo"}, {$set: {"name": "changwoo"}})

# 删
db.user_info.remove({"name": "changwoo"})
db.user_info.drop()

```

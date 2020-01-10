### tar
压缩
```
# 压缩并保存绝对路径
tar -zcPvf /spider3/storage/2019_12_30_18_43_02.tar.gz /spider2/storage/

# 压缩并保存部分路径结构（适用于python操作命令行，无法cd到相对路径下）
# 将"/spider2/storage/raw/..."下的"raw/..."压缩到"/spider3/storage/2019_12_30_18_43_02.tar.gz"
tar -zcPvf /spider3/storage/2019_12_30_18_43_02.tar.gz -C /spider2/storage/ raw

```
解压
```bash shell
# 解压到指定路径
tar zxvf 2019_12_30_18_42_52.tar.gz -C /spiders/storage/


```

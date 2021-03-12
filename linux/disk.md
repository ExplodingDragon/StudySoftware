# 磁盘管理

## tmpfs

### 挂载目录为 `tmpfs`

最简单的挂载方式如下：

```bash
mount -t tmpfs tmpfs /mnt/tmp
```

Linux 上的 tmpfs 支援三个挂载选项：

- size - 设定分配给此 tmpfs 文件系统的内存上限，默认是内存的一半。也可以在尾加上百分比 (%) 表示占用内存的百分比， 0 表示没有上限。
- nr_blocks - 和 size 一样设定分配内存上限，但单位为 PAGE_CACHE_SIZE ( 默认为 4 KiB) 。
- nr_inodes - 设定此 tmpfs 文件系统的 inode 上限，也就是限制可以存放文件的总数。
以上三个选项都可以在数值后面加上 k 、 m 、 g 来表示单位。

你可以在挂载的时候直接使用这些选项 :

```bash
 mount-t tmpfs -o size=1G tmpfs /mnt/mytmpfs 
```

也可以在挂载后，重新挂载 (remount) tmpfs 即可改变内存上限:

```bash
mount -o remount,size=512m /mnt/tmp
```

要在Linux系统中添加一个新硬盘并对/home目录进行扩容，您需要执行以下步骤：

1. 确认新硬盘已连接到系统。使用`lsblk`命令查看已连接的硬盘设备。例如，新硬盘可能显示为`/dev/sdb`。

2. 对新硬盘进行分区。使用`fdisk`或`parted`工具创建一个新分区。例如，使用`fdisk`：

```bash
sudo fdisk /dev/sdb
```

按照提示操作，创建一个新分区（例如，`/dev/sdb1`）。

3. 创建文件系统。使用`mkfs`命令为新分区创建文件系统。例如，创建一个ext4文件系统：

```bash
sudo mkfs.ext4 /dev/sdb1
```

4. 创建一个挂载点。创建一个新目录作为挂载点，例如`/mnt/new_home`：

```bash
sudo mkdir /mnt/new_home
```

5. 挂载新分区。将新分区挂载到新创建的目录：

```bash
sudo mount /dev/sdb1 /mnt/new_home
```

6. 复制/home目录的内容。使用`rsync`命令将/home目录的内容复制到新分区：

```bash
sudo rsync -aXS --exclude='/*/.gvfs' /home/. /mnt/new_home/.
```

7. 备份原始/home目录。将原始/home目录重命名为备份：

```bash
sudo mv /home /home_backup
```

8. 创建新的/home目录。创建一个新的空/home目录：

```bash
sudo mkdir /home
```

9. 将新分区挂载到/home目录：

```bash
sudo umount /mnt/new_home
sudo mount /dev/sdb1 /home
```

10. 更新`/etc/fstab`文件。将新分区添加到`/etc/fstab`文件，以便在系统启动时自动挂载。编辑`/etc/fstab`文件，添加以下行（请根据实际情况修改）：

```bash
/dev/sdb1 /home ext4 defaults 0 2
```

11. 重新挂载所有分区。运行以下命令以重新挂载所有分区：

```bash
sudo mount -a
```

现在，新硬盘已添加到系统，并已将其扩展到/home目录。请确保所有数据已成功迁移到新分区，并且系统正常运行。在确认一切正常后，您可以删除备份的/home_backup目录。
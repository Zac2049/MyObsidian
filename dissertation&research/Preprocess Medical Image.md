- 给定txt文件，从txt文件中读取各个图像路径
```python
for item in args.dataset_list:
        for line in open(args.data_txt_path + item +'_train.txt'):
            name = line.strip().split()[1].split('.')[0]
            train_img.append(args.data_root_path + line.strip().split()[0])
            train_lbl.append(args.data_root_path + line.strip().split()[1])
            train_post_lbl.append(args.data_root_path + name.replace('label','post_label') + '.h5')
            train_name.append(name)
    data_dicts_train = [{'image': image, 'label': label, 'post_label': post_label, 'name': name}
                for image, label, post_label, name in zip(train_img, train_lbl, train_post_lbl, train_name)]
```
- 输出--两种方式
	1. 返回dataloader
	2. 直接存回数据
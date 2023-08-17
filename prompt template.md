```
def replace(mask_data,new_mask,num_mask):
    for i in range(num_mask):
        array = new_mask & mask_data[i,:,:]
        count = np.any(array == 1)
        if count > 0:
            return i
    return -1
```
解释上述
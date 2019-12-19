# bitmap

用每个bit标识一个钟状态0或1，开或关

## 例子

记录vlan是否启用

```
int vlan_x;
int bitmap_vlan[128];   /* set 4096 bit for vlans */
memset(bitmap_vlan, 0, sizeof(bitmap_vlan));

bitmap_vlan[vlan_x / 32] | (0x1 << (vlan_x % 32));    /* vlan x 置 1 */

if(bitmap_vlan[vlan_x / 32] & (0x1 << (vlan_x % 32))) /* 判断 vlan x 是否置位 */
    do_something();
```

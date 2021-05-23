IPv6


## 地址类型



```
Allocation                            Prefix         Fraction of    To Hex

                                      (binary)       Address Space

-----------------------------------   --------       -------------  -------------------------

Reserved                              0000 0000      1/256          (0000 0000 -> 00)
Unassigned                            0000 0001      1/256          (0000 0001 -> 01)


Reserved for NSAP Allocation          0000 001       1/128          (0000 001x -> 02 ~ 03)
Reserved for IPX Allocation           0000 010       1/128          (0000 010x -> 04 ~ 05)


Unassigned                            0000 011       1/128          (0000 011x -> 06 ~ 07)
Unassigned                            0000 1         1/32           (0000 1xxx -> 08 ~ 0f)
Unassigned                            0001           1/16           (0001 xxxx -> 10 ~1f)

// 以上为 1/8 的 IPv6 地址空间，即 000x，也就是 8 进制的 0

Aggregatable Global Unicast Addresses 001            1/8            (001x xxxx -> 20 ~ 3f)
Unassigned                            010            1/8            
Unassigned                            011            1/8
Unassigned                            100            1/8
Unassigned                            101            1/8
Unassigned                            110            1/8            (110x xxxx -> c0 ~ df)

// 以上为 6/8 的 IPv6 地址空间，即 001x 到 110x。也就是除了 000x（8 进制 0） 和 111x（8 进制 7） 之外的所有 IP

Unassigned                            1110           1/16           (1110 xxxx -> e0 ~ ef)
Unassigned                            1111 0         1/32           (1111 0xxx -> f0 ~ f7)
Unassigned                            1111 10        1/64           (1111 10xx -> f8 ~ fb)
Unassigned                            1111 110       1/128          (1111 110x -> fc ~ fd )
Unassigned                            1111 1110 0    1/512          (1111 1110 0x -> fe00 ~ fe7f )


Link-Local Unicast Addresses          1111 1110 10   1/1024         1111 1110 10x -> fe80 ~ febf )
Site-Local Unicast Addresses          1111 1110 11   1/1024         1111 1110 11x -> fec0 ~ feff)


Multicast Addresses                   1111 1111      1/256          1111 1111 x -> ff00 ~ ffff)
```


注：16 进制的一个数字代表 4 位的 IPv6 地址长度

# unpack
### unpack : memory data -> integer
`u32(data, endian='little')`: 주어진 바이트열을 32비트 unsigned integer로 변환합니다. `endian` 매개변수는 바이트 순서를 지정하는데, 기본값은 little endian입니다.
```python
from pwn import *

edn = 'big'
data = b'\x01\x02\x03\x04'
num = u32(data,endian=edn)
print('endian : '+edn)
print(num) # 출력: 16909060
print(hex(num)) # 출력 : 0x01020304
```
```python
from pwn import *

edn = 'little'
data = b'\x01\x02\x03\x04'
num = u32(data,endian=edn)
print('endian : '+edn)
print(num) # 출력: 67305985
print(hex(num)) # 출력 : 0x04030201
```
`u64(data, endian='little')`: 주어진 바이트열을 64비트 unsigned integer로 변환합니다. `endian` 매개변수는 바이트 순서를 지정하는데, 기본값은 little endian입니다.
```python
from pwn import *
data = b'\x01\x02\x03\x04\x05\x06\x07\x08'
num = u64(data)
print(num)  # 출력: 72623859790382856
```
# Pack
### pack : integet -> memory data
`p32(num, endian='little')`: 주어진 32비트 unsigned integer를 바이트열로 변환합니다. `endian` 매개변수는 바이트 순서를 지정하는데, 기본값은 little endian입니다.
- 반드시 32bit(=4byte)여야함
```python
from pwn import *
num = 0x01020304
data = p32(num)
print(data)  # 출력: b'\x01\x02\x03\x04'
```
`p64(num, endian='big')`: 주어진 64비트 unsigned integer를 바이트열로 변환합니다. `endian` 매개변수는 바이트 순서를 지정하는데, 기본값은 little endian입니다.
- 반드시 64bit(=8byte)여야함
```python
from pwn import *
num = 0x0102030405060708
data = p64(num)
print(data)  # 출력: b'\x01\x02\x03\x04\x05\x06\x07\x08'
```
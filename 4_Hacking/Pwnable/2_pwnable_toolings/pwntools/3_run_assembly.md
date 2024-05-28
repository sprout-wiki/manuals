# run_assembly(asm)
### print hello and run
```python
from pwn import *

s = """
mov rdi, 1
mov rax, 0x6f6c6c6568
push rax
mov rsi, rsp
mov rdx, 5
mov rax, 1
syscall
"""
context(arch="amd64", os="linux")

p = run_assembly(s)

p.interactive()
```

### print hello and assemble
```python
from pwn import *

s = """
mov rdi, 1
mov rax, 0x6f6c6c6568
push rax
mov rsi, rsp
mov rdx, 5
mov rax, 1
syscall
"""
context(arch="amd64", os="linux")

bin = asm(s)
print(bin)

```
then, print
```plain
b'H\xc7\xc7\x01\x00\x00\x00H\xb8hello\x00\x00\x00PH\x89\xe6H\xc7\xc2\x05\x00\x00\x00H\xc7\xc0\x01\x00\x00\x00\x0f\x05'
```

# run_shellcode(bin)
### ljust str
```python
from pwn import *

# 어셈블리 코드 생성
s = """
mov rdi, 1
"""
# "hello" 문자열을 64비트 레지스터에 로드
s += 'mov rax, ' + str(u64(b'hello\n'.ljust(8, b'\x00')))
s += '''
push rax
mov rsi, rsp
mov rdx, 5
mov rax, 1
syscall
'''

# 어셈블리 코드 어셈블
context(arch="amd64", os="linux")
bin = asm(s)

# 어셈블리 코드 실행
p = run_shellcode(bin)

# 결과 출력
print(p.recv())
```


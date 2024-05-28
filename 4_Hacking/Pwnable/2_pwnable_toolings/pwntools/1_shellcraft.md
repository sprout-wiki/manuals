https://docs.pwntools.com/en/stable/encoders.html

```python
from pwn import *

# 어셈블리 코드 생성
shellcode = shellcraft.i386.linux.echo('Hello, World!')

# 어셈블리 코드를 어셈블하여 바이너리로 변환
binary_shellcode = asm(shellcode)

# 실행할 프로세스 생성
sh = process("")

# 어셈블리 코드를 프로세스에 전송
sh.send(binary_shellcode)

# 셸을 보기 위해 interactive 모드로 전환
sh.interactive()
```

shellcode + run_assembly
```python
from pwn import *

shellcode = shellcraft.amd64.linux.sh()

context(arch='amd64', os='linux')

p = run_assembly(shellcode)

p.interactive()
```
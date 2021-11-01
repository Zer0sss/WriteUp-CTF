# Challenge name: zoom2win


## Pseudocode
```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  char v4[32]; // [rsp+0h] [rbp-20h] BYREF
  puts("Let's not overcomplicate. Just zoom2win :)");
  return gets(v4, argv);
}
```

## Assembly code
```assembly
.text:00000000004011B2 ; int __cdecl main(int argc, const char **argv, const char **envp)
.text:00000000004011B2                 public main
.text:00000000004011B2 main            proc near               ; DATA XREF: _start+21↑o
.text:00000000004011B2
.text:00000000004011B2 var_20          = byte ptr -20h
.text:00000000004011B2
.text:00000000004011B2 ; __unwind {
.text:00000000004011B2                 endbr64
.text:00000000004011B6                 push    rbp
.text:00000000004011B7                 mov     rbp, rsp
.text:00000000004011BA                 sub     rsp, 20h
.text:00000000004011BE                 lea     rdi, s          ; "Let's not overcomplicate. Just zoom2win"...
.text:00000000004011C5                 call    _puts
.text:00000000004011CA                 lea     rax, [rbp+var_20]
.text:00000000004011CE                 mov     rdi, rax
.text:00000000004011D1                 mov     eax, 0
.text:00000000004011D6                 call    _gets
.text:00000000004011DB                 nop
.text:00000000004011DC                 leave
.text:00000000004011DD                 retn
.text:00000000004011DD ; } // starts at 4011B2
.text:00000000004011DD main            endp
```

## Methodology

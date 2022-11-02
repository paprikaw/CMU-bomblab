# phase_3
code:
``` s
 0x0000000000400f43 <+0>:     sub    $0x18,%rsp
   0x0000000000400f47 <+4>:     lea    0xc(%rsp),%rcx
   0x0000000000400f4c <+9>:     lea    0x8(%rsp),%rdx
   0x0000000000400f51 <+14>:    mov    $0x4025cf,%esi
   0x0000000000400f56 <+19>:    mov    $0x0,%eax
   0x0000000000400f5b <+24>:    callq  0x400bf0 <__isoc99_sscanf@plt>
   0x0000000000400f60 <+29>:    cmp    $0x1,%eax
   0x0000000000400f63 <+32>:    jg     0x400f6a <phase_3+39>
   0x0000000000400f65 <+34>:    callq  0x40143a <explode_bomb>
   0x0000000000400f6a <+39>:    cmpl   $0x7,0x8(%rsp)
   0x0000000000400f6f <+44>:    ja     0x400fad <phase_3+106>
   0x0000000000400f71 <+46>:    mov    0x8(%rsp),%eax
   0x0000000000400f75 <+50>:    jmpq   *0x402470(,%rax,8)
   0x0000000000400f7c <+57>:    mov    $0xcf,%eax
   0x0000000000400f81 <+62>:    jmp    0x400fbe <phase_3+123>
=> 0x0000000000400f83 <+64>:    mov    $0x2c3,%eax
   0x0000000000400f88 <+69>:    jmp    0x400fbe <phase_3+123>
   0x0000000000400f8a <+71>:    mov    $0x100,%eax
   0x0000000000400f8f <+76>:    jmp    0x400fbe <phase_3+123>
   0x0000000000400f91 <+78>:    mov    $0x185,%eax
   0x0000000000400f96 <+83>:    jmp    0x400fbe <phase_3+123>
   0x0000000000400f98 <+85>:    mov    $0xce,%eax
   0x0000000000400f9d <+90>:    jmp    0x400fbe <phase_3+123>
   0x0000000000400f9f <+92>:    mov    $0x2aa,%eax
   0x0000000000400fa4 <+97>:    jmp    0x400fbe <phase_3+123>
   0x0000000000400fa6 <+99>:    mov    $0x147,%eax
   0x0000000000400fab <+104>:   jmp    0x400fbe <phase_3+123>
   0x0000000000400fad <+106>:   callq  0x40143a <explode_bomb>
   0x0000000000400fb2 <+111>:   mov    $0x0,%eax
   0x0000000000400fb7 <+116>:   jmp    0x400fbe <phase_3+123>
   0x0000000000400fb9 <+118>:   mov    $0x137,%eax
   0x0000000000400fbe <+123>:   cmp    0xc(%rsp),%eax
   0x0000000000400fc2 <+127>:   je     0x400fc9 <phase_3+134>
   0x0000000000400fc4 <+129>:   callq  0x40143a <explode_bomb>
   0x0000000000400fc9 <+134>:   add    $0x18,%rsp
   0x0000000000400fcd <+138>:   retq
```

这里使用了switch结构
在global value里面存放了一个数字到地址的映射
我们注意到这一行代码
```
    0x0000000000400f75 <+50>:    jmpq   *0x402470(,%rax,8)
```
这里的意思是解析(0x402470 + $rax * 8)这个地址里面的内容，我们可以打印出这个地址里面存放的array;
```
(gdb) x/8g 0x402470
0x402470:       0x0000000000400f7c      0x0000000000400fb9
0x402480:       0x0000000000400f83      0x0000000000400f8a
0x402490:       0x0000000000400f91      0x0000000000400f98
0x4024a0:       0x0000000000400f9f      0x0000000000400fa6
```
这就得到了switch table中所有和数字对应的地址，我们可以根据这个地址跳过bomb，走到下一个光卡

# phase_4
当我们想要查看rsp指向的local value的值，我们需要使用x command。因为rsp是一个指针，只保存一个地址。
## Phase 4 codes: 

```s
Breakpoint 1, 0x000000000040100c in phase_4 ()
(gdb) disas phase_4 
Dump of assembler code for function phase_4:
=> 0x000000000040100c <+0>:     sub    $0x18,%rsp
   0x0000000000401010 <+4>:     lea    0xc(%rsp),%rcx
   0x0000000000401015 <+9>:     lea    0x8(%rsp),%rdx
   0x000000000040101a <+14>:    mov    $0x4025cf,%esi
   0x000000000040101f <+19>:    mov    $0x0,%eax
   0x0000000000401024 <+24>:    callq  0x400bf0 <__isoc99_sscanf@plt>
   0x0000000000401029 <+29>:    cmp    $0x2,%eax
   0x000000000040102c <+32>:    jne    0x401035 <phase_4+41>
   0x000000000040102e <+34>:    cmpl   $0xe,0x8(%rsp)
   0x0000000000401033 <+39>:    jbe    0x40103a <phase_4+46>
   0x0000000000401035 <+41>:    callq  0x40143a <explode_bomb>
   0x000000000040103a <+46>:    mov    $0xe,%edx
   0x000000000040103f <+51>:    mov    $0x0,%esi
   0x0000000000401044 <+56>:    mov    0x8(%rsp),%edi
   0x0000000000401048 <+60>:    callq  0x400fce <func4>
   0x000000000040104d <+65>:    test   %eax,%eax
   0x000000000040104f <+67>:    jne    0x401058 <phase_4+76>
   0x0000000000401051 <+69>:    cmpl   $0x0,0xc(%rsp)
   0x0000000000401056 <+74>:    je     0x40105d <phase_4+81>
   0x0000000000401058 <+76>:    callq  0x40143a <explode_bomb>
   0x000000000040105d <+81>:    add    $0x18,%rsp
   0x0000000000401061 <+85>:    retq 
```
func_4 codes:
``` s
(gdb) disas func4
Dump of assembler code for function func4:
   0x0000000000400fce <+0>:     sub    $0x8,%rsp
   0x0000000000400fd2 <+4>:     mov    %edx,%eax
   0x0000000000400fd4 <+6>:     sub    %esi,%eax
   0x0000000000400fd6 <+8>:     mov    %eax,%ecx
   0x0000000000400fd8 <+10>:    shr    $0x1f,%ecx
   0x0000000000400fdb <+13>:    add    %ecx,%eax
   0x0000000000400fdd <+15>:    sar    %eax
   0x0000000000400fdf <+17>:    lea    (%rax,%rsi,1),%ecx
   0x0000000000400fe2 <+20>:    cmp    %edi,%ecx
   0x0000000000400fe4 <+22>:    jle    0x400ff2 <func4+36>
   0x0000000000400fe6 <+24>:    lea    -0x1(%rcx),%edx
   0x0000000000400fe9 <+27>:    callq  0x400fce <func4>
   0x0000000000400fee <+32>:    add    %eax,%eax
   0x0000000000400ff0 <+34>:    jmp    0x401007 <func4+57>
   0x0000000000400ff2 <+36>:    mov    $0x0,%eax
   0x0000000000400ff7 <+41>:    cmp    %edi,%ecx
   0x0000000000400ff9 <+43>:    jge    0x401007 <func4+57>
   0x0000000000400ffb <+45>:    lea    0x1(%rcx),%esi
   0x0000000000400ffe <+48>:    callq  0x400fce <func4>
   0x0000000000401003 <+53>:    lea    0x1(%rax,%rax,1),%eax
   0x0000000000401007 <+57>:    add    $0x8,%rsp
   0x000000000040100b <+61>:    retq
```

# phase 5
``` s
Dump of assembler code for function phase_5:
   0x0000000000401062 <+0>:     push   %rbx
   0x0000000000401063 <+1>:     sub    $0x20,%rsp
   0x0000000000401067 <+5>:     mov    %rdi,%rbx
   0x000000000040106a <+8>:     mov    %fs:0x28,%rax
   0x0000000000401073 <+17>:    mov    %rax,0x18(%rsp)
   0x0000000000401078 <+22>:    xor    %eax,%eax
   0x000000000040107a <+24>:    callq  0x40131b <string_length>
   0x000000000040107f <+29>:    cmp    $0x6,%eax
   0x0000000000401082 <+32>:    je     0x4010d2 <phase_5+112>   # input 6 character
   0x0000000000401084 <+34>:    callq  0x40143a <explode_bomb>
   0x0000000000401089 <+39>:    jmp    0x4010d2 <phase_5+112>

    # Loop started
    # rax = 0
    # rbx: input string pointer
   0x000000000040108b <+41>:    movzbl (%rbx,%rax,1),%ecx # get character from input string
   0x000000000040108f <+45>:    mov    %cl,(%rsp) # rsp: current character
   0x0000000000401092 <+48>:    mov    (%rsp),%rdx # rdx: current character
   0x0000000000401096 <+52>:    and    $0xf,%edx # rounding
   0x0000000000401099 <+55>:    movzbl 0x4024b0(%rdx),%edx # get chacracter from global string
   0x00000000004010a0 <+62>:    mov    %dl,0x10(%rsp,%rax,1) # copy this character to target place
   0x00000000004010a4 <+66>:    add    $0x1,%rax # counter++
   0x00000000004010a8 <+70>:    cmp    $0x6,%rax # test if counter is less than 6
   0x00000000004010ac <+74>:    jne    0x40108b <phase_5+41>
   0x00000000004010ae <+76>:    movb   $0x0,0x16(%rsp)
   0x00000000004010b3 <+81>:    mov    $0x40245e,%esi
   0x00000000004010b8 <+86>:    lea    0x10(%rsp),%rdi
    0x00000000004010bd <+91>:    callq  0x401338 <strings_not_equal>
   0x00000000004010c2 <+96>:    test   %eax,%eax
   0x00000000004010c4 <+98>:    je     0x4010d9 <phase_5+119>
   0x00000000004010c6 <+100>:   callq  0x40143a <explode_bomb>
   0x00000000004010cb <+105>:   nopl   0x0(%rax,%rax,1)
   0x00000000004010d0 <+110>:   jmp    0x4010d9 <phase_5+119>
    # before loop started
   0x00000000004010d2 <+112>:   mov    $0x0,%eax 
   0x00000000004010d7 <+117>:   jmp    0x40108b <phase_5+41>

   0x00000000004010d9 <+119>:   mov    0x18(%rsp),%rax
   0x00000000004010de <+124>:   xor    %fs:0x28,%rax
   0x00000000004010e7 <+133>:   je     0x4010ee <phase_5+140>
   0x00000000004010e9 <+135>:   callq  0x400b30 <__stack_chk_fail@plt>
   0x00000000004010ee <+140>:   add    $0x20,%rsp
   0x00000000004010f2 <+144>:   pop    %rbx
   0x00000000004010f3 <+145>:   retq 
```

# phase 6
``` s
(gdb) disas phase_6
Dump of assembler code for function phase_6:
    # initialize local variable
    0x00000000004010f4 <+0>:     push   %r14
    0x00000000004010f6 <+2>:     push   %r13
    0x00000000004010f8 <+4>:     push   %r12
    0x00000000004010fa <+6>:     push   %rbp
    0x00000000004010fb <+7>:     push   %rbx
    0x00000000004010fc <+8>:     sub    $0x50,%rsp
    # r13 -> rsp, rsi => rsp
    0x0000000000401100 <+12>:    mov    %rsp,%r13
    0x0000000000401103 <+15>:    mov    %rsp,%rsi

    0x0000000000401106 <+18>:    callq  0x40145c <read_six_numbers> # Read six number from string
    0x000000000040110b <+23>:    mov    %rsp,%r14
    # do while Loop Make sure we dont have repetitive numbers
    0x000000000040110e <+26>:    mov    $0x0,%r12d # r12d is a counter
    0x0000000000401114 <+32>:    mov    %r13,%rbp
    0x0000000000401117 <+35>:    mov    0x0(%r13),%eax
    0x000000000040111b <+39>:    sub    $0x1,%eax
    0x000000000040111e <+42>:    cmp    $0x5,%eax
    0x0000000000401121 <+45>:    jbe    0x401128 <phase_6+52>
    0x0000000000401123 <+47>:    callq  0x40143a <explode_bomb>
    0x0000000000401128 <+52>:    add    $0x1,%r12d #counter start from 1
    0x000000000040112c <+56>:    cmp    $0x6,%r12d # if counter is less than 6
    0x0000000000401130 <+60>:    je     0x401153 <phase_6+95>
    0x0000000000401132 <+62>:    mov    %r12d,%ebx # ebx = counter1

    # loop 2
    0x0000000000401135 <+65>:    movslq %ebx,%rax # rax = counte2r
    0x0000000000401138 <+68>:    mov    (%rsp,%rax,4),%eax # retrieve input numbers based on index rax = input[rax];
    0x000000000040113b <+71>:    cmp    %eax,0x0(%rbp) # compare current retrive character to the first one and it should be not equal
    # each number in inputs should not be bigger than 6 
    0x000000000040113e <+74>:    jne    0x401145 <phase_6+81>
    0x0000000000401140 <+76>:    callq  0x40143a <explode_bomb>
    0x0000000000401145 <+81>:    add    $0x1,%ebx # ebx = counter + 1
    0x0000000000401148 <+84>:    cmp    $0x5,%ebx
    0x000000000040114b <+87>:    jle    0x401135 <phase_6+65> # 
    # loop2 ends
    0x000000000040114d <+89>:    add    $0x4,%r13
    0x0000000000401151 <+93>:    jmp    0x401114 <phase_6+32>
    # r12 d loop ends
    0x0000000000401153 <+95>:    lea    0x18(%rsp),%rsi #rsi pointers to the postion after the last element of input array
    0x0000000000401158 <+100>:   mov    %r14,%rax # rax = rsp
    0x000000000040115b <+103>:   mov    $0x7,%ecx # ecx = 7
    # Loop 3
    # for every element, map it to 7 - cur
    0x0000000000401160 <+108>:   mov    %ecx,%edx # edx = 7
    0x0000000000401162 <+110>:   sub    (%rax),%edx # edx = 7 - input[0]
    0x0000000000401164 <+112>:   mov    %edx,(%rax) # input [0] = edx
    0x0000000000401166 <+114>:   add    $0x4,%rax # rax = 4
    0x000000000040116a <+118>:   cmp    %rsi,%rax # rax = rsi
    0x000000000040116d <+121>:   jne    0x401160 <phase_6+108>
    # End of Loop 3

    # start to initialize the address node array based on input array
    0x000000000040116f <+123>:   mov    $0x0,%esi # rsi, esi = 0
    # start jump in middle
    0x0000000000401174 <+128>:   jmp    0x401197 <phase_6+163>
    # continue afrter if (ecx > 1)
    # 开始遍历6个node
    0x0000000000401176 <+130>:   mov    0x8(%rdx),%rdx # rdx, edx points to modal
    0x000000000040117a <+134>:   add    $0x1,%eax
    0x000000000040117d <+137>:   cmp    %ecx,%eax
    0x000000000040117f <+139>:   jne    0x401176 <phase_6+130>
    0x0000000000401181 <+141>:   jmp    0x401188 <phase_6+148>

    # jump to if (ecx <= 1)
    0x0000000000401183 <+143>:   mov    $0x6032d0,%edx # head node
    0x0000000000401188 <+148>:   mov    %rdx,0x20(%rsp,%rsi,2) # 在rsp+0x20的位置初始化一个长度为12bytes的node指针数组
    0x000000000040118d <+153>:   add    $0x4,%rsi
    0x0000000000401191 <+157>:   cmp    $0x18,%rsi
    0x0000000000401195 <+161>:   je     0x4011ab <phase_6+183>

    # jump in the middle
    0x0000000000401197 <+163>:   mov    (%rsp,%rsi,1),%ecx 
    0x000000000040119a <+166>:   cmp    $0x1,%ecx
    # if (ecx <= 1)
    0x000000000040119d <+169>:   jle    0x401183 <phase_6+143>
    # if (ecx > 1) needs to iterate the node
    0x000000000040119f <+171>:   mov    $0x1,%eax # eax = 1
    0x00000000004011a4 <+176>:   mov    $0x6032d0,%edx # edx = node1
    0x00000000004011a9 <+181>:   jmp    0x401176 <phase_6+130>
    # Loop: 将node后面一位的值放到当前node的后面
    0x00000000004011ab <+183>:   mov    0x20(%rsp),%rbx # rbx node对应的数据
    0x00000000004011b0 <+188>:   lea    0x28(%rsp),%rax # rax: counter start from array [1]
    0x00000000004011b5 <+193>:   lea    0x50(%rsp),%rsi #rsi = null
    0x00000000004011ba <+198>:   mov    %rbx,%rcx # rcx = node[0]
    0x00000000004011bd <+201>:   mov    (%rax),%rdx # rdx = node[counter]
    0x00000000004011c0 <+204>:   mov    %rdx,0x8(%rcx) # rcx地址的后面被赋予当前node的value
    0x00000000004011c4 <+208>:   add    $0x8,%rax 
    0x00000000004011c8 <+212>:   cmp    %rsi,%rax
    0x00000000004011cb <+215>:   je     0x4011d2 <phase_6+222>
    0x00000000004011cd <+217>:   mov    %rdx,%rcx
    0x00000000004011d0 <+220>:   jmp    0x4011bd <phase_6+201>

    0x00000000004011d2 <+222>:   movq   $0x0,0x8(%rdx) # rdx = 0
    0x00000000004011da <+230>:   mov    $0x5,%ebp # ebp = 5
    # Loop start
    0x00000000004011df <+235>:   mov    0x8(%rbx),%rax # rax = node[1] 64 bit
    0x00000000004011e3 <+239>:   mov    (%rax),%eax # rax[low bits]  = node[0]
    0x00000000004011e5 <+241>:   cmp    %eax,(%rbx) # node[1] compare to *node[1];
    0x00000000004011e7 <+243>:   jge    0x4011ee <phase_6+250>
    0x00000000004011e9 <+245>:   callq  0x40143a <explode_bomb>

    0x00000000004011ee <+250>:   mov    0x8(%rbx),%rbx
    0x00000000004011f2 <+254>:   sub    $0x1,%ebp
    0x00000000004011f5 <+257>:   jne    0x4011df <phase_6+235>
```

.global .main
.text

msg_prompt:
   .asciz "Nhap chuoi 10 ky tu: " 
msg_result:
   .asciz "So tu co trong chuoi: "
newline:
   .asciz "\n"
bss:
   .lcomm buffer, 15
   .lcomm num_string, 12

.text
main:
   movl $4, %eax
   movl $1, %ebx
   movl $msg_prompt, %ecx
   movl $23, %edx
   int $0x80

   movl $3, %eax
   movl $0, %ebx
   movl $buffer, %ecx
   movl $15, %ebx
   int $0x80

   movl $buffer, %esi
   xorl %eax, %eax
   xorl %ebx, %ebx

count_loop:
    movb (%esi), %cl        
    cmpb $0, %cl            
    je finish_count
    cmpb $10, %cl           
    je finish_count

   cmpb $32, %cl
   je is_space
   cmpb $9, %cl
   je is_space

   cmpl $0, %ebx
   jne next_char

   movl $1, %ebx          
   incl %eax              
   jmp next_char

is_space:
    movl $0, %ebx

next_char:
   incl %esi
   jmp count_loop

finish_count:
   movl %eax, %ebx        
    movl $num_str + 11, %edi 
    movb $0, (%edi)

convert_loop:
    decl %edi
    movl $0, %edx
    movl %ebx, %eax
    movl $10, %ecx
    divl %ecx               
    addb $48, %dl           
    movb %dl, (%edi)
    movl %eax, %ebx
    cmpl $0, %ebx
    jne convert_loop

  movl %edi, %esi

   movl $4, %eax           
    movl $1, %ebx           
    movl $msg_result, %ecx
    movl $23, %edx          
    int $0x80

  movl %esi, %ecx        
  movl %ecx, %edx

find_len:
    movb (%edx), %al
    cmpb $0, %al
    je print_num
    incl %edx
    jmp find_len

print_num:
    subl %esi, %edx         
    movl $4, %eax           
    movl $1, %ebx           
    movl %esi, %ecx         
    int $0x80

  movl $4, %eax
    movl $1, %ebx
    movl $newline, %ecx
    movl $1, %edx
    int $0x80

   movl $1, %eax
    xorl %ebx, %ebx
    int $0x80

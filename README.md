section .data
    mensaje db "Fibonacci: ", 0
    buffer times 20 db 0

section .bss
    a resq 1
    b resq 1

section .text
    global _start

_start:
    mov qword [a], 0
    mov qword [b], 1
    mov rcx, 10

    mov rax, [a]
    push rax
    call print_num
    add rsp, 8

fibonacci_loop:
    mov rax, [b]
    mov rbx, [a]
    add rax, rbx
    mov [a], rbx
    mov [b], rax

    push rax
    call print_num
    add rsp, 8

    dec rcx
    jnz fibonacci_loop

end_program:
    mov rax, 60
    xor rdi, rdi
    syscall

print_num:
    push rax
    push rcx
    push rdx

    mov rsi, buffer
    mov rdi, 10
    xor rcx, rcx

convert_loop:
    xor rdx, rdx
    div rdi
    add dl, '0'
    push rdx
    inc rcx
    test rax, rax
    jnz convert_loop

write_digits:
    pop rdx
    mov [rsi + rcx - 1], dl
    loop write_digits

    mov rax, 1
    mov rdi, 1
    mov rdx, rcx
    mov rsi, buffer
    syscall

    pop rdx
    pop rcx
    pop rax
    ret

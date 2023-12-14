---
sticker: emoji//270f-fe0f
---
<div style="text-align: center; font-size:30;font-weight:bold ">시스템 프로그래밍 HW2 과제</div>

<div style="text-align: right">컴퓨터공학과 12201709 김승환</div>

# 1. `printf()`함수 코드 설명
`printf()`함수는 compile Time interpositioning과 Link-time interpositioning의 코드가 거의 비슷하므로 `printf()` 함수는 compile Time interpositioning을 기준으로 설명하되, 차이점에 대해서는 Interpositioning 수행 과정 및 결과 설명에서 따로 설명하도록 하겠습니다.

## 1-1. 구현 코드
![[Pasted image 20231214140947.png]]
![[Pasted image 20231214141000.png]]
![[Pasted image 20231214141012.png]]
![[Pasted image 20231214141025.png]]
![[Pasted image 20231214141044.png]]
![[Pasted image 20231214141208.png]]

## 1-2. 코드 설명
모든 입력 값에 대한 출력은 format 인식 기능이 없는 출력 함수인 putchar를 통해 출력되도록 하였습니다. 
또한 `printf()`의 원형이 `int printf(const char* format, ...)`이므로 `char*` 타입의 format과 가변인자로 이루어지는데 이 가변인자를 `stdarg.h` 헤더 파일을 포함하여 `va_list`, `va_start`, `va_arg`, `va_end` 매크로를 사용하였습니다.

### `myprintf()`
`printf()`를 대체하는 함수입니다. format 인식에 대해서는 각각의 함수를 따로 만들어 구현하였습니다. 
```c
int myprintf(const char* format, ...) {
    va_list args;
    va_start(args, format);

    print_str("[Interpositioning] ");

    while (*format) {
        if (*format == '%' && *(format + 1)) {
            format++;
            switch (*format) {
                case 'd':
                    print_int(va_arg(args, int));
                    break;
                case 'c':
                    putchar(va_arg(args, int));
                    break;
                case 's':
                    print_str(va_arg(args, char*));
                    break;
                case 'x':
                    print_hex(va_arg(args, unsigned int));
                    break;
                default:
                    putchar(*format);
                    break;
            }
        } else {
            putchar(*format);
        }

        format++;
    }

    va_end(args);

    return 0;
}

```
#### `va_list args`는 가변인자의 포인터 역할로서  여러 개의 인자를 가진 함수인 `myprintf()`에서 
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
#### `myprintf()`에서 사용된 주요 함수 설명
##### `va_list args`
`va_list`는 가변인자의 포인터 역할로서  여러 개의 인자를 가진 함수인 `myprintf()`에서 가변 인자에 접근할 수 있도록 해줍니다.
##### `va_start (args, format)`
`va_start`는 `va_list`를 초기화 하여 `va_list`인 `args`를 고정 파라미터인 format의 다음 번지로 설정해주는 역할을 합니다.
##### `va_arg (args, type)`
`va_arg`는 가변 인자 목록에서 다음 가변 인자의 값을 읽어옵니다. 첫 번째 인자로 va_list를 주고 두 번째 인자로 가져올 값의 데이터 타입을 가져옵니다.
##### `va_end (args)`
`va_end`는 `va_list`를 정리하고, 가변 인자 포인터인 `format`의 사용을 끝내주는 역할을 합니다.
##### `int putchar(int character)`
 `putchar`는 하나의 문자를 콘솔에 출력하는 C언어의 표준 라이브러리 함수입니다. 인자로 출력할 문자의 ASCII 코드 값을 받아 출력합니다.
#### `myprintf()` 흐름 설명
`va_list`와 `va_start`를 통해 가변인자의 포인터를 설정하고 이를 고정 인자인 `format`의 다음 번지로 설정해줍니다.
이후에 원래 `printf()`함수와 구분할 수 있도록 직접 정의한 `print_str()`함수를 통해 출력 값 앞에 "\[Interpositioning] " 이 올 수 있도록 합니다. `print_str()` 함수에 관해서는 뒤에서 설명하도록 하겠습니다.

while문에서는 `myprintf()`함수에서 매개변수로 ㅂ 작업을 실행합니다. 따라서 format이 null이 아닐 때(더 이상 출력할 값이 없을 때) 까지 while문을 실행할 수 있도록 while(\*format)으로 설정해줍니다.  또한 %가 나온 후에 뒤에 d,c,s,x가 나온다면 
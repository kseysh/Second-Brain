---
sticker: emoji//270f-fe0f
---
<div style="text-align: center; font-size:30;font-weight:bold ">시스템 프로그래밍 HW2 과제</div>

<div style="text-align: right">컴퓨터공학과 12201709 김승환</div>

# 1. `printf()`함수 코드 설명
`printf()`함수는 compile Time interpositioning과 Link-time interpositioning의 코드가 거의 비슷하므로 `printf()` 함수는 compile Time interpositioning을 기준으로 설명하되, 차이점에 대해서는 Interpositioning 수행 과정 및 결과 설명에서 따로 설명하도록 하겠습니다.

## 1-1. 구현 코드
### `printf.h`
![[Pasted image 20231214153451.png]]
### `myprintf.c`
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

while문에서는 `myprintf()`함수에서 매개변수로 받은 값들을 출력하는 작업을 실행합니다. 따라서 format이 null이 아닐 때(더 이상 출력할 값이 없을 때) 까지 while문을 실행할 수 있도록 while(\*format)으로 설정해줍니다.  
또한 %가 나온 후에 뒤에 d,c,s,x가 나온다면 format을 인식하고 각각에 대해 parameter를 적절한 형태로 출력해야하므로 if문을 사용하여 format이 %이고, 그 % 뒤에 값이 d, c, s, x 라면 각각의 format에 맞게 이를 출력하고 d,c,s,x가 아니라면 putchar를 통해 값을 출력할 수 있는 함수를 실행하도록 하였습니다.

%d, %c, %s, %x를 출력할 때는 가변인자에서 값을 가져와서 가변인자에 있는 값을 출력해야 합니다. 따라서 `va_arg`를 이용하여 가변 인자 목록에서 다음 가변 인자의 값을 가져와 각각의 format에 맞게 작성해놓은 `print_int()`, `print_str()`, `print_hex`또는 `putchar`를 실행하여 가변 인자의 값을 출력할 수 있도록 하였습니다. 
만약 읽어오는 값이 %로 시작되지 않는다면 `putchar`를 통해 값을 한 문자씩 출력하며, 출력 이후에는  `format++`를 이용하여 포인터 주소를 8byte씩 증가시켜 다음 char 포인터의 값을 읽어올 수 있도록 하였습니다. 
while문을 이용하여 모든 format 포인터를 다 읽어들이게 되면 va_end를 이용해 가변 인자 포인터인 `format`의 사용을 끝내줄 수 있도록 하였습니다.

#### `print_int(int value)`
```c
static void print_int(int value) {
    if (value == 0) {
        putchar('0');
        return;
    }

    char buffer[20];
    int i = 0;

    while (value > 0) {
        buffer[i++] = '0' + value % 10;
        value /= 10;
    }

    while (--i >= 0) {
        putchar(buffer[i]);
    }
}

```
`myprintf()`함수에서 `%d`가 포함되면 실행되는 함수입니다. 가변 인자의 값을 매개변수로 받아와서 매개변수의 값을 int 형태로 출력합니다.
int를 `putchar`함수를 이용해 하나씩 끊어 출력해야 하므로 매개변수인 value값을 10 단위로 끊어주고, '0'을 더해주어 char 타입으로 형변환을 하여 buffer에 넣어줍니다. 10단위로 끊어주게 되면 buffer에는 1의 자리의 수부터 큰 자리의 수까지 값이 거꾸로 저장되게 됩니다. 따라서 다시 while문을 이용하여 `while(--i >= 0)`을 이용해 buffer를 거꾸로 읽어들이면서 `putchar`를 이용해 int 값을 전부 출력해줍니다.

#### `print_str(int value)`
```c
static void print_str(const char *str) {
    while (*str) {
        putchar(*str);
        str++;
    }
}
```
`myprintf()`함수에서 `%s`가 포함되면 실행되는 함수입니다. 가변 인자의 값을 매개변수로 받아와서 매개변수의 값을 string 형태로 출력합니다.
string은 char의 배열과 같습니다. 따라서 char 포인터를 받아와서 char 포인터를 읽을 수 있을 때까지 읽어 `putchar`를 통해 출력하면 되는 간단한 함수입니다.

#### `print_hex(unsigned int value)`
```c
static void print_hex(unsigned int value) {
    print_str("0x");

    if (value == 0) {
        putchar('0');
        return;
    }

    char buffer[20];
    int i = 0;

    while (value > 0) {
        int digit = value % 16;
        buffer[i++] = (digit < 10) ? ('0' + digit) : ('a' + digit - 10);
        value /= 16;
    }

    while (--i >= 0) {
        putchar(buffer[i]);
    }
}
```
`myprintf()`함수에서 `%x`가 포함되면 실행되는 함수입니다. 가변 인자의 값을 매개변수로 받아와서 매개변수의 값을 16진수 형태로 출력합니다. int를 출력하는 `print_int()`함수와 비슷하지만, int는 10진수 단위이므로 10 단위로 끊어주었다면, `print_hex()`함수는 16진수 단위이므로 16 단위로 끊어주어야 합니다. 16진수 단위로 끊어준 이후에, 끊어준 값이 10보다 작다면 '0'을 더해주어 `print_int()`함수와 같은 방법으로 출력해주고, 10보다 크다면 10을 뺀 값에 'a'를 더해주어 char 타입으로 변경해준 후 출력해주어 16진수로 출력될 수 있도록 합니다. 이외에 앞에 `print_str()`을 이용하여 0x를 앞에 붙여주는 것 외에는 `print_int()`와 유사한 방식으로 코드가 실행됩니다.

#### Multiple argument를 처리한 방법
%가 나오고 그 뒤에 값이 d, c, s, x라면 각각 타입에 맞는 가변인자를 출력하는 함수에 매개변수로 `va_arg (args, type)`를 통해 값을 전달해주고, 출력하므로써 Mutiple argument도 single argument와 같은 방식으로 처리할 수 있었습니다.

# Interpositioning 수행 과정 및 결과 설명
## Compile-Time Interpositioning
Compile-Time Interpositioning은 소스 코드를 컴파일하는 단계에서 함수 호출을 변경하거나 새로운 함수를 주입하여 프로그램의 동작을 수정합니다.

`printf.h`파일을 생성하고 `#define`을 통해 `printf`함수를 `myprintf`함수로 interposition 하겠다는 것을 정의하고, `myprintf()`함수를 헤더파일에 정의해줍니다.
이를 통해 컴파일러는 printf를 사용할 때 `printf`함수 대신 `myprintf`를 사용하게 됩니다.
### `gcc -DCOMPILETIME -c myprintf.c` 
compile-time interpositioning을 위해서는 `myprintf.c` 파일의 relocatable object파일이 필요합니다. 따라서 `myprintf.c` 파일을 따로 compile하여 relocatable object file로 만들어줍니다.
-DCOMPILETIME 옵션은 COMPILETIME  매크로를 정의하고 이로 인해서 `#ifdef COMPILETIME` 과 `#endif` 안의 코드를 활성화 할 수 있도록 합니다. 따라서 이 코드는 컴파일 시간에만 포함되는 코드가 됩니다.
### `gcc -I. -o hw2C main.c myprintf.o`
위 명령어를 통해 `main.c`와 `myprintf.o`파일을 함께 컴파일하여 `hw2C` 실행파일을 만들게 됩니다.
`-I.`옵션은 include 파일의 검색 경로를 지정하는 옵션으로 `.`으로 작성하여 현재 디렉토리를 나타내어 현재 디렉토리를 include 파일의 검색 경로로 지정할 수 있도록 하였습니다.
-o 옵션은 컴파일된 실행파일의 이름을 지정하는 옵션으로 이 과제에서는 hw2C라는 실행파일의 이름으로 지정하였습니다.

여기서 `myprintf.c`를 따로 컴파일 한 이유는 `myprintf.c`파일을 `-DCOMPILETIME` 옵션으로 컴파일하여 compile-time interpositioning을 할 수 있도록 한 것입니다. 이렇게 하면 `main.c` 파일를 컴파일 할 때 `printf`함수를 `myprintf` 함수로 변경하여 `main.c`파일에서 사용하게 됩니다. 
## 수행 결과
![[Pasted image 20231214155653.png]]
## Link-Time Interpositioning
Link-Time Interpositioning은 소스코드를 relocatable object file로 만들고, 이를 linking하여 Executable object file로 만들 때 Interpositioning을 진행하여 함수 호출을 변경하거나 추가하여 프로그램의 동작을 수정하는 기술입니다.
### `gcc -DLINKTIME -c myprintf.c`
compile-time-interpositioning과 비슷하게 `myprintf.c`파일을 relocatable object 파일로 만들어줍니다. 
`-DLINKTIME`옵션을 통해 LINKTIME 매크로를 정의하고 이로 인해 `#ifdef LINKTIME` 과  `#endif` 안의 코드를 활성화 할 수 있도록 합니다. 따라서 이 코드는 linking 시간에만 포함되는 코드가 됩니다.  
### `gcc -c main.c`
main.c도 linking-time-interpositioning을 진행하기 위해 relocatable object 파일로 변경해줍니다.
### `gcc -Wl,--wrap,printf -o hw2L main.o myprintf.o`
두 개의 relocatable object file의 링킹을 진행해줍니다. 이 때 `-Wl,--wrap,printf` 옵션을 통해 링커에게 `printf` 함수를 감싸는 방식으로 동작하도록 지시합니다. 이를 통해 `__wrap_printf`함수가 `printf`를 감싸서 호출되게 합니다. 

이해를 위해 link time interpositioning에서의 printf함수 정의 부분을 보면, 아래와 같이 `__real_printf(const char* format, ...)`과
`__wrap_printf(const char* format, ...)`를 정의하였는데, `__real_printf(const char* format, ...)`함수는 원래 `printf` 함수의 기능을 대신 수행하는 함수이며, `__wrap_printf(const char* format, ...)`는 감싸진 형태로 동작하는 함수로 실제 동작을 변경하거나 보완합니다.
```c
int __real_printf(const char* format, ...);

int __wrap_printf(const char* format, ...) {
    ... 재정의한 printf 함수 구현 부분 ...
}
```
## 수행 결과
![[Pasted image 20231214155718.png]]
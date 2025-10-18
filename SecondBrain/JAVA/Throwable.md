## Checked Exception (반드시 처리해야 하는 예외)
Exception 상속
RuntimeException 상속 X
ex) IOException, SQLException
## Unchecked Exception (처리하지 않아도 되는 예외)
Exception 상속, RuntimeException 상속 X
RuntimeException 상속 O
Checked Exception은 컴파일러가 코드 검사 시점에서 예외 처리 여부를 확인한다.
ex) NPE, IllegalArgumentException
## Error
시스템 레벨에서 발생하는 심각한 문제
ex) OutOfMemoryError, StackOverflowError
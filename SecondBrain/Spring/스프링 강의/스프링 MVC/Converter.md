타입 컨버터를 사용하려면 Converter 인터페이스를 사용한다.
## ConversionService
스프링은 개별 컨버터를 모아두고 편리하게 묶어서 사용할 수 있는 기능을 제공한다.
스프링의 DefaultConversionService는 아래 두 인터페이스를 구현한다
- ConversionService - 컨버터 사용에 초점
- ConverterRegistry - 컨버터 등록에 초점
스프링은 내부에서 ConversionService를 제공하므로 WebMvcConfigurer가 제공하는 addFormatters()를 사용해서 추가하고 싶은 컨버터를 등록하면 된다.

이를 통해 @RequestParam에서 String parameter 값을 객체 타입으로 쉽게 변환할 수 있다. 
ex) `?ipPort=127.0.0.1:8080`, `@RequestParam IpPort ipPort`

=> @RequestParam을 처리하는 RequestParamArgumentResolver가 ConversionService를 이용해서 타입을 변환하기 때문
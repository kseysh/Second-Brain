
## Spring 동작환경
![[Pasted image 20230624223254.png]]
model은 
```
@GetMapping("hello-mvc")  
public String helloMvc(@RequestParam("name") String name, Model model){  
    model.addAttribute("name",name);  
    return "hello-template";  
}
```
위처럼 코드를 작성하게 되면 get방식으로 name=value 로 값을 넣어주어야 hello-template에서 값을 받을 수 있다. (default = true)이기 때문, (default=false)로 바꾸어주면 값을 넣어주지 않아도 hello-template로 렌더링이 된다.



view는 화면에 관련된 일만,
controller는 비즈니스 로직이나 뒷단에 관련된 일만 처리한다.


![[Pasted image 20230624232946.png]]
@ResponseBody는 return된 곳으로 rendering하는 것이 아닌 http body부에 바로 값을 넣어 보내주는 기능을 한다. 따라서 ==json 형식==으로 값을 보내주게 된다.
전달된 값이 문자이면 StringConverter가 동작하며, 전달된 값이 객체이면 JsonConverter가 동작한다.


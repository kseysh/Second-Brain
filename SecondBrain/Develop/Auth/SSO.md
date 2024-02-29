# SSO (Single Sign On)
단일 로그인, 한 번의 로그인으로 여러개의 애플리케이션들을 이용할 수 있게 해주는 서비스
ex) 구글 계정 하나로, GMail,  Google Drive를 사용함

## SSO의 작동원리

**1. Delegation Model**  
![image](https://user-images.githubusercontent.com/15958325/136686444-e8e9daf9-29cf-48a8-b50a-fe07941fcd06.png)  
권한을 얻으려는 서비스의 인증방식을 변경하기 어려울 때 많이 사용되는 방식입니다.  
**대상 서비스의 인증방식을 변경하지 않고, 사용자의 인증 정보를 Agent가 관리하여 대신 로그인 해주는 방식입니다.**  
만약 Target Service에 로그인하기 위한 정보가 admin/passwd라면, Agent가 해당 정보를 가지고 있고 로그인할때 유저대신 대상 서비스로 정보를 전달해 로그인시켜줍니다.

**2. Propagation Model**  
![image](https://user-images.githubusercontent.com/15958325/136687288-b8b898bc-d6b2-4e93-a2e2-b15e9616fb5c.png)  
통합 인증을 수행하는 곳에서 인증을 받아 **“인증 토큰”이라는 것을 발급**받게됩니다.  
유저가 서비스에 접근할 때 발급받은 인증토큰을 서비스에 같이 전달하게 되고, 서비스는 토큰정보를 통해 사용자를 인식할 수 있게 하는 방식입니다.

서비스의 특성에 따라서 두가지 모델을 혼용해서 전체 시스템의 SSO를 구성하는 것이 일반적입니다.
> Makers에서는 인증 토큰을 이용한 Propagation Model을 사용해 여러 서비스를 인증토큰을 이용해 인증을 진행한다!

### 구현은 어떻게?

SSO 시스템은 여러 표준, 프레임워크에 의해서 구현될 수 있으며 대표적으로 SAML, OAuth/ OIDC 세가지 방식이 대표적이다.
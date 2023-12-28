> 33th DO SOPT APPJAM :: **HMH** <br>
## 🌼 서비스 소개

---
### 🔥 이름
#### HMH ( 사람들이 의식적으로 스마트폰을 이용할 수 있도록 )

### 🔥 소개
범죄 이력이 있는 출소자라면, 갱생되지 못할 것이라는 당연한 사회적 인식 속에서 우리는 그들을 사회에서 함께 교화될 수 없는 존재로 단정짓는 관념을 가지고 있다.<br/>
이러한 사회적 낙인은 범죄자들이 방치되며 재범으로 이어지는 악순환을 반복시킨다. <br/>
따라서 우리는 이들의 변화 의지를 일으켜 일상을 회복하도록 돕고, 더 나은 사회를 만드는 것에 이바지하려 한다.


<br/>

## 📌 Tech Stacks

---
- **Language** : Java (jdk-17)


- **Web application Framework** : Spring boot (3.2.0), Spring Data JPA


- **DataBase** : MySql (8.1.0)


- **Cloud/Infra** : Aws EC2, RDS, code deploy


- **web server** : Tomcat


- **Collaborative Tool** : Github


- **Version Control** : Git



<br/>


## 🖤  Developers

---
| 임주민 | 김승환 |
|:----:|:----:|
| <img width="300" alt="image" src="https://github.com/SOPT-33-iOS-Team-1/SOPKATHON_33-Server/assets/86935274/aa6b8ba8-f49c-45ad-bffd-69faa6703ca1"> | <img width="300" height="330" alt="image" src="https://github.com/SOPT-33-iOS-Team-1/SOPKATHON_33-Server/assets/86935274/b1308faa-06cb-4818-878e-aeb8e17ac14c">| 
| [jumining](https://github.com/jumining) | [kseysh](https://github.com/kseysh) |

<br/>

## 🙋🏻‍♀️ 역할 분담

---
<div markdown="1">  
 
  - `주민🤖` & `승환🍣`
  
| 기능명 | 담당자 | 완료 여부 |
| :-----: | :---: | :---: |
| 프로젝트 세팅 | `승환🤖` | 완료 |
| EC2 세팅 | `승환🤖` | 완료 |
| RDS 세팅 | `승환🤖` | 완료 |
| CI/CD 세팅 | `동규🍣` | 완료 |
| README 작성 | `동규🍣` | 완료 |
| DB 설계 | `승환🤖` `동규🍣` | 완료 |
| API 명세서 작성 | `승환🤖` `동규🍣` | 완료 |
| API 개발 | `승환🤖` `동규🍣` | 완료 |

</div>


### ⭐️ 서버는 하나 ⭐️
<img width="550" height="400" alt="team_image" src="">


<br/>

## ✅ Convention

---
### 🚀Convention
- [💻 협업 컨벤션](https://www.notion.so/msmmx/6fa22000670d4cf783559f7808c01d1a?pvs=4)
<br>
### 🚀 Branch Strategy
- [💻 브랜치 전략](https://www.notion.so/Branch-Strategy-19f824ec44124f478f38bed58ae517a2?pvs=4)

<br/>

## 💾 ERD

---
> 자세한 테이블 정보는 다음 노션 페이지에 정리해둘 예정입니다! (Comming Soon...)
- [📝 Database](https://www.notion.so/DB-Schema-Description-d0c91546b233435e908a1a2ab315295b?pvs=4)

<img width="884" alt="erd" src="https://github.com/SOPT-33-iOS-Team-1/SOPKATHON_33-Server/assets/69035864/a06f2a66-7e87-4321-b31b-ff450de1553b">

<br>

## 🖇 Api 명세서

[🍀 API 명세서](https://www.notion.so/342f6504080f4113afb4a89af506c2d7?v=a36becbc1f0f4239a802053bf782195c&pvs=4)

<br>

## ⚙️ Architecture

---
<img width="884" alt="architecture_image" src="https://github.com/SOPT-33-iOS-Team-1/SOPKATHON_33-Server/assets/69035864/9572b43a-18e2-4578-b496-2fd54f1dc476">

<br>

## 🗂️ Project Foldering

```
├── 📁 .github
│   ├── 📁 ISSUE_TEMPLATE
|   └── 📁 workflows
|   └── pull_request_template.md
│
├── appspec.yml
|  
├── HELP.md
|
├── build.gradle
├── gradlew
├── gradlew.bat
|
├── README.md
|
├── 📁 script
│   ├── start.sh
│   └── stop.sh
└── 📁 src
    └── 📁 main
        ├── 📁 java
        │   └── 📁 org
        │       └── 📁 sopt
        │           └── 📁 sopkerton
        │               ├── SopkertonApplication.java
        │               └── 📁 common
        │                   ├── 📁 exception
        │                   │   ├── GlobalError.java
        │                   │   ├── GlobalException.java
        │                   │   ├── GlobalSuccess.java
        │                   │   └── 📁 base
        │                   │       ├── ErrorBase.java
        │                   │       ├── ExceptionBase.java
        │                   │       ├── RootEnum.java
        │                   │       └── SuccessBase.java
        │                   └── 📁 response
        │                       ├── ApiResponse.java
        │                       └── CommonControllerAdvice.java
        └── 📁 resources
            └── application.yml
```

<br>

## ♻️ 실행 방법
clone 받아서 application run하고 localhost:8080으로 api 명세서대로 api를 호출한다.




```bash
The deployment failed because no instances were found for your deployment group. Check your deployment group settings to make sure the tags for your Amazon EC2 instances or Auto Scaling groups correctly identify the instances you want to deploy to, and then try again.
```

-> 조건에 **일치하는 EC2가 없을 때** 나오는 에러

배포그룹 생성시 codedeploy를 이용할 인스턴스를 Key-Value로 등록하게된다.

![[Pasted image 20240417183107.png]]

보통 **Name**키를 이용하여 **EC2**이름으로 식별하는데, 이 부분을 다시한번 확인해서 **codedeploy**를 사용할 인스턴스를 명시해주면 된다.

ec2의 이름을 임의로 변경하게 되면, codedeploy가 name으로 ec2를 식별하지 못해 ec2 not found error가 발생하게 된다.
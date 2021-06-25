# 인스턴스 생성 후 `고급 세부 정보`

~~~
#!/bin/bash
sudo yum update -y
sudo yum install -y java-1.8.0-openjdk-devel.x86_64
sudo rm /etc/localtime
sudo ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk
cd /usr/local
sudo wget http://mirror.apache-kr.org/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
sudo tar -xvf apache-maven-3.6.3-bin.tar.gz
sudo ln -s apache-maven-3.6.3 maven
sudo vi ~/.bachrc 
export MAVEN_HOME=/usr/local/maven
PATH=$PATH:$HOME/bin:$MAVEN_HOME/bin
export PATH
source ~/.bash_profile
sudo yum install git -y
git clone https://github.com/alalstjr/aws-spring-test.git
cd ~/aws-spring-test
mvn package
sudo java -jar target/demo-0.0.1-SNAPSHOT.jar &
fi
~~~

~~~
ps -ef | grep jar
kill 15235
~~~

이렇게 2개 생성

환경변수를 영구적으로 적용하는 방법
https://sosobaba.tistory.com/279
환견변수 설정방법
https://www.hanumoka.net/2018/04/30/centOs-20180430-centos-install-jdk/

# Load Balancer 설정

- https://ap-northeast-2.console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LoadBalancers:
- Load Balancer 생성
    - Application Load Balancer
        - 1. Load Balancer 구성
            - 이름 : lab-web-alb
            - 체계 : 웹용의 인터널인지 내부용 인터널인지 설정
            - 리스너 : ELB 가 통과하는 트래픽의 포트번호를 정의
            - 가용 영역 : 2a, 2c
            - 태그
                - Key : Name
                - Value : lab-web-alb
        - 3. 보안 그룹 구성 : ALB 용 보안그룹을 따로 생성합니다.
        - 4. 라우팅 구성 : 어떤 EC2 를 등록하는지 설정합니다.
            - 이름 : lab-web-alb-target
        - 5. 대상 등록 : EC2 를 타겟 그룹으로 넣습니다.
            - 방금전에 생성한 EC2 [ 2a, 2c ] 를 선택합니다.
    - 생성된 ALB 의 DNS 이름
        - EC2 로 바로 접근해서 웹 페이지를 보느것이 아니라 ALB 의 DNS 를 활용해서 생성한 2개의 EC2 웹 화면을 보도록 분산시키겠습니다.
        - 생성된 ALB 도메인 네임을 검색
            - http://lab-web-alb-1384992291.ap-northeast-2.elb.amazonaws.com/
                - 새로고침 할때마다 [ 2a, 2c ] 번갈아 가며 서버가 교체됩니다. 정상적으로 분산되어 있는것을 확인했습니다.
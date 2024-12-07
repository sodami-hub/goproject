# ch07-유닉스 도메인 소켓
- 모든 네트워크 프로그래밍이 노드 간에서만 발생하는 것은 아니다. 때때로 애플리케이션은 동일한 노드에 존재하는 데이터베이스 등의 서비스와 통신해야 할 필요가 있다.
- 애플리케이션에서 동일한 시스템 내 동작하는 데이터베이스에 접속하는 한 가지 방법으로 노드 자체의 IP 주소, 혹은 일반적으로 127.0.0.1로 사용되는 localhost  주소와 데이터베이스의 포트 번호를 사용하는 방법이 있다. 또 다른 방법으로는 유닉스 도메인 소켓을 사용하는 방법이 있다. 유닉스 도메인 소켓은 파일 시스템을 이용하여 패킷의 목적지 주소를 결정하는 통신 방법으로, 동일한 노드 내에 동작하는 서비스 간에 IPC(인터 프로세스 커뮤니케이션)라는 절차를 통해 데이터를 주고받을 수 있도록 해준다.

- 유닉스 도메인 소켓에 읽기와 쓰기를 제어하는 방법
- Go 언어에서 net 패키지를 사용하여 세 종류의 유닉스 도메인 소켓을 알아보고 각 종류의 소켓을 이용하여 간단한 에코 서버를 만든다.
- 유닉스 도메인 소켓을 이용하여 사용자와 그룹 아이디 정보로 클라이언트를 인증하는 서비스를 작성한다.


### 유닉스 도메인 소켓이란
- 네트워크 소켓이란 IP 주소와 포트 번호로 정의한다. 동일한 IP를 가진 동일한 노드 내에서 여러 어플리케이션에 대한 접근을 서로 다른 포트를 통해서 가능하게 해준다. IP 주소만 있고 포트 번호가 없다면 대기업에 한대의 전화기만 존재하는 것과 같은 상황일 것이다. 
- 유닉스 도메인 소켓은 소켓 주소 정의 원칙을 파일 시스템에 적용한다. 각 유닉스 도메인 소켓에는 파일 시스템 내에 연관된 파일이 존재하며, 그 파일은 네트워크 소켓의 IP 주소와 포트 번호에 대응한다. 해당 파일에 네트워크 연결을 맺고 데이터를 읽고 씀으로써 해당 소켓에서 수신 대기 중인 서비스와 통신할 수 있다. 마찬가지로 파일 시스템의 소유 권한과 접근 권한을 이용하여 소켓에 읽고 쓴느 접근을 제어할 수 있다.
- 유닉스 도메인 소켓은 운영체제의 네트워크 스택을 지나지 않기 때문에 트래픽 라우팅에 대한 오버헤드가 존재하지 않아 효울적이다. 동일한 이유로 패킷 파편화나 패킷 순서에 대해 걱정할 필요가 없다. 로컬 서비스를 사용할 때 네트워크 스택을 사용한다면, 중요한 보안상 이점과 성능 향상의 효과를 무시하게 된다.
- 주의할 점은 다른 노드간의 통신에는 사용할 수 없다.


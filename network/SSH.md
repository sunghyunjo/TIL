# SSH
### Secure Shell Protocol

- 네트워크 프로토콜 중 하나로 컴퓨터와 컴퓨터가 인터넷과 같은 Public Network를 통해 서로 통신할 때 보안적으로 안전하게 통신하기 위한 프로토콜
- 22 port 사용

## 인증 방식

 ![ssh인증 방식](https://swalloow.github.io/assets/images/ssh-key-auth-flow.png)

Public key와 Private Key를 사용하는 비대칭 암호방식 사용.

- 개인만 알 수 있는 private key를 외부에 유출하지 않은 채로 가지고 있고, 네트워크를 통해 전달한 Public key를 매칭하여 인증

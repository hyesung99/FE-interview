## Q1. 세션 기반 인증에 대해 설명해주세요.

사용자의 요청에 대해 서버가 허용해 줄 것인지 판단하는 인가(Authorization)의 한 종류이다.

세션 기반 인증은 서버가 사용자에게 세션 ID를 발급하고, 이 세션 ID를 쿠키를 통해 사용자의 브라우저에 저장한다. 이후 사용자의 요청이 있을 때마다 세션 ID를 확인하여 세션에 저장된 정보를 확인하고, 이를 기반으로 사용자의 요청을 처리한다.

## Q2. 세션 기반 인증의 문제점은 무엇인가요?

세션 기반 방식은 서버가 사용자의 세션을 저장하고 관리해야 한다. 서버는 이 세션을 저장하기 위한 저장소가 필요하며, 이를 위해 메모리, 데이터베이스를 사용하는데, 만약 많은 요청이 들어오게 되면 메모리에 부하가 걸려 서버가 다운될 수 있다. 즉, 서버는 세션에 대한 정보를 잃게된다.

또 다른 문제점은 서버가 여러 대일 경우, 사용자의 요청이 다른 서버로 전달되었을 때, 세션 정보가 유지되지 않는다. 이를 해결하기 위해 로드 밸런싱을 사용하여 사용자의 요청을 특정 서버로 보내는 방식을 사용할 수 있지만, 이는 까다롭고 복잡하다.

## Q3. 로드 발란싱이란 무엇인가요?

로드 밸런싱은 여러 대의 서버에게 들어오는 네트워크 트래픽을 고르게 분산시켜서 각 서버의 부하를 분산시키는 기술이다.

## Q4. 세션 기반 인증의 문제점을 해결하기 위한 방법은 무엇이 있나요?

세션 기반 인증의 문제점을 해결하기 위한 방법으로 토큰 기반 인증이 있다. 토큰 기반 인증은 세션 기반 인증과 달리 서버가 사용자의 세션을 저장하고 관리할 필요가 없다. 사용자의 정보를 토큰에 담아서 사용자에게 전달하고, 이 토큰을 이용하여 사용자의 요청을 처리한다. 이를 통해 서버의 부하를 줄일 수 있으며, 서버가 여러 대일 경우에도 사용자의 요청이 다른 서버로 전달되어도 세션 정보를 유지할 수 있다.

토큰 기반 인증의 대표적인 기술로 JWT(JSON Web Token)가 있다.

## Q5. JWT란 무엇인가요?

JWT란 JSON Web Token의 약자로, JSON 객체를 사용하여 사용자에 대한 정보를 저장하는 웹 토큰이다.

## Q6. JWT의 구조는 어떻게 되나요?

JWT는 세 부분으로 구성되어 있다. 각 부분은 Base64로 인코딩되어 있다.

1. 헤더
   헤더에는 토큰의 타입과 해싱 알고리즘을 지정한다. ex) `{"alg": "HS256", "typ": "JWT"}`와 같이 표현된다.
2. 페이로드
   페이로드에는 토큰에 담을 정보가 포함된다. ex) 사용자의 ID, 이름 등
3. 서명
   서명은 헤더에서 지정한 알고리즘을 통해 헤더와 페이로드의 인코딩 값과 서버에 저장된 비밀키를 이용하여 생성된다.

## Q7. JWT는 어떻게 사용자를 인증하나요?

사용자가 로그인을 하게 되면, 서버는 사용자에게 JWT를 발급하여 전달한다. 이후 사용자의 요청이 있을 때마다, 사용자는 이 JWT를 헤더에 담아서 서버에 전달한다. 서버는 이 JWT를 설정된 알고리즘을 통해 서명 값을 생성하고, 전달받은 JWT의 서명 값과 비교하여 유효성을 검증한다. 이를 통해 사용자의 요청을 처리한다.

## Q8. JWT의 단점은 무엇인가요?

JWT는 기본적으로 사용자의 정보를 저장하지 않기 때문에 한 번 발급된 토큰에 대해 정보를 변경할 수 없다. 토큰이 탈취되었을 때 해당 토큰을 만료시킬 수 없어 보안에 취약하다.

## Q9. JWT의 단점을 해결하기 위한 방법은 무엇이 있나요?

Refresh Token을 사용하여 해결할 수 있다. Refresh Token은 Access Token이 만료되었을 때, 새로운 Access Token을 발급받기 위한 토큰이다.

Access Token의 만료 시간을 짧게 설정하고, Refresh Token을 사용하여 Access Token을 갱신하는 방식을 사용하면, 만약 Access Token이 탈취되었을 때, 해당 토큰이 만료되어 더 이상 사용할 수 없게 된다.

하지만 탈취된 Access Token이 만료되기 전까지는 사용할 수 있기 때문에, 완벽한 해결 방안은 아니다.

## Q10. 그런 Refresh Token이 탈취 당했을 경우는 어떻게 해결하나요?

RTR (Refresh Token Rotation)이라는 방법을 도입해 볼 수 있다. RTR은 Refresh Token을 한번만 사용할 수 있도록 만드는 방법이다.

Refresh Token을 사용하여 새로운 Access Token을 발급받으면, 이전에 사용한 Refresh Token은 더 이상 사용할 수 없게 되고 새로운 Refresh Token이 발급된다.

하지만 이 방법 또한 한계가 존재한다. 사용하지 않은 Refresh Token을 탈취하거나, Access Token을 지속적으로 탈취하면 RTR을 통한 보안이 무효화된다.

## Q11. 그럼 JWT 토큰 방식의 한계를 어떻게 극복할 수 있나요?

JWT 토큰 방식의 한계점은 결국 JWT 장점이기도 한 stateless한 특성 때문에 발생한다. 따라서 이러한 한계점을 극복하기 위해서는 stateful한 세션 방식을 사용하는 것이다.

stateless의 장점을 가져가면서 사용자 인가를 하기 위해선 결국 클라이언트에서 XSS, CSRF 등의 공격을 방어할 수 있는 방법을 찾아야 한다.

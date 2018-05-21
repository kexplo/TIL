WIP

# DNS over HTTPS (DoH)

## Simple DNSCrypt

https://simplednscrypt.org/

## Intra

Android P에서 DoH를 지원하려는 [움직임](https://android-developers.googleblog.com/2018/04/dns-over-tls-support-in-android-p.html)이 보이지만, 현재 최신버전의 Android Oreo에서는 쓸 수 없다.

[Infra](https://play.google.com/store/apps/details?id=app.intra&hl=en_US)는 Android에서 DoH를 사용할 수 있게 해주는 앱이다.

동작 방식은 내부적으로 VPN을 맺어서 모든 연결에 대해 DoH를 적용하는 방식인 것 같다.

Infra앱에서는 Cloudflare와 Google, 두 가지의 DoH 서버를 설정할 수 있다.

보통은 Cloudflare를 추천한다. 로깅이슈가 있어서 그렇다고 한다. 

로깅? 궁금해서 찾아보니 theregister라는 곳의 [기사](https://www.theregister.co.uk/2018/04/03/cloudflare_dns_privacy/)에서 Cloudflare는 로그를 24-48시간 보관하지만, Google은 장기간 보관한다고 한다. 아무리 Google이라지만 나의 요청 기록이 없어지지 않고 장기간 남는다는 것은 꺼림직할 것 같다.

> // ref: https://www.theregister.co.uk/2018/04/03/cloudflare_dns_privacy/
>
> In this Cloudflare's venture is similar to Google's Public DNS (8.8.8.8), which claims that it keeps some data for just 24 to 48 hours. Google, however, keeps other non-personally identifiable information for longer periods.


## DNSCloak

iOS에서는 [DNSCloak](https://itunes.apple.com/kr/app/dnscloak-dnscrypt-doh-client/id1330471557?mt=8) 이라는 앱을 사용할 수 있다.

Infra와 마찬가지로 내부적으로 VPN을 맺어서 DoH를 적용하는 방식인 것 같다.


## Firefox

Firefox는 자체적인 DoH 기능을 가지고 있다. Firefox 60부터 사용할 수 있다. 이 글을 작성하는 시점에서는 Android와 Windows용 Firefox가 60 버전 이상인 것을 확인했다.

주소창에 `about:config`를 입력해 고흡 관경 설정 페이지로 이동한다. 

상단의 `검색`창에 `network.trr`을 입력해서 `network.trr`로 시작하는 설정들을 모아 볼 수 있게한다.

그리고 아래 항목의 값을 설정한다.

- `network.trr.bootstrapAddress`: 1.1.1.1
- `network.trr.mode`: 2
- `network.trr.uri`: https://mozilla.cloudflare-dns.com/dns-query

각 설정값의 의미를 설명해보자면 이렇다.
관심이 없다면 넘어가도 된다.

참고로 TRR은 Trusted Recursive Resolver의 약자.

`network.trr.uri`: 사용할 DoH 서버의 URI를 설정한다. 반드시 HTTPS 주소여야 한다.

`network.trr.bootstrapAddress`: `network.trr.uri`에서 설정한 호스트의 IP 주소를 설정한다. 이 값을 설정하면 시스템에서 호스트 IP를 얻어내는 것을 무시하고 설정한 값을 사용하게 된다. Cloudflare DoH를 설정했기 때문에 `1.1.1.1`을 사용했다.

`network.trr.mode`: 값에 따라 DoH의 동작을 설정한다.

- 0 - (기본값) DoH 기능을 끈다
- 1 - 시스템 기본 방식과 DoH에 동시에 요청을 보낸다. 빨리 응답이 오는 쪽을 사용
- 2 - DoH를 기본으료 사용하고, 응답이 실패할 경우에 시스템 기본 방식을 사용
- 3 - DoH만 사용한다. 시스템 기본 방식을 사용하지 않는다
- 4 - 타이밍 측정을 위해 DoH와 시스템 기본 방식을 병렬로 실행한다. 하지만 시스템 기본 방식의 응답만 사용한다
- 5 - 0과 같다. 0은 기본값, 5는 선택으로 인한 값을 표시하기 위해 사용한다

DoH만 사용하고 싶다면 `network.trr.mode`를 `3`으로 설정하면 된다.

이제 별다른 도구 없이 Firefox에서 자체적으로 DoH를 적용할 수 있다.

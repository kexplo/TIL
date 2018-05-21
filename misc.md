# 잡다

짧은 여러가지 주제를 모아 둠

## Table of Contents

* [GPKI 인증서 사태](#GPKI-인증서-사태)
* [회사 MITM에 대처하는 자세](#회사-MITM에-대처하는-자세)

## GPKI 인증서 사태

얼마전 GPKI 인증서 Wildcard 도메인에 대해 발급되었다는 충격적인 소식을 접했다.

- 참고1: [보안뉴스 기사](http://www.boannews.com/media/view.asp?idx=68221)
- 참고2: [트위터 쓰레드]( https://twitter.com/_Hoto_Cocoa_/status/981538520064905221)

트위터 쓰레드를 보면 내용이 가관이다. `*.or.kr`, `*.co.kr`, ... 등의 도메인에 대해 승인 되어있다. 심지어 `192.168`로 시작하는 내부 아이피에 대한 등록도 있다고 한다.

해당 트위터 쓰레드의 하단에는 후속 조치에 대한 내용도 있다.

후속 조치를 보면, [문제된 인증서를 폐기할 예정](https://twitter.com/_Hoto_Cocoa_/status/982479767801741312)에 있다고 하고, 최신 업데이트가 적용된 Windows에서 [GPKI 인증서가 신뢰되지 않게](https://twitter.com/_Hoto_Cocoa_/status/984800333095288832) 변경되었다고 한다.

원래는 Windows에서 GPKI 인증서가 신뢰 상태였기 때문에 문제가 되었다.

Firefox 처럼 OS의 인증서를 무조건 신뢰하지 않는 브라우저라면 괜찮겠지만, IE, Edge, Chrome 등의 브라우저를 쓰면 브라우저가 인증서를 신뢰하게 된다.

혹시 GPKI 인증서를 무력화 시키는 방법이 필요할 수도 있으니, 인터넷에 떠돌던 GPKI 인증서를 신뢰하지 않게 만드는 방법을 링크해 둔다.

https://twitter.com/hibiyasleep/status/981559511595999233

아래 순서대로 하면 된다:

`컴퓨터 인증서 관리` -> `신뢰할 수 있는 루트 인증 기관` -> `인증서` -> `GPKIRootCA1`의 속성 -> `이 인증서의 모든 용도를 사용 안 함`


## 회사 MITM에 대처하는 자세

회사에서 MITM을 걸고 있기 때문에, `curl` 등의 명령어는 전부 실패한다.

```bash
$ curl https://google.com
curl: (60) SSL certificate problem: self signed certificate in certificate chain
More details here: http://curl.haxx.se/docs/sslcerts.html
curl performs SSL certificate verification by default, using a "bundle"
 of Certificate Authority (CA) public keys (CA certs). If the default
 bundle file isn't adequate, you can specify an alternate file
 using the --cacert option.
If this HTTPS server uses a certificate signed by a CA represented in
 the bundle, the certificate verification probably failed due to a
 problem with the certificate (it might be expired, or the name might
 not match the domain name in the URL).
If you'd like to turn off curl's verification of the certificate, use
 the -k (or --insecure) option.
 ```

 `curl`이 MITM 인증서를 신뢰하지 않기 때문에 실패한다.

 `curl`에 `-k` 옵션을 사용하는 방법도 있겠지만, 다른 스크립트 등에서 `curl`을 사용하는 경우 `-k` 옵션을 주지 못하기 때문에 실패한다.

 이를 해결하기 위해서 `CURL_CA_BUNDLE` 환경 변수에 회사에서 제공하는 MITM 인증서 경로를 설정하면, `curl`은 무사히 넘어간다.

 아니면 `curl -v https://google.com` 명령어를 통해 `curl`이 시스템 인증서를 읽어오는 경로를 확인하고, 경로에 인증서를 넣어주는 방법도 있다.

 ```bash
 $ curl -v https://google.com
 * Rebuilt URL to: https://google.com/
*   Trying 216.58.197.142...
* Connected to google.com (216.58.197.142) port 443 (#0)
* found 148 certificates in /etc/ssl/certs/ca-certificates.crt
* found 593 certificates in /etc/ssl/certs
...
```

하지만 시스템 전역적으로 허용하긴 싫으니 넘어간다.

### terraform

이제 `terraform`을 보자

`terraform init`을 하면 처참하게 실패한다.

```bash
Initializing provider plugins...
- Checking for available provider plugins on https://releases.hashicorp.com...

Error installing provider "archive": Get https://releases.hashicorp.com/terraform-provider-archive/: x509: certificate signed by unknown authority.

Terraform analyses the configuration and state and automatically downloads
plugins for the providers used. However, when attempting to download this
plugin an unexpected error occured.

This may be caused if for some reason Terraform is unable to reach the
plugin repository. The repository may be unreachable if access is blocked
by a firewall.

If automatic installation is not possible or desirable in your environment,
you may alternatively manually install plugins by downloading a suitable
distribution package and placing the plugin's executable file in the
following directory:
    terraform.d/plugins/linux_amd64
```

오류를 봐선 `curl` 등을 사용하는게 아닌 것 같다

코드를 보자

https://github.com/golang/go/blob/master/src/crypto/x509/root_linux.go

```go
// Copyright 2015 The Go Authors. All rights reserved.
// Use of this source code is governed by a BSD-style
// license that can be found in the LICENSE file.

package x509

// Possible certificate files; stop after finding one.
var certFiles = []string{
	"/etc/ssl/certs/ca-certificates.crt",                // Debian/Ubuntu/Gentoo etc.
	"/etc/pki/tls/certs/ca-bundle.crt",                  // Fedora/RHEL 6
	"/etc/ssl/ca-bundle.pem",                            // OpenSUSE
	"/etc/pki/tls/cacert.pem",                           // OpenELEC
	"/etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem", // CentOS/RHEL 7

```

시스템의 인증서 파일을 직접 읽어서 사용하고 있다.

결국 회사의 MITM 인증서를 시스템에 전역적으로 깔 수 밖에 없겠다.

Ubuntu 같은 경우는 `/etc/ssl/certs`에 인증서의 심볼릭 링크를 만들어 주고, `update-ca-certificates` 명령을 실행해 주면 된다.

### npm

이제 `npm`을 실행해 보자

```bash
npm ERR! node v6.11.4
npm ERR! npm  v3.10.10
npm ERR! code SELF_SIGNED_CERT_IN_CHAIN

npm ERR! self signed certificate in certificate chain
npm ERR!
npm ERR! If you need help, you may report this error at:
npm ERR!     <https://github.com/npm/npm/issues>
```

시스템에 인증서를 설치했으나.. 처참하게 실패한다

이쯤되니 더 파보기 귀찮다. 내 생산성을 처참히 갉아먹는다. 그냥 SSL 옵션을 하나 끄도록 하자.

```bash
$ npm config set strict-ssl false
```

### git

이제 `git`을 실행해 볼까?

```bash
fatal: unable to access 'https://xxxxxxxxxxxxxxxxxxxxx.git/': SSL certificate problem: self signed certificate in certificate chain
error: Could not fetch origin
```

......

`git`의 SSL 옵션도 끄자

```bash
$ git config -g http.sslVerify "false"
```

### WSL

`WSL(Windows Subsystem for Linux)` 환경에서는 또 다른 문제가 있다. Windows 쪽에 설치한 MITM 인증서를 읽어오지 않는 것 같다.

위에서 언급했던 Ubuntu 설정은 (`/etc/ssl/certs`에 넣는..) 통하지 않아 보인다.

대신 `/usr/local/share/ca-certificates`에 인증서를 넣고, `sudo dpkg-reconfigure ca-certificates`를 설정해주면 된다.


### docker

`docker`도 사용해 볼까?

```bash
$ docker pull gcr.io/etcd-development/etcd:v3.2.9
Error response from daemon: Get https://gcr.io/v1/_ping: x509: certificate signed by unknown authority
```

인증서 에러가 난다.

이 경우엔 insecure registry로 등록해 주면 해결된다.

`/etc/docker/daemon.json` 파일을 열어서 다음 내용을 채워주고, docker daemon을 재시작 한다.

```json
{
    "insecure-registries" : [ "gcr.io" ]
}
```

### pip

python을 쓴다면 `pip`도 사용하게 된다.

```bash
$ pip install xxxxx==1.1.1
Downloading/unpacking xxxxx==1.1.1
  Getting page https://pypi.python.org/simple/xxxxx/
  Could not fetch URL https://pypi.python.org/simple/xxxxx/: connection error: [Errno 1] _ssl.c:510: error:14090086:SSL routines:SSL3_GET_SERVER_CERTIFICATE:certificate verify failed
```

또 인증서 에러가 난다.

`pip`의 경우엔 `--trusted-host` 또는 `PIP_TRUSTED_HOST` 환경변수에 `pypi.python.org`를 넣어서 해결할 수 있다.

하지만 `pip` 버전이 낮은 경우엔 지원되지 않는 옵션이기 때문에...

높은 버전의 `pip`의 `.whl` 파일을 `pypi.python.org`에서 수동으로 받아서 깔아주고, 수행한다.

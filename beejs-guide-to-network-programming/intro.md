# 1. 들어가며

이봐요! 소켓 프로그래밍이 당신을 우울하게 하나요? 매뉴얼 페이지만 읽어서 단박에
이해하기엔 조금 어렵다고 느끼시나요? 멋진 인터넷 프로그램밍을 하고는 싶지만,
수많은 구조체의 향연을 헤쳐나가며 bind() 전에 connect()를 호출해야 하는지를
비롯한 수많은 것들을 고민하기엔 시간이 부족할거에요.

걱정하지 마세요. 온갖 고약한 일들은 다 제가 처리해 놨고, 이제 그 정보를 모두와
공유하려고 하니까요! 제대로 찾아오신 겁니다. 이 문서는 보통 수준의 C
프로그래머가 네트워크 프로그래밍을 위해 알아야 할 온갖 지식들을 담고 있답니다.

그리고, 놀라지 마세요: 제가 드디어 시대에 발맞춰 가이드를 IPv6에 맞게 업데이트
했답니다! 즐기시길!


## 1.1. 이 글의 대상

이 문서는 튜토리얼로서 쓰여졌습니다. 완전한 레퍼런스가 아니구요. 이제 막 소켓
프로그래밍을 시작하려하는 중이고 발판이 필요하다고 느끼는 사람이 읽기에 아마
가장 좋을 거에요. 어떤 의미로든, 이 글은 소켓 프로그래밍을 위한 완전/완벽한
가이드는 아닙니다.

하지만, 적어도 이 문서를 읽고 나면 그놈의 매뉴얼 페이지들이 무슨 말을 하는지는
알아들을 수 있을거에요... 아마두요! :-)

## 1.2. 플랫폼과 컴파일러

The code contained within this document was compiled on a Linux PC using Gnu's
gcc compiler. It should, however, build on just about any platform that uses
gcc. Naturally, this doesn't apply if you're programming for Windows—see the
section on Windows programming, below.
이 문서에 등장하는 코드들은 모두 Gnu 재단의 gcc 컴파일러를 이용해 리눅스 PC
위에서 컴파일되었습니다. 하지만, 다른 어떤 플랫폼에서라도 gcc를 이용한다면
문제 없이 컴파일 될 거에요. 당연하지만, 만약 당신이 윈도우에서 프로그래밍
중이라면 얘기는 달라지겠죠 - 윈도우 환경에서 프로그래밍하는 분들을 위한 부분이
아래에 따로 마련되어 있으니 확인해보세요.

## 1.3. 공식 홈페이지와 서적 판매

This official location of this document is http://beej.us/guide/bgnet/. There
you will also find example code and translations of the guide into various
languages.
이 문서의 공식적인 위치는 [이 곳](http://beej.us/guide/bgnet/) 입니다. 예제 코드와 이
가이드의 다양한 언어로의 번역본들도 사이트에 있으니 확인해보세요.

To buy nicely bound print copies (some call them "books"), visit
http://beej.us/guide/url/bgbuy. I'll appreciate the purchase because it helps
sustain my document-writing lifestyle!
만약 잘 엮어진 인쇄된 복사본을 (어떤 사람들은 그걸 "책"이라고 부르던데...) 살
의향이 있다면, [이 곳](http://beej.us/guide/url/bgbuy) 를 이용하세요.

## 1.4. Solaris/SunOS 프로그래머를 위한 지침

Solaris 혹은 SunOS에서 컴파일을 할 때에는, 필요한 라이브러리를 링킹하기 위해
추가적으로 커맨드라인 스위치를 명시해줘야 합니다. 이를 위해서는 간단하게 "-lnsl
-lsocket -lresolv"를 컴파일 커맨드의 끝부분에 붙여주기만 하면 되요. 이렇게
말이죠:

`cc -o server server.c -lnsl -lsocket -lresolv`

만약 여전히 에러가 난다면, 그 커맨드라인의 끝부분에 "-lxnet" 까지 붙여보세요. 그
놈이 정확히 무슨 일을 하는지는 모르겠지만, 어떤 사람들은 그걸 필요로 하는 것
같더라구요.

setsockopt()를 호출하는 부분에서도 좀 문제가 생길 수 있습니다. 프로토타입이 저의
리눅스 머신과 다르기 때문인데,

`int yes=1;`

을 입력하는 대신

`char yes='1';`

를 입력해보세요.

제가 Sun 머신을 가지고 있지 않은 터라, 저는 위의 정보들 중 어떤 것도 테스트 해
보지 않았습니다. 그냥 사람들이 저한테 이메일로 알려준 정보들이에요.

## 1.5. Windows 프로그래머를 위한 지침

솔직히 말해, 저는 단순히 제가 Windows를 별로 좋아하지 않는다는 이유로, Windows에
대해 좀 매정하게 대해 왔습니다. 하지만 공정하게 이야기하자면 Windows는 거대한
사용자 기반을 가지고 있고, 꽤나 괜찮은 운영체제죠.

떨어져 있으면 사랑이 커진다고들 하던데, 이 경우엔 그 말이 맞는 것 같습니다.
(어쩌면 시간이 흘러서일수도 있겠지만요.) 제 개인적 작업에 십년이 넘게 Windows를
사용하지 않고나니, 전 훨씬 행복해졌어요! 그러다 보니, 이제는 의자에 반쯤 기대
앉은 채로 "그럼요, Windows를 사용하고 싶다면 그렇게 하세요!" 라고 말 할 수 있을
것 같 ... 아닙니다. 여전히 그 말을 하려니 이를 꽉 물게 되네요.

그래서 전 여전히 여러분께 Linux, BSD, 혹은 Unix 기반의 운영체제를 이용하길
추천드립니다.

하지만 취향의 자유란게 있는 법이죠. "Windows 애호가들"에겐 기쁜 소식인데, 이
가이드의 내용들은 약간의 수정을 통해 Windows 환경에서도 정상적으로 작동합니다.
만약 "Windows 애호가들"이란게 있긴 하다면 이야기지만, 뭐..

Cygwin을 설치하는 것도 멋진 방법이 될 수 있을 것 같네요. Cygwin은 Windows를 위한
Unix 툴들의 모음집인데요. 제가 아는 비밀 정보통에 따르면 이걸 깔면 여기 있는
프로그램들을 수정하지 않은 채로 컴파일 할 수 있다고 하더라구요.

하지만 어떤 분들은 "순정 Windows 방식"으로 일을 처리하고 싶을 수도 있겠죠. 참
성격 더러운 분들이시네! 여러분들이 해야 할 일은 하나입니다: 당장 뛰어 나가서
Unix 머신을 구해오세요! ... 농담, 농담입니다. 요즘 시대엔 (적어도 예전보단)
Windows에 우호적이여야 한다죠 아마?

당신이 Cygwin을 깔지 않았다면, 일단 제가 문서에서 언급한 거의 대부분의 시스템
헤더 파일은 무시하세요. 그 대신 다음과 같은 코드를 포함시켜야 합니다:

`#include <winsock.h>`

잠깐! 그것 뿐만 아니라, 소켓 라이브러리를 이용하기 전에 WSAStartup()을
호출해주어야 합니다. 이런 식으로요:

`
#include <winsock.h>

{
    WSADATA wsaData;   // if this doesn't work
    //WSAData wsaData; // then try this instead

    // MAKEWORD(1,1) for Winsock 1.1, MAKEWORD(2,0) for Winsock 2.0:

    if (WSAStartup(MAKEWORD(1,1), &wsaData) != 0) {
        fprintf(stderr, "WSAStartup failed.\n");
        exit(1);
    }
`

그 뿐만 아니라, wsock23.lib, winsock32.lib, 혹은 ws2_32.lib(for Winsock 2.0)
등의 이름을 갖는 Winsock 라이브러리를 링크하라고 컴파일러에게 알려줘야 합니다.
VC++ 환경에서 개발중이라면, Settings.... -> Project -> Link 로 들어가서
"Object/library modules"라는 이름의 박스에 "wsock32.lib" (혹은 여러분이 선호하는
관련 라이브러리)를 리스트에 추가하시면 되요.

뭐, 제가 듣기론 그렇다고 하더라구요.

마지막으로, 소켓 라이브러리 사용이 끝난 후엔 WSACleanup()을 호출해주어야 합니다.
자세한 내용은 온라인 도움말을 찾아보세요.

이 일련의 과정을 마치고 나면, 이 튜토리얼에 있는 대부분의 예제 코드들은
몇 개의 예외를 제외하곤 정상적으로 작동할겁니다. 하나 명심하셔야 할 건, 소켓을
닫기 위해서 close() 대신 closesocket()을 사용하셔야 한다는 거에요. 또한,
select()는 (stdin을 위한 0과 같은) 일반적인 파일 기술자가 아닌, 소켓 기술자
디스크립터에 대해서만 작동합니다.

CSocket이라는, 당신이 사용할 수 있는 소켓 클래스도 있습니다. 더 자세한 정보를
위해서는 컴파일러 도움말 페이지를 참조하세요.

Winsock에 대한 더 자세한 정보를 위해선, [Winsock FAQ](http://tangentsoft.net/wskfaq/)
를 참조하세요.

하나 남았네요. 불행하게도, Windows에는 - 제가 몇몇 예제에서 사용한 - fork()
시스템 콜이 없다고 들었습니다. POSIX 라이브러리 등을 링크해서 fork()가 작동하게
할 수도 있고, 그냥 CreateProcess()를 대신 사용하셔도 좋습니다. fork()는 인자를
하나도 받지 않고, CreateProcess()는 한 4억 8천만개 정도의 인자를 받긴 하지만요.
만약 그건 좀 아니다 싶으면, CreateThread()는 좀 더 사용하기 쉽겠지만 ...
아쉽게도 멀티쓰레딩에 관한 논의는 이 문서가 다루는 범위 밖에 있습니다. 뭐,
이야기는 이정도로 끝내죠!

## 1.6. 이메일 정책

I'm generally available to help out with email questions so feel free to write
in, but I can't guarantee a response. I lead a pretty busy life and there are
times when I just can't answer a question you have. When that's the case, I
usually just delete the message. It's nothing personal; I just won't ever have
the time to give the detailed answer you require.
보통의 경우, 저는 이메일 질문들에 답할 수 있으니 언제든지 메일을 주세요. 다만,
답장을 장담드릴 수는 없습니다. 저는 꽤나 바쁜 삶을 살고 있고, 도저히 당신의
질문에 답할 시간이 나지 않을 때가 종종 있거든요. 만약 그런 경우에 메일이 오면,
저는 그냥 삭제해 버립니다. 개인적인 감정이 있어서 그런건 절대 아니고, 다만
당신이 필요로 하는 자세한 답변을 드릴 시간이 없기 때문이에요.

규칙이 하나 있어요: 질문이 복잡하면 복잡할수록, 제가 답을 드릴 확률은
줄어듭니다. 메일을 보내기 전에 질문을 최대한 좁히고, 필요한 정보 (플랫폼,
컴파일러, 에러 메시지, 그 외에 트러블 슈팅에 도움이 될 어떤 것이든) 를 최대한
많이 포함시킨다면, 아마 답변을 받을 확률이 훨씬 커질거에요. 팁을 하나 드리자면,
ESR의 문서인 [똑똑하게 질문하기](http://www.catb.org/~esr/faqs/smart-questions.html) 를 읽어보세요.

만약 답변을 얻지 못 했다면, 다른 방법들을 시도해보고, 정답을 찾으려 노력해 본
뒤에, 그럼에도 불구하고 여전히 잘 모르겠다면, 당신이 얻어낸 정보들을 포함해서
다시 메일을 보내보세요. 그 정도면 제가 도와주기에 충분할겁니다.

저한테 어떻게 메일을 써야하고 어떻게 쓰지 말아야 댈지에 대해 한참 떠든 것
같은데, 무엇보다 이 가이드에 수년간 보내주신 칭찬들에 대해 제가 정말 감사하고
있다는 걸 알려드리고 싶습니다. 도움이 되었다는 말을 들을때마다 정말 기쁘고
사기가 충전되요! :-) 감사합니다!

## 1.7. 미러링

만약 이 사이트를 사적으로든 공적으로든 미러링하고 싶으시면, 환영합니다! 만약 이
사이트를 공개적으로 미러링 한 후에 제가 메인 페이지에 링크를 걸어주길
원하신다면, beej@beej.us 로 메일을 보내주세요.

## 1.8. 번역자들을 위해

만약 이 가이드를 다른 언어로 번역하고 싶으시다면, beej@beej.us 로 메일을 주시면
당신의 번역본을 메인 페이지에 링크하도록 하겠습니다. 당신의 이름, 연락처 정보
등을 번역본에 자유롭게 포함시키셔도 됩니다.

아래에 있는 저작권, 배포에 관한 항목의 라이센스 제한을 확인하세요.

만약 번역본을 제가 호스트해주길 원하신다면, 제게 물어보세요. 만약 당신이 직접
호스트한 후에 메일을 주시면 제가 링크만 걸 수도 있어요. 어느 쪽이든 상관
없습니다.

## 1.9. 저작권과 배포

Beej's Guide to Network Programming is Copyright © 2012 Brian "Beej Jorgensen"
Hall.

기본적으로 이 문서는 Creativ Commons Attribution -
Noncommercial- No Derivative Works 3.0 License 아래에 있습니다. 이 라이센스의
전문을 보시려면 [여기](http://creativecommons.org/licenses/by-nc-nd/3.0/) 를
방문하시거나 Creative Commons, 171 Second Street, Suite 300, San Francisco,
California, 94105, USA 로 메일을 보내세요.

다만 이 라이센스의 "No Derivative Works" 부분에는 예외가 존재합니다:
이 가이드는 번역이 정확하고, 가이드의 전부분이 번역 된다는 가정 하에 어떤
언어로든 자유롭게 번역될 수 있습니다. 라이센스의 나머지 부분은 번역본에도
동일하게 적용됩니다. 번역본은 번역자의 이름과 연락처 정보를 포함할 수 있습니다.

이 문서 내의 C 소스 코드는 공적으로 공개되어 있고, 어떤 라이센스 제한도 없이
완전히 자유롭게 사용하실 수 있습니다.

교육자들은 이 가이드의 복사본을 학생들에게 자유롭게 나누어줄 수 있습니다. 사실
그건 권장사항이에요!

더 자세한 정보를 위해선 beej@beej.us 로 연락 주세요.

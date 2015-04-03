# 2. 소켓이 뭔가요?

사람들이 "소켓"에 대해 여기저기서 이야기하는 것을 들으면서, 대체 그게 정확히 뭘
의미하는지 궁금해 했을 수 있을 것 같네요. 뭐, 여기 그 답이 있습니다: 표준 유닉스
파일 기술자를 사용해 다른 프로그램에게 이야기할 수 있는 방법.

뭐시여?

좋아요 - 유닉스 프로그래머가 이런 말을 하는 걸 들어본 적이 있을거에요.
"맙소사, 유닉스에선 정말 모든게 파일이구만!" 아마 그 프로그래머가 하고 싶었던
이야기는 유닉스 프로그램들이 어떤 종류의 파일 입/출력을 할 때면, 그들은 늘 파일
기술자로부터 읽거나(read) 쓰는(write) 형태로 그 작업을 수행한다는 것일거에요.
파일 기술자라는건 그저 현재 열려있는 하나의 파일과 연관된 하나의 정수일
뿐입니다. 하지만 그 파일은 네트워크 연결이 될수도, FIFO일수도,
파이프라인일수도, 터미널일수도, 실제 디스크 상의 파일일수도 있습니다. 사실
어느것일수도 있죠! 따라서 당신이 인터넷을 통해 다른 프로그램과 통신하고
싶다면, 당신은 파일 기술자를 통해 그 작업을 하게 될 겁니다. 믿으세요!

아마 "아이구 똑똑한 양반, 그래서 그 네트워크 커뮤니케이션을 위한 파일 기술자는 어디서
얻는건데?" 라는 질문을 떠올리고 있을지도 모르겠네요. 그 질문에 답하자면, 당신은
socket() 시스템 루틴을 호출하게 됩니다. 그 루틴은 당신에게 소켓 기술자를
던져줄거고, 이제 당신은 send()와 recv() 소켓 콜들을 이용해 그 소켓을 통해 통신할
수 있죠. (터미널에서 man send, man recv를 쳐 보세요!)

지금쯤 "이봐, 잠깐만!" 이란 말이 입 밖으로 튀어나오고 있을지도 모르겠네요.
"만약에 그게 그냥 파일 기술자라면, 대체 뭐 때문에 소켓을 통해 통신하는데에 그냥
read()나 write()를 쓰면 안 되는건데?" 짧게 대답하자면, "써도 됩니다!" 조금 더
길게 이야기하자면, "써도 되긴 하는데, send()와 recv()를 이용하면 데이터 통신을
컨트롤하기가 좀 더 쉬울거에요."

또 뭐가 있냐구요? 이건 어때요: 세상엔 수많은 종류의 소켓들이 있습니다. DARPA
인터넷 주소 (인터넷 소켓들)이 있고, 로컬 노드에서의 경로명 (유닉스 소켓들)이 있고,
CCITT X.25 주소 (무시해도 괜찮은 X.25 소켓들), 그리고 당신이
사용하는 유닉스 계열에 따라 수많은 다른 종류들이 있을 수 있습니다. 이 문서에서는
가장 처음 것에 대해서만 다루죠: 인터넷 소켓입니다.

## 2.1. 인터넷 소켓의 두 가지 종류

이건 또 뭐지? 인터넷 소켓에도 두 가지 종류가 있다고? 맞습니다. 음, 사실은
아니에요. 거짓말입니다. 두 가지보다 더 많은 종류의 인터넷 소켓들이 있지만, 겁을
주기가 싫었어요. 여기선 딱 두 종류에 대해서만 이야기할겁니다. 딱 이 문장만
제외하구요: "Raw Socket"이란 녀석도 매우 유용하니 시간이 나면 한 번 살펴보세요.

아무튼, 좋아요. 그래서 그 두 종류가 뭐냐구요? 하나는 "스트림 소켓"이고, 또 다른
한 녀석은 "데이타그램 소켓"입니다. 앞으로는 각각을 "SOCK_STREAM",
"SOCK_DGRAM"이라고 부를거에요. 데이타그램 소켓은 종종 "비연결 소켓"이라고
불리기도 합니다. (사실 여러분이 원하시면 데이타그램으로도 connect()를 구현할 수
있긴 하지만요.)

스트림 소켓들은 신뢰 가능한 양방향 연결 통신 스트림입니다. 만약 당신이 소켓에
"1, 2"의 순서로 두 항목을 보낸다면, 그 항목들은 정확히 "1, 2"의 순서로 반대쪽에
도착하게 됩니다. 또한 당신이 스트림 소켓을 사용한다면, 에러에 신경 쓸 필요가
없습니다. 사실 저는 스트림 소켓이 에러가 없단 것을 확신하고 있기 때문에, 누군가
그걸 반박하려 하면 손가락으로 두 귀를 막고 '라라라라라라~' 하고 노래나 부를지도
몰라요.

그럼 대체 스트림 소켓은 어디서 사용되나요? 아마 telnet 어플리케이션에 대해선
들어보셨겠죠? 텔넷이 스트림 소켓을 사용합니다. 당신이 타이핑한 문자들은 모두
타이핑 된 순서대로 도착해야 하잖아요? 또한, HTTP 프로토콜을 사용하는 웹
브라우저도 페이지를 얻어오기 위해 스트림 소켓을 이용합니다. 그러니까, 만약 어떤
웹사이트에 텔넷을 이용해 80번 포트로 접속해서 "GET / HTTP/1.0"을 친 뒤에
RETURN을 두 번 누르면 웹 사이트는 HTML을 당신에게 던져줄 거라는 이야기죠.

그럼 스트림 소켓은 이렇듯 양질의 데이터 전송을 어떻게 구현하는 걸까요? 스트림
소켓은 "전송 제어 프로토콜 The Transmission Control Protocol", 혹은 "TCP"라고
알려진 프로토콜을 사용합니다. (TCP에 대한 자세한 정보를 위해선 [RFC 793](http://tools.ietf.org/html/rfc793)를
참조하세요.) TCP는 당신의 데이터가 에러 없이 순차적으로 도달할 것을 보장합니다.
어쩌면 "TCP"라는 단어를 "인터넷 프로토콜"([RFC 791](http://tools.ietf.org/html/rfc791)을
보세요.) 를 의미하는 "IP"와 결합된 "TCP/IP"라는 단어에서 본 적이 있을지도
모르겠네요. IP는 기본적으로 인터넷 라우팅을 담당하고, 보통 데이터의 무결성과는
관련이 없습니다.

좋아요. 그럼 데이타그램 소켓은요? 왜 데이타그램 소켓은 "비연결"이라 불리는거죠?
무슨 일이 일어나고 있는 겁니까? 왜 그들은 신뢰할 수 없죠? 음, 일단 몇 가지
사실부터 짚고 넘어갑시다: 만약 당신이 데이타그램을 보낸다면, 그건 도착할 지도
모릅니다. 순서는 뒤바뀌었을 수도 있구요. 만약 도착하긴 했다면, 그 패킷 내의
데이터에 오류는 없을겁니다.

데이타그램 소켓 역시 라우팅을 위해 IP를 사용하지만, TCP는 사용하지 않습니다.
그 대신 "유저 데이타그램 프로토콜 User Datagram Protocol", "UDP"를 사용하죠.
([RFC 768](http://tools.ietf.org/html/rfc768)를 보세요.)

왜 데이타그램은 "비연결"일까요? 기본적으로, 그건 당신이 스트림 소켓을 이용할
때와는 다르게, 열린 연결을 유지할 필요가 없기 때문입니다. 그냥 패킷을 만들고,
목적지에 대한 정보를 담고 있는 IP 헤더를 붙이고, 전송하면 되는 거죠. 연결은
필요하지 않습니다. 데이타그램 소켓은 보통 TCP를 사용할 수 없는 상황, 혹은 몇
패킷의 데이터가 손실되도 우주가 멸망하지는 않는 상황에 사용합니다. 예를 들면
다음과 같죠: *tftp*(Trivial File Transfer Protocol, FTP의 조그만 동생격이죠),
*dhcpcd*(DHCP 클라이언트), 멀티플레이어 게임, 오디오 스트리밍, 영상 회의 등등.

"잠깐 기다려 봐! *tftp*랑 *dhcpcd*는 바이너리 어플리케이션을 한 호스트에서 다른
호스트로 전송하는데 쓰이는 거잖아! 도착했을 때 어플리케이션이 정상적으로
작동하려면 데이터가 소실되면 안 되는거 아닌가? UDP로 어떻게 그런 일을 할 수
있지? 흑마법이라도 쓰는거야?"

친애하는 나의 인간 친구여, tftp나 그 비슷한 프로그램들은 UDP 위에 자기들의
고유한 프로토콜을 또 가지고 있답니다. 예를 들어, tftp 프로토콜에서는 어떤 패킷을
받은 후에는 "잘 받았어!"라는 메세지를 담고 있는 패킷("ACK" 패킷)을 보내죠. 만약
처음에 패킷을 보낸 쪽에서 이 답장을, 예를 들어 5초 내에, 받지 못한다면 그는
ACK를 받을때까지 해당 패킷을 재전송합니다. 이러한 인식 절차는 SOCK_DGRAM을
이용해 신뢰할 수 있는 통신 어플리케이션을 구현할 때 매우 중요합니다.


For unreliable applications like games, audio, or video, you just ignore the
dropped packets, or perhaps try to cleverly compensate for them. (Quake players
will know the manifestation this effect by the technical term: accursed lag. The
word "accursed", in this case, represents any extremely profane utterance.)

Why would you use an unreliable underlying protocol? Two reasons: speed and
speed. It's way faster to fire-and-forget than it is to keep track of what has
arrived safely and make sure it's in order and all that. If you're sending chat
messages, TCP is great; if you're sending 40 positional updates per second of
the players in the world, maybe it doesn't matter so much if one or two get
dropped, and UDP is a good choice.

## 2.2. Low level Nonsense and Network Theory

Since I just mentioned layering of protocols, it's time to talk about how
networks really work, and to show some examples of how SOCK_DGRAM packets are
built. Practically, you can probably skip this section. It's good background,
however.
[Encapsulated Protocols Diagram]

Data Encapsulation.

Hey, kids, it's time to learn about Data Encapsulation! This is very very
important. It's so important that you might just learn about it if you take the
networks course here at Chico State ;-). Basically, it says this: a packet is
born, the packet is wrapped ("encapsulated") in a header (and rarely a footer)
by the first protocol (say, the TFTP protocol), then the whole thing (TFTP
header included) is encapsulated again by the next protocol (say, UDP), then
again by the next (IP), then again by the final protocol on the hardware
(physical) layer (say, Ethernet).

When another computer receives the packet, the hardware strips the Ethernet
header, the kernel strips the IP and UDP headers, the TFTP program strips the
TFTP header, and it finally has the data.

Now I can finally talk about the infamous Layered Network Model (aka "ISO/OSI").
This Network Model describes a system of network functionality that has many
advantages over other models. For instance, you can write sockets programs that
are exactly the same without caring how the data is physically transmitted
(serial, thin Ethernet, AUI, whatever) because programs on lower levels deal
with it for you. The actual network hardware and topology is transparent to the
socket programmer.

Without any further ado, I'll present the layers of the full-blown model.
Remember this for network class exams:


    Application
    Presentation
    Session
    Transport
    Network
    Data Link
    Physical

The Physical Layer is the hardware (serial, Ethernet, etc.). The Application
Layer is just about as far from the physical layer as you can imagine—it's the
place where users interact with the network.

Now, this model is so general you could probably use it as an automobile repair
guide if you really wanted to. A layered model more consistent with Unix might
be:

    Application Layer (telnet, ftp, etc.)
    Host-to-Host Transport Layer (TCP, UDP)
    Internet Layer (IP and routing)
    Network Access Layer (Ethernet, wi-fi, or whatever)

At this point in time, you can probably see how these layers correspond to the
encapsulation of the original data.

See how much work there is in building a simple packet? Jeez! And you have to
type in the packet headers yourself using "cat"! Just kidding. All you have to
do for stream sockets is send() the data out. All you have to do for datagram
sockets is encapsulate the packet in the method of your choosing and sendto() it
out. The kernel builds the Transport Layer and Internet Layer on for you and the
hardware does the Network Access Layer. Ah, modern technology.

So ends our brief foray into network theory. Oh yes, I forgot to tell you
everything I wanted to say about routing: nothing! That's right, I'm not going
to talk about it at all. The router strips the packet to the IP header, consults
its routing table, blah blah blah. Check out the IP RFC if you really really
care. If you never learn about it, well, you'll live.

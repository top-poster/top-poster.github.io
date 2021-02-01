---
layout: post
title: "Flutter 및 Dart로 채팅 앱 UI를 구축하는 방법
 "
author: 'Code Tower'
thumbnail: https://images.unsplash.com/photo-1552068751-34cb5cf055b3?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MXwxMTc3M3wwfDF8c2VhcmNofDd8fG1vYmlsZSUyMGNoYXR8ZW58MHx8fA&ixlib=rb-1.2.1&q=80&w=2000
tags: undefined
---


요즘 많은 사람들이 채팅 애플리케이션을 사용하여 스마트 폰을 통해 팀원, 친구 및 가족과 의사 소통합니다.
 따라서 이러한 메시징 응용 프로그램은 필수 통신 매체가됩니다.
 

또한 최첨단 기능을 갖춘 직관적이고 강력한 사용자 인터페이스에 대한 수요도 높습니다.
 사용자 인터페이스 (또는 UI)는 전체 사용자 경험에서 가장 영향력있는 측면이므로 올바르게하는 것이 중요합니다.
 

Flutter 앱 개발은 크로스 플랫폼 모바일 애플리케이션 개발 측면에서 전 세계를 강타했습니다.
 이를 사용하여 완벽한 픽셀 UI를 만들 수 있으며 오늘날 많은 개발 회사에서 Flutter를 사용합니다.
 

이 튜토리얼에서는 두 가지를 혼합하여 소개하겠습니다. Flutter / Dart 코딩 환경에서 전적으로 채팅 앱 UI를 구축 할 것입니다.
 Flutter에서 멋진 Chat UI 구현을 배우는 것과 함께, 코딩 워크 플로우와 구조가 어떻게 작동하는지 배우게됩니다.
 

자, 시작합시다!
 

## 새 Flutter 프로젝트를 만드는 방법
 

먼저 새 Flutter 프로젝트를 만들어야합니다.
 이를 위해 Flutter SDK 및 기타 Flutter 앱 개발 관련 요구 사항을 설치했는지 확인하세요.
 

모든 것이 올바르게 설정 되었으면 프로젝트를 생성하기 위해 원하는 로컬 디렉토리에서 다음 명령을 실행하면됩니다.
 

```undefined
flutter create ChatApp

```

프로젝트를 설정 한 후 프로젝트 디렉터리 내부를 탐색하고 터미널에서 다음 명령을 실행하여 사용 가능한 에뮬레이터 또는 실제 장치에서 프로젝트를 실행할 수 있습니다.
 

```undefined
flutter run

```

성공적으로 빌드되면 에뮬레이터 화면에 다음 결과가 표시됩니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-105.png)

## 메인 홈 화면 UI를 만드는 방법
 

이제 채팅 애플리케이션의 UI 구축을 시작하겠습니다.
 메인 홈 화면에는 2 개의 섹션이 있습니다.
 

- 별도의 페이지로 구현할 대화 화면
 
- 하단 네비게이션 바.
 

먼저 main.dart 파일의 기본 상용구 코드에 대한 몇 가지 간단한 구성을 만들어야합니다.
 몇 가지 기본 코드를 제거하고 지금은 빈`Container`를 가리키는 간단한`MaterialApp`을 홈 페이지로 추가합니다.
 

```undefined
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      debugShowCheckedModeBanner: false,
      home: Container(),
    );
  }
}

```

이제 빈 컨테이너 위젯 대신 `HomePage`화면 위젯을 호출합니다.
 하지만 먼저 화면을 구현해야합니다.
 

### 메인 홈 화면을 만드는 방법
 

루트 프로젝트 폴더의 ./lib 디렉토리 안에 ./screens라는 폴더를 만들어야합니다.
 이 폴더는 다른 화면에 대한 모든 dart 파일을 보관합니다.
 

./lib/screens/ 디렉토리 안에 homePage.dart라는 파일을 만들어야합니다.
 homePage.dart 파일 내에 아래 코드 스 니펫에 표시된대로 기본 상태 비 저장 위젯 코드를 추가해야합니다.
 

```undefined
import 'package:flutter/material.dart';

class HomePage extends StatelessWidget{
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(
        child: Center(child: Text("Chat")),
      ),

    );
  }
}

```

이제 아래 코드 스 니펫과 같이 main.dart 파일에서`HomePage` 클래스 위젯을 호출해야합니다.
 

```undefined
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      debugShowCheckedModeBanner: false,
      home: HomePage(),
    );
  }
}

```

이제 아래 에뮬레이터 스크린 샷에 표시된 결과를 얻을 수 있습니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-106.png)

### 아래쪽 탐색 모음을 만드는 방법
 

이제 `HomePage`화면에 하단 탐색 메뉴를 배치하겠습니다.
 이를 위해 `Scaffold`위젯에서 제공하는 `bottomNavigationBar`매개 변수의 `BottomNavigationBar`위젯을 사용합니다.
 

전체 코드 구현은 다음과 같습니다.
 

```undefined
return Scaffold(
      body: Container(
        child: Center(child: Text("Chat")),
      ),
      bottomNavigationBar: BottomNavigationBar(
        selectedItemColor: Colors.red,
        unselectedItemColor: Colors.grey.shade600,
        selectedLabelStyle: TextStyle(fontWeight: FontWeight.w600),
        unselectedLabelStyle: TextStyle(fontWeight: FontWeight.w600),
        type: BottomNavigationBarType.fixed,
        items: [
          BottomNavigationBarItem(
            icon: Icon(Icons.message),
            title: Text("Chats"),
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.group_work),
            title: Text("Channels"),
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.account_box),
            title: Text("Profile"),
          ),
        ],
      ),
    );

```

여기에서는 다양한 스타일 매개 변수로`BottomNavigationBar`를 구성하고`items` 매개 변수에 탐색 메뉴 항목을 유지했습니다.
 `body` 매개 변수의 경우`Text` 위젯과 함께 간단한`Container`를 사용했습니다.
 

이제 아래 에뮬레이터 스크린 샷과 같이 하단 탐색 메뉴가 표시됩니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-107.png)

이제 하단 탐색이 완료되었으므로 계속해서 하단 탐색 모음 바로 위에 대화 목록 섹션을 구현할 수 있습니다.
 

## 대화 목록 화면을 만드는 방법
 

여기에서는 헤더 섹션, 검색 창 및 대화 목록보기를 포함 할 대화 목록 섹션을 만들 것입니다.
 

먼저 ./lib/screens 폴더 안에 chatPage.dart라는 새 dart 파일을 만들어야합니다.
 그런 다음 아래 코드 스 니펫에 표시된대로 내부에 간단한 상태 저장 위젯 클래스 템플릿을 추가합니다.
 

```undefined
import 'package:flutter/material.dart';

class ChatPage extends StatefulWidget {
  @override
  _ChatPageState createState() => _ChatPageState();
}

class _ChatPageState extends State<ChatPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SingleChildScrollView(
        child: Center(child: Text("Chat")),
      ),
    );
  }
}

```

이제 아래 코드 스 니펫과 같이 homePage.dart의`Container` 위젯 대신`chatPage` 클래스 위젯을 호출해야합니다.
 

```undefined
return Scaffold(
      body: ChatPage(),
      bottomNavigationBar: BottomNavigationBar(

```

그러면 아래 에뮬레이터에 표시된대로 다음과 같은 결과가 제공됩니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-108.png)

### 대화 목록 페이지 머리글을 작성하는 방법
 

이제 텍스트 머리글과 단추가있는 대화 목록 섹션에 머리글을 추가합니다.
 전체 UI 구현 코드는 아래 코드 스 니펫에 제공됩니다.
 

```undefined
return Scaffold(
      body: SingleChildScrollView(
        physics: BouncingScrollPhysics(),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: <Widget>[
            SafeArea(
              child: Padding(
                padding: EdgeInsets.only(left: 16,right: 16,top: 10),
                child: Row(
                  mainAxisAlignment: MainAxisAlignment.spaceBetween,
                  children: <Widget>[
                    Text("Conversations",style: TextStyle(fontSize: 32,fontWeight: FontWeight.bold),),
                    Container(
                      padding: EdgeInsets.only(left: 8,right: 8,top: 2,bottom: 2),
                      height: 30,
                      decoration: BoxDecoration(
                        borderRadius: BorderRadius.circular(30),
                        color: Colors.pink[50],
                      ),
                      child: Row(
                        children: <Widget>[
                          Icon(Icons.add,color: Colors.pink,size: 20,),
                          SizedBox(width: 2,),
                          Text("Add New",style: TextStyle(fontSize: 14,fontWeight: FontWeight.bold),),
                        ],
                      ),
                    )
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );

```

여기서는 chatPage.dart의 본문 섹션이 완전히 스크롤 가능하도록`SingleChildScrollView`를 사용했습니다.
 

그런 다음`BouncingScrollPhysics` 인스턴스를 사용하여 사용자의 스크롤이 끝 또는 시작에 도달 할 때 바운싱 효과를 제공했습니다.
 

다음으로 오른쪽 하단에 텍스트 위젯과 컨테이너를 추가했습니다.
 

마지막으로,`SingleChildScrollView` 위젯의 자식으로`Column` 위젯이 있으므로 모든 것이 화면에 수직으로 표시됩니다.
 

그러면 아래 에뮬레이터에 표시된대로 다음과 같은 결과가 제공됩니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-109.png)

이제 헤더 섹션 바로 아래에 검색 창을 추가하겠습니다.
 

### 검색 창을 추가하는 방법
 

이전의`Column` 위젯에서 Header UI 섹션 바로 아래에 Search bar 위젯을 추가 할 것입니다.
 따라서`Column` 위젯의 두 번째 자식으로서 아래 코드 스 니펫에 제공된 다음 코드를 삽입해야합니다.
 

```undefined
Padding(
  padding: EdgeInsets.only(top: 16,left: 16,right: 16),
  child: TextField(
    decoration: InputDecoration(
      hintText: "Search...",
      hintStyle: TextStyle(color: Colors.grey.shade600),
      prefixIcon: Icon(Icons.search,color: Colors.grey.shade600, size: 20,),
      filled: true,
      fillColor: Colors.grey.shade100,
      contentPadding: EdgeInsets.all(8),
      enabledBorder: OutlineInputBorder(
          borderRadius: BorderRadius.circular(20),
          borderSide: BorderSide(
              color: Colors.grey.shade100
          )
      ),
    ),
  ),
),

```

그러면 아래 에뮬레이터에 표시된대로 다음과 같은 결과가 제공됩니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-110.png)

### 대화 목록을 작성하는 방법
 

이제 헤더 섹션과 검색 표시 줄이 있으므로 대화 목록 섹션을 구현할 것입니다.
 

이를 위해 대화 목록의 인스턴스를 저장하는 클래스 개체 모델을 구현해야합니다.
 

따라서 ./lib 폴더 안에 ./models라는 새 폴더를 만들어야합니다.
 ./models 안에 chatUsersModel.dart라는 파일을 만들어야합니다.
 

모델 파일에서 아래 코드 조각과 같이 모델 객체 클래스를 만들어야합니다.
 

```undefined
import 'package:flutter/cupertino.dart';

class ChatUsers{
  String name;
  String messageText;
  String imageURL;
  String time;
  ChatUsers({@required this.name,@required this.messageText,@required this.imageURL,@required this.time});
}

```

개체에는 사용자 이름, 문자 메시지, 이미지 URL 및 시간이 저장됩니다.
 

다음으로 아래 코드 스 니펫과 같이 chatPage.dart 내에 사용자 목록을 만들어야합니다.
 

```undefined
class _ChatPageState extends State<ChatPage> {
  List<ChatUsers> chatUsers = [
    ChatUsers(text: "Jane Russel", secondaryText: "Awesome Setup", image: "images/userImage1.jpeg", time: "Now"),
    ChatUsers(text: "Glady's Murphy", secondaryText: "That's Great", image: "images/userImage2.jpeg", time: "Yesterday"),
    ChatUsers(text: "Jorge Henry", secondaryText: "Hey where are you?", image: "images/userImage3.jpeg", time: "31 Mar"),
    ChatUsers(text: "Philip Fox", secondaryText: "Busy! Call me in 20 mins", image: "images/userImage4.jpeg", time: "28 Mar"),
    ChatUsers(text: "Debra Hawkins", secondaryText: "Thankyou, It's awesome", image: "images/userImage5.jpeg", time: "23 Mar"),
    ChatUsers(text: "Jacob Pena", secondaryText: "will update you in evening", image: "images/userImage6.jpeg", time: "17 Mar"),
    ChatUsers(text: "Andrey Jones", secondaryText: "Can you please share the file?", image: "images/userImage7.jpeg", time: "24 Feb"),
    ChatUsers(text: "John Wick", secondaryText: "How are you?", image: "images/userImage8.jpeg", time: "18 Feb"),
  ];

```

이제 모의 사용자의 대화 목록 데이터가 있으므로이를 대화 목록에 적용하여 목록보기를 만들 수 있습니다.
 

### 개별 대화를위한 별도의 클래스 위젯을 만드는 방법
 

여기에서는 대화 목록보기의 개별 항목에 대해 별도의 구성 요소 위젯을 만들 것입니다.
 

이를 위해 ./lib 안에 ./widgets라는 폴더를 만듭니다.
 그리고 ./widgets 안에서 우리는 currencyList.dart라는 파일을 만들어야합니다.
 새 위젯 파일 내에서 다음 코드 스 니펫의 코드를 사용할 수 있습니다.
 

```undefined
import 'package:flutter/material.dart';

class ConversationList extends StatefulWidget{
  String name;
  String messageText;
  String imageUrl;
  String time;
  bool isMessageRead;
  ConversationList({@required this.name,@required this.messageText,@required this.imageUrl,@required this.time,@required this.isMessageRead});
  @override
  _ConversationListState createState() => _ConversationListState();
}

class _ConversationListState extends State<ConversationList> {
  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: (){
      },
      child: Container(
        padding: EdgeInsets.only(left: 16,right: 16,top: 10,bottom: 10),
        child: Row(
          children: <Widget>[
            Expanded(
              child: Row(
                children: <Widget>[
                  CircleAvatar(
                    backgroundImage: NetworkImage(widget.imageUrl),
                    maxRadius: 30,
                  ),
                  SizedBox(width: 16,),
                  Expanded(
                    child: Container(
                      color: Colors.transparent,
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: <Widget>[
                          Text(widget.name, style: TextStyle(fontSize: 16),),
                          SizedBox(height: 6,),
                          Text(widget.messageText,style: TextStyle(fontSize: 13,color: Colors.grey.shade600, fontWeight: widget.isMessageRead?FontWeight.bold:FontWeight.normal),),
                        ],
                      ),
                    ),
                  ),
                ],
              ),
            ),
            Text(widget.time,style: TextStyle(fontSize: 12,fontWeight: widget.isMessageRead?FontWeight.bold:FontWeight.normal),),
          ],
        ),
      ),
    );
  }
}

```

이 위젯 파일은 사용자의 이름, 텍스트 메시지, 이미지 URL, 시간 및 부울 메시지 유형 값을 매개 변수로 사용합니다.
 그리고 값이 포함 된 템플릿을 반환합니다.
 

채팅 Page.dart의`ListView` 위젯 내에서 아래 코드 스 니펫에 표시된대로 필수 매개 변수를 전달하여`ConversationaList` 위젯을 호출해야합니다.
 

```undefined
ListView.builder(
  itemCount: chatUsers.length,
  shrinkWrap: true,
  padding: EdgeInsets.only(top: 16),
  physics: NeverScrollableScrollPhysics(),
  itemBuilder: (context, index){
    return ConversationList(
      name: chatUsers[index].name,
      messageText: chatUsers[index].messageText,
      imageUrl: chatUsers[index].imageURL,
      time: chatUsers[index].time,
      isMessageRead: (index == 0 || index == 3)?true:false,
    );
  },
),

```

이`ListView` 위젯은`chatPage` 화면에서`Column` 위젯의 첫 번째 하위로 유지됩니다.
 

그러면 아래 에뮬레이터에 표시된대로 다음과 같은 결과가 제공됩니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-111.png)

이것으로 대화 목록 화면과 메인 홈 페이지 화면 전체의 UI 구현이 완료되었습니다.
 이제 채팅 세부 정보 화면 구현으로 이동합니다.
 

## 채팅 세부 정보 화면을 만드는 방법
 

이제 채팅 상세 화면을 만들겠습니다.
 이를 위해 ./lib/screens/ 폴더 안에 chatDetailPage.dart라는 새 파일을 만들어야합니다.
 지금은 아래 코드 스 니펫에 표시된대로 기본 코드를 추가하겠습니다.
 

```undefined
import 'package:flutter/material.dart';

class ChatDetailPage extends StatefulWidget{
  @override
  _ChatDetailPageState createState() => _ChatDetailPageState();
}

class _ChatDetailPageState extends State<ChatDetailPage> {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Chat Detail"),
      ),
      body: Container()
    );
  }
}

```

여기서는 Text가 포함 된 기본 `AppBar`와 `Scaffold`위젯의 `body`로 빈 컨테이너를 반환했습니다.
 

이제 아래 코드 스 니펫에 표시된대로 currencyList.dart 위젯 파일에있는`GestureHandler` 위젯의`onTap` 메소드에서`ChatDetailPage`에 탐색을 추가 할 것입니다.
 

```undefined
GestureDetector(
      onTap: (){
        Navigator.push(context, MaterialPageRoute(builder: (context){
          return ChatDetailPage();
        }));
      },

```

이제 아래 에뮬레이터 데모와 같이 채팅 세부 정보 화면으로 이동할 수 있습니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/2021-01-20-13.40.11.gif)

### 채팅 세부 정보 화면을위한 사용자 지정 앱 바를 만드는 방법
 

여기에서는 채팅 상세 화면 상단에 사용자 지정 앱 바를 추가하겠습니다.
 이를 위해 아래 코드 스 니펫과 같이 다양한 매개 변수 구성과 함께`AppBar` 위젯을 사용할 것입니다.
 

```undefined
return Scaffold(
      appBar: AppBar(
        elevation: 0,
        automaticallyImplyLeading: false,
        backgroundColor: Colors.white,
        flexibleSpace: SafeArea(
          child: Container(
            padding: EdgeInsets.only(right: 16),
            child: Row(
              children: <Widget>[
                IconButton(
                  onPressed: (){
                    Navigator.pop(context);
                  },
                  icon: Icon(Icons.arrow_back,color: Colors.black,),
                ),
                SizedBox(width: 2,),
                CircleAvatar(
                  backgroundImage: NetworkImage("<https://randomuser.me/api/portraits/men/5.jpg>"),
                  maxRadius: 20,
                ),
                SizedBox(width: 12,),
                Expanded(
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: <Widget>[
                      Text("Kriss Benwat",style: TextStyle( fontSize: 16 ,fontWeight: FontWeight.w600),),
                      SizedBox(height: 6,),
                      Text("Online",style: TextStyle(color: Colors.grey.shade600, fontSize: 13),),
                    ],
                  ),
                ),
                Icon(Icons.settings,color: Colors.black54,),
              ],
            ),
          ),
        ),
      ),
      body: Container()
    );

```

그러면 아래 에뮬레이터에 표시된대로 다음과 같은 결과가 제공됩니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-112.png)

### 하단 텍스트 상자를 구현하는 방법
 

채팅 세부 정보 화면 하단에 텍스트 편집기와 메시지를 보내는 버튼이 포함 된 메시징 섹션을 추가해야합니다.
 

이를 위해 `정렬`위젯을 사용하여 위젯 내부의 자식을 화면 하단에 정렬합니다.
 전체 코드는 아래 코드 스 니펫에 제공됩니다.
 

```undefined
body: Stack(
        children: <Widget>[
          Align(
            alignment: Alignment.bottomLeft,
            child: Container(
              padding: EdgeInsets.only(left: 10,bottom: 10,top: 10),
              height: 60,
              width: double.infinity,
              color: Colors.white,
              child: Row(
                children: <Widget>[
                  GestureDetector(
                    onTap: (){
                    },
                    child: Container(
                      height: 30,
                      width: 30,
                      decoration: BoxDecoration(
                        color: Colors.lightBlue,
                        borderRadius: BorderRadius.circular(30),
                      ),
                      child: Icon(Icons.add, color: Colors.white, size: 20, ),
                    ),
                  ),
                  SizedBox(width: 15,),
                  Expanded(
                    child: TextField(
                      decoration: InputDecoration(
                        hintText: "Write message...",
                        hintStyle: TextStyle(color: Colors.black54),
                        border: InputBorder.none
                      ),
                    ),
                  ),
                  SizedBox(width: 15,),
                  FloatingActionButton(
                    onPressed: (){},
                    child: Icon(Icons.send,color: Colors.white,size: 18,),
                    backgroundColor: Colors.blue,
                    elevation: 0,
                  ),
                ],
                
              ),
            ),
          ),
        ],
      ),

```

그러면 메시지를 입력 할 수있는 텍스트 필드와 메시지를 보낼 수있는 버튼이있는 메시지 섹션이 제공됩니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-113.png)

또한 메시지를위한 다른 메뉴 옵션을 추가하는 데 사용할 수있는 버튼이 왼쪽에 있습니다.
 

### 채팅 화면에서 메시지 목록 섹션을 설정하는 방법
 

이제 채팅 상세 화면에 나타나는 메시지에 대한 UI를 만들겠습니다.
 

먼저 메시지 인스턴스 객체를 반영하는 모델을 만들어야합니다.
 

이를 위해 ./models 폴더 내에 chatMessageModel.dart라는 파일을 생성하고 다음과 같이 클래스 객체를 정의해야합니다.
 

```undefined
import 'package:flutter/cupertino.dart';

class ChatMessage{
  String messageContent;
  String messageType;
  ChatMessage({@required this.messageContent, @required this.messageType});
}

```

클래스 개체는 메시지 내용 및 메시지 유형 (발신자 또는 수신자 여부에 관계없이 인스턴스 값으로 사용)을 허용합니다.
 

이제 chatDetailPage.dart에서 아래 코드 조각과 같이 표시 할 메시지 목록을 만들어야합니다.
 

```undefined
List<ChatMessage> messages = [
    ChatMessage(messageContent: "Hello, Will", messageType: "receiver"),
    ChatMessage(messageContent: "How have you been?", messageType: "receiver"),
    ChatMessage(messageContent: "Hey Kriss, I am doing fine dude. wbu?", messageType: "sender"),
    ChatMessage(messageContent: "ehhhh, doing OK.", messageType: "receiver"),
    ChatMessage(messageContent: "Is there any thing wrong?", messageType: "sender"),
  ];

```

다음으로, 아래 코드 스 니펫에 표시된대로 `정렬`위젯 위에 `스택`위젯의 하위 항목 위에 메시지에 대한 목록보기를 만들 것입니다.
 

```undefined
body: Stack(
        children: <Widget>[
          ListView.builder(
            itemCount: messages.length,
            shrinkWrap: true,
            padding: EdgeInsets.only(top: 10,bottom: 10),
            physics: NeverScrollableScrollPhysics(),
            itemBuilder: (context, index){
              return Container(
                padding: EdgeInsets.only(left: 16,right: 16,top: 10,bottom: 10),
                child: Text(messages[index].messageContent),
              );
            },
          ),
          Align(

```

이제 메시지가 아래 에뮬레이터 스크린 샷과 같이 목록 형식으로 나타납니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-114.png)

이제 화면에 메시지가 표시되지만 채팅 화면에서 원하는 스타일이 아닙니다.
 

### 보낸 사람과받는 사람을 기준으로 메시지 스타일 및 위치 지정 방법
 

이제 채팅 메시지 풍선으로 표시되도록 메시지 목록의 스타일을 지정하겠습니다.
 또한 아래 코드 스 니펫에 표시된대로`Align` 위젯을 사용하여 메시지 유형에 따라 배치 할 것입니다.
 

```undefined
ListView.builder(
  itemCount: messages.length,
  shrinkWrap: true,
  padding: EdgeInsets.only(top: 10,bottom: 10),
  physics: NeverScrollableScrollPhysics(),
  itemBuilder: (context, index){
    return Container(
      padding: EdgeInsets.only(left: 14,right: 14,top: 10,bottom: 10),
      child: Align(
        alignment: (messages[index].messageType == "receiver"?Alignment.topLeft:Alignment.topRight),
        child: Container(
          decoration: BoxDecoration(
            borderRadius: BorderRadius.circular(20),
            color: (messages[index].messageType  == "receiver"?Colors.grey.shade200:Colors.blue[200]),
          ),
          padding: EdgeInsets.all(16),
          child: Text(messages[index].messageContent, style: TextStyle(fontSize: 15),),
        ),
      ),
    );
  },
),

```

그러면 아래 에뮬레이터에 표시된대로 다음과 같은 결과가 제공됩니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-115.png)

아래에서 앱의 전체 UI에 대한 전체 데모를 볼 수 있습니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/2021-01-20-13.52.45.gif)

축하합니다!
 Flutter 및 Dart 생태계 전체에서 직관적이고 현대적인 채팅 앱 UI를 구축했습니다.
 

## 요약
 

소셜 메시징 애플리케이션은 오늘날 필수적인 커뮤니케이션 매체입니다.
 오디오 및 비디오 통화, 이미지 및 파일 첨부 등의 최신 기술과 강력한 채팅 인터페이스를 갖춘 이러한 채팅 응용 프로그램은 커뮤니케이션을 훨씬 더 효율적으로 만들었습니다.
 이러한 앱은 우리를 위해 세상을 더 작게 만들었습니다.
 

이 기사의 주요 목적은 Flutter 생태계에서 현대적인 디자인으로 채팅 애플리케이션을위한 간단하고 직관적 인 UI를 개발하는 방법을 보여주기위한 것입니다.
 단계별 구현은 앱의 UI에 대한 자세한 쇼케이스를 제공하고 Flutter 코딩 환경에 대한 개요도 제공했습니다.
 

이 튜토리얼이 Flutter를 사용하여 다음 채팅 애플리케이션을 만드는 데 도움이되기를 바랍니다.
 

또한 시장에 나와있는 최고의 Flutter 채팅 앱 템플릿에서 채팅 앱 UI 및 기능 개발에 대한 영감을 얻을 수 있습니다.
 그들도 확인하십시오.
 
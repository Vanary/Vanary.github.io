# 입문 개발자가 느낀 '자바스크립트 객체 이런게 되다니!' Top 3

코드스쿼드 프론트엔드 레벨2로 프로그래밍과 자바스크립트에 입문한지 2개월이 되었습니다.
입문 커리큘럼을 따라 공부해보니 객체 개념이 참 재미있더라구요.

오늘은 객체를 가지고 실험하며 즐거웠던 기억을 되새기면서, 가장 짜릿했던 세 가지 '오 이런 게 되다니!' 순간을 공유하고자 합니다.  


## 3위 : 조건문을 대체하기

```javascript
function guessWho(name) {
    if(name === '영희') {
        return '안녕 영희야!'
    } else if(name === '철수') {
        return '철수 철수!'
    } else if (name === '에미넴') {
        return `Yo wassup!`
    }
    // 기타등등...  switch로 만들면 이것보다 더 길었겠죠?
}
```
이렇게 험하게 조건문이 반복된 함수가 있을 때 아래와 같이 객체를 이용해 바꿀 수 있었습니다.

```javascript
function guessWho(name) {
    const nameList = {
        영희() {return '안녕 영희야!'},
        철수() {return '철수 철수!'},
        에미넴() {return 'Yo wassup!'},
    };
    
    return nameList[name]();
}
```

이렇게 바꾸니 조건을 추가/삭제/변경할 때 코드를 읽기 쉬웠습니다.
해보지는 않았지만 조건들이 객체이니 아예 다른 곳에 빼두고 필요할 때 가져다 써서 함수를 읽기좋은 길이로 줄일 수도 있겠구요.  
사소하게는 코드 에디터에서 조건 부분만 한번에 접을 수 있다는 장점도 있었습니다.


## 2위 : 메서드 체이닝


>*소개팅 웹서비스를 기획하던 친구가 썸이 없다 대성통곡을 하네.js*
```javascript
[여사친_남사친_배열].map('이상형')
    .filter('배열 내 이상형 존재')
    .reduce('이상형 맞는 그룹들을 묶어보기', [])
    .some('혹시 나를 이상형으로 꼽은 친구가?')
// false
```


배열을 조작할 때 단계마다 변수에 저정하고 다시 메서드를 호출하는 대신에, 한번에 연속으로 메서드를 거치게(메서드 체이닝) 할 수 있다는 걸 배웠습니다.

도구가 주어졌으니 그냥 그것만 감사하게 쓸 뻔 했는데,
원하는 동작을 구현하기 위해 고민하다가 제가 직접 체이닝 가능한 메서드를 만들 수 있다는 사실을 깨달았습니다.

```javascript
const jungle = {
    message : '정글 숲을 지나서가자',
    엉금() {
        this.message = ``;
        console.log('엉금');
        return this
        },
    기어서() {
        this.message += '악어떼가 ';
        console.log('기어서');
        return this
    },
    가자() {
        this.message += '나온다';
        console.log('가자');
        return this
    },
    늪지대를기어서가면() {
        this.message += '(악어떼!)'
        console.log('늪지대를 기어서가면');
        console.log(this.message);
        return true
    },
};
console.log(jungle.message); 
// 정글 숲을 지나서가자
jungle.엉금().엉금().기어서().가자().늪지대를기어서가면(); 
// 엉금 엉금 기어서 가자 늪지대를 기어서가면
// 악어떼가 나온다(악어떼!)
```

위 예시처럼 객체를 통해 내 필요에 맞는 메서드 체인을 만들 수 있었습니다.

이렇게 메서드 체이닝을 구현하면서 참 좋았던 건, 프로그래머가 눈으로 데이터가 변해가는 과정을 확인할 수 있다는 점이었습니다.

메서드를 쪼개서 수직으로 코드를 나열할 수도 있지만, (제가 문과라 그런지) 사람이 쓴 글처럼 왼쪽에서 오른쪽으로 읽어서 흐름을 이해할 수 있는게 친근하고 재미있게 느껴졌습니다.


## 1위 : 배열/함수에 속성 추가하기


![전자저울 있는 캐리어](http://image.auction.co.kr/itemimage/16/83/3d/16833d5a66.jpg)

배열과 함수도 객체의 인스턴스이기 때문에, 속성을 추가할 수 있습니다.
글로만 읽었을 때는 별다른 감흥이 없었는데, 실제로 적용한 사례를 보니 너무나 재미있었습니다. 겉보기와 다른 숨겨진 기능을 넣어둘 수 있다니!  
(유지보수하기 힘들지만 장난치기 좋은 기능이 있다니!!)

위 사진에 나온 '전자저울 캐리어'처럼, 담긴 물건의 총 무게를 보여주는 배열을 만들어 보겠습니다.


```javascript
const carrier = [
    {name: '선크림', weight: 10},
    {name: '밀짚모자', weight: 25},
    {name: '수영복', weight: 40},
    {name: '컵라면', weight: 15},
];

carrier.전자저울 = function() {
    let sum = 0;
	for (let el of this){
		sum+= el.weight;
    }
    console.log(`${sum} 근`);
};

carrier.전자저울(); // 90 근
```

이렇게 배열에 추가된 속성은 일반적으로 배열을 조작하는 데는 영향을 주지 않는게 또 흥미롭습니다.

```javascript
console.log(carrier.length) 
// 4 

for (let item of carrier) {
    console.log(item);
}
// { name: '선크림', weight: 10 }, ...
// { name: '컵라면', weight: 15 } ='전자저울'은 나오지 않습니다!

const newCarrier = carrier.slice();
newCarrier.전자저울 // undefined
```

숨겨둔 속성 정보는 배열이 아닌 객체로 접근할 때에야 비로소 볼 수 있구요.
```javascript
Object.keys(carrier) 
// ['0', '1', '2', '3', '전자저울'] 

const newCarrier = Object.assign([], carrier);
newCarrier.전자저울() // 90 근
```

함수에도 속성을 추가할 수 있지만, ~~장난치는 것 이외에~~ 아직 쓸모를 못 찾았습니다. 함수 내부의 변수에 접근할 수 없으니 말이죠.


```javascript
function secretChest() {
    const tresure = 'One Piece';
    const lock = [02337];
    let password = '';

    console.log( (lock === password) ? tresure : '함정이다!' );
}
secretChest.trueKeyHole = function() {
    const trueTresure = '원피스를 향한 여정과 동료들이 진정한 보물이다'
    console.log(trueTresure);
};

secretChest() // 어떻게 해도 '함정이다!'
secretChest.trueKeyHole() // 진짜 보물
```


---

여기까지가 객체를 가지고 놀면서 재밌었던 Top 3 경험이었습니다.
조금씩 공부하면서 그때그때 이런저런 가능성을 생각해보니 질리지 않고 새로운 개념을 익힐 수 있었던 것 같습니다.

이제 다음 커리큘럼으로 넘어가면서 새로운 개념과 도구들을 접하게 될텐데, 새로 장난감 세트를 받아본 아이의 마음으로 즐겁게 하나씩 풀어보고 놀아봐야겠습니다.

또 다른 흥미로운 장난감을 찾으면 눈을 빛내며 놀이터에 들고 오겠습니다.
---
layout: post
title: "메모 사용 vs. 활용 콜백 실용가이드"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/react_callback.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/react_callback.png?fit=730%2C487&ssl=1)

HTML 페이지의 크기와 복잡성이 증가함에 따라 효율적인 React 코드를 만드는 것이 그 어느 때보다 중요해졌다. 대형 구성요소를 재렌딩하는 데는 많은 비용이 들며, 단일 페이지 애플리케이션(SPA)을 통해 브라우저에 상당한 양의 작업을 제공하면 처리 시간이 길어지고 잠재적으로 사용자를 몰아낼 수 있습니다.

본 자습서는 개발자가 코드와 관련이 없는 성능 문제를 피하기 위해 Retact가 제공하는 두 가지 방법인 useMemo와 useCallback을 살펴본다. 이러한 기능은 렌더링 간에 개체를 보존하고 응용 프로그램 성능을 향상시키는 데 도움이 됩니다.

즐겨찾는 웹 서버에 응답 프로젝트를 만들어 계속 진행합니다.

## 기능에 대한 성능 최적화 대응

Retact는 이미 DOM 요소를 다시 생성하지 않도록 `Ract.memo()`를 제공하지만 이 방법은 함수에서 작동하지 않습니다. 따라서, 자바스크립트에서는 일류 시민임에도 불구하고, 기능들은 잠재적으로 모든 용도로 다시 만들어질 수 있다.

사용메모와 사용콜백은 특정 상황에서 기능을 재생성하거나 다시 실행하지 않도록 돕는다. 항상 유용한 것은 아니지만, useMemo 또는 useCallback은 대량의 데이터나 동작을 공유하는 많은 구성 요소를 처리할 때 현저한 차이를 일으킬 수 있다. 예를 들어, 주식 또는 디지털 통화 거래 플랫폼을 만들 때 특히 유용할 수 있는 경우가 있습니다.

## use Callback이란?

응답으로 구성 요소를 감싸는 중입니다.메모(`)는 코드를 재사용하려는 의도를 나타냅니다. 매개 변수로 전달된 함수로 자동으로 확장되지는 않습니다.

react는 `use Callback`으로 감싸면 함수에 대한 참조를 저장합니다. 렌더링 시간을 줄이기 위해 이 참조를 새 구성 요소에 속성으로 전달합니다.

## 실제 콜백 예제

콜백은 다른 인라인 방법과 다를 바 없습니다. JavaScript에서와 마찬가지로 포장된 기능을 사용할 수 있습니다.

다음 예제 구성 요소를 고려하십시오.

```js
function TransactionList({props}){
   const purchase = useCallback(event =>{
   const id = event.currentTarget.id;
   const price = event.currentTarget.getAttribute("value");
   // perform purchase operation and render elsewhere
   // ...
   console.log("Purchased " + id + " at " + price);
   }, [props]);
   return(<TransactionListItems onItemClick={purchase}/>)
}
 
export default TransactionList;
```

이 개체는 `TransactionListItems` 하위 구성 요소에 의존합니다.

```js
function TransactionListItems({onItemClick}){
    // potentially large list typically from a database
    var prices = [
        {"id": 1, "price": 1000.14},
        {"id": 2, "price": 2000.05},
        {"id": 3, "price": 300.00},
        {"id": 4, "price": 400.00}];
    var priceItems = [];
    // render items
    prices.forEach(priceElement =>{
       priceItems.push(<div style={"width": "100%", "height": "2em"} key={priceElement.id} name="child-price-item" id={priceElement.id} value={priceElement.price} onClick={onItemClick}>${priceElement.price}</div>);
    });
    // display
    return (<div name="prices">
        {priceItems}
    </div>)
}
export default React.memo(TransactionListItems);
```

거래목록에는 상품 구매 기능이 포장돼 있어 일정 가격에 상품을 구입할 수 있다. `TransactionList Items`의 각 항목은 이 방법을 사용합니다. 이 경우 useCallback은 불필요한 함수 렌더링을 없앤다. 이 기능이 없으면 자바스크립트는 클릭 한 번으로 `구입`을 할 수 있다.

우리 주식 시장의 예에서, 하루 종일 빠르게 매수될 수 있다. 이 `이용 콜백` 방식은 캐주얼 트레이더들이 다음 기회를 놓치는 것을 피하고 소프트웨어가 실시간으로 거래를 수행하는 데 도움이 될 수 있다.

## use Memo는 무엇입니까?

useMemo 함수는 비슷한 목적으로 사용되지만 전체 함수가 아닌 반환 값을 내재화한다. React는 동일한 기능에 핸들을 전달하는 대신 함수를 건너뛰고 파라미터가 변경될 때까지 이전 결과를 반환합니다.

따라서 필요할 때까지 잠재적으로 비용이 많이 드는 작업을 반복적으로 수행하지 않아도 됩니다. 함수에 정의된 변경 변수는 `useMemo`의 동작에 영향을 미치지 않으므로 이 방법을 주의하여 사용하십시오. 예를 들어 타임스탬프 추가를 수행하는 경우 이 방법은 시간이 변경되는 것을 상관하지 않고 함수 매개 변수가 다를 뿐입니다.

## 반응 함수 메모 사용

useMemo의 작동 방식을 보려면 사용자가 특정 가격대에 대해 필터링하려는 총계를 업데이트해야 하는 경우를 생각해 보십시오. 이러한 활용 사례는 증권 또는 디지털 통화 시장과 협력하는 대량의 거래 플랫폼에게 타당하다. 거래 횟수는 특히 플랫폼이 자산을 단편적으로 매각할 때 많을 수 있다.

이 튜토리얼의 경우 공통 속성 집합을 만드는 부모 클래스를 만듭니다.

```php
function Purchases({props}){
    const globalPurchaseStats = {
        "purchases": [1, 3, 4, 12, 200, 100],
        "total": 0,
        "greaterThan": 10
    };
    const [globalPurchaseState, setGlobalPurchaseState] = useState(globalPurchaseStats);
    return (<div>
        <PurchasesMade setState={setGlobalPurchaseState}/>
        <PurchaseStatistics purchaseState={globalPurchaseState} />
    </div>);
}
export default Purchases;
```

상태 개체는 각 어린이가 동일한 개체를 조작할 수 있도록 허용합니다. 또한 `구입` 어레이의 변경으로 인해 메모리 기능이 실행되도록 합니다.

다음으로 글로벌 상태 변수의 세터(Setter)를 통과하는 "PurchasesMade"를 구축한다.

```js
function PurchasesMade({setState}){
    const update_purchases = () =>{
        const priceArr = [];
        let priceStr = "";
        let total = 0;
        for(var i = 0; i < 10; i++){
            const newPrice = Math.random();
            priceArr.push(newPrice);
            if(priceStr.length > 0){
                priceStr += ", ";
            }
            priceStr += newPrice * 100;
            total += newPrice * 100;
        };
        console.log(priceArr);
        console.log(priceStr)
        setState({"total": total, "purchases": priceArr});
        document.getElementById("current-purchases").innerText = priceStr;
    };
    return <div stye={"width": "100%", "height": "2em"} className="purchases" id={"current-purchases"} onClick={update_purchases}>No Purchases yet</div>
}
export default PurchasesMade;
```

마지막으로, 실제 상태 객체를 사용하여 `구매 통계` 구성요소를 작성합니다.

```js
function PurchaseStatistics({purchaseState}){
    const filter_purchases = useMemo(() => {
        console.log("Recalculating Purchase Statistics");
        const greaterThan = purchaseState.greaterThan;
        let purchases = purchaseState.purchases;
        let total = 0;
        if(greaterThan > 0) {
            purchases = purchases.filter(price => price > greaterThan);
        }
        purchases.forEach(price =>{
            total += price;
        });
        total += 0.0;
        document.getElementById("filter-total").textContent = "Filtered: $" + total;
    }, [purchaseState.purchases, purchaseState.greaterThan])
    return(<button onClick={filter_purchases}>No Update Required</button>)
}
export default PurchaseStatistics;
```

사용자는 업데이트를 useMemo로 포장하여 변경 시와 같이 필요할 때 입력을 그대로 유지할 때 기능을 실행하지 않고 강제 업데이트를 할 수 있다. 이것은 높은 불안감의 기간 동안 상인들이 행복하게 지낼 수 있도록 도와준다.

또한 웹 사이트가 `구입` 배열을 업데이트할 때까지 저장된 결과를 반환하여 처리 시간을 단축할 수 있습니다. 유사한 코드를 사용하여 자산 판매 동작을 작성할 수 있습니다.

## use Callback 대 use Memo in React로 작업

사용콜백과 사용메모 기능은 겉보기에도 비슷하다. 그러나 각각에 대한 특별한 사용 사례가 있습니다.

다음과 같은 경우 `사용법 콜백`으로 기능 포장:

- 사용자 메서드를 속성으로 받아들이는 `Ract.memo()`의 기능 구성 요소 포장
- 다른 후크에 대한 종속성으로 기능 전달

memo 사용:

- 입력이 점진적으로 변화하는 함수의 경우
- 데이터 값이 너무 커서 메모리 문제가 발생할 수 있는 경우
- 매개 변수가 너무 작지 않아서 비교 비용이 래퍼의 사용을 능가하는 경우

콜백은 그렇지 않으면 모든 통화로 코드가 다시 컴파일될 때 잘 작동합니다. 암기 결과는 시간이 지남에 따라 입력이 점차 변할 때 반복적으로 함수를 호출하는 비용을 줄이는 데 도움이 될 수 있다. 반면에, 거래 사례에서, 우리는 지속적으로 변화하는 주문서에 대한 결과를 외우고 싶지 않을 수도 있습니다.

## 콜백 사용 및 메모 사용 안티패턴 사용메모(Memo) 안티패턴 사용

모든 기능에 대해 콜백 사용이나 메모 사용 등을 활용할 수 있다고 생각하는 것은 유혹적일 수 있지만 그렇지 않다. 래핑 기능과 관련된 오버헤드가 있습니다. 각 호출에는 함수 호출을 풀고 진행 방법을 결정하는 추가 작업이 필요합니다.

`update_purchases` 함수가 콜백이 아닌 반면 `purchase` 함수는 콜백이 아닙니다. 업데이트 기능은 한 번 생성되고 공유되지 않기 때문에 기준을 충족하지 않습니다. 반대로 `트랜잭션 리스트 항목`의 각 변경 리스트 항목은 `구입` 기능을 사용합니다.

마찬가지로, 우리는 use Memo를 사용하여 구매한 것에 대한 업데이트를 포장했지만, 자주 바뀌는 데이터를 다루는 기능은 포장하지 않을 것이다. 매번 입력 매개 변수에 대해 반응 검사를 수행한 다음 함수를 호출해야 합니다.

## 사용 사례 식별

이 포장지를 사용할지 여부를 결정할 때는 먼저 기존 성능을 고려하십시오. 이러한 기능을 사용할 경우 얻는 이점은 상대적으로 적기 때문에 코드를 개선하는 것부터 시작하는 것이 좋습니다.

프런트 엔드 응용 프로그램 성능 모니터링 도구의 도움을 받아 잠재적인 취약점을 식별합니다. 그런 다음 JavaScript를 리팩터링해 보십시오. 이 작업을 수행할 수 없는 경우 이 가이드를 사용하여 앞으로 나아가는 최선의 옵션을 결정합니다.

## 결론

use Callback과 use Memo 기능은 React를 미세 조정하기 위한 도구이다. 각 제품의 사용 방법과 사용 시기를 알면 잠재적으로 애플리케이션 성능이 향상될 수 있습니다. 그럼에도 불구하고, 어떤 래퍼도 제대로 작성되지 않은 코드베이스의 대체물이 되지 않는다.

여기서는 이러한 툴의 사용 방법을 이해할 수 있는 가이드를 제공했지만, 이러한 툴을 사용하면 비용이 발생한다는 점을 유념하십시오.
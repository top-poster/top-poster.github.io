---
layout: post
title: "판매자와 함께 온라인 상점 구축"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/saleor.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/saleor.png?fit=730%2C487&ssl=1)

현대 웹 개발 시대에, 머리 없는 솔루션은 어떤 종류의 제품이라도 만드는 인기 있는 방법이 된다. 예를 들어, 컨텐츠 관리 시스템, 전자상거래 솔루션 등이 있습니다. 이 기사에서는 업계에서 인기가 높은 헤드리스 전자상거래 제품 Saleor.io을 이용해 온라인 스토어를 구축하는 방법을 알아보려고 한다.

## Saleor.io이란 무엇입니까?

셀러는 헤드리스 그래프QL E커머스 플랫폼으로, 사업주와 개발팀이 더 짧은 시간 내에 제품을 더 빠르게 구축할 수 있도록 지원합니다. 전자 상거래 스토어를 확장 가능한 방식으로 운영하는 데 필요한 모든 모듈을 제공합니다.

### 판매자 또는 아키텍처

판매자에는 백엔드 솔루션 및 인프라, 전자상거래 스토어를 위한 관리 대시보드, 전자상거래 클라이언트 측 솔루션을 포함한 세 가지 중요한 모듈이 포함되어 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/saleor-sdk-flowchart.png?resize=730%2C161&ssl=1)

당사의 스토어(또는 클라이언트)는 Sale 또는 SDK를 통해 백엔드와 통신합니다. 동시에 관리 대시보드는 GraphQL API를 사용하여 백엔드와의 통신을 수행합니다.

이 튜토리얼에서는 전자상거래 스토어에 대한 판매 또는 백엔드 및 관리 대시보드를 설정하는 방법을 알아봅니다. 전자 상거래 애플리케이션을 위한 스토어 프런트(클라이언트)를 구축하는 방법도 알아보겠습니다.

## 설치 및 설정

참고: 개발 환경에서 판매자를 설정하고 실행하려면 도커를 사용하는 것이 가장 좋습니다.

우리는 개발 환경에서 설치하는데 도커를 사용할 것입니다.

```bash
git clone https://github.com/mirumee/saleor-platform.git --recursive --jobs 3
cd saleor-platform
docker-compose build
```

위의 명령을 사용하여 로컬 컴퓨터에 Docker 이미지를 작성합니다.

```css
docker-compose run --rm api python3 manage.py migrate
docker-compose run --rm api python3 manage.py collectstatic --noinput
```

Saleor 서버는 Python에서 실행되므로, 우리를 위해 서버와 데이터베이스 마이그레이션을 설정합니다.

```undefined
//first command is optional
docker-compose run --rm api python3 manage.py populatedb 
docker-compose run --rm api python3 manage.py createsuperuser
```

그런 다음 더미 데이터로 데이터베이스를 채운 다음(선택 사항) 관리 대시보드에 대한 슈퍼 사용자를 생성합니다.

마지막으로 서비스를 실행하려면 다음 명령을 사용합니다.

```undefined
docker-compose up
```

이제 URL을 방문하면 다음과 같은 정보가 표시됩니다.

`http://localhost:3000/` – 클라이언트 측
`http://localhost:8000/graphql/` – GraphQL 놀이터
`http://localhost:9000/` – 관리 대시보드

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/information-about-store.png?resize=730%2C346&ssl=1)

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/final-reduction.png?resize=730%2C358&ssl=1)

## 판매자를 사용하여 스티커 스토어 구축

이제 온라인 스토어를 구축하기 위한 기본 Salesor를 설정되었습니다. 세일러 매장 프론트를 커스터마이징하여 개발자 스티커 매장을 만들어 봅시다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/devops.png?resize=730%2C370&ssl=1)

다음은 우리가 중점적으로 다룰 모든 중요한 모듈/기능입니다.

- 범주를 기준으로 제품 나열
- 피처링 제품
- 검색상품
- 사용자 – 로그인/가입
- 장바구니에 넣기
- 체크 아웃 상품
- 지급

그래프QL 서버에서 데이터를 얻기 위해서는 아폴로 클라이언트가 필요합니다. 내부적으로 @saleor/sdk가 아폴로를 사용하기 때문이다.

리액트 응용 프로그램에서 아폴로 클라이언트를 설정하겠습니다.

```coffeescript
npm install @saleor/sdk
```

```js
import { SaleorProvider, useAuth } from "@saleor/sdk";

const config = { apiUrl: "http://localhost:8000/graphql/" };
const apolloConfig = {
  /* 
    Optional custom Apollo client config.
    Here you may append custom Apollo cache, links or the whole client. 
    You may also use import { createSaleorCache, createSaleorClient, createSaleorLinks } from "@saleor/sdk" to create semi-custom implementation of Apollo.
  */
};

const rootElement = document.getElementById("root");
ReactDOM.render(
  <SaleorProvider config={config} apolloConfig={apolloConfig}>
    <App />
  </SaleorProvider>,
  rootElement
);

const App = () => {
  const { authenticated, user, signIn } = useAuth();

  const handleSignIn = async () => {
    const { data, dataError } = await signIn("admin@example.com", "admin");

    if (dataError) {
      /**
       * Unable to sign in.
       **/
    } else if (data) {
      /**
       * User signed in succesfully.
       **/
    }
  };

  if (authenticated && user) {
    return <span>Signed in as {user.firstName}</span>;
  } else {
    return <button onClick={handleSignIn}>Sign in</button>;
  }
};
```

이제 구성 요소를 구축하고 GraphQL 서버를 사용하여 데이터를 나열하고 서버에 업데이트해야 합니다.

@saleor/sdk는 많은 맞춤형 훅을 제공합니다. 그것은 우리가 그 가게를 짓는데 많은 개발 시간을 절약할 수 있도록 도와줍니다. 그 중 몇 가지는 다음과 같습니다.

- useAuth – 인증 메커니즘을 처리하는 인증 후크. 로그인한 사용자 정보를 제공하고 사용자가 인증인지 등을 확인합니다.
- `cart 사용` – cart에 제품을 추가하고 cart에서 제품을 제거하는 기능을 제공합니다.
- use SignOut – 스토어에서 사용자를 로그아웃합니다.
- 사용자 세부 정보 사용 – 로그인한 사용자에 대한 정보를 반환합니다.
- 사용자 세부 정보 사용 – 카트에 있는 모든 항목을 체크 아웃하는 기능을 제공합니다.

이런 맞춤 훅이 많다. 나는 단지 여기서 몇 가지 중요한 것들을 지적하고 있을 뿐이다. @saleor/sdk에서 모든 후크를 확인할 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/sdk-react.png?resize=730%2C397&ssl=1)

이제 우리는 서버에서 제품과 제품 카테고리를 가져올 것입니다. 그러나 설치 시 더미 데이터로 데이터베이스를 채우는 단계를 건너뛰었다면 `제품`과 `카테고리`를 데이터베이스에 추가해야 할 수도 있습니다.

우리는 매장 관리 대시보드를 통해 그렇게 할 수 있습니다. 관리 대시보드의 `카탈로그 => 제품`으로 이동하여 제품을 추가합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/Catalog.png?resize=730%2C1096&ssl=1)

범주도 마찬가지입니다. 이렇게 하면 서버에서 가져올 `카테고리`와 `제품`을 가질 수 있습니다.

두 가지를 모두 추가하면 GraphQL 쿼리를 작성하여 제품과 범주를 가져올 수 있습니다.

```undefined
query ProductsList {
    products(first: 5) {
      edges {
        node {
          id
          name
          pricing{
            onSale
            discount{
              gross{
                amount
              }
              currency
            }
          }
          description
          category{
            id
            name
          }
          images{
            id
            url
          }
        }
      }
    }
    categories(level: 0, first: 4) {
      edges {
        node {
          id
          name
          backgroundImage {
            url
          }
        }
      }
    }
  }
```

서버에서 제품과 범주를 모두 가져옵니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/productlist.png?resize=730%2C362&ssl=1)

이 데이터를 사용하여 저희 매장에 있는 모든 제품과 카테고리를 나열할 수 있습니다.

```coffeescript
import React, { useRef, useState } from "react";
import {
  ProductsList_categories,
  ProductsList_shop,
  ProductsList_shop_homepageCollection_backgroundImage,
} from "./gqlTypes/ProductsList";
import { structuredData } from "../../core/SEO/Homepage/structuredData";
// import noPhotoImg from "../../images/no-photo.svg";
import ProductItem from '../../components/product/ProductItem'
import Boundary from './boundary'
const Page = ({ loading, categories, products, backgroundImage, shop }) => {
  const [columnCount, setColumnCount] = useState(10);
  const categoriesExist = () => {
    return categories && categories.edges && categories.edges.length > 0;
  };
  const productListWrapper = useRef(null);
  return (
    <>
      <script className="structured-data-list" type="application/ld+json">
        {structuredData(shop)}
      </script>
      <main className="content">
        <section className="product-list-wrapper">
          <Boundary>
            <div
              className="product-list"
              ref={productListWrapper}
              style={ gridTemplateColumns: `repeat(${columnCount}, 160px)` }
            >
              {products.edges.map((product, index) => {
                return (
                  <ProductItem
                    foundOnBasket={false}
                    product={product.node}
                    key={`product-skeleton ${index}`}
                  />
                )
              })}
            </div>
          </Boundary>
        </section>
      </main>
    </>
  );
};
export default Page;
```

지금까지 우리는 제품과 카테고리 리스트 기능을 가지고 있습니다. 이제 제품을 바구니에 추가하고 사용자 로그인/가입 기능을 추가하는 방법을 구현해야 합니다.

## 카트에 추가 기능

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/devops.gif?resize=730%2C411&ssl=1)

사용자가 카트에 추가 버튼을 클릭하면 @saleor/sdk 사용자 지정 후크 `useCart`를 사용하여 카트에 항목을 추가합니다.

```coffeescript
import React from 'react';
import { displayMoney } from './utils';
import { useCart } from "@sdk/react";
const ProductItem = ({
    product,
    onOpenModal,
    displaySelected,
    foundOnBasket
}) => {
    const { addItem, removeItem } = useCart()
    const onClickItem = () => {
        if (product.id) {
            onOpenModal();
            displaySelected(product);
        }
    };
    const onAddToBasket = () => {
        addItem(product.id, 1)
    };
    return (
        <div
            className={`product-card ${!product.id ? 'product-loading' : ''}`}
            style={
                border: foundOnBasket ? '1px solid #cacaca' : '',
                boxShadow: foundOnBasket ? '0 10px 15px rgba(0, 0, 0, .07)' : 'none'
            }
        >
            {foundOnBasket && <i className="fa fa-check product-card-check" />}
            <div
                className="product-card-content"
                onClick={onClickItem}
            >
                <div className="product-card-img-wrapper">
                    {product.images[0].url ? (
                        <img
                            className="product-card-img"
                            src={product.images[0].url}
                        />
                    ) : null}
                </div>
                <div className="product-details">
                    <h5 className="product-card-name text-overflow-ellipsis margin-auto">{product.name || null}</h5>
                    <p className="product-card-brand">{product.brand || null}</p>
                    <h4 className="product-card-price">{product.price ? displayMoney(product.price) : null}</h4>
                </div>
            </div>
            {product.id && (
                <button
                    className={`product-card-button button-small button button-block ${foundOnBasket ? 'button-border button-border-gray' : ''}`}
                    onClick={onAddToBasket}
                >
                    {foundOnBasket ? 'Remove from basket' : 'Add to basket'}
                </button>
            )}
        </div>
    );
};
export default ProductItem;
```

여기서는 사용자 정의 후크 `사용 카트`를 구현한다. 이를 통해 카트에 항목을 추가하고 카트에서 항목을 제거할 수 있습니다.

```cpp
const { addItem, removeItem } = useCart()
```

## 카트 표시

카트에서 모든 항목을 가져오려면 동일한 사용자 지정 후크를 사용할 수 있습니다.

```cpp
const {
    items,
    removeItem,
    subtotalPrice,
    shippingPrice,
    discount,
    totalPrice,
  } = useCart();
```

카트에 추가된 모든 아이템을 우리에게 돌려줍니다. 이제 우리는 이 `아이템`을 이용하여 전자 상점에서 그것을 보여줄 수 있다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/t-shirt.png?resize=730%2C1249&ssl=1)

## 사용자 로그인/가입

로그인 기능을 구현하는 것은 매우 간단합니다. 로그인 후 데이터를 반환하는 @saleor/sdk의 사용자 정의 후크 `useSignIn`을 사용해야 합니다.

```coffeescript
import "./scss/index.scss";
import * as React from "react";
import { useSignIn } from "@sdk/react";
import { maybe } from "@utils/misc";
import { Button, Form, TextField } from "..";
const LoginForm = ({ hide }) => {
  const [signIn, { loading, error }] = useSignIn();
  const handleOnSubmit = async (evt, { email, password }) => {
    evt.preventDefault();
    const authenticated = await signIn({ email, password });
    if (authenticated && hide) {
      hide();
    }
  };
  return (
    <div className="login-form">
      <Form
        errors={maybe(() => error.extraInfo.userInputErrors, [])}
        onSubmit={handleOnSubmit}
      >
        <TextField
          name="email"
          autoComplete="email"
          label="Email Address"
          type="email"
          required
        />
        <TextField
          name="password"
          autoComplete="password"
          label="Password"
          type="password"
          required
        />
        <div className="login-form__button">
          <Button type="submit" {...(loading && { disabled: true })}>
            {loading ? "Loading" : "Sign in"}
          </Button>
        </div>
      </Form>
    </div>
  );
};
export default LoginForm;
```

사용자 등록은 로그인 기능과 약간 다릅니다. 우리는 GraphQL 돌연변이를 사용해야 한다.

```bash
mutation RegisterAccount(
    $email: String!
    $password: String!
    $redirectUrl: String!
  ) {
    accountRegister(
      input: { email: $email, password: $password, redirectUrl: $redirectUrl }
    ) {
      errors {
        field
        message
      }
      requiresConfirmation
    }
  }
```

사용자가 성공적으로 등록하면 지정된 URL로 리디렉션됩니다.

## 카트 체크아웃 구현

카트 체크아웃은 기능 측면에서 완료해야 하는 4단계입니다. 그 이유는 다음과 같습니다.

- 사용자로부터 주소 가져오기
- 지급
- 컨펌
- 배송방법구성

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/shipping-address.png.gif?resize=730%2C411&ssl=1)

다른 기능과 마찬가지로 체크아웃에 필요한 모든 기능을 제공하는 사용자 정의 후크 `usse Checkout`을 사용할 수 있다.

### 사용자로부터 주소 가져오기

우리는 사용자로부터 주소를 받아야 합니다. 새 주소를 추가하도록 요청하거나 기존 주소를 선택할 수 있습니다. 이를 위해 use Checkout의 기능을 사용할 수 있습니다.

```cpp
const {
    checkout,
    setShippingAddress,
    selectedShippingAddressId,
  } = useCheckout();
```

### 지급

판매인은 지불을 위해 다음과 같은 기능을 제공합니다.

```cpp
const {
    checkout,
    billingAsShipping,
    setBillingAddress,
    setBillingAsShippingAddress,
    selectedBillingAddressId,
    availablePaymentGateways,
    promoCodeDiscount,
    addPromoCode,
    removePromoCode,
    createPayment,
  } = useCheckout();
```

createPayment 기능을 사용하여 주문에 대한 트랜잭션을 완료할 수 있습니다.

```undefined
const handleProcessPayment = async (
    gateway,
    token,
    cardData
  ) => {
    const { dataError } = await createPayment(gateway, token, cardData);
    const errors = dataError?.error;
    changeSubmitProgress(false);
    if (errors) {
      setGatewayErrors(errors);
    } else {
      setGatewayErrors([]);
      history.push(CHECKOUT_STEPS[2].nextStepLink);
    }
  };
```

### 확인/검토

이는 사용자가 지금까지 모든 것이 올바른지 검토하고 확인하기 위한 것입니다. 이 단계에서는 다음 기능을 사용하여 체크아웃을 완료할 수 있습니다.

```cpp
const { checkout, payment, completeCheckout } = useCheckout();
```

## 결론

요약하자면, 판매자는 전자상거래 개발을 위한 강력한 GraphQL이다. SDK에서 제공하는 모든 필수 기능을 제공합니다. 결과적으로, 우리는 전자 상거래 스토어의 UI에만 집중할 수 있습니다. 백엔드 관리 패널의 복잡한 설정을 방지하는 데 도움이 됩니다.
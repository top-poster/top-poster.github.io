---
layout: post
title: "Redux Toolkit과 함께 TypeScript 사용
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/Using-TypeScript-with-Redux-Toolkit.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Using-TypeScript-with-Redux-Toolkit.png?fit=730%2C487&ssl=1)

Redux를 사용하는 React의 상태 관리는 매우 어려울 수 있습니다.
 핵심 개념과 아키텍처 패턴에 대해 머리를 감싸는 것 외에도 훨씬 더 어렵고 혼란스럽게 만들 수있는 엄청난 양의 상용구가 있습니다.
 Redux를 만든 Dan Abramov는 좌절감을 나보다 더 잘 설명합니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Dan-Abramov-Redux-Tweet.png?resize=603%2C480&ssl=1)

## 구출을위한 Redux Toolkit
 

적절한 수준의 추상화를 갖춘 Redux Toolkit은 Redux로 작업하는 것을 덜 겁 내고 더 직관적으로 만드는 더 성공적인 시도 중 하나입니다.
 대규모 분산 프로젝트에서 쉽게 확장하고 적용 할 수 있습니다.
 그러나 이것이이 게시물의 주요 요지는 아닙니다.
 

Redux Toolkit이 현명한 선택 인 이유에 대해 더 자세히 살펴볼 수 있습니다.
 대신 TypeScript에서 RTK를 사용하는 방법에 집중하겠습니다.
 

Redux Toolkit의 세심한 접근 방식과 TypeScript의 유형 안전성을 결합하면 더 강력하고 유지 관리 가능하며 확장 가능한 Redux 프로젝트가 생성됩니다.
 그러나 TypeScript를 사용하여 RTK를 설정하는 것이 항상 간단한 것은 아닙니다. 이것이이 기사에서 설명 할 것입니다.
 

### 이 게시물에서 다룰 내용
 

- 설치 및 초기 설정
 
- 상점 구성
 
- Redux 프로젝트를 구성하는 방법
 
- `createSlice`로 액션 리듀서 만들기
 
- 썽크, 오류 처리 및로드 상태와 비동기
 
- `useDispatch` 및`useSelector` 후크를 사용하여 저장소에 연결
 

## 설치 및 초기 설정
 

React-Redux에서 막 시작하는 경우`create-react-app`으로 프로젝트 설정이 쉽습니다.
 `--template redux-typescript` 플래그가 트릭을 수행합니다!
 

```undefined
npx create-react-app my-app --template redux-typescript
//or
yarn create-react-app my-app --template redux-typescript
```

다음과 같은 프로젝트 구조로 끝나야합니다.
 특히 흥미로운 것은`app` 디렉토리입니다. 여기에는`store`와`feature` 디렉토리가 포함되어 있으며이 디렉토리에는 앱의 주요 기능이 하위 디렉토리로 포함되어야합니다.
 나중에 다시 살펴 보겠습니다.
 

### 기존 프로젝트에서 설정
 

Redux Toolkit을 기존 React 프로젝트에 드롭하는 것도 똑같이 쉽습니다.
 물론 모든 피어 종속성을 설치해야합니다.
 

```coffeescript
npm install @types/react-redux react-redux @reduxjs/toolkit
```

RTK는 전통적인 Redux 스토어보다 설정하기 훨씬 쉬운`configureStore` API를 노출합니다.
 미들웨어 및 인핸서 배열을 제공 할 수 있습니다.
 `applyMiddleware` 및`compose`가 자동으로 호출됩니다.
 

## `configureStore`로 상점 구성
 

가장 간단한 방법은 루트 감속기를 사용하여 저장소를 설정하는 것입니다.
 `src / app / rootReducer.ts` 및`src / app / store.ts`를 만들고 다음을 추가합니다.
 

```js
// src/app/rootReducer.ts
import { combineReducers } from '@reduxjs/toolkit'

const rootReducer = combineReducers({})

export type RootState = ReturnType

export default rootReducer
```

아래처럼 모든 감속기를 추가 할 빈`rootReducer`를 설정했습니다.
 

```cpp
const rootReducer = combineReducers({
    oneReducer,
    anotherReducer,
    yetAnotherReducer
})
```

또한 스토어에서`RootState`를 설정하여 나중에`selector` 및 액션`dispatch`에 사용할 것입니다.
 개별 상태 및 작업을 입력 할 때 올바르게 추론 된 강력한 형식의 저장소를 얻습니다.
 나중에 이것에 대해 더 자세히 알아보십시오!
 

```js
// src/app/store.ts
import { configureStore, Action } from '@reduxjs/toolkit'
import { useDispatch } from 'react-redux'
import { ThunkAction } from 'redux-thunk'

import rootReducer, { RootState } from './rootReducer'

const store = configureStore({
    reducer: rootReducer,
})

export type AppDispatch = typeof store.dispatch
export const useAppDispatch = () => useDispatch()
export type AppThunk = ThunkAction<void, RootState, unknown, Action>

export default store
```

간단합니다. 이제 Redux 스토어를 성공적으로 설정했습니다.
 미들웨어를 구성하고 저장소에 추가 할 수도 있습니다.
 

```js
import { configureStore, getDefaultMiddleware } from '@reduxjs/toolkit';
import logger from 'redux-logger';
const middleware = [...getDefaultMiddleware(), logger];

export default configureStore({
  reducer,
  middleware,
});
```

## Redux 프로젝트를 구성하는 방법
 

RTK는 매우 독단적이기 때문에 "기능 폴더"구조를 권장합니다.
 물론 자신에게 가장 적합한 구조를 자유롭게 사용할 수 있지만 개인적으로이 구조를 이해하기 쉽다는 것을 알게되었습니다.
 공식 문서의 예와 같이 GitHub 문제 추적기가 있다고 가정 해 보겠습니다.
 다음과 같은 폴더 구조가 있습니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Redux-GitHub-Issues-Tracker-Feature-Folder.png?resize=643%2C540&ssl=1)

## `createSlice`로 액션 리듀서 만들기
 

상점의`combinedReducer`를 기억하십니까?
 이제`createSlice`로 첫 번째 감속기를 만들 것입니다.
 이것이 앱의`authReducer`라고 상상해보십시오.
 모든 감속기는 사용자 정의 논리에 따라 동일한 흐름을 따릅니다.
 

```coffeescript
// src/features/auth/authSlice.ts
import { createSlice, PayloadAction } from '@reduxjs/toolkit'

import { AppThunk } from '../../app/store'
import { RootState } from '../../app/rootReducer'

export interface AuthError {
    message: string
}

export interface AuthState {
    isAuth: boolean
    currentUser?: CurrentUser
    isLoading: boolean
    error: AuthError
}

export interface CurrentUser {
    id: string
    display_name: string
    email: string
    photo_url: string
}
export const initialState: AuthState = {
    isAuth: false,
    isLoading: false,
    error: {message: 'An Error occurred'},
}

export const authSlice = createSlice({
    name: 'auth',
    initialState,
    reducers: {
        setLoading: (state, {payload}: PayloadAction&lt) => {
            state.isLoading = payload
        },
        setAuthSuccess: (state, { payload }: PayloadAction) => {
            state.currentUser = payload
            state.isAuth = true
        },
        setLogOut: (state) => {
            state.isAuth = false
            state.currentUser = undefined
        },
        setAuthFailed: (state, { payload }: PayloadAction) => {
            state.error = payload
            state.isAuth = false
        },
    },
})

export const { setAuthSuccess, setLogOut, setLoading, setAuthFailed} = authSlice.actions

export const authSelector = (state: RootState) => state.auth
```

올바른 형식의 저장소를 보장하기 위해`CurrentUser` 유형을`PayloadAction`에 제네릭으로 전달했습니다.
 

여기에서 한 가지 중요한 문제가 있습니까?
 슬라이스의 모든 메서드는 동기식입니다.
 실제로는 인증 상태를 확인하기 위해 API와 비동기 적으로 대화해야합니다.
 이 내용은 나중에 비동기 썽크 섹션에서 다룰 것이지만이 동기 방식으로 상태를 시각화하면 데이터 흐름을 이해하는 데 도움이됩니다.
 

RTK는 또한 자동으로 액션을 생성합니다.
 그런 다음 나중에`authSlice.actions` 객체에서 구조를 해제 할 수 있습니다.
 

선택기 (이 경우 `authSelector`)를 사용하면 컴포넌트 트리의 어느 부분에서나 스토어의 일부를 가져올 수 있습니다.
 나중에 다시 설명하겠습니다.
 

## 썽크, 오류 처리 및로드 상태가있는 비동기 작업
 

내부적으로 RTK는 비동기 논리를 처리하기 위해`redux-thunk`를 사용합니다.
 원하는 경우`redux-saga`로 전환하거나 다른 대안으로 전환 할 수 있습니다.
 `authSlice`에서 비동기 로직 처리를 살펴 보겠습니다.
 

```js
import React from 'react'
import { login, logOut, authSelector, CurrentUser } from './auth'
import { useSelector, useDispatch } from 'react-redux'
import './App.css'

export function App() {
        const dispatch = useDispatch()
        const { currentUser, isLoading, error, isAuth } = useSelector(authSelector)
        if (isLoading) return <div>....loading</div>
        if (error) return <div>{error.message}</div>
        return (
                <div className="App">
                        {isAuth ? (
                                <button onClick={() => dispatch(logOut)}> Logout</button>
                        ) : (
                                <button onClick={() => dispatch(login)}>Login</button>
                        )}
            <UserProfile user={currentUser}/>
                </div>
        )
}

interface UserProfileProps {
        user?: CurrentUser
}
function UserProfile({ user }: UserProfileProps) {
        return <div>{user?.display_name}</div>
}
```

위의 접근 방식이 충분히 간단하다는 것을 알았지 만 또 다른 접근 방식은`createAsyncThunk`를 사용하는 것입니다.
 그러면 전달한 작업 유형 접두사를 기반으로 약속 수명주기 작업 유형이 생성됩니다. 그런 다음 약속 콜백을 실행하고 반환 된 약속에 따라 수명주기 작업을 전달하는 썽크 작업 생성자를 반환합니다.
 

## `useDispatch` 및`useSelector` 후크를 사용하여 상점에 연결
 

앱을 스토어에 연결하고 액션을 전달하고 스토어의 일부를 선택하는 방법을 살펴 보겠습니다.
 

```js
import React from 'react'
import { login, logOut, authSelector, CurrentUser } from './auth'
import { useSelector, useDispatch } from 'react-redux'
import './App.css'

export function App() {
        const dispatch = useDispatch()
        const { currentUser, isLoading, error, isAuth } = useSelector(authSelector)
        if (isLoading) return <div>....loading</div>
        if (error) return <div>{error.message}</div>
        return (
                <div className="App">
                        {isAuth ? (
                                <button onClick={() => dispatch(logOut)}> Logout</button>
                        ) : (
                                <button onClick={() => dispatch(login)}>Login</button>
                        )}
            <UserProfile user={currentUser}/>
                </div>
        )
}

interface UserProfileProps {
        user?: CurrentUser
}
function UserProfile({ user }: UserProfileProps) {
        return <div>{user?.display_name}</div>
}
```

## 결론
 

TypeScript와 함께 Redux Toolkit을 형식 안전 저장, 디스패치 및 작업에 사용하는 방법을 살펴 보았습니다.
 `createSlice` API를 사용하면 몇 줄의 코드만으로 쉽게 상점을 설정할 수 있습니다.
 `useSelector` 후크를 사용하면 컴포넌트 트리 내의 어느 지점에서나 스토어의 모든 슬라이스를 선택할 수 있으며,`useDispatch`는 스토어를 업데이트하는 액션의 디스패치를 허용합니다.
 
---
layout: post
title: "TypeScript에 대응 컨텍스트를 사용하는 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/React-Context-Typescript.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/React-Context-Typescript.png?fit=730%2C487&ssl=1)

TypeScript는 정적 유형 검사 견고성, 이해 가능성 및 유형 인터페이스와 같이 최근에 코드에 추가된 많은 이점 때문에 점점 더 인기를 끌고 있다.

또한 TypeScript를 React와 함께 사용하면 개발자의 경험을 개선하고 프로젝트에 대한 예측 가능성을 높일 수 있습니다.

이 가이드에서는 처음부터 작업관리 앱을 구축하여 TypeScript와 Retact Context를 사용하는 방법에 대해 알아봅니다. 안으로 들어가자.

## 전제조건

이 튜토리얼을 최대한 활용하려면 React 및 TypeScript에 대한 기본적인 이해가 필요합니다. 웹 개발을 처음 접하는 경우 이 React TypeScript 튜토리얼에서 시작하여 이러한 기술에 익숙해질 수 있습니다.

## 설정

새로운 앱을 만들기 위해, 저는 번거로움 없이 현대적인 구성을 가질 수 있도록 반응 앱을 만들 것입니다. 그러나 웹 팩을 사용하여 처음부터 새 앱을 설정하는 것은 좋습니다.

먼저 터미널을 열고 다음 명령을 실행합니다.

```undefined
npx create-react-app react-context-todo --template typescript
```

CreateReact App을 사용할 때 TypeScript를 사용하려면 `--template typescript` 플래그를 추가해야 합니다. 그렇지 않으면 앱에서 JavaScript만 지원합니다.

다음으로, 프로젝트를 다음과 같이 구성하겠습니다.

```undefined
├── src
|  ├── components
|  |  ├── AddTodo.tsx
|  |  └── Todo.tsx
|  ├── containers
|  |  └── Todos.tsx
|  ├── context
|  |  └── todoContext.tsx
|  ├── App.tsx
|  ├── index.css
|  ├── index.tsx
|  ├── react-app-env.d.ts
|  └── type.d.ts
├── tsconfig.json
├── package.json
└── yarn.lock
```

여기에 밑줄칠 파일이 두 개 있습니다.

- 프로젝트의 컨텍스트 역할을 하는 `context/todoContext.tsx` 파일
- TypeScript Type을 포함하는 `type.d.ts` 파일입니다. 확장자 `.d.ts`는 다른 파일의 형식을 가져오지 않고 사용할 수 있도록 합니다.

이걸 설치하면, 우리는 이제 손을 더럽히고 의미 있는 무언가를 코드화할 수 있습니다.

## 작업관리 유형 작성

TypeScript Type을 사용하면 컴파일러가 런타임 전에 오류를 잡을 수 있도록 변수나 함수를 값으로 지정할 수 있습니다.

type.d.ts

```coffeescript
interface ITodo {
  id: number
  title: string
  description: string
  status: boolean
}

type ContextType = {
  todos: ITodo[]
  saveTodo: (todo: ITodo) => void
  updateTodo: (id: number) => void
}
```

보시는 바와 같이 `인터페이스`는ITo do`는 작업관리 객체의 모양을 정의합니다. 다음으로, 일련의 작업관리 및 작업관리 추가 또는 업데이트 방법을 예상하는 `ContextType` 유형이 있다.

## 컨텍스트 생성

React Context를 사용하면 소품을 전달하지 않고도 구성 요소 간에 상태를 공유하고 관리할 수 있습니다. 컨텍스트는 데이터를 사용해야 하는 구성 요소에만 데이터를 제공합니다.

`context/todoContext.tsx`

```js
import * as React from "react";

export const TodoContext = React.createContext(null);

const TodoProvider: React.FC = ({ children }) => {
  const [todos, setTodos] = React.useState<ITodo[]>([
    {
      id: 1,
      title: "post 1",
      description: "this is my first description",
      status: false
    },
    {
      id: 2,
      title: "post 2",
      description: "this is my second description",
      status: true
    }
  ]);
```

여기서는 새 컨텍스트를 만드는 것으로 시작하여 해당 유형을 컨텍스트 유형 또는 null과 일치하도록 설정합니다. 후자는 null 값으로 컨텍스트를 초기화하기 위해 here를 사용하고 있다. 다음으로 구성 요소 소비자에게 컨텍스트를 제공하는 구성 요소 `TodoProvider`를 생성한다. 여기서는 작업을 수행하기 위해 데이터를 사용하여 상태를 초기화합니다.

`context/todoContext.tsx`

```coffeescript
const saveTodo = (todo: ITodo) => {
  const newTodo: ITodo = {
    id: Math.random(), // not really unique - but fine for this example
    title: todo.title,
    description: todo.description,
    status: false,
  }
  setTodos([...todos, newTodo])
}

const updateTodo = (id: number) => {
  todos.filter((todo: ITodo) => {
    if (todo.id === id) {
      todo.status = true
      setTodos([...todos])
    }
  })
}
```

SaveTodo(저장 작업) 함수는 `인터페이스`를 기반으로 새로운 작업관리를 작성합니다.ITO를 수행한 다음 개체를 작업관리 배열에 추가합니다. 다음 기능인 `업데이트`TODO는 작업관리 배열에서 매개 변수로 전달된 작업관리 ID를 찾아 업데이트한다.

`context/todoContext.tsx`

```coffeescript
 return (
    
      {children}
    
  );
};

export default TodoProvider;
```

다음으로, 우리는 해당 값을 컨텍스트에 전달하여 구성요소에 사용할 수 있도록 합니다.

`context/todoContext.tsx`

```js
import * as React from 'react'

export const TodoContext = React.createContext(null)

const TodoProvider: React.FC = ({ children }) => {
  const [todos, setTodos] = React.useState<ITodo[]>([
    {
      id: 1,
      title: 'post 1',
      description: 'this is a description',
      status: false,
    },
    {
      id: 2,
      title: 'post 2',
      description: 'this is a description',
      status: true,
    },
  ])

  const saveTodo = (todo: ITodo) => {
    const newTodo: ITodo = {
      id: Math.random(), // not really unique - but fine for this example
      title: todo.title,
      description: todo.description,
      status: false,
    }
    setTodos([...todos, newTodo])
  }

  const updateTodo = (id: number) => {
    todos.filter((todo: ITodo) => {
      if (todo.id === id) {
        todo.status = true
        setTodos([...todos])
      }
    })
  }

  return (
    
      {children}
    
  )
}

export default TodoProvider
```

이것으로 우리는 이제 맥락을 소비할 수 있게 되었다. 다음 섹션에서 구성 요소를 생성해 보겠습니다.

## 구성 요소 생성 및 컨텍스트 사용

`구성 요소/AddTodo.tsx`

```xml
import * as React from 'react'
import { TodoContext } from '../context/todoContext'

const AddTodo: React.FC = () => {
  const { saveTodo } = React.useContext(TodoContext) as ContextType
  const [formData, setFormData] = React.useState<ITodo | {}>()

  const handleForm = (e: React.FormEvent<HTMLInputElement>): void => {
    setFormData({
      ...formData,
      [e.currentTarget.id]: e.currentTarget.value,
    })
  }

  const handleSaveTodo = (e: React.FormEvent, formData: ITodo | any) => {
    e.preventDefault()
    saveTodo(formData)
  }

  return (
    <form className='Form' onSubmit={(e) => handleSaveTodo(e, formData)}>
      <div>
        <div>
          <label htmlFor='name'>Title</label>
          <input onChange={handleForm} type='text' id='title' />
        </div>
        <div>
          <label htmlFor='description'>Description</label>
          <input onChange={handleForm} type='text' id='description' />
        </div>
      </div>
      <button disabled={formData === undefined ? true : false}>Add Todo</button>
    </form>
  )
}

export default AddTodo
```

여기에는 사용자가 `useState` 후크를 사용하여 입력한 데이터를 처리할 수 있는 폼 컴포넌트가 있다. 양식 데이터를 가져오면 컨텍스트 개체에서 `저장 작업` 함수를 사용하여 새 작업관리를 추가합니다.

처음에는 컨텍스트가 null이므로 TypeScript 던지기 오류를 방지하려면 `useContext` 후크에 typecasting을 사용합니다.

`구성요소/Todo.tsx`

```js
import * as React from 'react'

type Props = {
  todo: ITodo
  updateTodo: (id: number) => void
}

const Todo: React.FC<Props> = ({ todo, updateTodo }) => {
  const checkTodo: string = todo.status ? `line-through` : ''
  return (
    <div className='Card'>
      <div className='Card--text'>
        <h1 className={checkTodo}>{todo.title}</h1>
        <span className={checkTodo}>{todo.description}</span>
      </div>
      <button
        onClick={() => updateTodo(todo.id)}
        className={todo.status ? `hide-button` : 'Card--button'}
      >
        Complete
      </button>
    </div>
  )
}

export default Todo
```

여기 보시는 바와 같이, 단일 작업관리 항목을 보여주는 프레젠테이션 구성 요소가 있습니다. 작업관리 객체와 이를 위에서 정의한 `Props` 유형과 일치해야 하는 매개 변수로 업데이트하는 기능을 수신한다.

`컨테이너/Todos.tsx`

```coffeescript
import * as React from 'react'

import { TodoContext } from '../context/todoContext'
import Todo from '../components/Todo'

const Todos = () => {
  const { todos, updateTodo } = React.useContext(TodoContext) as ContextType
  return (
    <>
      {todos.map((todo: ITodo) => (
        
      ))}
    </>
  )
}

export default Todos
```

이 구성 요소는 페이지가 로드될 때 수행할 작업 목록을 표시합니다. todos와 update 함수를 끌어온다.해야 할 일에서 해야 할 일. 다음으로, 어레이를 순환하고 표시할 개체로 `Todo`Todo` 구성 요소로 전달한다. 이 단계를 진행함으로써 이제 App.tsx 파일의 작업관리 컨텍스트를 제공하여 애플리케이션 구축을 완료할 수 있습니다. 그러면 다음 부분에서 컨텍스트 제공자를 사용해 보겠습니다.

## 컨텍스트 제공

```js
import * as React from 'react'
import TodoProvider from './context/todoContext'
import Todos from './containers/Todos'
import AddTodo from './components/AddTodo'
import './styles.css'

export default function App() {
  return (
    <TodoProvider>
      <main className='App'>
        <h1>My Todos</h1>
        <AddTodo />
        <Todos />
      </main>
    </TodoProvider>
  )
}
```

여기서는 소비자를 대상으로 하는 `Todo Provider` 구성 요소를 가져옵니다. 그렇긴 하지만 이제 다른 구성 요소에서 `사용 컨텍스트` 후크를 사용하여 `작업관리` 배열과 작업관리 추가 또는 업데이트 기능에 액세스할 수 있다.

이제 터미널에서 프로젝트를 열고 다음 명령을 실행할 수 있습니다.

```undefined
  yarn start
```

아니면

```coffeescript
  npm start
```

모든 작업이 수행되면 브라우저에서 다음을 확인할 수 있습니다.

- host://localhost:3000

좋아요! 마지막 터치로 이제 Retact Context와 TypeScript를 사용하여 할일 앱을 만들게 되었습니다.

완성된 프로젝트는 여기서 찾을 수 있습니다.

## 결론

TypeScript는 우리의 코드를 더 좋게 만드는 훌륭한 언어이다. 이 튜토리얼에서는 TypeScript를 React Context와 함께 사용하는 방법에 대해 알아봤습니다. 바라건대, 그것은 당신의 다음 프로젝트 진행에 도움이 될 것입니다. 읽어주셔서 감사합니다.
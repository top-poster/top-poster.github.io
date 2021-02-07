---
layout: post
title: "Ionic React로 Android 앱 쓰기"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/12/android-app-ionic-react.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/android-app-ionic-react.png?fit=730%2C487&ssl=1)

React 웹 앱을 만드는 방법을 알고 있다면 이와 동일한 기술을 사용하여 Ionic 프레임워크를 사용하여 모바일 앱을 개발할 수 있습니다. 이 기사에서는 아이오닉 프레임워크와 리액션으로 안드로이드 레시피 앱을 만드는 방법에 대해 알아보겠습니다.

## 시작 중

몇 가지 설치부터 시작하겠습니다. 먼저 다음을 실행하여 Ionic CLI를 전체적으로 설치하십시오.

```coffeescript
npm install -g @ionic/cli native-run cordova-res
```

다음으로, Ionic 앱 프로젝트를 만들어 보겠습니다.

```undefined
ionic start react-ionic-recipes-app blank  --type=react --capacitor
```

`blank`는 빈 앱을 만든다는 의미입니다. 빈 앱에 라우팅이 추가되었습니다. 그것만 있으면 됩니다. type은 react로 설정되며, 이는 Retact 프로젝트를 만들겠다는 의미입니다. 마지막으로 --capacitor는 프로젝트 파일에서 네이티브 앱을 실행하고 구축할 수 있는 Ionic의 크로스 플랫폼 네이티브 런타임인 Capacter를 추가합니다.

그런 다음 `ionic-app` 프로젝트 폴더에서 다음 명령을 실행하여 프로젝트에 대한 React Hooks를 설치합니다.

```coffeescript
npm install @ionic/react-hooks @ionic/pwa-elements
```

마지막으로 브라우저에서 앱을 실행해 보겠습니다.

```undefined
ionic serve
```

우리는 http://localhost:8100에서 우리의 앱을 볼 수 있을 것이다.

## Ionic React로 Recipe 앱 만들기

이제 우리는 Genymotion과 같은 안드로이드 모바일 기기나 에뮬레이터에서 실행할 수 있는 프로젝트를 가져야 합니다.

`App.tsx`는 프로젝트의 진입점 구성 요소이며, 라우팅 코드가 있는 곳이기도 합니다. 다음을 작성하여 경로를 더 추가하겠습니다.

```js
// src/App.tsx

import React from 'react';
import { Redirect, Route } from 'react-router-dom';
import { IonApp, IonRouterOutlet } from '@ionic/react';
import { IonReactRouter } from '@ionic/react-router';
import Home from './pages/Home';

/* Core CSS required for Ionic components to work properly */
import '@ionic/react/css/core.css';

/* Basic CSS for apps built with Ionic */
import '@ionic/react/css/normalize.css';
import '@ionic/react/css/structure.css';
import '@ionic/react/css/typography.css';

/* Optional CSS utils that can be commented out */
import '@ionic/react/css/padding.css';
import '@ionic/react/css/float-elements.css';
import '@ionic/react/css/text-alignment.css';
import '@ionic/react/css/text-transformation.css';
import '@ionic/react/css/flex-utils.css';
import '@ionic/react/css/display.css';

/* Theme variables */
import './theme/variables.css';
import RecipeForm from './pages/RecipeForm';

const App: React.FC = () => (
  <IonApp>
    <IonReactRouter>
      <IonRouterOutlet>
        <Route path="/home" component={Home} exact={true} />
        <Route exact path="/" render={() => <Redirect to="/home" />} />
        <Route exact path="/recipe-form/:id" component={RecipeForm} />
        <Route exact path="/recipe-form" component={RecipeForm} />
      </IonRouterOutlet>
    </IonReactRouter>
  </IonApp>
);

export default App;
```

아이오닉은 자체 라우터와 함께 제공되므로 원하는 대로 사용할 수 있습니다. 노선을 추가하기 위해 아이온라우터 아웃렛에 레시피폼 노선을 추가했다. `정확한` 소품은 이온 라우터가 경로에서 정확히 일치함을 의미한다. 물론 `path`는 경로의 경로를 정의하고, `:id`는 경로 매개 변수 자리 표시자입니다. `component`는 지정된 경로로 이동할 때 탑재할 구성 요소를 정의합니다.

### 레시피 페이지 작성

저희 앱 홈페이지에 레시피 목록을 표시하려고 합니다. 이를 위해 "Home.tsx" 파일을 "page" 폴더에 추가합니다. 그런 다음 다음과 같이 씁니다.

```js
// src/pages/Home.tsx

import { IonButton, IonCol, IonContent, IonHeader, IonItem, IonLabel, IonList, IonPage, IonRow, IonTitle, IonToolbar } from '@ionic/react';
import React, { useEffect, useState } from 'react';
import './Home.css';
import { Plugins } from '@capacitor/core';
import { RouteComponentProps } from 'react-router';
import { Recipe } from '../interfaces/recipe';
const { Storage } = Plugins;

const Home: React.FC<RouteComponentProps> = ({ match }) => {
  const [recipes, setRecipes] = useState<Recipe[]>([])

  const getRecipes = async () => {
    const ret = await Storage.get({ key: 'recipes' });
    const recipes: Recipe[] = JSON.parse(ret.value as string) || [];
    setRecipes(recipes)
  }

  useEffect(() => {
    getRecipes()
  }, [match])

  const saveRecipes = async (data: Recipe[]) => {
    await Storage.set({
      key: 'recipes',
      value: JSON.stringify(data)
    });
  }

  const deleteRecipe = async (id: string) => {
    const ret = await Storage.get({ key: 'recipes' });
    const recipes: Recipe[] = JSON.parse(ret.value as string) || [];
    await saveRecipes(recipes.filter(r => r.id !== id))
    await getRecipes()
  }

  return (
    <IonPage>
      <IonHeader>
        <IonToolbar>
          <IonTitle>Recipe App</IonTitle>
        </IonToolbar>
      </IonHeader>
      <IonContent fullscreen>
        <IonRow>
          <IonCol size="12">
            <IonButton routerLink="/recipe-form">Add Recipe</IonButton>
          </IonCol>
        </IonRow>
        <IonRow>
          <IonCol>
            <IonList>
              {recipes.map(({ id, title }) => (
                <IonItem key={id}>
                  <IonLabel>{title}</IonLabel>
                  <IonButton routerLink={`/recipe-form/${id}`}>Edit</IonButton>
                  <IonButton onClick={deleteRecipe.bind(undefined, id)}>Delete</IonButton>
                </IonItem>
              ))}
            </IonList>
          </IonCol>
        </IonRow>
      </IonContent>
    </IonPage>
  );
};

export default Home;
```

이 파일에서는 캐패시터 스토리지 라이브러리를 사용하여 스토리지를 비동기식으로 조작할 수 있습니다.

우리의 경우 로컬 스토리지는 적은 양의 임시 데이터만 저장할 수 있기 때문에 로컬 스토리지보다 더 나은 선택입니다. 또한 로컬 스토리지에는 데이터가 스토리지 디바이스 공간의 50% 이상을 차지하면 공간을 확보할 수 있는 데이터 제거 정책이 있습니다. 캐패시터의 스토리지 API도 비동기적이므로 UI를 지원하지 않습니다.

home.tsx에서는 getRecips 기능을 통해 핵심 Recipe가 포함된 데이터를 얻는다. 그런 다음, 우리는 `세트 레시피`를 불러 `레시피` 상태를 설정합니다.

매치 소품이 바뀔 때마다 getRecips 기능을 실행한다. 매치(match) 소품에는 URL, URL 파라미터와 같은 탐색 데이터가 포함되어 있어 탐색이 완료될 때마다 최신 데이터를 얻을 수 있습니다.

저장 Recips 기능은 delete Recipe를 호출할 때 호출됩니다. 우리는 핵심 `리셉스`로 값을 얻고, 그것을 구문 분석한 다음, 주어진 `id`가 있는 항목이 없는 `세이브리셉스`라고 부른다. 그러면 지정된 `id`를 사용하여 항목을 삭제할 수 있습니다.

이 앱은 너무 작기 때문에 성능 저하를 우려하지 않고 모든 항목을 가져올 수 있습니다. 하지만 앱에 데이터가 많이 저장되어 있다면 다른 스토리지 솔루션을 고려해야 합니다.

우리가 삭제 단추를 클릭하면 우리는, 편집을 클릭합니다. 그래서 우리는 지정된 ID가 있는 항목을 편집할 수 있다면 우리는`recipe-form` URL로 ID를 요리법에서 함께 갑니다 `deleteRecipe`라고 부른다. Recipe-form으로 이동할 수 있는 Add Recipe 버튼도 있습니다. 아이온라벨에는 우리가 목록에 표시하는 제목이 있다.

### 레시피 추가 및 편집

이제 레시피를 추가하거나 편집할 페이지를 만들어야 합니다. 이를 위해 `src/pages` 폴더에 `RecipeForm.tsx` 파일을 생성하고 다음을 작성합니다.

```js
// src/pages/RecipeForm.tsx

import {
  IonBackButton,
  IonButton,
  IonButtons,
  IonContent,
  IonHeader,
  IonImg,
  IonInput,
  IonItem,
  IonLabel,
  IonList,
  IonPage,
  IonTextarea,
  IonTitle,
  IonToolbar
} from '@ionic/react';
import React, { useCallback, useEffect, useState } from 'react';
import { useCamera } from '@ionic/react-hooks/camera';
import { CameraResultType, CameraSource } from "@capacitor/core";
import { RouteComponentProps } from 'react-router';
import { Plugins } from '@capacitor/core';
import { v4 as uuidv4 } from 'uuid';
import { base64FromPath } from "@ionic/react-hooks/filesystem"
import { Recipe } from '../interfaces/recipe';
const { Storage } = Plugins;

function usePhotoGallery() {
  const { getPhoto } = useCamera();
  const [photo, setPhoto] = useState<string>('');

  const takePhoto = async () => {
    const cameraPhoto = await getPhoto({
      resultType: CameraResultType.Uri,
      source: CameraSource.Camera,
      quality: 100
    });

    const base64Data = await base64FromPath(cameraPhoto.webPath!);
    setPhoto(base64Data)
  };

  return {
    photo,
    takePhoto
  };
}

interface RecipeFormProps extends RouteComponentProps<{
  id?: string;
}> { }

const RecipeForm: React.FC<RecipeFormProps> = ({ match, history }) => {
  const { photo, takePhoto } = usePhotoGallery();
  const [title, setTitle] = useState<string>('')
  const [steps, setSteps] = useState<string>('')
  const [existingPhoto, setExistingPhoto] = useState<string>('')

  const saveRecipes = async (data: Recipe[]) => {
    await Storage.set({
      key: 'recipes',
      value: JSON.stringify(data)
    });
  }

  const save = async () => {
    if (!title) {
      alert('Title is required')
      return
    }

    if (!steps) {
      alert('Steps is required')
      return
    }
    const recipe: Recipe = {
      photo,
      title,
      steps,
      id: uuidv4()
    }

    const ret = await Storage.get({ key: 'recipes' });
    const recipes: Recipe[] = JSON.parse(ret.value as string) || [];
    if (!match.params.id) {
      await saveRecipes([...recipes, recipe])
      setExistingPhoto('')
      setTitle('')
      setSteps('')
    }
    else {
      const recipeIndex = recipes.findIndex(r => r.id === match.params.id)
      recipes[recipeIndex] = {
        photo,
        title,
        steps,
        id:match.params.id
      }
      await saveRecipes(recipes)
    }
    history.push('/');
  }

  const getRecipe = useCallback(async () => {
    if (match.params.id) {
      const ret = await Storage.get({ key: 'recipes' });
      const recipes: Recipe[] = JSON.parse(ret.value as string) || [];
      const recipe = recipes.find(r => r.id === match.params.id)
      if (match.params.id && recipe) {
        const { photo, title, steps } = recipe;
        setExistingPhoto(photo)
        setTitle(title)
        setSteps(steps)
      }
    }
  }, [match.params.id])

  useEffect(() => {
    getRecipe()
  }, [getRecipe, match.params.id])

  useEffect(() => {
    setExistingPhoto(photo)
  }, [photo])

  return (
    <IonPage>
      <IonHeader>
        <IonToolbar>
          <IonButtons slot="start">
            <IonBackButton defaultHref="/" />
          </IonButtons>
          <IonTitle>{match.params.id ? 'Edit' : 'Add'} Recipe Form</IonTitle>
        </IonToolbar>
      </IonHeader>
      <IonContent fullscreen>
        <IonHeader collapse="condense">
          <IonToolbar>
            <IonTitle size="large">Recipe Form</IonTitle>
          </IonToolbar>
        </IonHeader>
        <IonList>
          <IonItem>
            <IonButton onClick={takePhoto}>Take Recipe Photo</IonButton>
          </IonItem>
          <IonItem>
            <IonLabel>Recipe Photo</IonLabel>
            {existingPhoto && <IonImg src={existingPhoto} />}
          </IonItem>
          <IonItem lines="none">
            <IonInput value={title} placeholder="Title" onIonChange={e => setTitle(e.detail.value!)}></IonInput>
          </IonItem>
          <IonItem lines="none">
            <IonTextarea rows={10} value={steps} placeholder="Steps" onIonChange={e => setSteps(e.detail.value!)}></IonTextarea>
          </IonItem>
          <IonItem lines="none">
            <IonButton onClick={save}>Save</IonButton>
          </IonItem>
        </IonList>
      </IonContent>
    </IonPage >
  );
};

export default RecipeForm;
```

이 구성 요소에서는 사진을 찍고 Base64 문자열 버전을 반환하기 위해 `사진 갤러리 사용` 후크를 만듭니다.

후크에서는 카메라 사용 후크를 사용하여 get Photo(사진 가져오기) 기능을 호출합니다. 결과Type`은 `CameraResultType`으로 설정되어 있습니다.우리당은 사진에 URL을 반환해야 한다. 우리는 `카메라 소스`로 사진 `소스`를 휴대전화 카메라에 맞췄다.카메라. 품질은 숫자로 설정되며 100은 고품질입니다.

카메라 사진 데이터와의 약속을 반환합니다. base64FromPath를 사용하면 사진의 base64 문자열 버전을 얻을 수 있습니다. 사진을 받기 위해 set Photo(세트 사진)라고 부르고, Recipe Form(레시피 양식) 구성 요소에 사용할 수 있도록 사진 및 사진 촬영 개체를 반환합니다.

Recipe Form 구성 요소에는 Recipe 항목의 ID를 얻을 수 있는 match(매치) 받침이 있습니다. history에는 브라우저 history 객체가 있어 프로그래밍 방식으로 탐색할 수 있습니다.

SaveRecips 기능은 명확하지 않은 경우 Recipe 데이터를 저장합니다. 이 기능은 Save 기능에 사용됩니다. 그 안에서 우리는 `제목`과 `단계`의 상태를 확인한다. 설정된 항목이 있으면 해당 항목을 저장합니다.

`스텝`은 레시피 제목이고, 스텝은 레시피 단계의 내용을 담고 있다. 그런 다음 ID를 사용하여 `레시피` 개체를 만듭니다. 새 항목에 사용됩니다. 우리는 `저장소`라고 부른다.모든 항목을 핵심 `레시피`로 가져오면 JSON.parse로 구문 분석한다.

match.params.id이 허위일 경우, 즉 엔트리를 추가한다는 뜻이라면 레시피의 배열 끝에 엔트리를 추가해 세이브 레시피를 부른다. 그 후 우리는 모든 주를 비운다. 그렇지 않으면 `find Index` 방식으로 `reecipes` 배열의 인덱스를 찾은 다음 해당 항목을 주(州)로 업데이트합니다.

그런 다음 Save Recipes를 불러 데이터를 저장한 다음 history를 저장합니다.홈페이지로 돌아가라고 재촉하다 getRecipe 기능은 match.params.id이 존재하는 경우 기존 항목의 레시피 데이터를 얻는 데 사용된다.

검색된 항목에서 사진, 제목, 스텝의 상태를 설정했습니다. 우리는 match.params.id을 보기 때문에 새로운 가치를 얻으면 get Recipe라고 부른다.

그런 다음 match.params.id을 보기 위해 effect 후크를 사용하고, match.params.id이 바뀌면 get Recipe를 부른다. match.params.id은 레시피의 ID를 가지고 있기 때문에 편집을 클릭하면 레시피를 얻을 수 있습니다.

사진 상태가 업데이트되면 기존 사진 상태도 설정해 표시하도록 하는데, 두 가지 조건을 모두 다르게 표시하는 것보다 기존 사진만 표시하기가 더 쉽다. 레시피 사진의 기본 64줄은 포토와 기존 사진이다.

JSX에는 사진을 찍기 위해 사진 찍기 버튼이 있는 `아이온리스트`가 있으며 제목과 단계에 대한 입력 정보가 있다. 저장하기 버튼을 추가하여 저장하기 위해 항목을 저장했습니다.

마지막으로, 구성 요소를 위한 인터페이스를 구축할 것입니다. `src/interface/reecipe.ts` 파일을 생성하고 다음을 추가합니다.

```undefined
// src/interfaces/recipe.ts

export interface Recipe {
  photo: string;
  title: string;
  steps: string;
  id: string;
}
```

### Genymotion을 사용하여 앱 실행

Genymotion으로 앱을 실행하려면 몇 가지 작업을 더 수행해야 합니다. 먼저 다음을 실행하여 자산 폴더를 생성합니다.

```undefined
ionic build
```

Android 앱을 실행하고 구축하려면 이것이 필요합니다. 다음으로, 다음을 실행하여 Android 종속성을 추가합니다.

```bash
npx cap add android
npx cap sync
```

이제 Android Studio와 Genymotion을 설치한 다음 Android Studio용 Genymotion 플러그인을 설치해야 합니다. 작업이 완료되면 다음 명령으로 Ionic React 프로젝트를 실행합니다.

```undefined
ionic capacitor run android --livereload --external --address=0.0.0.0
```

메시지가 표시되면 `VirtualBox Virtual Adapter`를 선택해야 합니다. 이렇게 하면 우리는 지니모션에서 인터넷을 사용할 수 있다. 그런 다음 Android Studio에서 Alt+Shift+F10을 누르면 Genymotion에서 앱이 실행되는 것을 볼 수 있습니다.

## 결론

Ionic을 통해 React에 대한 지식을 활용하여 하드웨어에 액세스하고 스토리지 기기로 작업할 수 있는 Android 모바일 앱을 만들 수 있습니다.
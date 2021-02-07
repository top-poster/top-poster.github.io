---
layout: post
title: "솔리드 원칙: JavaScript 프레임워크의 단일 책임"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2021/02/single-responsibility-principle-javascript.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/single-responsibility-principle-javascript.png?fit=730%2C487&ssl=1)

단일 책임 원칙은 솔리드 설계 원칙을 구성하는 5가지 객체 지향 설계(OOD) 지침 중 하나입니다.

이 튜토리얼에서는 단일 책임 원칙에 초점을 맞추고 설계 결정(특히 Angular and React)을 자바스크립트 프레임워크에서 어떻게 안내할 수 있는지 시연합니다.

다음 내용을 다루겠습니다.

- 솔리드 원칙이란 무엇입니까?
- 단일 책임 원칙은 무엇입니까?
- React의 단일 책임 원칙
- 반응 구성 요소의 문제 분리
- Angular(각도)의 단일 책임 원칙
- 단일 책임: 부작용
- 반응에서의 부작용
- 각도의 부작용
- 컨테이너 및 프레젠테이션 구성 요소

## 솔리드 원칙이란 무엇입니까?

SOLID는 유명한 소프트웨어 엔지니어 Robert C에 의해 약술된 최초의 5가지 OOD 원칙을 나타내는 약어입니다. 마틴. SOLID 원칙은 개발자가 강력하고 유지관리가 가능한 애플리케이션을 설계하는 데 도움이 되도록 설계되었습니다.

5가지 솔리드 원칙은 다음과 같습니다.

- 단일책임원칙
- 개방-폐쇄 원리
- 리스코프 대체 원리
- 인터페이스 분리 원리
- 종속성 반전 원리

## 단일 책임 원칙은 무엇입니까?

자바스크립트의 단일 책임 원칙은 모듈의 응집성을 다룬다. 기능 및 클래스는 하나의 작업만 가져야 한다고 명시되어 있습니다.

예를 들어 `자동차` 모델을 예로 들어보자.

```js
class Car {
    constructor(name,model,year) {
        this.name=name
        this.model=model
        this.year=year
    }
getCar(id) {
        return this.http.get('api/cars/'+id)
    }
saveCar() {
        return this.post('api/cars', { name: this.name, year: this.year, model: this.model })
    }
}
```

위의 예는 단일 책임 원칙을 위반하는 것이다. 왜 그럴까? 카 모델은 차를 고정/표현하기 위한 것이었지만, 인터넷에서 차를 가져오는 getCar 방법이 있다. 그것은 끝점에서 차를 얻는 또 다른 책임을 부여한다.

자동차 등급의 책임에 대한 선을 그어야 한다. 그것이 모델로 사용될 것인가, 아니면 객체로 사용될 것인가?

만약 우리가 `세이브카`나 `겟카` 중 하나를 터치해서 변화를 만든다면, 이러한 변화는 우리가 `카` 모델을 추가 혹은 `카` 클래스에 다른 것을 추가함으로써 재설계해야 할 수도 있다. 이 작업을 수행하지 않을 경우 해당 응용 프로그램이 예기치 않은 방식으로 중단될 수 있습니다.

다음과 같은 다양한 등급으로 책임을 분리할 수 있습니다.

```js
class Car {
    constructor(name, model, year) {
        this.name = name
        this.model = model
        this.year = year
    }
}
class CarService {
    getCar(id) {
        return this.http.get('api/cars/'+id)
    }
    saveCar(car) {
        this.http.post('api/cars', car)
    }
}
```

이 예에서 볼 수 있듯이, 이제 책임은 분리되어 있습니다. 이제 `카` 모델은 차를 관리하고 `카 서비스`는 차를 엔드포인트에서 구하고 구할 책임이 있다.

한 클래스에 둘 이상의 책임이 있는 경우 해당 책임은 결합됩니다. 한 가지 책임에 대한 변화는 다른 책임에 대한 학급의 능력을 방해할 수 있다. 이러한 결합은 변경 시 예상치 못한 방식으로 깨지는 깨지기 쉬운 디자인으로 이어집니다.

아래 예제에서는 반응 및 각도 구성 요소에서 단일 책임 원칙을 사용하는 방법을 보여 줍니다. 이러한 예는 Vue.js, Svelte 등과 같은 다른 자바스크립트 프레임워크에서도 적용할 수 있다.

## React의 단일 책임 원칙

다음 React 구성 요소가 있다고 가정해 보겠습니다.

```coffeescript
class Movies extends Component {
    componentDidMount() {
        store.subscribe(() => this.forceUpdate())
    }
render() {
        const state = store.getState()
        const movies = state.movies.map((movie, index) => {
                <div className="movie-card" key={index}>
                    {movie.name}
                    Year: {movie.year}
                    Gross: {movie.gross}
                </div>
        })
        return (
            <div>
                <div className="movie-header">Movies App</div>
                <div className="movies-list">
                    {movies} 
                </div>
            </div>
        )
    }
}
```

이 구성 요소에는 몇 가지 문제가 있습니다.

- 상태 관리 — 구성 요소가 스토어에 가입합니다.
- 데이터 가져오기 - 저장소에서 상태를 가져옵니다.
- UI 프레젠테이션 — 동영상 목록을 렌더링합니다.
- 비즈니스 논리 - 애플리케이션의 비즈니스 논리(영상을 얻는 방법에 대한 논리)와 연결되어 있습니다.

이 React 구성 요소는 재사용할 수 없습니다. 앱의 다른 구성 요소(예: 높은 수익의 영화, 연도별 영화 등을 표시하는 구성 요소)에 있는 동영상 목록을 다시 사용하려면 각 구성 요소의 코드를 동일한 경우에도 다시 작성해야 합니다.

이 구성 요소는 부품이 너무 많아서 유지 관리가 어렵습니다. 부품 하나가 변경되면 중단되는 변경 사항이 발생합니다. 최적화도 안 되고 부작용도 생기며, Retact 구성 요소를 효과적으로 메모하여 성능을 향상시킬 수 없습니다. 이렇게 하면 오래된 데이터가 생성되기 때문입니다.

### 반응 구성 요소의 문제 분리

위의 React 구성 요소 예제를 계속 진행하려면 `Movies` 구성 요소에서 UI 프레젠테이션을 추출해야 합니다.

우리는 이것을 다루기 위해 또 다른 요소인 영화 목록을 만들 것이다. Movies List 구성 요소는 다음과 같은 소품에서 영화 배열을 기대할 것이다.

```coffeescript
class MoviesList extends Component {
    render() {
        const movies = props.movies.map((movie, index) => {
                <div className="movie-card" key={index}>
                    {movie.name}
                    Year: {movie.year}
                    Gross: {movie.gross}
                </div>
        })
        return (
            <div className="movies-list">
                {movies} 
            </div>
        )
    }
}
class Movies extends Component {
    componentDidMount() {
        store.subscribe(() => this.forceUpdate())
    }
render() {
        const state = store.getState()
        const movies = state.movies        return (
            <div>
                <div className="movie-header">Movies App</div>
                <MoviesList movies={movies} />
            </div>
        )
    }
}
```

우리는 영화 요소를 리팩터링하고 UI 프레젠테이션 코드를 분리했다. 이제는 매장에 가입하고, 매장에 있는 영화 데이터를 받아 영화목록에 전달하는 방법에만 관심이 있다. 더 이상 영화를 어떻게 만들 것인가에 대한 관심이 없어졌다; 그것은 이제 영화 목록 구성 요소의 책임이다.

MoviesList 구성 요소는 프레젠테이션 구성 요소입니다. 영화 소품을 통해 주어진 영화만 선보인다. 스토어에서 영화를 구하든 로컬 스토리지에서 구하든, 더미 서버/더미 데이터에서 구하든 상관없다.

이를 통해 우리는 리액트 앱의 어느 곳이나 심지어 다른 프로젝트에서도 영화 목록 구성 요소를 재사용할 수 있습니다. 이 React 구성 요소는 비트 클라우드와 공유하여 전 세계의 다른 사용자가 프로젝트에서 해당 구성 요소를 사용할 수 있도록 지원합니다.

## Angular(각도)의 단일 책임 원칙

각 앱은 구성 요소로 구성됩니다. 구성요소는 요소로 구성된 단일 보기를 보유합니다.

구성 요소를 사용하면 단순한 단일 뷰 단위로 복잡한 앱을 쉽게 구축할 수 있습니다. 컴포넌트는 복잡한 앱을 만들기 위해 먼저 뛰어들지 않고, 소형 유닛에서 앱을 분해하고 구성할 수 있게 해줍니다.

예를 들어, 페이스북 같은 소셜 미디어 앱을 만들고 싶다고 가정해 보자. HTML 파일만 만들고 요소를 넣을 수는 없습니다. HTML 파일을 다음과 같은 구조로 구성하려면 작은 보기 단위로 세분해야 합니다.

- 피드 페이지
- 프로필 페이지
- 등록 페이지
- 로그인 페이지

각 파일은 구성요소로 구성됩니다. 예를 들어, 피드 페이지는 친구의 피드, 의견, 좋아요, 공유로 구성되어 몇 가지 이름을 지정합니다. 이 모든 것은 개별적으로 처리되어야 한다.

이를 구성 요소로 구성하면 API에서 가져온 피드 배열을 가져오는 `FeedList` 구성 요소와 데이터 피드 표시를 처리하는 `FeedView` 구성 요소가 있다.

새 Angular(각도) 애플리케이션을 구축할 때 다음을 기준으로 시작하십시오.

- 응용 프로그램을 별도의 구성 요소로 세분화
- 각 구성 요소의 책임 설명
- 각 구성요소의 입력과 출력, 즉 공용 인터페이스를 설명합니다.

우리가 작성하는 대부분의 구성요소는 단일 책임 원칙을 위반합니다. 예를 들어 엔드포인트에서 동영상을 나열하는 앱이 있다고 가정해 보겠습니다.

```coffeescript
@Component({
    selector: 'movies',
    template: `
        <div>
            <div>
                <div *ngFor="let movie of movies">
                    <h3>{movie.name}</h3>
                    <h3>{movie.year}</h3>
                    <h3>{movie.producer}</h3>
                    <button (click)="delMovie(movie)">Del</button>
                </div>
            </div>
        </div>
    `
})
export class MoviesComponent implements OnInit {
    this.movies = []
    constructor(private http: Http) {}
ngOnInit() {
        this.http.get('api/movies/').subscribe(data=> {
            this.movies = data.movies
        })
    }
delMovie(movie) {
        // deletion algo
    }
}
```

이 구성 요소는 다음을 담당합니다.

- api/movies API에서 영화 가져오기
- 영화 배열 관리

이건 사업에 좋지 않아. 왜? 이 구성 요소는 한 가지 작업을 담당해야 합니다. 두 가지 작업을 모두 담당할 수는 없습니다.

각 구성 요소를 하나의 책임으로 할당하는 목적은 구성 요소를 재사용하고 최적화할 수 있도록 하는 것입니다. 일부 책임을 다른 구성요소에 미룰 수 있도록 예제 구성요소를 재제조해야 합니다. 또 다른 구성요소는 영화 배열을 처리해야 하며 데이터 수집 논리는 서비스 클래스로 처리해야 한다.

```coffeescript
@Injectable() {
    providedIn: 'root'
}
export class MoviesService {
    constructor(private http: Http) {}
getAllMoives() {...}
    getMovies(id) {...}
    saveMovie(movie: Movie) {...}
    deleteMovie(movie: Movie) {...}
}
@Component({
    selector: 'movies',
    template: `
        <div>
            <div>
                <movies-list [movies]="movies"></movies-list>
            </div>
        </div>
    `
})
export class MoviesComponent implements OnInit {
    this.movies = []
    constructor(private moviesService: MoviesService) {}
ngOnInit() {
        this.moviesService.getAllMovies().subscribe(data=> {
            this.movies = data.movies
        })
    }
}
@Component({
    selector: 'movies-list',
    template: `
        <div *ngFor="let movie of movies">
            <h3>{movie.name}</h3>
            <h3>{movie.year}</h3>
            <h3>{movie.producer}</h3>
            <button (click)="delMovie(movie)">Del</button>
        </div>
    `
})
export class MoviesList {
    @Input() movies = null
delMovie(movie) {
        // deletion algo
    }
}
```

여기서 우리는 `영화 구성 요소`의 여러 가지 우려 사항을 분리했다. 현재 영화목록은 영화의 배열을 다루고 있으며 영화구성요소는 영화 입력을 통해 영화목록에 영화 배열을 보내는 모체이다. Movies Component는 배열의 포맷 및 렌더링 방법을 알지 못합니다. 이는 MoviesList 구성 요소에 달려 있습니다. 영화목록의 유일한 책임은 영화 입력을 통해 영화 배열을 받아들이고 영화를 전시/관리하는 것이다.

최근 영화 또는 관련 영화를 동영상 프로파일 페이지에 표시하려고 합니다. 새 구성 요소를 작성하지 않고도 영화 목록을 다시 사용할 수 있습니다.

```coffeescript
@Component({
    template: `
    <div>
        <div>
            <h3>Movie Profile Page</h3>
            Name: {movie.name}
            Year: {movie.year}
            Producer: {movie.producer}        
        </div>
<br />
<h4>Movie Description</h4>
        <div>
            {movie.description}
        </div>
        <h6>Related Movies</h6>
        <movies-list [movies]="relatedMovies"></movies-list>
    </div>    
    `
})
export class MovieProfile {
    movie: Movie = null;
    relatedMovies = null;
    constructor(private moviesService: MoviesService) {}
}
```

우리 영화 구성품은 응용 프로그램의 메인 페이지에 있는 영화를 표시하는 데 사용되기 때문에 사이드바의 영화 목록을 사용하여 최신 유행 영화, 최고 등급 영화, 최고 수익 영화, 최고 애니메이션 영화 등을 표시할 수 있다. 어떤 일이 있어도 영화목록 구성요소는 완벽하게 들어맞을 수 있다. 무비 클래스에 추가 속성을 추가할 수도 있고 무비리스트 구성 요소를 사용하는 경우 코드가 깨지지 않습니다.

다음으로, 우리는 영화 데이터 수집 논리를 영화 서비스로 이동시켰다. 이 서비스는 당사의 영화 API에 대한 모든 CRUD 작업을 처리합니다.

```js
@Injectable() {
    providedIn: 'root'
}
export class MoviesService {
    constructor(private http: Http) {}
getAllMovies() {...}
    getMovies(id) {...}
    saveMovie(movie: Movie) {...}
    deleteMovie(movie: Movie) {...}
}
```

무비 컴포넌트는 무비서비스를 도입해 필요한 모든 방법을 동원한다. 우려 분리의 한 가지 이점은 낭비되는 렌더링을 방지하기 위해 이 클래스를 최적화할 수 있다는 것입니다.

Angular(각도)의 Change detection(변경 감지)은 루트 구성 요소 또는 이를 트리거하는 구성 요소에서 시작됩니다. Movies Component는 Movie List를 렌더링하며 CD를 실행할 때마다 Movies Component가 리렌더되고 Movies List가 그 뒤를 잇는다. 입력이 변경되지 않은 경우 구성 요소를 다시 렌더링하는 것은 낭비일 수 있습니다.

영화 컴포넌트는 스마트 컴포넌트로, 영화 리스트는 바보 컴포넌트로 생각해보라. 왜일까? Movies Component는 렌더링할 데이터를 가져오지만 Movies List는 렌더링할 동영상을 수신하기 때문입니다. 아무것도 받지 않으면 아무것도 안 된다.

스마트 구성요소는 예측할 수 없는 부작용을 일으키므로 최적화할 수 없습니다. 이러한 데이터를 최적화하려고 하면 잘못된 데이터가 표시됩니다. 덤프 성분은 예측이 가능하기 때문에 최적화될 수 있습니다. 주어진 것을 출력하고 그래프가 선형입니다. 스마트 구성요소의 그래프는 수많은 이상 징후 차이를 가진 프랙탈 곡선과 같습니다.

즉, 스마트 부품은 불순한 기능과 같고 덤 구성 요소는 Redux의 리듀서와 같은 순수한 기능이다. OnPush에 Change Detection을 추가하여 MovieList 요소를 최적화할 수 있습니다.

```coffeescript
@Component({
    selector: 'movies-list',
    template: `
        <div *ngFor="let movie of movies">
            <h3>{movie.name}</h3>
            <h3>{movie.year}</h3>
            <h3>{movie.producer}</h3>
            <button (click)="delMovie(movie)">Del</button>
        </div>
    `,
    changeDetection: ChangeDetectionStrategy.OnPush
})
export class MoviesList {
    @Input() movies = null
delMovie(movie) {
        // deletion algo
    }
}
```

`MovieList`는 다음 경우에만 다시 렌더링됩니다.

- 동영상 배열 입력 변경
- `Del` 버튼을 클릭합니다.

확인해 보세요. 이전 영화 값이 다음과 같은 경우:

```bash
[
    {
        name: 'MK',
        year: 'Unknown'
    }
]
```

현재 값은 다음과 같습니다.

```bash
[
    {
        name: 'MK',
        year: 'Unknown'
    },
    {
        name: 'AEG',
        year: '2019'
    }
]
```

구성 요소는 새로운 변경사항을 반영하기 위해 다시 렌더링해야 합니다. Del 버튼을 클릭하면 리렌딩이 이루어집니다. 여기서 Angular는 루트에서 다시 렌더링하기 시작하지 않고 `MovieList` 구성 요소의 상위 구성 요소에서 시작됩니다. 그 이유는 동영상 배열에서 동영상을 제거하여 구성 요소가 나머지 배열을 반영하도록 렌더링하기 때문입니다. 이 구성 요소는 동영상 배열에서 동영상을 삭제하므로 재사용이 제한될 수 있습니다.

상위 구성 요소가 배열에서 두 개의 동영상을 삭제하려는 경우 어떻게 됩니까? 우리는 변화에 적응하기 위해 영화 목록을 만지는 것이 단일 책임 원칙에 위배된다고 볼 것이다.

실제로 영화를 배열에서 삭제해서는 안 됩니다. 상위 구성 요소가 이벤트를 선택하고, 해당 배열에서 동영상을 삭제하고, 배열의 나머지 값을 구성 요소로 다시 전달하도록 하는 이벤트를 내보내야 합니다.

```coffeescript
@Component({
    selector: 'movies-list',
    template: `
        <div *ngFor="let movie of movies">
            <h3>{movie.name}</h3>
            <h3>{movie.year}</h3>
            <h3>{movie.producer}</h3>
            <button (click)="delMovie(movie)">Del</button>
        </div>
    `,
    changeDetection: ChangeDetectionStrategy.OnPush
})
export class MoviesList {
    @Input() movies = null
    @Output() deleteMovie = new EventEmitter()
delMovie(movie) {
        // deletion algo
        this.deleteMovie.emit(movie)
    }
}
```

따라서, 상위 구성요소가 두 개의 동영상을 삭제하려면 두 개의 이벤트를 내보낼 수 있습니다.

```coffeescript
@Component({
    selector: 'movies',
    template: `
        <div>
            <div>
                <movies-list [movies]="movies" (deleteMovie)="delMovie"></movies-list>
            </div>
        </div>
    `
})
export class MoviesComponent implements OnInit {
    this.movies = []
    constructor(private moviesService: MoviesService) {}
ngOnInit() {
        this.moviesService.getAllMovies().subscribe(data=> {
            this.movies = data.movies
        })
    }
    delMovie() {
        this.movies.splice(this.movies.length,2)
    }
}
```

보시다시피 덤프 구성 요소는 상위 구성 요소와 사용자 상호 작용을 기반으로 렌더링되며, 이는 예측 가능하고 따라서 최적화 가능합니다.

다음 `OnPush` 변경 감지 전략을 추가하여 스마트 구성요소를 최적화할 수 있습니다.

```coffeescript
@Component({
    selector: 'movies',
    template: `
        <div>
            <div>
                <movies-list [movies]="movies"></movies-list>
            </div>
        </div>
    `,
    changeDetection: ChangeDetctionStrategy.OnPush
})
export class MoviesComponent implements OnInit {
    this.movies = []
    constructor(private moviesService: MoviesService) {}
ngOnInit() {
        this.moviesService.getAllMovies().subscribe(data=> {
            this.movies = data.movies
        })
    }
}
```

그러나 이는 부작용을 낳아 수없이 촉발시킬 수 있고 온푸시 전략을 무용지물로 만들 수 있다.

덤프 구성 요소는 최적화되어 고성능에 도움이 되기 때문에 애플리케이션의 대부분을 차지해야 합니다. 스마트 구성요소를 너무 많이 사용하면 최적화되지 않기 때문에 앱이 느려질 수 있습니다.

## 단일 책임: 부작용

앱 상태가 특정 기준점에서 변경될 때 부작용이 발생할 수 있습니다. 성능에 어떤 영향을 미칩니까?

다음과 같은 기능이 있다고 가정해 보겠습니다.

```undefined
let globalState = 9
function f1(i) {
    return i * 90
}
function f2(i) {
    return i * globalState
}
f1 can be optimized to stop running when the input is the same as prev, but f2 cannot be optimized because it is unpredictable, it depends on the globalState variable. It will store its prev value but the globalState might have been changed by an external factor it will make optimizing f2 hard. f1 is predictable because it doesn't depend on an outside variable outside its scope.
```

### 반응에서의 부작용

부작용으로 인해 오래된 데이터나 부정확한 데이터가 발생할 수 있습니다. 이를 방지하기 위해 React는 콜백에서 부작용을 수행하는 데 사용할 수 있는 `use Effect` 후크를 제공합니다.

```js
function SmartComponent() {
  const [token, setToken] = useState('')
  useEffect(() => {
    // side effects code here...
    const _token = localStorage.getItem("token")
    setToken(token)
  })
  return (
    <div>
      Token: {token}
    </div>
  )
}
```

여기서는 부작용인 로컬 스토리지(local storage)를 사용하여 외부 데이터를 얻고 있습니다. 이 작업은 사용 효과 후크 안에서 수행됩니다. `use Effect` 후크의 콜백 기능은 구성 요소가 마운트/업데이트/언마운트될 때마다 호출됩니다.

종속성 배열이라는 두 번째 인수를 전달함으로써 `use Effect` Hook을 최적화할 수 있다. 변수는 각 업데이트에서 `사용 효과`를 확인하여 리렌더에서 실행을 건너뛸지 여부를 확인하는 것입니다.

### 각도의 부작용

스마트 부품은 OnPush로 최적화하면 데이터가 부정확해진다.

예를 들어, "Movies Component"를 예로 들어 보겠습니다. OnPush로 최적화하고 특정 데이터를 수신하는 입력을 가지고 있다고 가정합시다.

```js
@Component({
    template: `
        ...
        <button (click)="refresh">Refresh</button>
    `,
    changeDetection: ChangeDetectionStartegy.OnPush
})
export class MoviesComponent implements OnInit {
    @Input() data = 9
    this.movies = []
    constructor(private moviesService: MoviesService) {}
ngOnInit() {
        this.moviesService.getAllMovies().subscribe(data=> {
            this.movies = data.movies
        })
    }
refresh() {
        this.moviesService.getAllMovies().subscribe(data=> {
            this.movies = data.movies
        })        
    }
}
```

이 구성 요소는 HTTP 요청을 수행하여 부작용을 일으킵니다. 이 요청은 구성 요소 내부의 동영상 배열에서 데이터를 변경하며 동영상 배열을 렌더링해야 합니다. 우리의 데이터는 `9`의 가치가 있다. 이 구성 요소가 다시 렌더링되면 새로 고침 메서드를 실행하는 버튼을 클릭하면 네트워크에서 새 영화 배열을 가져오는 HTTP 요청이 발생하고 이 구성 요소에 대해 `변경 감지`가 실행됩니다. 이 구성 요소의 @Input() 데이터가 상위 항목에서 변경되지 않으면 이 구성 요소가 다시 렌더링되지 않아 동영상 배열이 부정확하게 표시됩니다. 이전 동영상이 표시되지만 새 동영상도 가져옵니다.

이제 부작용에 대해 알아보셨을 겁니다. 부작용을 일으키는 성분은 예측할 수 없고 최적화하기 어렵다.

부작용은 다음과 같습니다.

- HTTP 요청
- 글로벌 상태 변경(Redux에서)

### ngrx 효과

ngrx는 Angular에 대한 반응성 확장의 모음입니다. 앞서 살펴본 바와 같이 구성 요소는 서비스 기반입니다. 구성 요소는 상태를 제공하기 위해 네트워크 요청과 다른 작업을 수행하기 위해 서비스를 주입합니다. 이러한 서비스는 또한 다른 서비스를 업무에 투입하므로 구성 요소의 책임도 달라집니다.

영화 컴포넌트처럼 영화 서비스(Movies Service)를 도입해 영화 API에서 CRUD 작업을 수행했다.

또한 이 서비스는 네트워크 요청을 수행하기 위해 HTTP 서비스 클래스를 삽입합니다. 이로 인해 우리의 영화 구성요소는 영화 서비스 클래스에 의존하게 된다. 영화서비스 클래스가 큰 변화를 일으킬 경우 영화 구성요소에 영향을 미칠 수 있다. 여러분의 앱이 서비스를 주입하는 수백 개의 구성요소로 증가한다고 상상해 보십시오. 여러분은 서비스를 재공급하는 모든 구성요소를 뒤져보고 있는 자신을 발견할 수 있을 것입니다.

많은 스토어 기반 애플리케이션은 RxJS 기반 부작용 모델을 통합한다. 효과는 당사 구성요소의 수많은 책임을 덜어줍니다.

예를 들어, `MoviesComponent`에서 효과를 사용하고 영화 데이터를 `Store`로 이동하도록 하겠습니다.

```coffeescript
@Component({
    selector: 'movies',
    template: `
        <div>
            <div>
                <movies-list [movies]="movies | async"></movies-list>
            </div>
        </div>
    `
})
export class MoviesComponent implements OnInit {
    movies: Observable<Movies[]> = this.store.select(state => state.movies)
constructor(private store: Store) {}
ngOnInit() {
        this.store.dispatch({type: 'Load Movies'})
    }
}
```

더 이상 영화 서비스가 없습니다. 영화 효과 클래스에 위임되었습니다.

```js
class MoviesEffects {
    loadMovies$ = this.actions.pipe(
        ofType('Load Movies'),
        switchMap(action =>
            this.moviesService.getMovies()
            .map(res => ({ type: 'Load Movies Success',payload: res }))
            .catch(err => Observable.of({ type: 'Load Movies Failure', payload: err }))
            );
    )
constructor(private moviesService: MoviesService, private actions: Actions) {}
}
```

영화서비스는 더 이상 영화 구성요소의 책임이 아니다. 무비서비스 변경도 무비 컴포넌트에는 영향을 주지 않는다.

## 컨테이너 및 프레젠테이션 구성 요소

컨테이너 구성요소는 자체 데이터를 생성하고 렌더링할 수 있는 자체 포함 구성요소입니다. 컨테이너 구성 요소는 자체 샌드박스 경계 내에서 내부 작업이 어떻게 작동하는지에 대해 우려합니다.

Oren Farhi에 따르면 컨테이너 구성요소는 몇 가지 작업을 수행하고 몇 가지 결정을 내릴 수 있을 만큼 충분히 똑똑합니다.

- 종종 표시될 수 있는 데이터 가져오기를 담당합니다.
- 다른 여러 구성 요소로 구성될 수 있습니다.
- 그것은 "주장하다"는 뜻으로, 특정 상태를 관리할 수도 있음을 의미합니다.
- 내부 구성 요소의 이벤트 및 비동기 작업을 처리합니다.

컨테이너 구성 요소는 스마트 구성 요소라고도 합니다.

프레젠테이션 구성 요소는 상위 구성 요소로부터 데이터를 가져옵니다. 상위로부터 입력을 받지 않으면 데이터가 표시되지 않습니다. 그들은 그들 자신의 데이터를 생성할 수 없다는 점에서 멍청합니다; 이것은 부모에게 달려있습니다.

## 결론

우리는 React/Angular에서 부품을 재사용할 수 있도록 깊이 연구했습니다. 코드 작성이나 코드 작성 방법만 아는 것이 아니라, 코드 작성도 잘 할 줄 아는 것입니다.

복잡한 것들을 만드는 것으로 시작하지 말고 작은 요소들로 그것들을 만들어라. 단일 책임 원칙을 통해 깨끗하고 재사용 가능한 코드를 작성할 수 있습니다.
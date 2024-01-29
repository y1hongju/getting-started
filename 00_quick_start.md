# [Quick Start](https://react.dev/learn)

본 문서는 앞으로 매일 사용하게 될 리액트 개념의 80%를 소개하고 있다.
- Component를 만들고 중첩(nest)하는 방법
- Markup과 style을 더하는 방법
- 데이터를 표시(display)하는 방법
- Condition과 list를 렌더링하는 방법
- Event에 반응하여 screen을 업데이트 하는 방법

## Creating and nesting components

리액트 애플리케이션은 컴포넌트들로 구성된다. 하나의 컴포넌트는 그것의 logic과 apperance를 가지고 있는 UI의 일종이다. 컴포넌트는 하나의 작은 버튼이 될 수도 있고, 페이지 전체가 될 수도 있다. 컴포넌트는 markup을 반환하는 자바스크립트 함수이다.

```javascript
function MyButton() {
    return (
        <button>I'm a button</button>
    );
}
```

위 `MyButton`은 다른 컴포넌트에 중첩될 수 있다.

```javascript
export default function MyApp() {
    return (
        <div>
            <h1>Welcome to my app</h1>
            <MyButton />
        </div>
    );
}
```

리액트 컴포넌트의 이름은 항상 대문자로 시작하며 이것이 항상 소문자로만 구성되는 HTML 태그와의 차이점이며 `export default` 키워드가 파일 내 메인 컴포넌트를 지정하는 데 사용된다.

## Writing markup with JSX

위 코드에서 사용된 markup과 관련한 syntax를 JSX라고 칭한다. 이는 선택사항이지만 대부분의 리액트 프로젝트에선 편리성을 이유로 이 방식을 사용한다.

JSX는 자바스크립트보다 (문법에)엄격하다. `<br />` 처럼 태그를 반드시 닫아야 한다. 또한 컴포넌트은 두 개 이상의 JSX 태그를 반환할 수 없다. 여러 개의 태그를 반환해야 하는 경우 공유된 부모 태그를 감싸야 한다. `<div>...<div>`나 `<>...<>` 같은 빈 wrapper로 감쌀 수 있다.

```js
function AboutPage() {
    return (
        <>
            <h1>About</h1>
            <p>Hello there.<br />How do you do?</p>
        </>
    );
}
```

## Adding sytles

리액트에선 CSS class를 `className`으로 지정할 수 있다. 이는 HTML의 `class` attribute와 동일하게 작동한다.

```js
<img className="avater" />
```

이후 별도의 CSS 파일에서 내용을 정의하면 된다.

```css
/* In your CSS */
.avatar {
    border-raduis: 50%;
}
```

리액트는 CSS 파일을 추가하는 방법을 규정으로 가지고 있지 않다. 간단하게 HTML에 `<link>` 태그를 추가하여 사용할 수도 있고, 만약 빌드 도구나 프레임워크를 사용한다면 해당 도구에서 제공하는 방식을 따르면 된다.

## Displaying data

JSX를 사용하면 마크업을 자바스크립트 안에 넣을 수 있다. 중괄호를 사용하여 다시 자바스크립트 모드로 전환할 수 있고(escape back) 코드의 어떤 변수를 임베딩할 수 있으며 사용자의 화면에 표시할(display) 수도 있다. 아래는 user.name을 화면에 표시하는 예시이다.

```js
return (
    <h1>
        {users.name}
    </h1>
)
```

JSX attribute에서도 자바스크립트 모드를 사용할 수 있으며 반드시 따옴표가 아닌 중괄호를 사용해야 한다.

```js
return (
    <img
        className="avatar"  // avatar string을 CSS class로 전달
        src={user.imageUrl}  // user.imageUrl 변수의 값을 읽고 src에 전달
    />
)
```

```js
/* App.js */

const user = {
    name: 'Hedy Lamarr',
    imageUrl: 'https://i.imgur.com/yXOvdOSs.jpg',
    imageSize: 90,
}

export default function Profile() {
    return (
        <>
            <h1>{user.name}</h1>
            <img
                className="avatar"
                src={user.imageUrl}
                alt={'Photo of ' + user.name}
                style={
                    {
                        width: user.imageSize,
                        height: user.imageSize,
                    }
                }
            />
        </>
    )
}
```

## Conditional rendering 

리액트에는 별도의 조건문 작성을 위한 문법은 없다. 다만 기존에 자바스크립트를 사용하던 방식을 사용하면 될 뿐이다.

```js
let content;

if (isLoggedIn) {
    content = <AdminPanel />;
} else {
    content = <LoginForm />;
}

return (
    <div>
        {content}
    </div>
)
```

더 간결한 코드를 원한다면 `?` 연산자를 사용할 수 있다. 이는 JSX 내부에서도 사용할 수 있는 방식이다.

```js
<div>
    {
        isLoggedIn ? (
            <AdminPanel />
        ) : (
            <LoginForm />
        )
    }
</div>
```

`else` 분기가 필요하지 않다면 `&&` 연산자를 사용할 수도 있다.

```js
<div>
    {isLoggedIn && <AdminPanel />}
</div>
```

위 방식을 사용하면 attribute에 조건문을 적용할 수도 있다.

## Rendering lists

컴포넌트의 리스트를 렌더링할 때 다음과 같이 `map()` 함수를 사용할 수 있다.

```js
const products = [
    { title: 'Cabbage', id: 1 },
    { title: 'Garlic', id: 2 },
    { title: 'Apple', id: 3 },
];

// Transform an array of products into an array of <li> items
const listItems = productm.map(product =>
    <li key={product.id}>
        {product.title}
    </li>
);

return (
    <ul>{listItems}</ul>
);
```

```js
const products = [
    { title: 'Cabbage', isFruit: false, id: 1 },
    { title: 'Garlic', isFruit: false, id: 2 },
    { title: 'Apple', isFruit: true, id: 3 },
];

export default function ShoppingList() {
    const listItems = products.map(product =>
        <li
            key={product.id}
            style={
                {
                    color: product.isFruit ? 'magenta' : 'darkgreen'
                }
            }
        >
            {product.title}
        </li>
    );

    return (
        <ul>{listItems}</ul>
    );
}
```

## Rseponding to events

컴포넌트 내부에 'event handler'를 선언하여(declaring) 이벤트에 대응할 수 있다.

```js
function MyButton() {
    function handleClick() {
        alert('You clicked me!');
    }

    return (
        <button onClick={handleClick}>
            Click me
        </button>
    )
}
```

이벤트 핸들러 함수를 호출하지 않도록 하는 것에 유의해야 한다. 이벤트가 발생하면 리액트가 함수를 호출하기 때문이다.

## Updating the screen

컴포넌트가 어떤 정보를 기억하고(remember) 표시하도록(display) 할 수 있다. 같은 컴포넌트를 여러번 렌더링 할 때는 각 컴포넌트가 고유의 상태값을 가지게 된다.

```js
import { useState } from 'react';

export default function MyApp() {
    return (
        <div>
            <h1>Counters that update seperately</h1>
            <MyButton />
            <MyButton />
        </div>
    )
}

function MyButton() {
    /*
    Two things from `useState`

    count: 현재 상태(state)
    setCount: 상태를 업데이트 할 수 있는 함수
    */
    const [count, setCount] = useState(0);

    function handleClick() {
        setCount(count + 1)
    }

    return (
        <button onClick={handleClick}>
            Clicked {count} times
        </button>
    )
}
```

## Using Hooks

함수의 이름이 `use`로 시작하는 경우에는 이 함수를 훅이라(Hook) 칭한다. `useState`의 경우 리액트가 제공하는 내장(built-in) 훅이다. 사용자는 내장 훅을 조합하여 새로운 훅을 만들 수 있다.

훅은 다른 함수보다 더 엄격함이 요구된다. 일례로 컴포넌트나 다른 훅의 상부에서만(at the top) 훅을 호출할 수 있다. 또 조건문이나 반복문 내부에서 `useState`를 사용하려면 새로운 컴포넌트를 추출하고(extract) 그 내부에 포함해야 한다.

## Sharing data between components

컴포넌트들 간에 데이터를 공유하고 동시에 업데이트 되도록 할 수 있다.

```js
import { useState } from 'react';

export default function MyApp() {    
    const [count, setCount] = useState(0);  // Moved state `count`

    function handleClick() {
        setCount(count + 1);
    }

    return (
        <div>
            <h1>Counters that update separately</h1>
            <MyButton count={count} onClick={handleClick} />
            <MyButton count={count} onClick={handleClick} />
            <MyButton />
        </div>
    );
}

function MyButton({ count, onClick }) {  // props
    return (
        <button onClick={onClick}>
            Clicked {count} times
        </button>
    );
}
```

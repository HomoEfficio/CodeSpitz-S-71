# ES6 Warming Up

이 글은 Hika Maeng님의 Code Spitz - S71 강의 내용을 정리한 글입니다.

본격적인 객체지향 디자인 패턴에 들어가기에 앞서,
ES2015+로 워밍업 좀 해보자.

## json 읽어서 테이블로 표시하기

강의 내용에 있던 소스에 주석 정도만 추가 

```html
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>CodeSpitz71-1</title>
</head>
<body>

<section id="data"></section>

<script>
const Table = (_ => {
	// 여기는 private static 영역
	const Private = Symbol();

	return class {
		// 생성자
		constructor(parent) {
			// 가드 올려라(Shield Pattern)
			if (typeof parent != 'string' || !parent) throw "invalid param";
			this[Private] = {parent};
		}
		// load(url) {
		// 	fetch(url).then(response => {
		// 		return response.json();
		// 	}).then(json => {
		// 		this._render();
		// 	})
		// }

		// public 메서드	
		// promise 보다 간단한 async/await라는게 있다니 써보자.
		async load(url) {
			// url에 대한 가드도 있으면 좋아요!

			const response = await fetch(url);
			// 가드 올려라(Shield Pattern)라
			if (!response.ok) throw "invalid response";
			const json = await response.json();
			const {title, header, items} = json;
			// 가드 올려라(Shield Pattern)라
			if (!items.length) throw "no items";

			Object.assign(this[Private], {title, header, items});
			this._render();
		}

		// private 메서드
		_render() {
			// 부모, 데이터 체크
			const fields = this[Private];
			const parent = document.querySelector(fields.parent);
			// 가드 올려라(Shield Pattern)라
			if (!parent) throw "invalid parent";
			if (!fields.items || !fields.items.length) {
				parent.innerHTML = "no data";
				return;
			} else {
				parent.innerHTML = "";
			}
			// Table 생성
			const table = document.createElement("table");			
			// Caption 생성
			const caption = document.createElement("caption");
			caption.innerHTML = fields.title;
			table.appendChild(caption);
			// THEAD, TH 생성
			table.appendChild(
				fields.header.reduce((thead, data) => {
					const th = document.createElement("th");
					th.innerHTML = data;
					thead.appendChild(th);
					return thead;
				}, document.createElement("thead"))
			);
			// TR, TD 생성 및 부모에 TABLE 추가
			parent.appendChild(
				fields.items.reduce((table, row) => {
					table.appendChild(
						row.reduce((tr, col) => {
							const td = document.createElement("td");
							td.innerHTML = col;
							tr.appendChild(td);
							return tr;
						}, document.createElement("tr"))
					)
					return table;
				}, table)
			);
		}
	};

})();

const table = new Table("#data");
table.load("https://gist.githubusercontent.com/hikaMaeng/717dc66225e40a8fe8d1c40366d40957/raw/447d44b800ed98817b0d29681be90aa1ec36e4ac/71_1.json");
</script>
</body>
</html>
```

워~ 이건 워밍업이 아냐~ 진 다 빠졌..
저거 JavaScript 맞아? ㄷㄷㄷ

## 공부해야 할 ES2015+

디자인 패턴 좀 배워보러 왔더니 첫날부터 이게 웬 암호학 강의야.. 이대로는 안 되겠다. 공부 좀 해야겠..

### arrow function

- 다른 언어에서는 보통 람다(Lambda)라고 불린다.
- arrow function은 함수지만 arrow function 내에서의 `this`는 JavaScript의 일반적인 함수 안에서의 `this`와는 다르게, 별도로 `this`를 바인딩하지 않고 arrow function이 사용되고 있는 scope에서의 `this`를 가리킨다.
- `function`, `var` 등과 같이 ES3.1에서만 존재하고 ES2015+에는 없는 키워드를 사용하면 ES3.1 호환 모드로 동작하게 되고 성능 저하가 발생한다.

### Symbol

- 식별자를 만드는 ES2015+의 새로운 방식

### async/await

- `promise`보다 더 간략하게 비동기로 함수를 호출하고, 비동기로 값을 받아올 수 있게 해주는 키워드
- `async`는 함수 앞에 붙일 수 있으나, 다른 함수의 파라미터로 넘겨지는 함수 앞에는 붙일 수 없다.
- `await`는 Java의 `ConcurrentFuture`와 거의 같다..(라고 했지만 실은 `ConcurrentFuture`가 아니라 `CompletableFuture`임)

### fetch

- XHR을 더 간단한 방식으로 대체하는 ES2015+의 새로운 기능

### Object.assign()

- 객체를 복사해서 할당(assign). 이걸 보자. https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign

### reduce()

- 여러 개의 값을 하나의 값으로 **집계**하는 것
- 같은 말이라도 **벡터를 스칼라로 만들어 주는 것**이라고 하니 몸서리치게 간지난다.
- 백엔드에서 빅데이터 하둡 맵리듀스 어쩌고 하는데 그건가? 그 맵reduce 맞음



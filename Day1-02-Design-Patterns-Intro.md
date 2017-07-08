# Design Patterns Intro

이 글은 Hika Maeng님의 Code Spitz - S71 강의 내용을 정리한 글입니다.

## 모든 프로그램은 변한다.

예전에도 그래왔고, 앞으로도 변하지 않을 유일한 원칙

>**모든 프로그램은 변한다.**

변경의 발생 자체는 개발자가 제어할 수 없다. 

개발자가 할 수 있는 일은 **발생한 변경에 의해 퍼져나가는 영향을 최소화하는 것** 뿐이다.

어떻게 최소화 할 것인가?


## 격리(Isolation)

파도가 육지로 밀려들어서 발생하는 피해를 막으려면, 방파제를 만들어서 파도와 육지를 격리시켜야 한다.

![Imgur](http://i.imgur.com/RDfolJS.jpg)

프로그램에서도 변경에 의한 여파를 최소화하려면, **격벽**을 만들어서 **변경되는 부분과 변경되지 않는 부분을 격리**시켜야 한다.

소프트웨어 공학의 상당 부분은 이 **격벽**을 어디에, 어떻게 만들 것인가에 대한 **격리 전략**을 다룬다.

**격리**란 변경되는 부분과 변경되지 않는 부분을 구분하는 것이라고 했는데, 모든 프로그램은 변한다고 했으니 변경되지 않는 부분이란 있을 수 없는 것 아닌가.

그렇다. 변경되지 않는 부분이란 없다.

그래서 변경되는 부분과 변경되지 않는 부분을 기준으로 격리한다는 표현보다, **변화율을 기준으로 격리한다**는 표현이 더 적합하다.

**변화율**이란 `시간적인 대칭성`을 말하는데, 쉽게 말해 프로그램의 부분마다 **변화의 원인과 발생 빈도가 다르다**는 성질을 나타내는 것이 **변화율**이라고 이해하자.

>격리 전략 by Kent Beck(켄트 벡의 책 '구현 패턴'에 나옴)
>- 변화율(Rate of Change)에 따라 작성하라

어떻게 해야 변화율에 따라 작성할 수 있을까?

## 강한 응집성 & 약한 의존성

변화율에 따라 작성할 수 있도록 도와주는 실천 수칙으로 **강한 응집성**과 **약한 의존성**을 들 수 있다(높은 응집도와 낮은 결합도라고 표현하기도 한다).

(내부)응집성, (외부)의존성, 이렇게 수식어를 붙이면 조금 더 쉽게 이해할 수 있다(아님 말고 ㅋ).

>- 변화율이 같은 것들을 모아두면 응집성이 높아진다.
>    - 변화율이 같은 것들을 분리하면 응집성이 낮아지고, 의존성이 높아진다.
> 
>- 변화율이 다른 것들을 분리하면 의존성이 낮아진다. 
>    - 변화율이 다른 것들을 모아두면 응집성이 낮아지고, 의존성이 높아진다.

- 응집도: cohesion, 결합도: coupling

    ![](http://www.ccs.neu.edu/research/demeter/DAJ/docs/thesis-sung_2002_05_29_final_draft_files/image131.gif)

그럼 변화율이 같은 것과 다른 것을 어떻게 구분할 것인가?

## 단일 책임

하나의 객체는 하나의 책임만을 지게 하자.

책임의 뒷면은 역할이다.

즉, 하나의 객체는 하나의 역할만을 담당하도록 하자.

**하나의 역할만을 수행하는데 필요한 데이터나 로직은 변화율이 같을 가능성이 높다.**

## 객체 지향 프로그래밍

알고리듬의 본질은 **상태와 제어문(조건문, 반복문)을 활용해서 문제를 푸는 것**이다.

그래서 문제를 푸는 데 필요한 모든 것을 한 곳에 동원해서 상태와 제어문을 통해 처리하게 된다.

이에 비해 객체 지향 프로그래밍은 문제를 푸는 데 필요한 모든 것을 한데 묶어 처리하는 대신, 다음과 같이 푼다.

>- 문제를 푸는 데 필요한 역할에 따라 여러 객체로 나누고, 
>- 여러 객체로 구성된 객체망에서,
>- 객체의 협력을 통해 문제를 푼다.

## Warming Up에서 풀었던 방식

앞서 Warming Up의 `JSON을 읽어서 테이블로 표현하기`를 떠올려보자.

제목에서 직관적으로 알 수 있듯이 문제를 풀려면,

1. JSON을 읽어와야 하고,
2. 읽어온 데이터를 테이블로 그려야 한다.

두 가지 역할이 도출되는 것을 알 수 있다.

하지만 Warming Up에서는 위 두 가지 역할을 메서드 수준에서만 분리했을 뿐, **`Table`이라는 하나의 객체에서 모든 일을 다 처리했다.**

이제 똑같은 문제를 객체 지향 프로그래밍으로 풀어보고, 어떤 부분이 달라지고 좋아지는지 알아보자.

## JSON 읽어 오기와 테이블 그리기의 분리 및 협력

*참고: 원래 강의에서는 Data Load가 아니라 Data Supply로 이름을 바꾸는 것이 좋다고 했으나, 개인적으로는 그냥 Data Load가 더 낫다고 생각합니다.*
*이유는 데이터를 가져오는 것(Data Load)과 데이터를 제공하는 것(Data Supply)은 다른 책임이므로, 데이터를 가져와서 제공하는 것을 하나로 묶어 Data Supply라고 할 필요는 없어 보이기 때문입니다. 그래서 제 노트에서는 그냥 Data Load로 씁니다. ^^*

Warming Up에서 데이터를 가져오는 부분의 소스는 다음과 같다.

```javascript
	async load(url) {
		// url에 대한 가드도 있으면 좋아요!

		const response = await fetch(url);
		// 가드 올려라(Shield Pattern)
		if (!response.ok) throw "invalid response";
		const json = await response.json();
		console.log('json:', json);
		const {title, header, items} = json;
		// 가드 올려라(Shield Pattern)
		if (!items.length) throw "no items";

		Object.assign(this[Private], {title, header, items});
		this._render();
	}
```

위 코드는 크게 데이터를 가져오는 부분(fetch)과 그리는 부분(_render)으로 나눌 수 있다.

가져오는 부분과 그리는 부분을 아예 다른 객체로 구분하면 대략 다음과 같은 틀로 구성할 수 있다.

```javascript
const data = new DataLoad("71_1.json");

const renderer = new Renderer();
renderer.render(data);
```

이제 JSON 데이터를 읽어서 테이블로 그리는 작업을 수행하기 위해 `JsonData`와 `Renderer` 객체가 객체망을 이루며 서로 협력한다.

자 이제 JSON 데이터를 가져오는 부분을 다음과 같이 구현하고,

```javascript
const DataLoad = class {
	constructor(data) {
		this._data = data;
	}

	async getData() {
		if (typeof this._data == 'string') {
			const response = await fetch(this._data);
			return await response.json();
		} else return this._data;
	}
}
```

테이블을 그리는 부분을 다음과 같이 구현하면,

```javascript
const Renderer = class {
	constructor() {}
	async render(data) {
		if (!(data instanceof JsonData)) throw "invalid data type";
		const json = await data.getData();

		// 아까 그렸던 방법으로 그린다.
	}
}
```

오~ 데이터 읽어오기와 그리기라는 **역할로 분리해서 협력하게 만들어 놓으니, 자동적으로 변화율에 따라 작성하기도 달성**한 것 같다.

짠~ 이제 객체 지향 완료?

객체 지향은 맞는데 완료는 아니다. 아직 더 객체 지향스럽게 만들 여지가 많다. 먼저 눈에 띄는 부분은 **`값`을 주고 받는 부분**이다.

## 값과 객체

코드를 다시 보자.

```javascript
const DataLoad = class {
	constructor(data) {
		this._data = data;
	}

	async getData() {
		if (typeof this._data == 'string') {
			const response = await fetch(this._data);
			return await response.json();    // <-- 여기!!
		} else return this._data;
	}
}


const Renderer = class {
	constructor() {}
	async render(data) {
		if (!(data instanceof JsonData)) throw "invalid data type";
		const json = await data.getData();    // <-- 여기!!

		// 아까 그렸던 방법으로 그린다.
	}
}
```

위에 `여기!!`라고 표시한 부분을 보면, json을 반환하고, json을 받아서 그리는데 사용한다. 정확하게는 json **값**을 반환하고, json **값**을 받아서 그린다.

그래도 잘 동작하는데 뭐가 문제일까?

**값을 구별하는 방법은 값 하나하나를 모두 비교해서 같은지 다른지 알아보는 방법 밖에는 없다.** 그래서 이렇게 값으로 주고 받는 관계에서는, **받는 쪽에서 받은 값 하나하나를 비교해서 사용할 수 있는 값인지 검증을 해야 한다.**

그럼 검증해서 하면 되는거 아닌가?

그렇다. 하지만 객체에는 일정한 Type을 부여할 수 있으므로, 값으로 전달하는 것보다는 객체로 전달하는 것이 편리하다.

그 외에도 값과 관련하여 더 중요한 근본적인 문제가 하나 있지만, 일단은 여기까지만 짚고 다음으로 넘어가자.

다음 문제는 데이터 읽어오기라는 역할의 내부와 그리기라는 역할의 내부에서 발견할 수 있다.

## 데이터를 읽어오는 방식의 변화

데이터를 읽어오는 역할은 그대로인데, 그 역할 내에서의 방식은 달라질 수 있다.

지금은 JSON 데이터를 읽어오지만, XML 데이터를 읽어올 수도 있고, CSV 데이터를 읽어올 수도 있다. 그렇게 되면 현재의 `DataLoad`에 변경이 발생한다. 

그 변경의 모습은 여러 가지가 있겠지만, 대략 다음과 같이 어딘가에 조건문이 심어질 것이다.

```javascript
const DataLoad = class {
	constructor(data) {
		this._data = data;
	}

	async getData() {
		switch(source) {
			case 'json':
				// json loading...
			break;
			case 'xml':
				// xml loading...
			break;
			case 'csv':
				// csv loading...
			break;
		}
	}
}
```

이렇게 되면 데이터를 읽어오는 방식이 추가될 때마다 조건문이 추가되어 코드에 변경이 필요하게 된다.

## 상속의 도입

데이터를 읽어오는 것은 그대로인데 데이터를 읽어오는 방식이 달라질 수 있다. 즉, **하나는 그대로인데 다른 하나는 달라질 수 있다는 건 변화율이 다르다는 얘기**다.

그래서 **변화율에 따라 작성하기**라는 원칙에 따라, 데이터를 읽어오는 것과 읽어오는 방식을 아래와 같이 상속 관계로 나눌 수 있다.

![Imgur](http://i.imgur.com/nyDe4Tl.png)

데이터 가져오기는 다음과 같이 작성된다.

```javascript
const DataLoad = class {
	async getData() { throw "getData must override"; }
};

const JsonData = class extends DataLoad {
	constructor(data){
		super();
		this._data = data;
	}

	async getData(){
		if(typeof this._data == 'string'){
			const response = await fetch(this._data);
			return await response.json();
		}else return this._data;
	}
};

const XmlData = class extends DataLoad {
	constructor(data){
		super();
		this._data = data;
	}

	async getData(){
		if(typeof this._data == 'string'){
			const response = await fetch(this._data);
			return await response.xml();
		}else return this._data;
	}
};
```

부모 객체인 `DataLoad`의 `getData()`는 자식 객체인 `JsonData`, `XmlData`에 의해 override 된다.

이렇게 하면 데이터를 읽어오는 방식이 달라지면, 아래와 같이 달라진 구현체만 넣어주면 될 뿐 `DataLoad`에 조건문이 필요하지 않으며, 따라서 코드도 달라질 필요가 없게 된다.

```javascript
// json 일 때 
const dataLoad = new JsonData(jsonDataSource);
dataLoad.getData();  // json 데이터 반환

// xml 일 때
const dataLoad = new XmlData(xmlDataSource);
dataLoad.getData();  // xml 데이터 반환
```

이런 **방식의 변화**는 데이터 읽어오기에만 해당되는 것이 아니라 그리기에도 해당된다. 

지금은 테이블에 그리기를 하고 있지만, 동일한 데이터로 엑셀에 그릴 수도 있고, 다른 그리드 라이브러리에 그릴 수도 있다. 

그렇다면 `Renderer`도 상속을 통해 분리할 수 있다. 

상속을 통한 분리 과정은 `DataLoad`와 거의 같으므로 `Renderer`에 대해서는 코드 설명 없이 그냥 최종 코드를 보는 것으로 하고 넘어간다. 

## 값 대신 규약

앞의 값과 객체 에서 잠시 언급했던 문제를 다시 살펴보자.

값으로 주고 받다 보니 받는 쪽에서 검증을 위해 값 하나하나 모두 검증해야만하는 문제가 있고, 이를 해결하기 위해 객체로 보내는 것이 좋겠다고 했다.

그 객체를 `Info`라고 하고 다음과 같이 구현하자.

```javascript
const Info = class{
	constructor(json){
		const {title, header, items} = json;
		if(typeof title != 'string' || !title) throw "invalid title";
		if(!Array.isArray(header) || !header.length) throw "invalid header";
		if(!Array.isArray(items) || !items.length) throw "invalid items";
		items.forEach((v, idx)=>{
			if(!Array.isArray(v) || v.length != header.length){
				throw "invalid items:" + idx;
			}
		});
		this._private = {title, header, items};
	}
	get title(){return this._private.title;}
	get header(){return this._private.header;}
	get items(){return this._private.items;}
};
```

여기서 눈여겨 볼 것은 **검증 로직이 `Info` 안에 포함되어 있다**는 것이다. 

앞에서 '값과 관련하여 더 중요한 근본적인 문제가 하나 있다'고 했는데, 그것은 바로 **변화율**이다.

**값으로 넘기면 받는 쪽에 검증 로직이 필요하고, 주는 쪽에서의 값이 변경되면 받는 쪽에서의 값 검증 로직도 변경되어야 하며**, 이렇게 되면 **주는 쪽과 받는 쪽의 변화율이 같아진다**는 근본적인 문제가 발생한다.

하지만 위와 같이 **값 뿐아니라 그 구조와 검증 로직을 응집성 있게 모두 가지고 있는 `Info`라는 객체를 사용**하면, `Info` 객체가 성공적으로 생성되었다는 것은 그 객체의 데이터가 검증을 통과한 우량한 상태라는 것을 의미하므로, **`Info`를 받아서 사용하는 쪽에서는 검증 로직을 제거할 수 있고, 주는 쪽과 받는 쪽의 변화율을 다르게 유지**할 수 있게 된다.

주는 쪽과 받는 쪽은 **값 대신에 `Info`라는 규약(protocol)에 의지하게 바꿈으로써 변화율을 다르게 유지할 수 있게 된다.**

이렇게 json이라는 값을 `Info`로 대체한 코드는 아래와 같다.

```javascript
const JsonData = class extends Data{
	constructor(data){
		super();
		this._data = data;
	}
	async getData(){
		let json;
		if(typeof this._data == 'string'){
			const response = await fetch(this._data);
			json = await response.json();
		}else json = this._data;
		return new Info(json);    // <--여기!!
	}
};
```

## Info 객체를 누가 생성하는 것이 맞는가

json을 `Info`로 대체하면서 주는 쪽과 받는 쪽을 `Info`라는 규약에 의해 멋지게 격리할 수 있었다. 그런데 한 가지 눈에 걸리는 것이 있는데, `Info` 객체를 주는 것이 실제로는 `DataLoad`가 아니라, `JsonData`라는 점이다.

```javascript
const DataLoad = class {
	async getData() { throw "getData must override"; }
};

const JsonData = class extends Data{
	constructor(data){
		super();
		this._data = data;
	}

	async getData(){
		let json;
		if (typeof this._data == 'string') {
			const response = await fetch(this._data);
			json = await response.json();
		} else json = this._data;
		return new Info(json);    // <--여기!!
	}
};
```

`JsonData`가 직접 `new Info(json)`을 통해 `Info` 객체를 생성하고 있다. 바꿔 말하면, **`DataLoad`의 구현체인 `JsonData`가 `DataLoad`와 `Renderer` 사이의 규약인 `Info`에 대해 알고 있다**는 것이다. 이게 왜 이상한지는 글보다는 객체망 그림으로 보면 훨씬 명백하게 할 수 있다.

![Imgur](http://i.imgur.com/GNWXc0E.png)

빨간색 선으로 표시한 것처럼, 뭔가 내부에 있는 것 같은 `JsonData`가 외부와의 협력에 사용되는 `Info`를 직접 알고 있는 것은 일단 모양새부터 이상하다는 게 보인다. 

`JsonData` 같은 **내부 구현체는 `Info`를 모르게 하고, `DataLoad`만 `Info`를 알도록** 하는 게 좋을 것 같다. 

코드를 다음과 같이 바꾸자.

```javascript
const Data = class {
	async getData() {
		const json = await this._getData();
		return new Info(json);      // <--여기!!
	}
	async _getData() {      // <--여기!!
		throw "_getData must overrided";
	}
};
const JsonData = class extends Data {
	constructor(data) {
		super();
		this._data = data;
	}
	async _getData() {      // <--여기!!
		if (typeof this._data == 'string') {
			const response = await fetch(this._data);
			return await response.json();
		} else return this._data;
	}
};
```

이 과정에 사용된 패턴이 **템플릿 메서드 패턴**이다.

이참에 내부와 외부의 구분을 조금 더 명확한 표현으로 알아보자.

## 내부 계약과 외부 계약

데이터를 읽어오는 것과 그리는 것은 역할 자체가 완전히 다르므로 서로를 **외부인**으로 간주할 수 있다. **외부인으로서 `Info`라는 규약을 통해 협력**하고 있을 뿐이다.

반면에 `JsonData`와 `DataLoad`의 관계는 조금 다르다. 데이터를 읽어오는 같은 역할을 하고 있고, **상속을 통해 협력**하고 있다.

>`JsonData`와 `DataLoad`처럼 **상속 관계에 있거나 또는 구성(composition) 관계로 묶여 있는 객체들은 내부 계약으로 묶여 있다**고 한다.
>
>반면에 `DataLoad`와 `Renderer`처럼 `Info`라는 **별도의 규약을 통해 협력하는 관계는 외부 계약으로 묶여 있다**고 한다.

외부 계약은 컴파일 타임에 확정(`Info`를 통해 컴파일 시에 검증 가능)되며, 내부 계약은 런타임에 확정(`DataLoader`의 구현체가 `JsonData`인지 `XmlData`인지는 런타임에 확정)된다.

## (강의에서의) 최종 코드

지금까지의 내용을 바탕으로 작성한 코드는 아래와 같다.

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
const Info = class{
	constructor(json){
		const {title, header, items} = json;
		if(typeof title != 'string' || !title) throw "invalid title";
		if(!Array.isArray(header) || !header.length) throw "invalid header";
		if(!Array.isArray(items) || !items.length) throw "invalid items";
		items.forEach((v, idx)=>{
			if(!Array.isArray(v) || v.length != header.length) throw "invalid items:" + idx + ":" +v + ":" + v.length + ":" + header.length;
		});
		this._private = {title, header, items};
	}
	get title(){return this._private.title;}
	get header(){return this._private.header;}
	get items(){return this._private.items;}
}
const Data = class{
	async getData(){
		const json = await this._getData();
		return new Info(json);
	}
	async _getData(){
		throw "_getData must overrided";
	}
};

const JsonData = class extends Data{
	constructor(data){
		super();
		this._data = data;
	}
	async _getData(){
		let json;
		if(typeof this._data == 'string'){
			const response = await fetch(this._data);
			return await response.json();
		}else return this._data;
	}
};

const Renderer = class{
	async render(data){
		if(!(data instanceof Data)) throw "invalid data type";
		this._info = await data.getData();
		this._render();
	}
	_render(){
		throw "_render must overried";
	}
}
const TableRenderer = class extends Renderer{
	constructor(parent){
		if(typeof parent != 'string' || !parent) throw "invalid param";
		super();
		this._parent = parent;
	}
	_render(){
		const parent = document.querySelector(this._parent);
		if(!parent) throw "invaild parent";
		parent.innerHTML = "";
		const [table, caption] = "table,caption".split(",").map(v=>document.createElement(v));
		caption.innerHTML = this._info.title;
		table.appendChild(caption);
		table.appendChild(
			this._info.header.reduce(
				(thead, data)=>(thead.appendChild(document.createElement("th")).innerHTML = data, thead),
				document.createElement("thead"))
		);
		parent.appendChild(
			this._info.items.reduce(
				(table, row)=>(table.appendChild(
					row.reduce(
						(tr, data)=>(tr.appendChild(document.createElement("td")).innerHTML = data, tr),
						document.createElement("tr"))
				), table),
				table)
		);
	}
}

const data = new JsonData("https://gist.githubusercontent.com/hikaMaeng/717dc66225e40a8fe8d1c40366d40957/raw/447d44b800ed98817b0d29681be90aa1ec36e4ac/71_1.json");

const renderer = new TableRenderer("#data");
renderer.render(data);
</script>
</body>
</html>
```

그런데 파일을 열어보면 에러가 나고, 무슨 에러인지 친절하게 알려주고 있다.

강의 중에 우리눈에는 보이지 않는다고 하던 바로 그 에러다. ㅋㅋ

에러를 발생시킨 데이터는 무시하고 에러를 발생시키지 않는 데이터만 테이블에 그리도록 코드를 수정해보자. 

여러 방식이 있겠지만, 단 한 줄로 수정할 수 있는 방법이 있으니 재미삼아 한 번 시도해 보자. ^^

## 두 걸음 더 나아가자

*참고: 이 부분은 강의에는 없던 내용입니다. 아마 의도적으로 남겨놓으신 걸로 보이는데 한 번 같이 알아보죠. 제가 잘못 이해한 것일 수도 있으니 참고만 하세요. ^^*

### 1. `Renderer`의 개선점

그런데 최종 코드를 유심히 보면, `Renderer` 쪽에서는 규약으로 정해놓은 `Info` 뿐아니라 `DataLoad`(위 코드에는 `Data`라고 나와있다)에도 의존하고 있는 게 눈에 보인다.

```javascript
const Renderer = class{
	async render(data){
		if(!(data instanceof Data)) throw "invalid data type";    // <--여기!!
		this._info = await data.getData();
		this._render();
	}
	_render(){
		throw "_render must overried";
	}
}
```

그림으로 보면 다음과 같이 이상해 보인다.

![Imgur](http://i.imgur.com/QINavPN.png)

`Renderer`는 다음 그림처럼 `Info`에만 의존해야 하지 않을까?

![Imgur](http://i.imgur.com/9bWVV4F.png)

위의 그림을 반영하면 `Renderer`는 다음과 같이 바뀐다.

```javascript
const Renderer = class {
	async render(info) {    // <--Info를 전달받는다.
		if (!(info instanceof Info)) throw "data is NOT Info type";
		this._info = info;		
		this._render();
	}

	_render() {
		throw "render must be overriden."
	}
}
```

그리고 `Renderer`를 호출하는 부분도 다음과 같이 바뀌어야 한다.

```javascript
const data = new JsonData("https://gist.githubusercontent.com/hikaMaeng/717dc66225e40a8fe8d1c40366d40957/raw/447d44b800ed98817b0d29681be90aa1ec36e4ac/71_1.json");

// const renderer = new TableRenderder("#data");
// renderer.render(data);

const infoPromise = data.getData();
infoPromise.then(info => {
	const renderer = new TableRenderer("#data");
	renderer.render(info);	  // <--Info를 전달한다.
});
```

이제 `Renderer`는 `DataLoad`에는 의존하지 않게 되었고, `DataLoad`와 `Renderer`는 `Info`에만 의존하게 되었다.

만세!!

그런데 하나가 더 눈에 걸린다.

### 2. `TableRenderer`의 개선점

```javascript
const TableRenderer = class extends Renderer {
	constructor(parent) {
		if (typeof parent != 'string' || !parent) throw "invalid param";
		super();
		this._parent = parent;
	}
	_render() {
		const parent = document.querySelector(this._parent);
		if(!parent) throw "invaild parent";
		parent.innerHTML = "";
		const [table, caption] = "table,caption".split(",").map(v=>document.createElement(v));
		caption.innerHTML = this._info.title;    // <--여기!!
```

`TableRenderer`가 `this._info`라는 부모 객체의 속성을 통해 `this._info.title`이라는 값에 접근하고 있으므로 문제될 게 없어 보인다.

그런데 `this._info`는 `Info` 객체를 그대로 받아온 객체이므로, `this._info.title`이라고 쓴 것은 `Info`객체 안에 `title`이라는 속성이 있다는 것을 알고 있다는 얘기다. 어떻게 알았을까? 

`TableRenderer`는 코드 상에서는 `Info`의 존재에 대해 전혀 모르고 있다. 그런데 `Info` 안에 `title`이라는 속성이 있다는 걸 마치 알고 있는 것처럼 태연하게 `this._info.title`이라고 읽어 오고 있다. 

그림으로 보면 다음과 같다.

![Imgur](http://i.imgur.com/TY3L66e.png)

이를 해결하려면 `TableRenderer`가 `Info`의 속성을 모르고 `Renderer`의 속성만 알게 하면 된다. 

코드는 다음과 같이 바꿀 수 있다.

```javascript
const Renderer = class {
	async render(info) {
		if (!(info instanceof Info)) throw "data is NOT Info type";
		// this._info = info;
		this._title = info.title;      // <--여기!!
		this._headers = info.headers;  // <--여기!!
		this._items = info.items;      // <--여기!!
		this._render();
	}

	_render() {
		throw "render must be overriden."
	}
}

const TableRenderer = class extends Renderer {
	constructor(parent) {
		if (typeof parent != 'string' || !parent) throw "invalid param";
		super();
		this._parent = parent;
	}
	_render() {
		const parent = document.querySelector(this._parent);
		if(!parent) throw "invaild parent";
		parent.innerHTML = "";
		const [table, caption] = "table,caption".split(",").map(v=>document.createElement(v));
		// caption.innerHTML = this._info.title;
		caption.innerHTML = this._title;    // <--여기!! 이하 this._info.header와 this._info.items도 바뀐다.
```

`TableRenderer`는 `Info`에 대해서는 전혀 모르고 그저 부모인 `Renderer`에 `_title` 속성이 있다는 것만 알면 된다.

이제 드디어 대망의 막을 내릴 시간이 왔다.

### 최종 소스

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
const Info = class {
	constructor(json) {
		// json에는 가드 올리지 않은 이유: destructuring 중 예외 발생 시 JavaScript가 throw 해주니까
		const {title, header, items} = json;
		
		// 가드 올려라(Shield Pattern)
		if(typeof title != 'string' || !title) throw "invalid title";
		if(!Array.isArray(header) || !header.length) throw "invalid header";
		if(!Array.isArray(items) || !items.length) throw "invalid items";
		// items.forEach((item, idx) => {
		// 	if (!Array.isArray(item) || item.length != header.length) {
		// 		throw `${idx}th item is invalid`;
		// 	}
		// });
		// this._private = {title, headers: header, items};

		// 또는 아래와 같이 filter를 써서 
		// invalid item은 누락시키고 valid한 item만으로 Info를 구성할 수도 있다.
		const validItems = items.filter((item) => (Array.isArray(item) && item.length === header.length));
		this._private = {title, headers: header, items: validItems};
	}

	get title() { return this._private.title; }
	get headers() { return this._private.headers; }
	get items() { return this._private.items; }
};

const DataLoader = class {
	async getData() {
		const json = await this._getData();
		return new Info(json);
	}

	async _getData() {
		throw "_getData() must be overriden."
	}
};

const JsonData = class extends DataLoader {
	constructor(url) {
		super();
		this._url = url;
	}

	async _getData() {
		let json;
		if (typeof this._url == 'string') {
			const response = await fetch(this._url);
			if (!response.ok) throw "invalid response";
			json = await response.json();
		} else {
			json = this._data;
		}
		return json;
	}
};

const Renderer = class {
	async render(info) {
		if (!(info instanceof Info)) throw "data is NOT Info type";
		// this._info = info;
		this._title = info.title;
		this._headers = info.headers;
		this._items = info.items;
		this._render();
	}

	_render() {
		throw "render must be overriden."
	}
}

const TableRenderer = class extends Renderer {
	constructor(parent) {
		if (typeof parent != 'string' || !parent) {
			throw "invalid parent";
		}
		super();
		this._parent = parent;
	}

	_render() {
		const parent = document.querySelector(this._parent);
		if (!parent) throw "invalid parent";    // TODO: 생성자에서의 메시지와 구별되는 메시지로
		parent.innerHTML = "";
		const [table, caption] = "table,caption".split(',').map(v => document.createElement(v));
		// caption.innerHTML = this._info.title;
		caption.innerHTML = this._title;
		table.appendChild(caption);
		table.appendChild(
			// this._info.headers.reduce(
			this._headers.reduce(
				(thead, header) => (thead.appendChild(document.createElement('th')).innerHTML = header, thead),
				document.createElement('thead')
			)
		);
		parent.appendChild(
			// this._info.items.reduce(
			this._items.reduce(
				(table, row) => (
					table.appendChild(
						row.reduce(
							(tr, col) => (tr.appendChild(document.createElement('td')).innerHTML = col, tr),
							document.createElement('tr')
						)
					), table
				),
				table
			)
		);
	}
}

const data = new JsonData("https://gist.githubusercontent.com/hikaMaeng/717dc66225e40a8fe8d1c40366d40957/raw/447d44b800ed98817b0d29681be90aa1ec36e4ac/71_1.json");

// const renderer = new TableRenderder("#data");
// renderer.render(data);

const infoPromise = data.getData();
infoPromise.then(info => {
	const renderer = new TableRenderer("#data");
	renderer.render(info);	
});
</script>
</body>
</html>
```

## 기타 자투리

### 레거시 코드란?

레거시 코드란
>- 남이 짠 코드 ㅋ
>- 내가 어제 짠 코드 ㅋ

라기보다는 **현재 요구사항에 대응하지 못하는 코드**

### 동적 타입 언어에서 안전성을 확보하는 방법

>- **주고 받을 때 값 대신 객체를 활용**
>- **어디에선가 데이터를 받아올 때마다 바로 검증을 하고, 검증에 실패하면 바로 throw 하도록**

### 절차 지향 알고리듬에서 객체 지향 프로그래밍으로의 전환 전략

>**조건문에 의한 분기를 변화율에 따른 격리로 바꾼다.**

### 깨알 망언

>객체 지향은 알고리듬으로 되어 있지 않고 객체간의 통신으로 되어 있어서 **읽기가 쉽다.**
>
>.. 우리가 읽는 사람들이냐 짜는 사람들이지.. 짜기는 ㅈㄹ 어려움.. ㅠㅜ

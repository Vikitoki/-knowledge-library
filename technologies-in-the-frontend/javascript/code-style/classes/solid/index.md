# SOLID

## S - Single responsibility principle

👎**Плохо:**

```javascript
class Auto {
  constructor(model: string) {}
  getCarModel() {}
  saveCustomerOrder(o: Auto) {}
  setCarModel() {}
  getCustomerOrder(id: string) {}
  removeCustomerOrder(id: string) {}
  updateCarSet(set: object) {}
}
```

👍**Хорошо:**

```javascript
class Auto {
  constructor(model: string) {}
  getCarModel() {}
  setCarModel() {}
}

class CustomerAuto {
  saveCustomerOrder(o: Auto) {}
  getCustomerOrder(id: string) {}
  removeCustomerOrder(id: string) {}
}

class AutoDB {
  updateCarSet(set: object) {}
}
```

## O - Open/closed principle

👎**Плохо:**

```javascript
class Auto {
	constructor(public model: string) { }
	getCarModel() { }
}

const shop: Array<Auto> = [
	new Auto('Tesla'),
	new Auto('Audi'),
];

const getPrice = (auto: Array<Auto>): string | void => {
	for (let i = 0; i < auto.length; i++) {
		switch (auto[i].model) {
			case 'Tesla': return '80 000$';
			case 'Audi': return '50 000$';
			default: return 'No Auto Price';
		}
	}
}

getPrice(shop);
```

👍**Хорошо:**

```javascript
abstract class CarPrice {
	abstract getPrice(): string;
}

class Tesla extends CarPrice {
	getPrice() {
		return '80 000$';
	}
}

class Audi extends CarPrice {
	getPrice() {
		return '50 000$';
	}
}

class Bmw extends CarPrice {
	getPrice() {
		return '70 000$';
	}
}

const shop: Array<CarPrice> = [
	new Tesla(),
	new Audi(),
	new Bmw(),
];

const getPrice = (auto: Array<CarPrice>): string | void => {
	for (let i = 0; i < auto.length; i++) {
		auto[i].getPrice();
	}
}

getPrice(shop);
```

## L - Liskov substitution principle

👎**Плохо:**

```javascript
class Rectangle {
	constructor(public width: number, public height: number) {}

	setWidth(width: number) {
		this.width = width;
	}

	setHeight(height: number) {
		this.height = height;
	}

	areaOf(): number {
		return this.width * this.height;
	}
}

class Square extends Rectangle {
	width: number = 0;
	height: number = 0;

	constructor(size: number) {
		super(size, size);
	}

	setWidth(width: number) {
		this.width = width;
		this.height = width;
	}

	setHeight(height: number) {
		this.width = height;
		this.height = height;
	}
}
```

👍**Хорошо:**

```javascript
interface Figure {
  setWidth(width: number): void;
  setHeight(height: number): void;
  areaOf(): void;
}

class Rectangle implements Figure {
  setWidth(width: number) {}
  setHeight(height: number) {}
  areaOf() {}
}

class Square implements Figure {
  setWidth(width: number) {}
  setHeight(height: number) {}
  areaOf() {}
}
```

## I - Interface segregation principle

👎**Плохо:**

```javascript
interface AutoSet {
  getTeslaSet(): any;
  getAudiSet(): any;
  getBmwSet(): any;
}

class Tesla implements AutoSet {
  getTeslaSet(): any {}
  getAudiSet(): any {}
  getBmwSet(): any {}
}

class Audi implements AutoSet {
  getTeslaSet(): any {}
  getAudiSet(): any {}
  getBmwSet(): any {}
}

class Bmw implements AutoSet {
  getTeslaSet(): any {}
  getAudiSet(): any {}
  getBmwSet(): any {}
}
```

👍**Хорошо:**

```javascript
interface TeslaSet {
  getTeslaSet(): any;
}

interface AudiSet {
  getAudiSet(): any;
}

interface BmwSet {
  getBmwSet(): any;
}

class Tesla implements TeslaSet {
  getTeslaSet(): any {}
}

class Audi implements AudiSet {
  getAudiSet(): any {}
}

class Bmw implements BmwSet {
  getBmwSet(): any {}
}
```

## D - Dependency inversion principle

👎**Плохо:**

```javascript
class xmlHttpRequestService { }

// Low level
class xmlHttpService extends xmlHttpRequestService {
	request(url: string, type: string) { }
}

// High level module
class Http {
	constructor(private xmlHttpService: xmlHttpService) { }

	get(url: string, options: any) {
		this.xmlHttpService.request(url, 'GET');
	}

	post(url: string) {
		this.xmlHttpService.request(url, 'POST');
	}
}
```

👍**Хорошо:**

```javascript
class xmlHttpRequestService {
	open() { }
	send() { }
}

interface Connection {
	request(url: string, options: any): any;
}

// Low level
class xmlHttpService implements Connection {
	xhr = new xmlHttpRequestService();

	request(url: string, type: string) {
		this.xhr.open();
		this.xhr.send();
	}
}

// High level module
class Http {
	constructor(private httpConnection: Connection) { }

	get(url: string, options: any) {
		this.httpConnection.request(url, 'GET');
	}

	post(url: string) {
		this.httpConnection.request(url, 'POST');
	}
}
```

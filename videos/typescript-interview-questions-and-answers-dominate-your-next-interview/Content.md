https://www.youtube.com/watch?v=4pqeoHLf9IU
___
## How to define basic types inside Typescript?

Ao criar a variavel, devemos atribuir o tipo para forçar que o valor seja daquele mesmo tipo!

```ts
const foo: string = "Foo"
const age: number = 28
const isActive: boolean = true
const user: { id: number, name: string } = { id: 1, name: 'André' }

const users: { id: number, name: string }[] = [user]
const users2: Array<{ id: number, name: string }> = [user]
```

___
## What is the difference between explicit vs implicit types?

Recomenda-se utilizar sempre o método **explicito**, para evitar deixar este trabalho para o typescript realizar internamente.

### Explicit

```ts
const foo: string = "Foo"
```

### Implicit

O typescript definira implicitamente o tipo da variavel porque foi atribuido a ela um valor do tipo string.

```ts
const foo = "Foo" 
```

___
## Write a function "getFullName" which gets name and surname and returns a full name.

```ts
const getFullName = (name: string, surname: string): string => name + ' ' + surname
```

___
## What is an interface in Typescript?

Ele é uma das coisas mais útil no typescript, porque nos permite tipar **objetos**.

```ts
interface User {
	id: number;
	name: string;
}

const user: User = { id: 1, name: 'André' }

const getName = (user: User): string => user.name

getName(user)
```

___
## What is a type in Typescript?

```ts
type ID = string

const id: ID = "1"
```

**Porque não utilizar a tipagem string diretamente?**

Porque podemos reutilizar o tipo **ID** em todo o código, caso um dia queira mudar o type **ID** para number ou qualquer outro, vamos ter que mudar somente em um local.

```ts
type ID = number

const id: ID = 1
```

Também melhora a legibilidade do código:

```ts
type Numbers = number[]

const numbers: Numbers = [1,2,3]
```

```ts
interface User {
	id: number;
	name: string;
}

type Users = User[]

const users: Users = []
```

___
## What is the difference between type and interface?

São as mesmas coisas, somente é implementado de formas distintas. 

Podemos utilizar as duas de forma intercambiavel, mas essencialmente utilizamos as interfaces simplesmente para trabalhar com objetos, e tipos para trabalhar com tudo, incluindo objetos.
### Interface

A **interface** é apenas um **contrato** sobre como nosso objeto deve ser estruturado.

```ts
interface User {
	id: number;
	name: string;
}

interface Admin extends User {
	permissions: string[];
}
```

```ts
interface Person {
	name: string;
	greet(): string;
}

class Student implements Person {
	name = "André"
	
	greet(): string {
		return 'Hello'
	}
}
```

### Type

```ts
type User = {
	id: number;
	name: string;
}

type Admin = User & { permissions: string[] };
```

```ts
type Person = {
	name: string;
	greet(): string;
}

class Student implements Person {
	name = "André"
	
	greet(): string {
		return 'Hello'
	}
}
```

Então com a **interface**, pode-se notar que se utiliza o **extends** e no **type** utilizamos a **intercessão (&)**.

___
## What is union in Typescript?

```ts
type stringOrNumber = number | string;

const foo: stringOrNumber = "bar";
const foo: stringOrNumber = 1;
```

```ts
const getId = (id: string | number) => {
	return id.toFixed() // isso vai gerar um erro, se eu enviar um numero. Temos                            que usar algo chamado "type narrowing"
}

const getId = (id: string | number) => {
	if(typeof id === 'string'){
		return id
	} else {
		return id.toFixed()
	}
}
```

```ts
type LoadingState = {
	state: "loading";
}

type FailedState = {
	state: "failed";
	code: number
}

type SuccessState = {
	state: "success";
	response: {
		title: string;
	}
}

type NetworkState = LoadingState | FailedState | SuccessState;
```

___
## What do you know about type narrowing?

```ts
type LoadingState = {
	state: "loading";
}

type FailedState = {
	state: "failed";
	code: number
}

type SuccessState = {
	state: "success";
	response: {
		title: string;
	}
}

type NetworkState = LoadingState | FailedState | SuccessState;

const foo = (value: Date | string) => {
	if(value instanceof Date){
		return value.toUTCString();
	}
	
	return value
}

const foo = (networkState: NetworkState) => {
	if (networkState.state === 'success') {
		return networkState.response
	} else if (networkState.state === 'failed') {
		return networkState.code
	}
	return networkState
}

// ou (mas não tão recomendado)

const foo = (networkState: NetworkState) => {
	if("code" in networkState){
		return networkState.code;
	}
	
	if("response" in networkState){
		return networkState.response;
	}
	
	return networkState
}
```
___

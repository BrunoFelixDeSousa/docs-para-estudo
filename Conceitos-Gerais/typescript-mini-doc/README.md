# [⬅ Voltar para o índice principal](../../README.md)

# Guia Completo de TypeScript Moderno

## Sumário

1. [Introdução ao TypeScript](#introdução-ao-typescript)
2. [Configurando o Ambiente](#configurando-o-ambiente)
3. [Tipos Básicos](#tipos-básicos)
4. [Interfaces e Types](#interfaces-e-types)
5. [Classes](#classes)
6. [Funções em TypeScript](#funções-em-typescript)
7. [Generics](#generics)
8. [Módulos e Namespaces](#módulos-e-namespaces)
9. [Decorators](#decorators)
10. [Utility Types](#utility-types)
11. [Type Assertions e Type Guards](#type-assertions-e-type-guards)
12. [Integração com JavaScript](#integração-com-javascript)
13. [Configurações Avançadas do tsconfig.json](#configurações-avançadas-do-tsconfigjson)
14. [TypeScript com Frameworks Modernos](#typescript-com-frameworks-modernos)
15. [Práticas Recomendadas](#práticas-recomendadas)
16. [Recursos Adicionais](#recursos-adicionais)

## Introdução ao TypeScript

TypeScript é um superset de JavaScript desenvolvido pela Microsoft que adiciona tipagem estática opcional à linguagem. Ele compila para JavaScript limpo, permitindo que seja executado em qualquer ambiente que suporte JavaScript.

### Vantagens do TypeScript

- **Tipagem estática**: Detecta erros em tempo de compilação
- **Melhor suporte para IDEs**: Autocompletar, refatoração e navegação pelo código
- **Melhor documentação**: Os tipos servem como uma forma de documentação
- **Recursos de ES Next**: Acesso a funcionalidades futuras do JavaScript hoje
- **Interoperabilidade com JavaScript**: Trabalha com bibliotecas JS existentes

### Desvantagens do TypeScript

- **Curva de aprendizado**: Adiciona complexidade ao desenvolvimento
- **Tempo de compilação**: Necessita de um passo adicional de compilação
- **Configuração inicial**: Requer setup e configuração

## Configurando o Ambiente

### Instalação do TypeScript

```bash
# Instalação global
npm install -g typescript

# Instalação local em um projeto
npm install --save-dev typescript
```

### Criando um Projeto TypeScript

```bash
# Inicializa um projeto npm
npm init -y

# Instala o TypeScript localmente
npm install --save-dev typescript

# Cria o arquivo tsconfig.json
npx tsc --init
```

### Estrutura básica do tsconfig.json

```json
{
  "compilerOptions": {
    "target": "es2020",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "outDir": "./dist",
    "rootDir": "./src"
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "**/*.test.ts"]
}
```

### Compilando seu Código TypeScript

```bash
# Compila o projeto conforme configurado no tsconfig.json
npx tsc

# Compila um arquivo específico
npx tsc src/index.ts

# Compila e observa por mudanças
npx tsc --watch
```

## Tipos Básicos

### Tipos Primitivos

```typescript
// Tipos primitivos básicos
let isDone: boolean = false;
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
let color: string = "blue";
let list: number[] = [1, 2, 3];
let x: null = null;
let u: undefined = undefined;

// Exemplos com saídas de console.log:
console.log(typeof isDone); // Saída: boolean
console.log(typeof decimal); // Saída: number
console.log(typeof color); // Saída: string
console.log(Array.isArray(list)); // Saída: true
```

### Arrays

```typescript
// Duas maneiras de definir arrays
let list1: number[] = [1, 2, 3];
let list2: Array<number> = [1, 2, 3];

// Array de tipos mistos
let list3: (number | string)[] = [1, 2, "três"];

// Arrays multidimensionais
let matrix: number[][] = [
  [1, 2, 3],
  [4, 5, 6],
];

console.log(list1); // Saída: [1, 2, 3]
console.log(matrix[0][1]); // Saída: 2
console.log(list3); // Saída: [1, 2, "três"]
```

### Tuples

```typescript
// Tupla (array com número fixo de elementos de tipos diferentes)
let tuple: [string, number] = ["hello", 10];

// Acessando elementos da tupla
console.log(tuple[0]); // Saída: hello
console.log(tuple[1]); // Saída: 10

// O que não se deve fazer (erro em tempo de compilação):
// tuple = [10, "hello"]; // Erro: Type 'number' is not assignable to type 'string'

// Tupla com elementos opcionais
let optionalTuple: [string, number?] = ["hello"];
console.log(optionalTuple); // Saída: ["hello"]
```

### Enum

```typescript
// Definindo um enum
enum Color {
  Red,
  Green,
  Blue,
}

// Usando o enum
let c: Color = Color.Green;
console.log(c); // Saída: 1 (os valores começam em 0 por padrão)

// Enum com valores personalizados
enum Status {
  Active = 1,
  Inactive = 2,
  Pending = 3,
}

let status: Status = Status.Active;
console.log(status); // Saída: 1
console.log(Status[1]); // Saída: Active (consulta reversa)

// Enum como flags (usando operações binárias)
enum Permission {
  Read = 1 << 0, // 1
  Write = 1 << 1, // 2
  Execute = 1 << 2, // 4
}

let userPermission: Permission = Permission.Read | Permission.Write;
console.log(userPermission); // Saída: 3
console.log(!!(userPermission & Permission.Read)); // Saída: true
console.log(!!(userPermission & Permission.Execute)); // Saída: false
```

### Any, Unknown e Never

```typescript
// Type any - desativa a verificação de tipo
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // agora um boolean
console.log(notSure); // Saída: false

// Type unknown - mais seguro que any
let value: unknown = 10;
// Para usar value, precisamos verificar seu tipo
if (typeof value === "number") {
  let sum = value + 10;
  console.log(sum); // Saída: 20
}

// Type never - representa valores que nunca ocorrem
function error(message: string): never {
  throw new Error(message);
}

// Esta função nunca retorna, pois sempre lança um erro
try {
  error("Erro!");
} catch (e) {
  console.log(e.message); // Saída: Erro!
}
```

### Object

```typescript
// Objeto tipado
let user: { name: string; age: number } = {
  name: "João",
  age: 30,
};

console.log(user); // Saída: { name: "João", age: 30 }

// Usando o tipo Record para objetos dinâmicos
const countries: Record<string, string> = {
  BR: "Brasil",
  US: "Estados Unidos",
  FR: "França",
};

console.log(countries.BR); // Saída: Brasil
```

### Union e Intersection Types

```typescript
// Union Types (ou)
let id: number | string;

id = 101;
console.log(typeof id); // Saída: number

id = "202";
console.log(typeof id); // Saída: string

// Intersection Types (e)
type Employee = {
  id: number;
  name: string;
};

type Manager = {
  department: string;
  level: number;
};

type ManagerEmployee = Employee & Manager;

const manager: ManagerEmployee = {
  id: 123,
  name: "Maria",
  department: "TI",
  level: 2,
};

console.log(manager);
// Saída: { id: 123, name: "Maria", department: "TI", level: 2 }
```

### Type Aliases

```typescript
// Type Alias para simplificar tipos complexos
type Point = {
  x: number;
  y: number;
};

type Shape = {
  area: number;
  perimeter: number;
};

type Circle = Point &
  Shape & {
    radius: number;
  };

const circle: Circle = {
  x: 0,
  y: 0,
  area: 78.5,
  perimeter: 31.4,
  radius: 5,
};

console.log(circle);
// Saída: { x: 0, y: 0, area: 78.5, perimeter: 31.4, radius: 5 }
```

### Literal Types

```typescript
// Literal types - valores específicos
type Direction = "North" | "East" | "South" | "West";

let direction: Direction = "North";
console.log(direction); // Saída: North

// Erro em tempo de compilação:
// direction = "Northeast"; // Error: Type '"Northeast"' is not assignable to type 'Direction'.

// Literais numéricos
type DiceValue = 1 | 2 | 3 | 4 | 5 | 6;

let roll: DiceValue = 6;
console.log(roll); // Saída: 6
```

## Interfaces e Types

### Definindo Interfaces

```typescript
// Interface básica
interface User {
  id: number;
  name: string;
  email: string;
  active?: boolean; // Propriedade opcional
}

const newUser: User = {
  id: 1,
  name: "Maria",
  email: "maria@example.com",
};

console.log(newUser);
// Saída: { id: 1, name: "Maria", email: "maria@example.com" }

// Interface com propriedades somente leitura
interface Point {
  readonly x: number;
  readonly y: number;
}

const point: Point = { x: 10, y: 20 };
console.log(point); // Saída: { x: 10, y: 20 }
// point.x = 5; // Erro: Cannot assign to 'x' because it is a read-only property
```

### Extensão de Interfaces

```typescript
// Estendendo interfaces
interface Person {
  name: string;
  age: number;
}

interface Employee extends Person {
  employeeId: number;
  department: string;
}

const employee: Employee = {
  name: "Carlos",
  age: 35,
  employeeId: 12345,
  department: "Engenharia",
};

console.log(employee);
// Saída: { name: "Carlos", age: 35, employeeId: 12345, department: "Engenharia" }

// Implementando múltiplas interfaces
interface Named {
  name: string;
}

interface Aged {
  age: number;
}

interface Worker {
  job: string;
}

// Classe implementando múltiplas interfaces
class Adult implements Named, Aged, Worker {
  name: string;
  age: number;
  job: string;

  constructor(name: string, age: number, job: string) {
    this.name = name;
    this.age = age;
    this.job = job;
  }
}

const adult = new Adult("Ana", 28, "Desenvolvedora");
console.log(adult);
// Saída: Adult { name: "Ana", age: 28, job: "Desenvolvedora" }
```

### Interfaces vs Type Aliases

```typescript
// Interface - pode ser estendida ou implementada
interface Animal {
  name: string;
}

interface Dog extends Animal {
  breed: string;
}

// Type alias - pode ser união, interseção e outros tipos complexos
type Animal2 = {
  name: string;
};

type Dog2 = Animal2 & {
  breed: string;
};

// Ambos funcionam de maneira similar para objetos simples
const dog1: Dog = {
  name: "Rex",
  breed: "Labrador",
};

const dog2: Dog2 = {
  name: "Max",
  breed: "Golden",
};

console.log(dog1); // Saída: { name: "Rex", breed: "Labrador" }
console.log(dog2); // Saída: { name: "Max", breed: "Golden" }
```

### Index Signatures

```typescript
// Interface com index signature
interface Dictionary {
  [key: string]: string | number;
}

const dict: Dictionary = {
  name: "Dicionário",
  size: 5,
  author: "Desconhecido",
};

console.log(dict.name); // Saída: Dicionário
console.log(dict["author"]); // Saída: Desconhecido

// Index signature com tipos específicos
interface NumericDictionary {
  [index: number]: string;
  length: number; // ok, length é um número
  // name: string;  // Erro: Propriedade 'name' do tipo 'string' não é atribuível ao tipo numérico 'number'
}

const arr: NumericDictionary = ["zero", "um", "dois"];
arr.length = 3;

console.log(arr[1]); // Saída: um
console.log(arr.length); // Saída: 3
```

### Interfaces para Funções

```typescript
// Interface para funções
interface SearchFunc {
  (source: string, subString: string): boolean;
}

const mySearch: SearchFunc = function (src, sub) {
  return src.indexOf(sub) > -1;
};

console.log(mySearch("Hello World", "World")); // Saída: true
console.log(mySearch("Hello World", "TypeScript")); // Saída: false
```

### Interfaces Híbridas

```typescript
// Interface híbrida (objeto + função)
interface Counter {
  (start: number): string;
  interval: number;
  reset(): void;
}

function getCounter(): Counter {
  const counter = function (start: number) {
    return `O contador começou em ${start}`;
  } as Counter;

  counter.interval = 123;
  counter.reset = function () {
    console.log("Contador resetado");
  };

  return counter;
}

const c = getCounter();
console.log(c(10)); // Saída: O contador começou em 10
console.log(c.interval); // Saída: 123
c.reset(); // Saída: Contador resetado
```

## Classes

### Classes Básicas

```typescript
// Classe básica
class Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  greet() {
    return `Olá, meu nome é ${this.name} e tenho ${this.age} anos`;
  }
}

const person = new Person("Ana", 30);
console.log(person.greet());
// Saída: Olá, meu nome é Ana e tenho 30 anos
```

### Modificadores de Acesso

```typescript
// Modificadores de acesso
class BankAccount {
  public accountHolder: string;
  private balance: number;
  protected accountNumber: string;

  constructor(accountHolder: string, initialBalance: number) {
    this.accountHolder = accountHolder;
    this.balance = initialBalance;
    this.accountNumber = Math.floor(Math.random() * 1000000).toString();
  }

  public deposit(amount: number): void {
    this.balance += amount;
    console.log(`Depositado: ${amount}. Novo saldo: ${this.balance}`);
  }

  public withdraw(amount: number): boolean {
    if (amount <= this.balance) {
      this.balance -= amount;
      console.log(`Sacado: ${amount}. Novo saldo: ${this.balance}`);
      return true;
    }
    console.log("Saldo insuficiente");
    return false;
  }

  public getBalance(): number {
    return this.balance;
  }
}

const account = new BankAccount("Pedro", 1000);
account.deposit(500);
// Saída: Depositado: 500. Novo saldo: 1500

account.withdraw(200);
// Saída: Sacado: 200. Novo saldo: 1300

console.log(`Saldo atual: ${account.getBalance()}`);
// Saída: Saldo atual: 1300

// Tentativas de acesso direto
console.log(account.accountHolder); // Saída: Pedro
// console.log(account.balance); // Erro: Property 'balance' is private and only accessible within class 'BankAccount'
// console.log(account.accountNumber); // Erro: Property 'accountNumber' is protected and only accessible within class 'BankAccount' and its subclasses
```

### Classes com Propriedades Readonly

```typescript
// Classe com propriedade readonly
class Circle {
  readonly radius: number;
  readonly diameter: number;

  constructor(radius: number) {
    this.radius = radius;
    this.diameter = radius * 2;
  }

  getArea(): number {
    return Math.PI * this.radius * this.radius;
  }
}

const circle = new Circle(5);
console.log(`Área do círculo: ${circle.getArea().toFixed(2)}`);
// Saída: Área do círculo: 78.54

// circle.radius = 10; // Erro: Cannot assign to 'radius' because it is a read-only property
```

### Heranças

```typescript
// Herança de classe
class Animal {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  move(distance: number = 0) {
    console.log(`${this.name} moveu-se ${distance}m.`);
  }
}

class Dog extends Animal {
  constructor(name: string) {
    super(name); // Chama o construtor da classe pai
  }

  bark() {
    console.log("Au au!");
  }

  // Sobrescrevendo método da classe pai
  move(distance: number = 5) {
    console.log("Correndo...");
    super.move(distance); // Chama o método move() da classe pai
  }
}

const dog = new Dog("Rex");
dog.bark(); // Saída: Au au!
dog.move(); // Saída: Correndo... (depois) Rex moveu-se 5m.
```

### Classes Abstratas

```typescript
// Classes abstratas
abstract class Shape {
  color: string;

  constructor(color: string) {
    this.color = color;
  }

  abstract getArea(): number; // Método abstrato que deve ser implementado pelas subclasses

  displayColor() {
    console.log(`A cor é ${this.color}`);
  }
}

class Square extends Shape {
  sideLength: number;

  constructor(color: string, sideLength: number) {
    super(color);
    this.sideLength = sideLength;
  }

  getArea(): number {
    return this.sideLength * this.sideLength;
  }
}

const square = new Square("vermelho", 10);
console.log(`Área do quadrado: ${square.getArea()}`);
// Saída: Área do quadrado: 100

square.displayColor();
// Saída: A cor é vermelho

// Não pode criar uma instância da classe abstrata
// const shape = new Shape("azul"); // Erro: Cannot create an instance of an abstract class
```

### Propriedades e Métodos Estáticos

```typescript
// Classes com membros estáticos
class MathUtils {
  static readonly PI: number = 3.14159;

  static add(x: number, y: number): number {
    return x + y;
  }

  static subtract(x: number, y: number): number {
    return x - y;
  }
}

console.log(`PI: ${MathUtils.PI}`);
// Saída: PI: 3.14159

console.log(`5 + 3 = ${MathUtils.add(5, 3)}`);
// Saída: 5 + 3 = 8

console.log(`5 - 3 = ${MathUtils.subtract(5, 3)}`);
// Saída: 5 - 3 = 2
```

### Acessadores (Getters e Setters)

```typescript
// Classe com getters e setters
class Employee {
  private _fullName: string = "";

  get fullName(): string {
    return this._fullName;
  }

  set fullName(newName: string) {
    if (newName.length > 3) {
      this._fullName = newName;
    } else {
      console.log("Nome deve ter mais de 3 caracteres");
    }
  }
}

const employee = new Employee();
employee.fullName = "Bob"; // Saída: Nome deve ter mais de 3 caracteres
console.log(employee.fullName); // Saída: ""

employee.fullName = "Roberto";
console.log(employee.fullName); // Saída: "Roberto"
```

### Padrão Singleton com Classes

```typescript
// Implementando o padrão Singleton
class Singleton {
  private static instance: Singleton;
  private constructor() {} // Construtor privado para evitar instanciação direta

  public static getInstance(): Singleton {
    if (!Singleton.instance) {
      Singleton.instance = new Singleton();
    }
    return Singleton.instance;
  }

  public someMethod() {
    console.log("Método chamado na instância Singleton");
  }
}

// Não podemos usar o construtor diretamente
// const singleton = new Singleton(); // Erro

// Usamos o método estático para obter a instância
const instance1 = Singleton.getInstance();
const instance2 = Singleton.getInstance();

instance1.someMethod();
// Saída: Método chamado na instância Singleton

console.log(instance1 === instance2);
// Saída: true - Confirma que são a mesma instância
```

## Funções em TypeScript

### Funções Básicas

```typescript
// Função tipada básica
function add(a: number, b: number): number {
  return a + b;
}

const result = add(5, 3);
console.log(result); // Saída: 8

// Função com parâmetros opcionais
function buildName(firstName: string, lastName?: string): string {
  if (lastName) {
    return `${firstName} ${lastName}`;
  }
  return firstName;
}

console.log(buildName("Bob")); // Saída: Bob
console.log(buildName("Bob", "Smith")); // Saída: Bob Smith
```

### Parâmetros Default

```typescript
// Funções com parâmetros padrão
function createGreeting(name: string, greeting: string = "Olá"): string {
  return `${greeting}, ${name}!`;
}

console.log(createGreeting("Maria")); // Saída: Olá, Maria!
console.log(createGreeting("João", "Bom dia")); // Saída: Bom dia, João!
```

### Parâmetros Rest

```typescript
// Função com parâmetros rest
function sum(...numbers: number[]): number {
  return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2)); // Saída: 3
console.log(sum(1, 2, 3, 4, 5)); // Saída: 15

// Parâmetros obrigatórios com rest
function buildFullName(firstName: string, ...restOfName: string[]): string {
  return `${firstName} ${restOfName.join(" ")}`;
}

console.log(buildFullName("Maria", "Silva")); // Saída: Maria Silva
console.log(buildFullName("João", "da", "Silva", "Santos")); // Saída: João da Silva Santos
```

### Function Overloading

```typescript
// Sobrecarga de funções
function parseCoordinate(x: number, y: number): { x: number; y: number };
function parseCoordinate(obj: { x: number; y: number }): { x: number; y: number };
function parseCoordinate(str: string): { x: number; y: number };
function parseCoordinate(arg1: unknown, arg2?: unknown): { x: number; y: number } {
  let coord = { x: 0, y: 0 };

  if (typeof arg1 === "number" && typeof arg2 === "number") {
    coord = { x: arg1, y: arg2 };
  } else if (typeof arg1 === "string") {
    try {
      const obj = JSON.parse(arg1);
      coord = { x: obj.x, y: obj.y };
    } catch (e) {
      // Mantém as coordenadas padrão
    }
  } else if (typeof arg1 === "object" && arg1 !== null) {
    const obj = arg1 as { x: number; y: number };
    coord = { x: obj.x, y: obj.y };
  }

  return coord;
}

console.log(parseCoordinate(10, 20)); // Saída: { x: 10, y: 20 }
console.log(parseCoordinate({ x: 10, y: 20 })); // Saída: { x: 10, y: 20 }
console.log(parseCoordinate('{"x": 10, "y": 20}')); // Saída: { x: 10, y: 20 }
```

### Arrow Functions

```typescript
// Arrow functions
const doubleNumber = (num: number): number => num * 2;
console.log(doubleNumber(5)); // Saída: 10

// Arrow function como callback
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((n: number): number => n * 2);
console.log(doubled); // Saída: [2, 4, 6, 8, 10]

// Arrow function com objeto de retorno implícito
const createUser = (name: string, age: number) => ({
  name,
  age,
  created: new Date(),
});

const user = createUser("Carlos", 30);
console.log(user);
// Saída: { name: "Carlos", age: 30, created: [Date object] }
```

### this em Funções

```typescript
// Tipando o 'this' em funções
interface Button {
  text: string;
  onClick(this: Button, event: Event): void;
}

const button: Button = {
  text: "Clique Aqui",
  onClick(this: Button) {
    // 'this' é tipado como Button
    console.log(`Botão "${this.text}" foi clicado`);
  },
};

// Simulando um evento de clique
button.onClick.call(button, {} as Event);
// Saída: Botão "Clique Aqui" foi clicado
```

### Funções como Tipos

```typescript
// Usando funções como tipos
type MathOperation = (x: number, y: number) => number;

const add: MathOperation = (a, b) => a + b;
const subtract: MathOperation = (a, b) => a - b;
const multiply: MathOperation = (a, b) => a * b;
const divide: MathOperation = (a, b) => a / b;

function calculate(a: number, b: number, operation: MathOperation): number {
  return operation(a, b);
}

console.log(calculate(10, 5, add)); // Saída: 15
console.log(calculate(10, 5, subtract)); // Saída: 5
console.log(calculate(10, 5, multiply)); // Saída: 50
console.log(calculate(10, 5, divide)); // Saída: 2
```

### Function Type Literals

```typescript
// Usando function type literals
let greet: (name: string) => string;

greet = function (name: string) {
  return `Olá, ${name}!`;
};

console.log(greet("Maria")); // Saída: Olá, Maria!

// Function type com interface
interface SearchFunction {
  (source: string, target: string): boolean;
}

const searchStr: SearchFunction = function (src, tgt) {
  return src.includes(tgt);
};

console.log(searchStr("Hello world", "world")); // Saída: true
```

### Funções Assíncronas

```typescript
// Funções assíncronas
async function fetchUserData(userId: string): Promise<object> {
  // Simulando uma chamada de API
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve({
        id: userId,
        name: "Usuário " + userId,
        email: `user${userId}@example.com`,
      });
    }, 1000);
  });
}

// Usando a função assíncrona com async/await
async function displayUserInfo(userId: string): Promise<void> {
  console.log("Buscando dados...");
  const user = await fetchUserData(userId);
  console.log("Dados do usuário:", user);
}

// Não podemos usar await fora de funções assíncronas no escopo global
// Então envolvemos em uma função auto-executável
(async () => {
  await displayUserInfo("123");
  // Saída após 1s:
  // Buscando dados...
  // Dados do usuário: { id: '123', name: 'Usuário 123', email: 'user123@example.com' }
})();
```

## Generics

### Generics Básicos

```typescript
// Função genérica básica
function identity<T>(arg: T): T {
  return arg;
}

const num = identity<number>(42);
console.log(num); // Saída: 42

const str = identity<string>("hello");
console.log(str); // Saída: hello

// Inferência de tipos genéricos
const bool = identity(true); // TypeScript infere T como boolean
console.log(bool); // Saída: true
```

### Interfaces Genéricas

```typescript
// Interface genérica
interface Box<T> {
  value: T;
}

// Usando a interface genérica
const box1: Box<string> = { value: "texto" };
console.log(box1.value); // Saída: texto

const box2: Box<number> = { value: 42 };
console.log(box2.value); // Saída: 42

// Interface genérica com múltiplos tipos
interface Pair<T, U> {
  first: T;
  second: U;
}

const pair: Pair<string, number> = { first: "idade", second: 30 };
console.log(pair); // Saída: { first: "idade", second: 30 }
```

### Classes Genéricas

```typescript
// Classe genérica
class Queue<T> {
  private data: T[] = [];

  push(item: T): void {
    this.data.push(item);
  }

  pop(): T | undefined {
    return this.data.shift();
  }

  peek(): T | undefined {
    return this.data[0];
  }

  size(): number {
    return this.data.length;
  }
}

// Queue de números
const numQueue = new Queue<number>();
numQueue.push(1);
numQueue.push(2);
numQueue.push(3);

console.log(numQueue.pop()); // Saída: 1
console.log(numQueue.peek()); // Saída: 2
console.log(numQueue.size()); // Saída: 2

// Queue de strings
const strQueue = new Queue<string>();
strQueue.push("a");
strQueue.push("b");
strQueue.push("c");

console.log(strQueue.pop()); // Saída: a
console.log(strQueue.size()); // Saída: 2
```

### Restrições de Tipos Genéricos

```typescript
// Restrições usando extends
interface HasLength {
  length: number;
}

function printLength<T extends HasLength>(arg: T): number {
  console.log(`Comprimento: ${arg.length}`);
  return arg.length;
}

printLength("Hello"); // Saída: Comprimento: 5
printLength([1, 2, 3]); // Saída: Comprimento: 3
printLength({ length: 10, value: 3 }); // Saída: Comprimento: 10

// Erro em tempo de compilação
// printLength(10); // Erro: Argument of type 'number' doesn't have 'length' property

// Restrições com múltiplos tipos
function merge<T extends object, U extends object>(obj1: T, obj2: U): T & U {
  return { ...obj1, ...obj2 };
}

const merged = merge({ name: "João" }, { age: 30 });
console.log(merged); // Saída: { name: "João", age: 30 }
```

### Parâmetros de Tipo Padrão

```typescript
// Parâmetros de tipo padrão
interface Response<T = any> {
  data: T;
  status: number;
  statusText: string;
}

function fetchAPI<T = any>(url: string): Promise<Response<T>> {
  // Simulação de uma API
  return Promise.resolve({
    data: {} as T,
    status: 200,
    statusText: "OK",
  });
}

// Interface específica para o tipo de dados esperado
interface User {
  id: number;
  name: string;
}

// Usando a função com o tipo específico
const getUser = async (): Promise<void> => {
  const response = await fetchAPI<User>("/api/user");
  const user = response.data;
  console.log(`Usuário: ${user.name}`);
};

// getUser(); // Obs: Descomentando, a saída seria: Usuário: undefined (como é uma simulação)
```

### Type Operators com Generics

```typescript
// keyof com Generics
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user = {
  id: 123,
  name: "Marcos",
  email: "marcos@exemplo.com",
};

console.log(getProperty(user, "name")); // Saída: Marcos
// console.log(getProperty(user, "age")); // Erro: Argument of type '"age"' is not assignable to parameter of type 'keyof { id: number; name: string; email: string; }'

// Mapeamento de tipos usando Generics
type ReadOnly<T> = {
  readonly [P in keyof T]: T[P];
};

const readOnlyUser: ReadOnly<typeof user> = {
  id: 456,
  name: "Ana",
  email: "ana@exemplo.com",
};

console.log(readOnlyUser);
// Saída: { id: 456, name: "Ana", email: "ana@exemplo.com" }

// readOnlyUser.name = "Outro nome"; // Erro: Cannot assign to 'name' because it is a read-only property
```

### Uso Avançado de Generics

```typescript
// Inferência de tipo condicional
type Flatten<T> = T extends Array<infer U> ? U : T;

type Arr = number[];
type NumType = Flatten<Arr>; // Resulta em: number

type Str = string;
type StrType = Flatten<Str>; // Resulta em: string

// Factory Pattern com Generics
interface Product {
  id: string;
  name: string;
}

class ProductFactory {
  static create<T extends Product>(type: new () => T): T {
    return new type();
  }
}

class Phone implements Product {
  id: string = "phone-1";
  name: string = "Smartphone";
  call() {
    console.log("Fazendo ligação...");
  }
}

class Laptop implements Product {
  id: string = "laptop-1";
  name: string = "Notebook";
  openLid() {
    console.log("Abrindo a tampa...");
  }
}

const phone = ProductFactory.create(Phone);
phone.call(); // Saída: Fazendo ligação...

const laptop = ProductFactory.create(Laptop);
laptop.openLid(); // Saída: Abrindo a tampa...
```

## Módulos e Namespaces

### Exportando e Importando

```typescript
// math.ts (arquivo separado)
export function add(x: number, y: number): number {
  return x + y;
}

export function subtract(x: number, y: number): number {
  return x - y;
}

export const PI = 3.14159;

// main.ts (arquivo separado)
import { add, PI } from "./math";
import * as Math from "./math";

console.log(add(5, 3)); // Saída: 8
console.log(PI); // Saída: 3.14159
console.log(Math.subtract(10, 4)); // Saída: 6
```

### Export Default

```typescript
// user.ts (arquivo separado)
export default class User {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
}

// app.ts (arquivo separado)
import User from "./user";
import { add } from "./math"; // Importando named export de outro arquivo

const user = new User("João", 30);
console.log(`Nome: ${user.name}, Idade: ${user.age}`);
// Saída: Nome: João, Idade: 30
```

### Re-exports

```typescript
// index.ts (arquivo separado que funciona como um barril)
export { add, subtract, PI } from "./math";
export { default as User } from "./user";

// app.ts
import { add, User } from "./index";

console.log(add(10, 20)); // Saída: 30
const u = new User("Maria", 25);
console.log(u); // Saída: User { name: "Maria", age: 25 }
```

### Namespaces

```typescript
// Usando namespaces (mais antigo, CommonJS)
namespace Validation {
  export interface StringValidator {
    isValid(s: string): boolean;
  }

  export class EmailValidator implements StringValidator {
    isValid(s: string): boolean {
      // Validação simples para demonstração
      return s.includes("@");
    }
  }

  export class ZipCodeValidator implements StringValidator {
    isValid(s: string): boolean {
      return /^\d{5}(-\d{4})?$/.test(s);
    }
  }
}

// Usando os validadores do namespace
let emailValidator = new Validation.EmailValidator();
let zipCodeValidator = new Validation.ZipCodeValidator();

console.log(emailValidator.isValid("teste@email.com")); // Saída: true
console.log(emailValidator.isValid("teste.email.com")); // Saída: false

console.log(zipCodeValidator.isValid("12345")); // Saída: true
console.log(zipCodeValidator.isValid("123456")); // Saída: false
```

### Dynamic Imports

```typescript
// Usando import dinâmico
async function loadMathModule() {
  // Carrega o módulo dinamicamente apenas quando necessário
  const math = await import("./math");
  console.log(math.add(5, 10)); // Saída: 15
}

// Chamada da função que usa o import dinâmico
loadMathModule();
```

## Decorators

### Decorators de Classe

```typescript
// Decorator de classe
function sealed(constructor: Function) {
  Object.seal(constructor);
  Object.seal(constructor.prototype);
  console.log(`Classe ${constructor.name} foi selada`);
}

// Aplicando o decorator
@sealed
class Person {
  name: string;

  constructor(name: string) {
    this.name = name;
  }
}

const p = new Person("Maria");
console.log(p.name); // Saída: Maria
// Saída adicional do decorator: Classe Person foi selada
```

### Decorators de Método

```typescript
// Decorator de método
function log(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;

  descriptor.value = function (...args: any[]) {
    console.log(`Chamando método ${propertyKey} com argumentos: ${JSON.stringify(args)}`);
    const result = originalMethod.apply(this, args);
    console.log(`Método ${propertyKey} retornou: ${result}`);
    return result;
  };

  return descriptor;
}

class Calculator {
  @log
  add(a: number, b: number): number {
    return a + b;
  }
}

const calc = new Calculator();
calc.add(5, 3);
// Saída:
// Chamando método add com argumentos: [5,3]
// Método add retornou: 8
```

### Decorators de Propriedade

```typescript
// Decorator de propriedade
function format(formatString: string) {
  return function (target: any, propertyKey: string) {
    let value: any;
    const getter = function () {
      return value;
    };
    const setter = function (newVal: any) {
      if (formatString === "uppercase") {
        value = newVal.toUpperCase();
      } else if (formatString === "lowercase") {
        value = newVal.toLowerCase();
      } else {
        value = newVal;
      }
    };

    Object.defineProperty(target, propertyKey, {
      get: getter,
      set: setter,
      enumerable: true,
      configurable: true,
    });
  };
}

class User {
  @format("uppercase")
  name: string;

  @format("lowercase")
  email: string;

  constructor(name: string, email: string) {
    this.name = name;
    this.email = email;
  }
}

const user = new User("João", "JOAO@EXAMPLE.COM");
console.log(user.name); // Saída: JOÃO
console.log(user.email); // Saída: joao@example.com
```

### Decorators de Parâmetro

```typescript
// Decorator de parâmetro
function required(target: Object, propertyKey: string, parameterIndex: number) {
  // Armazena quais parâmetros são obrigatórios para cada método
  const requiredParams: { [key: string]: number[] } = Reflect.getMetadata("required", target) || {};

  // Obtém ou inicializa o array para este método
  requiredParams[propertyKey] = requiredParams[propertyKey] || [];

  // Adiciona o índice deste parâmetro ao array
  requiredParams[propertyKey].push(parameterIndex);

  // Atualiza os metadados
  Reflect.defineMetadata("required", requiredParams, target);
}

// Decorator de método que valida os parâmetros obrigatórios
function validate(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const method = descriptor.value;

  descriptor.value = function (...args: any[]) {
    const requiredParams: number[] = Reflect.getMetadata("required", target, propertyKey) || [];

    for (const index of requiredParams) {
      if (args[index] === undefined || args[index] === null) {
        throw new Error(`Parâmetro na posição ${index} é obrigatório`);
      }
    }

    return method.apply(this, args);
  };

  return descriptor;
}

// Para usar estes decorators, você precisaria habilitar a reflexão em seu projeto:
// npm install reflect-metadata
// E então importar:
// import "reflect-metadata";

class UserService {
  @validate
  updateUser(id: number, @required name: string, age?: number) {
    console.log(`Atualizando usuário ${id}: nome=${name}, idade=${age}`);
  }
}

const service = new UserService();
service.updateUser(1, "Maria", 30);
// Saída: Atualizando usuário 1: nome=Maria, idade=30

// Isso lançaria um erro:
// service.updateUser(1, null); // Erro: Parâmetro na posição 1 é obrigatório
```

### Factory Decorators

```typescript
// Factory decorator
function log(prefix: string) {
  return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;

    descriptor.value = function (...args: any[]) {
      console.log(`${prefix} Chamando ${propertyKey}`);
      const result = originalMethod.apply(this, args);
      console.log(`${prefix} Resultado: ${result}`);
      return result;
    };

    return descriptor;
  };
}

class MathService {
  @log("[MathService]")
  sum(a: number, b: number): number {
    return a + b;
  }
}

const math = new MathService();
math.sum(5, 5);
// Saída:
// [MathService] Chamando sum
// [MathService] Resultado: 10
```

## Utility Types

### Partial

```typescript
// Partial - Torna todas as propriedades opcionais
interface User {
  id: number;
  name: string;
  email: string;
  active: boolean;
}

function updateUser(user: User, updates: Partial<User>): User {
  return { ...user, ...updates };
}

const user: User = {
  id: 1,
  name: "João",
  email: "joao@example.com",
  active: true,
};

const updatedUser = updateUser(user, { email: "joao.novo@example.com" });
console.log(updatedUser);
// Saída: { id: 1, name: "João", email: "joao.novo@example.com", active: true }
```

### Required

```typescript
// Required - Torna todas as propriedades obrigatórias
interface PartialUser {
  id?: number;
  name?: string;
  email?: string;
}

// Força todas as propriedades a serem obrigatórias
function createUser(userData: Required<PartialUser>): PartialUser {
  return userData;
}

const user = createUser({
  id: 1,
  name: "Ana",
  email: "ana@example.com",
});

console.log(user);
// Saída: { id: 1, name: "Ana", email: "ana@example.com" }

// Isso causaria um erro:
// const invalidUser = createUser({ id: 2 }); // Erro: Property 'name' is missing
```

### Readonly

```typescript
// Readonly - Torna todas as propriedades somente leitura
interface Config {
  host: string;
  port: number;
  secure: boolean;
}

const config: Readonly<Config> = {
  host: "localhost",
  port: 3000,
  secure: true,
};

console.log(config);
// Saída: { host: "localhost", port: 3000, secure: true }

// Erro de compilação:
// config.port = 4000; // Erro: Cannot assign to 'port' because it is a read-only property
```

### Record

```typescript
// Record - Cria um tipo com chaves e valores especificados
type UserRoles = "admin" | "manager" | "user";

const permissions: Record<UserRoles, string[]> = {
  admin: ["create", "read", "update", "delete"],
  manager: ["create", "read", "update"],
  user: ["read"],
};

console.log(permissions.admin);
// Saída: ["create", "read", "update", "delete"]

// Record com outros tipos
const userAges: Record<string, number> = {
  John: 30,
  Mary: 25,
  Bob: 40,
};

console.log(userAges);
// Saída: { John: 30, Mary: 25, Bob: 40 }
```

### Pick

```typescript
// Pick - Seleciona um subconjunto de propriedades
interface UserDetails {
  id: number;
  name: string;
  email: string;
  address: string;
  phone: string;
}

// Criando um tipo apenas com as propriedades desejadas
type UserIdentity = Pick<UserDetails, "id" | "name" | "email">;

const userIdentity: UserIdentity = {
  id: 1,
  name: "Pedro",
  email: "pedro@example.com",
};

console.log(userIdentity);
// Saída: { id: 1, name: "Pedro", email: "pedro@example.com" }
```

### Omit

```typescript
// Omit - Exclui propriedades específicas
interface Product {
  id: number;
  name: string;
  price: number;
  category: string;
  description: string;
  inStock: boolean;
}

// Criando um tipo excluindo algumas propriedades
type ProductSummary = Omit<Product, "description" | "inStock">;

const product: ProductSummary = {
  id: 1,
  name: "Laptop",
  price: 1299.99,
  category: "Electronics",
};

console.log(product);
// Saída: { id: 1, name: "Laptop", price: 1299.99, category: "Electronics" }
```

### Exclude e Extract

```typescript
// Exclude - Remove tipos da união
type NumberOrString = number | string | boolean;
type OnlyNumbers = Exclude<NumberOrString, string | boolean>;

const num: OnlyNumbers = 42;
console.log(num); // Saída: 42

// Isso causaria um erro:
// const str: OnlyNumbers = "hello"; // Erro: Type 'string' is not assignable to type 'number'

// Extract - Extrai tipos da união
type Primitive = number | string | boolean | undefined | null;
type StringOrBoolean = Extract<Primitive, string | boolean>;

const str: StringOrBoolean = "hello";
const bool: StringOrBoolean = true;

console.log(str); // Saída: hello
console.log(bool); // Saída: true

// Isso causaria um erro:
// const num2: StringOrBoolean = 42; // Erro: Type 'number' is not assignable to type 'string | boolean'
```

### NonNullable

```typescript
// NonNullable - Remove null e undefined dos tipos possíveis
type MaybeString = string | null | undefined;
type DefinitelyString = NonNullable<MaybeString>;

const definiteStr: DefinitelyString = "Hello";
console.log(definiteStr); // Saída: Hello

// Isso causaria um erro:
// const nullStr: DefinitelyString = null; // Erro: Type 'null' is not assignable to type 'string'
```

### ReturnType, Parameters e ConstructorParameters

```typescript
// ReturnType - Obtém o tipo de retorno de uma função
function createUser(name: string, age: number) {
  return {
    id: Date.now(),
    name,
    age,
    createdAt: new Date(),
  };
}

type User = ReturnType<typeof createUser>;

const user: User = {
  id: 1,
  name: "Carol",
  age: 25,
  createdAt: new Date(),
};

console.log(user);
// Saída: { id: 1, name: "Carol", age: 25, createdAt: [Date object] }

// Parameters - Obtém os tipos dos parâmetros como uma tupla
type CreateUserParams = Parameters<typeof createUser>;
const params: CreateUserParams = ["Bob", 30];

console.log(params);
// Saída: ["Bob", 30]

// ConstructorParameters - Obtém os tipos dos parâmetros do construtor
class Person {
  constructor(public name: string, public age: number) {}
}

type PersonConstructorParams = ConstructorParameters<typeof Person>;
const personParams: PersonConstructorParams = ["Alice", 28];

console.log(personParams);
// Saída: ["Alice", 28]
```

### ThisType

```typescript
// ThisType - Marca o tipo de `this` em um objeto
interface Logger {
  log(msg: string): void;
}

interface LoggerContext {
  prefix: string;
}

// O tipo ThisType informa ao compilador que métodos dentro de LoggerMethods
// usarão LoggerContext como seu tipo 'this'
type LoggerMethods = {
  debug(msg: string): void;
  error(msg: string): void;
} & ThisType<LoggerContext>;

const loggerMethods: LoggerMethods = {
  debug(msg: string) {
    console.log(`${this.prefix} [DEBUG]: ${msg}`);
  },
  error(msg: string) {
    console.log(`${this.prefix} [ERROR]: ${msg}`);
  },
};

// Combinando o contexto com os métodos
const logger = Object.assign({}, { prefix: "[App]" }, loggerMethods);

logger.debug("Aplicação iniciada");
// Saída: [App] [DEBUG]: Aplicação iniciada

logger.error("Ocorreu um erro");
// Saída: [App] [ERROR]: Ocorreu um erro
```

## Type Assertions e Type Guards

### Type Assertions

```typescript
// Type Assertions
let someValue: any = "string de exemplo";

// Usando sintaxe de ângulo (não funciona em JSX)
let strLength1: number = (<string>someValue).length;

// Usando sintaxe "as"
let strLength2: number = (someValue as string).length;

console.log(strLength1); // Saída: 17
console.log(strLength2); // Saída: 17

// Type Assertion mais complexa
interface User {
  id: number;
  name: string;
}

const userData: any = JSON.parse('{"id": 1, "name": "João"}');
const user = userData as User;

console.log(user.name); // Saída: João
```

### Type Guards

```typescript
// Type Guards com typeof
function processValue(value: string | number) {
  if (typeof value === "string") {
    // Neste bloco, o TypeScript sabe que value é uma string
    console.log(value.toUpperCase());
  } else {
    // Neste bloco, o TypeScript sabe que value é um número
    console.log(value.toFixed(2));
  }
}

processValue("hello"); // Saída: HELLO
processValue(42.5678); // Saída: 42.57
```

### instanceof Type Guards

```typescript
// Type Guards com instanceof
class Animal {
  move() {
    console.log("Animal se movendo");
  }
}

class Dog extends Animal {
  bark() {
    console.log("Au au!");
  }
}

class Cat extends Animal {
  meow() {
    console.log("Miau!");
  }
}

function makeSound(animal: Animal) {
  if (animal instanceof Dog) {
    // TypeScript sabe que animal é um Dog aqui
    animal.bark();
  } else if (animal instanceof Cat) {
    // TypeScript sabe que animal é um Cat aqui
    animal.meow();
  } else {
    animal.move();
  }
}

makeSound(new Dog()); // Saída: Au au!
makeSound(new Cat()); // Saída: Miau!
```

### User-Defined Type Guards

```typescript
// Type Guards definidos pelo usuário
interface Bird {
  fly(): void;
  layEggs(): void;
}

interface Fish {
  swim(): void;
  layEggs(): void;
}

// Type guard definido pelo usuário usando type predicate
function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}

function move(pet: Fish | Bird) {
  if (isFish(pet)) {
    // TypeScript sabe que pet é um Fish aqui
    pet.swim();
  } else {
    // TypeScript sabe que pet é um Bird aqui
    pet.fly();
  }
}

const fish = {
  swim() {
    console.log("Nadando...");
  },
  layEggs() {
    console.log("Botando ovos de peixe...");
  },
};

const bird = {
  fly() {
    console.log("Voando...");
  },
  layEggs() {
    console.log("Botando ovos de pássaro...");
  },
};

move(fish); // Saída: Nadando...
move(bird); // Saída: Voando...
```

### Discriminated Unions

```typescript
// Discriminated Unions para Type Guards
interface Circle {
  kind: "circle";
  radius: number;
}

interface Square {
  kind: "square";
  sideLength: number;
}

type Shape = Circle | Square;

function getArea(shape: Shape): number {
  switch (shape.kind) {
    case "circle":
      // TypeScript sabe que shape é um Circle aqui
      return Math.PI * shape.radius ** 2;
    case "square":
      // TypeScript sabe que shape é um Square aqui
      return shape.sideLength ** 2;
    default:
      // Nunca deve chegar aqui se todos os casos forem cobertos
      const _exhaustiveCheck: never = shape;
      return _exhaustiveCheck;
  }
}

const circle: Circle = { kind: "circle", radius: 5 };
const square: Square = { kind: "square", sideLength: 10 };

console.log(getArea(circle)); // Saída: 78.53981633974483
console.log(getArea(square)); // Saída: 100
```

### Assertion Functions

```typescript
// Funções de asserção
function assert(condition: any, msg?: string): asserts condition {
  if (!condition) {
    throw new Error(msg || "Assertion failed");
  }
}

function assertIsString(val: any): asserts val is string {
  if (typeof val !== "string") {
    throw new Error("Value is not a string");
  }
}

function processText(text: unknown) {
  assertIsString(text); // Garante que text é uma string

  // Após a afirmação, TypeScript sabe que text é uma string
  console.log(text.toUpperCase());
}

try {
  processText("hello"); // Saída: HELLO
  processText(123); // Lança um erro
} catch (e) {
  console.log(e.message); // Saída: Value is not a string
}

// Uso com assert
function divide(a: number, b: number) {
  assert(b !== 0, "Divisor não pode ser zero");
  return a / b;
}

console.log(divide(10, 2)); // Saída: 5
// divide(10, 0); // Lança um erro: Divisor não pode ser zero
```

## Integração com JavaScript

O TypeScript foi projetado para coexistir e interagir com código JavaScript existente. Vamos ver como integrar TypeScript em projetos JavaScript e como usar bibliotecas JavaScript no TypeScript.

### Migrando de JavaScript para TypeScript

```typescript
// Arquivo original em JavaScript: usuario.js
// function criarUsuario(nome, idade, ativo) {
//     return {
//         nome: nome,
//         idade: idade,
//         ativo: ativo || true,
//         criadoEm: new Date()
//     };
// }

// Versão TypeScript: usuario.ts
interface Usuario {
  nome: string;
  idade: number;
  ativo: boolean;
  criadoEm: Date;
}

function criarUsuario(nome: string, idade: number, ativo: boolean = true): Usuario {
  return {
    nome,
    idade,
    ativo,
    criadoEm: new Date(),
  };
}

const usuario = criarUsuario("Carlos", 28);
console.log(usuario);
```

**Saída:**

```
{
  nome: 'Carlos',
  idade: 28,
  ativo: true,
  criadoEm: 2023-06-15T10:30:00.000Z
}
```

### Arquivos de Definição de Tipos

Os arquivos `.d.ts` permitem adicionar tipos a bibliotecas JavaScript existentes:

```typescript
// exemplo.d.ts
declare module "minha-biblioteca-js" {
  export function somar(a: number, b: number): number;
  export function multiplicar(a: number, b: number): number;

  export interface ResultadoCalculo {
    valor: number;
    timestamp: Date;
  }

  export class Calculadora {
    constructor(precisao?: number);
    calcular(a: number, b: number, operacao: string): ResultadoCalculo;
  }
}

// Como usar em seu código TypeScript
import { somar, Calculadora } from "minha-biblioteca-js";

const resultado = somar(5, 3);
console.log(resultado); // 8

const calc = new Calculadora(2);
const res = calc.calcular(10, 5, "dividir");
console.log(res.valor); // 2.00
```

### Usando TypeScript e JavaScript no mesmo projeto

```typescript
// Arquivo: config.js (JavaScript)
// Exemplo de arquivo JavaScript que será importado
// module.exports = {
//     apiUrl: "https://api.exemplo.com",
//     timeout: 5000,
//     debug: true
// };

// Arquivo: app.ts (TypeScript)
// Importando um módulo JavaScript em TypeScript

// Método 1: Criando um arquivo de declaração
// config.d.ts
// declare module "./config" {
//     export const apiUrl: string;
//     export const timeout: number;
//     export const debug: boolean;
// }

// Método 2: Usar importação com tipo
import * as config from "./config";

// Método 3: Usar asserção de tipo
const settings = require("./config") as {
  apiUrl: string;
  timeout: number;
  debug: boolean;
};

function inicializarApp() {
  console.log(`Conectando a ${config.apiUrl}`);
  console.log(`Timeout: ${settings.timeout}ms`);
}

inicializarApp();
```

**Saída:**

```
Conectando a https://api.exemplo.com
Timeout: 5000ms
```

### Usando JSDoc para adicionar tipos a código JavaScript

```javascript
// usuario.js
/**
 * Representa um usuário do sistema
 * @typedef {Object} Usuario
 * @property {string} nome - O nome do usuário
 * @property {number} idade - A idade do usuário
 * @property {boolean} [ativo] - Se o usuário está ativo
 */

/**
 * Cria um novo usuário
 * @param {string} nome - O nome do usuário
 * @param {number} idade - A idade do usuário
 * @param {boolean} [ativo=true] - Status de ativação
 * @returns {Usuario} O objeto usuário criado
 */
function criarUsuario(nome, idade, ativo = true) {
  return {
    nome,
    idade,
    ativo,
    criadoEm: new Date(),
  };
}

const usuario = criarUsuario("Daniela", 32);
console.log(usuario);
```

**Saída:**

```
{
  nome: 'Daniela',
  idade: 32,
  ativo: true,
  criadoEm: 2023-06-15T10:35:00.000Z
}
```

### Strict Mode e Configurações

```typescript
// Exemplo de código com strict mode habilitado
// tsconfig.json:
// {
//   "compilerOptions": {
//     "strict": true
//   }
// }

function processarDados(dados: string[] | null) {
  // Verificação de nulo obrigatória em strict mode
  if (dados === null) {
    return [];
  }

  // Sem strict mode, isso poderia ser:
  // return dados.map(item => item.toUpperCase());

  // Com strict mode, é necessário tratar valores possíveis de undefined
  return dados.map((item) => (item ? item.toUpperCase() : ""));
}

console.log(processarDados(["a", "b", null, "c"]));
```

**Saída:**

```
["A", "B", "", "C"]
```

## Configurações Avançadas do tsconfig.json

O arquivo `tsconfig.json` configura o comportamento do compilador TypeScript. Vamos explorar algumas configurações avançadas:

### Estrutura básica do tsconfig.json

```json
{
  "compilerOptions": {
    "target": "es2020",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "outDir": "./dist",
    "rootDir": "./src"
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "**/*.spec.ts"]
}
```

### Opções de compilação importantes

```typescript
// Exemplo com várias configurações explicadas

// 1. target - Define o nível do ECMAScript para o qual o código será transpilado
// "target": "es2020" - Permite uso de recursos modernos como optional chaining
const usuario = { endereco: null };
const rua = usuario.endereco?.rua; // Sem erro em es2020, erro em es5

// 2. lib - Especifica quais bibliotecas de definição de tipos incluir
// "lib": ["dom", "es2020"] - Permite acesso a tipos DOM e ES2020
const promessa = Promise.resolve(42);
document.querySelector("#app"); // Disponível com "dom"

// 3. strict - Habilita todas as verificações de tipo estritas
// "strict": true
let valor: number;
// console.log(valor); // Erro: Variable 'valor' is used before being assigned

// 4. strictNullChecks - Verifica valores null e undefined
// "strictNullChecks": true
function processa(texto: string | null) {
  // texto.toLowerCase(); // Erro com strictNullChecks
  if (texto) {
    texto.toLowerCase(); // OK
  }
}

// 5. noImplicitAny - Proíbe tipos 'any' implícitos
// "noImplicitAny": true
// function exemplo(param) { // Erro com noImplicitAny
function exemplo(param: unknown) {
  // OK
  console.log(param);
}

// 6. paths - Permite configurar aliases para importações
// "paths": {
//   "@services/*": ["src/services/*"],
//   "@utils/*": ["src/utils/*"]
// }
// import { formatarData } from "@utils/data"; // Usando alias

// 7. baseUrl - Define a base para resolução de módulos não relativos
// "baseUrl": "."

// 8. moduleResolution - Estratégia de resolução de módulos
// "moduleResolution": "node"

// 9. esModuleInterop - Melhora interoperabilidade com módulos CommonJS
// "esModuleInterop": true
// import React from 'react'; // Funciona bem com esModuleInterop

// 10. sourceMap - Gera arquivos .map para debugging
// "sourceMap": true

// 11. declaration - Gera arquivos .d.ts para projetos de biblioteca
// "declaration": true

console.log("TypeScript compilado com sucesso!");
```

### Project References

O TypeScript 3.0 introduziu referências de projeto para trabalhar com bases de código grandes e divididas:

```json
// tsconfig.json no projeto raiz
{
  "files": [],
  "references": [
    { "path": "./packages/core" },
    { "path": "./packages/utils" },
    { "path": "./packages/ui" }
  ]
}

// tsconfig.json em ./packages/core
{
  "compilerOptions": {
    "composite": true,
    "declaration": true,
    "outDir": "./dist",
    "rootDir": "./src"
  }
}
```

### Configurações para bibliotecas vs. aplicações

```typescript
// Para bibliotecas:
/*
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "declaration": true,
    "outDir": "./lib",
    "strict": true
  },
  "include": ["src"],
  "exclude": ["node_modules", "**/*.test.ts"]
}
*/

// Para aplicações web:
/*
{
  "compilerOptions": {
    "target": "es2020",
    "module": "esnext",
    "moduleResolution": "node",
    "jsx": "react",
    "outDir": "./dist",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src"]
}
*/

// Para aplicações Node.js:
/*
{
  "compilerOptions": {
    "target": "es2020",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "resolveJsonModule": true
  },
  "include": ["src/**/*"]
}
*/
```

## TypeScript com Frameworks Modernos

### TypeScript com React

```tsx
// Exemplo de componente React com TypeScript
import React, { useState, useEffect } from "react";

// Definindo interfaces para props
interface UserProps {
  name: string;
  age: number;
  role?: string;
  onStatusChange: (status: boolean) => void;
}

// Componente funcional com tipos
const UserProfile: React.FC<UserProps> = ({ name, age, role = "Usuário", onStatusChange }) => {
  // Estado tipado
  const [isActive, setIsActive] = useState<boolean>(false);

  // Efeito com tipagem
  useEffect(() => {
    console.log(`Status do usuário ${name} alterado para: ${isActive ? "Ativo" : "Inativo"}`);
    onStatusChange(isActive);
  }, [isActive, name, onStatusChange]);

  return (
    <div className="user-profile">
      <h2>{name}</h2>
      <p>Idade: {age}</p>
      <p>Função: {role}</p>
      <button onClick={() => setIsActive(!isActive)}>{isActive ? "Desativar" : "Ativar"}</button>
    </div>
  );
};

// Usando o componente
const App: React.FC = () => {
  const handleStatusChange = (status: boolean) => {
    console.log(`Status alterado para: ${status}`);
  };

  return (
    <div className="app">
      <UserProfile name="João Silva" age={32} role="Administrador" onStatusChange={handleStatusChange} />
    </div>
  );
};

export default App;
```

### TypeScript com Vue 3

```typescript
// Componente Vue 3 com Composition API e TypeScript
import { defineComponent, ref, computed, onMounted } from "vue";

interface Task {
  id: number;
  title: string;
  completed: boolean;
}

export default defineComponent({
  name: "TaskList",

  props: {
    maxTasks: {
      type: Number,
      default: 10,
    },
  },

  setup(props) {
    // Estado reativo tipado
    const tasks = ref<Task[]>([]);
    const newTaskTitle = ref<string>("");

    // Computado tipado
    const completedTasks = computed<Task[]>(() => {
      return tasks.value.filter((task) => task.completed);
    });

    // Método tipado
    const addTask = (): void => {
      if (newTaskTitle.value.trim() && tasks.value.length < props.maxTasks) {
        tasks.value.push({
          id: Date.now(),
          title: newTaskTitle.value,
          completed: false,
        });
        newTaskTitle.value = "";
      }
    };

    const toggleTask = (id: number): void => {
      const task = tasks.value.find((t) => t.id === id);
      if (task) {
        task.completed = !task.completed;
      }
    };

    // Lifecycle hook
    onMounted(() => {
      console.log("Componente montado");
      // Mock de dados iniciais
      tasks.value = [
        { id: 1, title: "Aprender TypeScript", completed: true },
        { id: 2, title: "Aprender Vue 3", completed: false },
      ];
    });

    return {
      tasks,
      newTaskTitle,
      completedTasks,
      addTask,
      toggleTask,
    };
  },
});

/* Template Vue (não é TypeScript, mas incluído para contexto)
<template>
  <div class="task-list">
    <h2>Lista de Tarefas ({{ tasks.length }}/{{ maxTasks }})</h2>
    
    <form @submit.prevent="addTask">
      <input v-model="newTaskTitle" placeholder="Nova tarefa..." />
      <button type="submit">Adicionar</button>
    </form>
    
    <ul>
      <li v-for="task in tasks" :key="task.id" 
          :class="{ completed: task.completed }"
          @click="toggleTask(task.id)">
        {{ task.title }}
      </li>
    </ul>
    
    <div>Tarefas completas: {{ completedTasks.length }}/{{ tasks.length }}</div>
  </div>
</template>
*/
```

### TypeScript com Angular

```typescript
// Componente Angular com TypeScript
import { Component, OnInit, Input, Output, EventEmitter } from "@angular/core";

interface Product {
  id: number;
  name: string;
  price: number;
  available: boolean;
}

@Component({
  selector: "app-product-list",
  template: `
    <div class="product-list">
      <h2>{{ title }}</h2>
      <div *ngFor="let product of products" class="product-item">
        <h3>{{ product.name }}</h3>
        <p>Preço: {{ product.price | currency }}</p>
        <button [disabled]="!product.available" (click)="onProductSelect(product)">
          {{ product.available ? "Comprar" : "Indisponível" }}
        </button>
      </div>
    </div>
  `,
})
export class ProductListComponent implements OnInit {
  // Inputs tipados
  @Input() title: string = "Produtos";
  @Input() products: Product[] = [];

  // Output tipado
  @Output() productSelected = new EventEmitter<Product>();

  constructor() {}

  ngOnInit(): void {
    console.log("Componente inicializado");
  }

  onProductSelect(product: Product): void {
    if (product.available) {
      this.productSelected.emit(product);
    }
  }

  // Método com tipo de retorno
  getTotalAvailable(): number {
    return this.products.filter((p) => p.available).length;
  }
}

// Serviço Angular com TypeScript
import { Injectable } from "@angular/core";
import { HttpClient } from "@angular/common/http";
import { Observable } from "rxjs";
import { map } from "rxjs/operators";

@Injectable({
  providedIn: "root",
})
export class ProductService {
  private apiUrl = "https://api.example.com/products";

  constructor(private http: HttpClient) {}

  getProducts(): Observable<Product[]> {
    return this.http.get<Product[]>(this.apiUrl);
  }

  getProductById(id: number): Observable<Product | undefined> {
    return this.http.get<Product>(`${this.apiUrl}/${id}`);
  }

  searchProducts(term: string): Observable<Product[]> {
    return this.http
      .get<Product[]>(`${this.apiUrl}?search=${term}`)
      .pipe(map((products) => products.filter((product) => product.name.toLowerCase().includes(term.toLowerCase()))));
  }
}
```

### TypeScript com Node.js e Express

```typescript
// Aplicação Express com TypeScript
import express, { Request, Response, NextFunction } from "express";
import { body, validationResult } from "express-validator";

// Interfaces para tipos personalizados
interface User {
  id: number;
  name: string;
  email: string;
  age: number;
}

// Estendendo tipos do Express
interface AuthenticatedRequest extends Request {
  user?: User; // Usuário adicionado pelo middleware de autenticação
}

const app = express();
app.use(express.json());

// Middleware com tipagem
const authenticateUser = (req: AuthenticatedRequest, res: Response, next: NextFunction): void => {
  // Simulação de autenticação
  const token = req.headers.authorization;

  if (!token) {
    res.status(401).json({ error: "Não autenticado" });
    return;
  }

  // Simulação de decodificar token
  req.user = {
    id: 1,
    name: "Usuário Teste",
    email: "usuario@teste.com",
    age: 30,
  };

  next();
};

// Tipo para armazenar os usuários
const users: User[] = [
  { id: 1, name: "João", email: "joao@exemplo.com", age: 25 },
  { id: 2, name: "Maria", email: "maria@exemplo.com", age: 30 },
];

// Rotas com validação e tipagem
app.get("/users", (req: Request, res: Response) => {
  res.json(users);
});

app.get("/users/:id", (req: Request, res: Response) => {
  const id = parseInt(req.params.id);
  const user = users.find((u) => u.id === id);

  if (!user) {
    return res.status(404).json({ error: "Usuário não encontrado" });
  }

  res.json(user);
});

app.post(
  "/users",
  // Validação com express-validator
  [
    body("name").notEmpty().withMessage("Nome é obrigatório"),
    body("email").isEmail().withMessage("Email inválido"),
    body("age").isInt({ min: 18 }).withMessage("Idade deve ser pelo menos 18"),
  ],
  (req: Request, res: Response) => {
    const errors = validationResult(req);

    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }

    const newUser: User = {
      id: users.length + 1,
      ...req.body,
    };

    users.push(newUser);
    res.status(201).json(newUser);
  }
);

// Rota protegida que requer autenticação
app.get("/profile", authenticateUser, (req: AuthenticatedRequest, res: Response) => {
  res.json(req.user);
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Servidor rodando na porta ${PORT}`);
});

// Saída:
// Servidor rodando na porta 3000
```

### TypeScript com Next.js

```typescript
// Exemplo de página Next.js com TypeScript
// pages/products/[id].tsx

import { GetServerSideProps, NextPage } from "next";
import { useRouter } from "next/router";
import Head from "next/head";

// Interfaces para tipos
interface Product {
  id: number;
  name: string;
  description: string;
  price: number;
  imageUrl: string;
}

interface ProductPageProps {
  product: Product | null;
}

// Componente de página tipado
const ProductPage: NextPage<ProductPageProps> = ({ product }) => {
  const router = useRouter();

  // Enquanto a página está carregando
  if (router.isFallback) {
    return <div>Carregando...</div>;
  }

  // Se o produto não foi encontrado
  if (!product) {
    return (
      <div>
        <h1>Produto não encontrado</h1>
        <button onClick={() => router.push("/products")}>Voltar para lista de produtos</button>
      </div>
    );
  }

  return (
    <>
      <Head>
        <title>{product.name} | Minha Loja</title>
        <meta name="description" content={product.description} />
      </Head>

      <div className="product-detail">
        <img src={product.imageUrl} alt={product.name} />
        <h1>{product.name}</h1>
        <p className="description">{product.description}</p>
        <p className="price">
          {product.price.toLocaleString("pt-BR", {
            style: "currency",
            currency: "BRL",
          })}
        </p>
        <button>Adicionar ao carrinho</button>
      </div>
    </>
  );
};

// GetServerSideProps tipado
export const getServerSideProps: GetServerSideProps<ProductPageProps> = async ({ params }) => {
  try {
    const id = params?.id;

    // Simulação de chamada à API
    // Na vida real, isso seria uma chamada fetch ou cliente API
    const productData = {
      id: 1,
      name: "Smartphone XYZ",
      description: "Um smartphone avançado com recursos incríveis.",
      price: 2499.99,
      imageUrl: "/images/smartphone.jpg",
    };

    return {
      props: {
        product: productData,
      },
    };
  } catch (error) {
    console.error("Erro ao buscar produto:", error);

    return {
      props: {
        product: null,
      },
    };
  }
};

export default ProductPage;
```

## Práticas Recomendadas

### Convenções de Nomeação e Estilo

```typescript
// Convenções de nomeação
// Classes - PascalCase
class UserService {
  // Propriedades privadas - camelCase com prefixo _
  private _apiUrl: string;

  // Construtor conciso com parâmetros de propriedade
  constructor(private _httpClient: HttpClient) {
    this._apiUrl = "https://api.example.com/users";
  }

  // Métodos públicos - camelCase
  async getUserById(id: number): Promise<User> {
    return this._fetchUser(id);
  }

  // Métodos privados - camelCase com prefixo _
  private async _fetchUser(id: number): Promise<User> {
    // Implementação
    return {} as User;
  }
}

// Interfaces - PascalCase, sem prefixo 'I'
interface User {
  id: number;
  name: string;
}

// Enum - PascalCase com nomes singulares
enum UserRole {
  Admin = "ADMIN",
  Editor = "EDITOR",
  Viewer = "VIEWER",
}

// Types - PascalCase
type UserId = number | string;

// Constantes - UPPER_SNAKE_CASE para valores realmente constantes
const MAX_LOGIN_ATTEMPTS = 3;

// camelCase para outras variáveis
const userService = new UserService({} as HttpClient);
```

### Padronização de Código

```typescript
// Usando ESLint e Prettier para padronização
// .eslintrc.js
/*
module.exports = {
  parser: '@typescript-eslint/parser',
  plugins: ['@typescript-eslint'],
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/recommended',
    'prettier'
  ],
  rules: {
    // Regras personalizadas
    '@typescript-eslint/explicit-function-return-type': 'error',
    '@typescript-eslint/no-explicit-any': 'error',
    '@typescript-eslint/no-unused-vars': ['error', { argsIgnorePattern: '^_' }]
  }
};
*/

// .prettierrc
/*
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 100,
  "tabWidth": 2
}
*/

// Código seguindo padrões
function calculateTax(income: number, taxRate: number): number {
  if (income < 0) {
    throw new Error("Income cannot be negative");
  }

  return income * taxRate;
}

// Usando formatação consistente
interface Product {
  id: number;
  name: string;
  price: number;
}

// Evitando any
// function process(data: any): any {} // ESLint acusaria erro

// Versão correta
function process<T>(data: T): T {
  // Processamento genérico
  return data;
}
```

### Técnicas de Tipagem Avançada

```typescript
// 1. Mapped Types
type Optional<T> = {
  [P in keyof T]?: T[P];
};

interface User {
  id: number;
  name: string;
  email: string;
}

// Torna todas as propriedades opcionais
type PartialUser = Optional<User>;

const userUpdate: PartialUser = {
  name: "Novo Nome",
  // email e id são opcionais
};

// 2. Conditional Types
type NonNullable<T> = T extends null | undefined ? never : T;
type StringOrNumber = string | number | null;
type NonNullStringOrNumber = NonNullable<StringOrNumber>; // string | number

// 3. Utility Types incorporados
interface Todo {
  title: string;
  description: string;
  completed: boolean;
  createdAt: Date;
}

// Partial - torna todas as propriedades opcionais
type TodoPartial = Partial<Todo>;

// Pick - seleciona algumas propriedades
type TodoPreview = Pick<Todo, "title" | "completed">;

// Omit - omite algumas propriedades
type TodoWithoutDates = Omit<Todo, "createdAt">;

// Record - cria um tipo de objeto com chaves e valores especificados
type TodosByCategory = Record<string, Todo[]>;

// Exemplos de uso
const todoUpdate: Partial<Todo> = {
  completed: true,
};

const todoPreview: TodoPreview = {
  title: "Estudar TypeScript",
  completed: false,
};

const userTodos: TodosByCategory = {
  work: [{ title: "Projeto A", description: "Completar projeto", completed: false, createdAt: new Date() }],
  personal: [{ title: "Exercícios", description: "Ir à academia", completed: true, createdAt: new Date() }],
};

console.log(todoUpdate);
console.log(todoPreview);
```

**Saída:**

```
{ completed: true }
{ title: 'Estudar TypeScript', completed: false }
```

### Padrões de Design em TypeScript

```typescript
// 1. Singleton Pattern
class DatabaseConnection {
  private static instance: DatabaseConnection;
  private connectionString: string;

  private constructor(connectionString: string) {
    this.connectionString = connectionString;
    console.log("Nova conexão criada com DB");
  }

  public static getInstance(connectionString: string): DatabaseConnection {
    if (!DatabaseConnection.instance) {
      DatabaseConnection.instance = new DatabaseConnection(connectionString);
    }
    return DatabaseConnection.instance;
  }

  public query(sql: string): void {
    console.log(`Executando query: ${sql} na conexão ${this.connectionString}`);
  }
}

const db1 = DatabaseConnection.getInstance("mongodb://localhost:27017");
const db2 = DatabaseConnection.getInstance("mongodb://localhost:27017");
db1.query("SELECT * FROM users");
db2.query("SELECT * FROM products");
console.log(db1 === db2); // true - mesma instância

// 2. Factory Method Pattern
interface ILogger {
  log(message: string): void;
}

class ConsoleLogger implements ILogger {
  log(message: string): void {
    console.log(`[Console]: ${message}`);
  }
}

class FileLogger implements ILogger {
  log(message: string): void {
    console.log(`[File]: ${message}`);
  }
}

class LoggerFactory {
  public static createLogger(type: "console" | "file"): ILogger {
    switch (type) {
      case "console":
        return new ConsoleLogger();
      case "file":
        return new FileLogger();
      default:
        throw new Error(`Logger type "${type}" not supported.`);
    }
  }
}

const consoleLogger = LoggerFactory.createLogger("console");
consoleLogger.log("Teste de log");

// 3. Repository Pattern
interface UserRepository {
  findById(id: number): Promise<User>;
  findAll(): Promise<User[]>;
  save(user: User): Promise<User>;
}

class PostgresUserRepository implements UserRepository {
  async findById(id: number): Promise<User> {
    console.log(`Buscando usuário ${id} no Postgres`);
    return { id, name: "Usuário Exemplo", email: "exemplo@email.com" };
  }

  async findAll(): Promise<User[]> {
    console.log("Buscando todos os usuários no Postgres");
    return [
      { id: 1, name: "Usuário 1", email: "usuario1@email.com" },
      { id: 2, name: "Usuário 2", email: "usuario2@email.com" },
    ];
  }

  async save(user: User): Promise<User> {
    console.log(`Salvando usuário no Postgres: ${JSON.stringify(user)}`);
    return { ...user, id: user.id || Date.now() };
  }
}

// Uso do Repository
const userRepo = new PostgresUserRepository();
async function testRepo(): Promise<void> {
  const user = await userRepo.findById(1);
  console.log(user);

  const savedUser = await userRepo.save({ name: "Novo Usuário", email: "novo@email.com" } as User);
  console.log(savedUser);
}

// testRepo(); // Executar assincronamente
```

**Saída:**

```
Nova conexão criada com DB
Executando query: SELECT * FROM users na conexão mongodb://localhost:27017
Executando query: SELECT * FROM products na conexão mongodb://localhost:27017
true
[Console]: Teste de log
Buscando usuário 1 no Postgres
{ id: 1, name: 'Usuário Exemplo', email: 'exemplo@email.com' }
Salvando usuário no Postgres: {"name":"Novo Usuário","email":"novo@email.com"}
{ name: 'Novo Usuário', email: 'novo@email.com', id: 1621500000000 }
```

### Tratamento de Erros em TypeScript

```typescript
// 1. Classes de erro personalizadas
class ApplicationError extends Error {
  constructor(public code: string, message: string) {
    super(message);
    this.name = this.constructor.name;
    // Preserva o stack trace
    if (Error.captureStackTrace) {
      Error.captureStackTrace(this, this.constructor);
    }
  }
}

class DatabaseError extends ApplicationError {
  constructor(message: string, public sqlState?: string) {
    super("DB_ERROR", message);
  }
}

class ValidationError extends ApplicationError {
  constructor(message: string, public field?: string) {
    super("VALIDATION_ERROR", message);
  }
}

// 2. Tratamento de erros tipado
function processUserData(user: User): void {
  try {
    // Simulação de erro
    if (!user.name) {
      throw new ValidationError("Nome é obrigatório", "name");
    }

    console.log(`Processando usuário: ${user.name}`);

    // Simulação de erro de banco de dados
    if (user.id === 0) {
      throw new DatabaseError("Erro ao salvar usuário", "SQL_INSERT_FAILED");
    }
  } catch (error) {
    if (error instanceof ValidationError) {
      console.error(`Erro de validação no campo ${error.field}: ${error.message}`);
    } else if (error instanceof DatabaseError) {
      console.error(`Erro de banco de dados (${error.sqlState}): ${error.message}`);
      // Log adicional ou notificação para administradores
    } else {
      console.error("Erro desconhecido:", error);
    }
  }
}

// 3. Tratamento assíncrono
async function fetchUserData(id: number): Promise<User> {
  try {
    // Simulação de chamada API
    if (id < 0) {
      throw new Error("ID inválido");
    }

    // Simulação de response
    return { id, name: "Usuário API", email: "api@exemplo.com" };
  } catch (error) {
    console.error("Erro ao buscar dados:", error);
    throw error; // Re-throw para tratamento em nível superior
  }
}

// 4. Function result pattern
interface Result<T> {
  success: boolean;
  data?: T;
  error?: Error;
}

async function tryExecute<T>(fn: () => Promise<T>): Promise<Result<T>> {
  try {
    const data = await fn();
    return { success: true, data };
  } catch (error) {
    return {
      success: false,
      error: error instanceof Error ? error : new Error(String(error)),
    };
  }
}

// Uso do pattern
async function exampleOperation(): Promise<void> {
  const result = await tryExecute(() => fetchUserData(1));

  if (result.success && result.data) {
    console.log("Dados obtidos com sucesso:", result.data);
  } else {
    console.error("Falha na operação:", result.error?.message);
  }
}

// exampleOperation(); // Executar assincronamente
```

**Saída:**

```
Processando usuário: Usuário API
Dados obtidos com sucesso: { id: 1, name: 'Usuário API', email: 'api@exemplo.com' }
```

## Recursos Adicionais

### Livros Recomendados

- **Programming TypeScript** - Boris Cherny
- **Effective TypeScript: 62 Specific Ways to Improve Your TypeScript** - Dan Vanderkam
- **TypeScript Deep Dive** - Basarat Ali Syed (disponível gratuitamente online)

### Documentação Oficial

A documentação oficial do TypeScript é um recurso excelente e sempre atualizado:

- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)
- [TypeScript Playground](https://www.typescriptlang.org/play)

### Cursos e Tutoriais

- [TypeScript for Beginners](https://www.udemy.com/course/typescript/)
- [Understanding TypeScript](https://www.udemy.com/course/understanding-typescript/)
- [Advanced TypeScript](https://www.pluralsight.com/courses/typescript-advanced)

### Comunidade e Blogs

- [TypeScript GitHub Repository](https://github.com/microsoft/TypeScript)
- [TypeScript Weekly Newsletter](https://www.typescript-weekly.com/)
- [Dev.to TypeScript Tag](https://dev.to/t/typescript)

### Ferramentas de Suporte

- **ESLint com TypeScript**: `@typescript-eslint/parser` e `@typescript-eslint/eslint-plugin`
- **Prettier**: Para formatação consistente
- **TSLint** (legado, considere ESLint): Verificação estática de estilo
- **VSCode**: Excelente suporte integrado ao TypeScript
- **ts-node**: Execute TypeScript diretamente
- **Jest com ts-jest**: Para testes em TypeScript

### Exemplos Avançados e Playground

- [TypeScript Playground](https://www.typescriptlang.org/play) - Testar código TypeScript online
- [TypeScript GitHub Exemplos](https://github.com/microsoft/TypeScript-Website/tree/v2/packages/playground-examples/copy/en)

### Comunidades de Ajuda

- Stack Overflow: [Tag TypeScript](https://stackoverflow.com/questions/tagged/typescript)
- Reddit: [r/typescript](https://www.reddit.com/r/typescript/)
- Discord: [TypeScript Community](https://discord.com/invite/typescript)

---

## Conclusão

TypeScript é uma ferramenta poderosa que continua a evoluir e melhorar a cada nova versão. Ao adotar TypeScript em seus projetos, você ganha:

1. **Detecção antecipada de erros**: Muitos bugs são capturados durante o desenvolvimento, não em produção.
2. **Melhor documentação**: Os tipos funcionam como documentação viva do seu código.
3. **Melhor experiência de desenvolvimento**: Autocomplete mais inteligente e refatoração mais segura.
4. **Integração com frameworks modernos**: React, Vue, Angular e Node.js têm excelente suporte.
5. **Escalabilidade**: Projetos grandes se beneficiam enormemente da verificação de tipos.

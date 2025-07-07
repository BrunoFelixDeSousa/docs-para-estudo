# [⬅ Voltar para o índice principal](../../../README.md)

# Documentação Completa do Jest

## Índice

1. [Introdução ao Jest](#1-introdução-ao-jest)
2. [Instalação e Configuração](#2-instalação-e-configuração)
3. [Escrevendo Testes](#3-escrevendo-testes)
4. [Matchers](#4-matchers)
5. [Mock Functions](#5-mock-functions)
6. [Setup e Teardown](#6-setup-e-teardown)
7. [Testes Assíncronos](#7-testes-assíncronos)
8. [Snapshot Testing](#8-snapshot-testing)
9. [Timer Mocks](#9-timer-mocks)
10. [Manual Mocking](#10-manual-mocking)
11. [Integração com TypeScript](#11-integração-com-typescript)
12. [Migração de Outros Frameworks](#12-migração-de-outros-frameworks)
13. [Configuração Avançada](#13-configuração-avançada)
14. [Práticas Recomendadas](#14-práticas-recomendadas)
15. [Recursos Adicionais](#15-recursos-adicionais)
16. [Testes no NestJS](#16-testes-no-nestjs)

## 1. Introdução ao Jest

### O que é Jest?

Jest é um framework de teste JavaScript desenvolvido pelo Facebook, focado em simplicidade e suporte para aplicações modernas JavaScript, incluindo React, Angular, Vue, Node.js, TypeScript, e muito mais. Jest foi projetado para ser fácil de configurar e usar, oferecendo uma experiência "zero-configuração" para a maioria dos projetos JavaScript.

### Principais Características

- **Velocidade:** Execução paralela de testes para maior performance
- **Watch Mode:** Reexecuta testes automaticamente quando arquivos são modificados
- **Snapshot Testing:** Captura e verifica saídas de componentes UI
- **Code Coverage:** Relatórios integrados de cobertura de código
- **Mocking simplificado:** Sistema robusto para testes de unidade
- **Isolamento de teste:** Cada teste executa em seu próprio ambiente, prevenindo efeitos colaterais
- **API intuitiva:** API clara e expressiva para escrever testes
- **Matchers expressivos:** Para validar diferentes tipos de dados e condições

### Por que usar Jest?

- Configuração mínima necessária
- Integração perfeita com ecossistemas React/JavaScript
- Execução rápida de testes (paralelismo)
- Ótima experiência para desenvolvedores com feedback instantâneo
- Documentação abrangente e comunidade ativa

## 2. Instalação e Configuração

### Instalação Básica

```bash
# Utilizando npm
npm install --save-dev jest

# Utilizando yarn
yarn add --dev jest
```

### Configuração no package.json

```json
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage"
  }
}
```

### Arquivo de Configuração Jest

Crie um arquivo `jest.config.js` na raiz do projeto:

```javascript
module.exports = {
  // A lista de caminhos para módulos que executam código antes de cada teste
  setupFiles: ["<rootDir>/setup.js"],

  // O diretório onde o Jest deve produzir sua cobertura
  coverageDirectory: "coverage",

  // Indica se cada teste individual deve ser relatado durante a execução
  verbose: true,

  // Padrão de arquivos de teste
  testMatch: ["**/__tests__/**/*.js", "**/?(*.)+(spec|test).js"],

  // Transforma arquivos
  transform: {
    "^.+\\.jsx?$": "babel-jest",
  },

  // Coletar cobertura de código
  collectCoverage: true,

  // Pastas a ignorar
  testPathIgnorePatterns: ["/node_modules/", "/dist/"],

  // Configuração de ambiente
  testEnvironment: "jsdom", // ou 'node'
};
```

### Configuração com Babel

Se você usar ES6 ou JSX, precisará configurar o Babel:

1. Instalar dependências:

```bash
npm install --save-dev babel-jest @babel/core @babel/preset-env
```

2. Configurar Babel (arquivo `.babelrc`):

```json
{
  "presets": ["@babel/preset-env"]
}
```

## 3. Escrevendo Testes

### Estrutura Básica de um Teste

```javascript
// soma.js
function soma(a, b) {
  return a + b;
}
module.exports = soma;

// soma.test.js
const soma = require("./soma");

test("soma 1 + 2 para resultar em 3", () => {
  expect(soma(1, 2)).toBe(3);
});
```

### Funções Globais do Jest

- `describe(name, fn)`: Cria um bloco de testes agrupados
- `test(name, fn)` ou `it(name, fn)`: Define um teste individual
- `beforeEach(fn)`: Executa antes de cada teste
- `afterEach(fn)`: Executa após cada teste
- `beforeAll(fn)`: Executa uma vez antes de todos os testes
- `afterAll(fn)`: Executa uma vez após todos os testes

### Exemplo de Organização de Testes

```javascript
describe("Módulo Calculadora", () => {
  let calculadora;

  beforeEach(() => {
    calculadora = require("./calculadora");
  });

  describe("função soma", () => {
    it("deve somar dois números positivos", () => {
      expect(calculadora.soma(2, 3)).toBe(5);
    });

    it("deve lidar com números negativos", () => {
      expect(calculadora.soma(-1, -5)).toBe(-6);
    });
  });

  describe("função multiplicação", () => {
    it("deve multiplicar dois números", () => {
      expect(calculadora.multiplicacao(2, 3)).toBe(6);
    });
  });
});
```

### Testes Somente e Ignorar

- `test.only('descrição', fn)`: Executa apenas este teste
- `test.skip('descrição', fn)`: Pula este teste
- `describe.only('descrição', fn)`: Executa apenas este grupo de testes
- `describe.skip('descrição', fn)`: Pula este grupo de testes

## 4. Matchers

### Comparações Comuns

```javascript
// Igualdade exata
expect(value).toBe(expected);          // Comparação com ===
expect(value).toEqual(expected);       // Verifica igualdade estrutural

// Verdadeiro/Falso
expect(value).toBeTruthy();            // Qualquer valor considerado verdadeiro
expect(value).toBeFalsy();             // Qualquer valor considerado falso

// Nulidade
expect(value).toBeDefined();           // Não é undefined
expect(value).toBeUndefined();         // É undefined
expect(value).toBeNull();              // É null

// Números
expect(value).toBeGreaterThan(3);      // Maior que
expect(value).toBeGreaterThanOrEqual(3.5); // Maior ou igual
expect(value).toBeLessThan(5);         // Menor que
expect(value).toBeLessThanOrEqual(4.5); // Menor ou igual
expect(value).toBeCloseTo(0.3);        // Para igualdade com ponto flutuante

// Strings
expect(string).toMatch(/expressão/);   // Corresponde à expressão regular
expect(string).toContain('substring');  // Contém substring

// Arrays e iteráveis
expect(array).toContain(item);         // Contém o item
expect(array).toHaveLength(3);         // Tem comprimento específico

// Objetos
expect(object).toHaveProperty('prop');  // Tem propriedade
expect(object).toMatchObject({...});    // Corresponde a subobjeto
```

### Matchers de Negação

Você pode inverter qualquer matcher com `.not`:

```javascript
expect(value).not.toBe(expected);
expect(array).not.toContain(item);
```

### Matchers Personalizados

Você pode criar seus próprios matchers:

```javascript
expect.extend({
  toBeEven(received) {
    const pass = received % 2 === 0;
    if (pass) {
      return {
        message: () => `Expected ${received} not to be an even number`,
        pass: true,
      };
    } else {
      return {
        message: () => `Expected ${received} to be an even number`,
        pass: false,
      };
    }
  },
});

test("é um número par", () => {
  expect(2).toBeEven();
});
```

## 5. Mock Functions

### Criando Mock Functions

```javascript
// Criar uma função mock
const mockFn = jest.fn();

// Com implementação
const mockFnWithImpl = jest.fn(() => "valor de retorno");

// Mock condicional
const mockComplex = jest.fn().mockReturnValueOnce(10).mockReturnValueOnce("x").mockReturnValue(true);
```

### Usando Mock Functions

```javascript
test("teste com mock function", () => {
  const mockCallback = jest.fn((x) => x + 42);

  [0, 1].forEach(mockCallback);

  // A função mock foi chamada duas vezes
  expect(mockCallback.mock.calls.length).toBe(2);

  // O primeiro argumento da primeira chamada foi 0
  expect(mockCallback.mock.calls[0][0]).toBe(0);

  // O primeiro argumento da segunda chamada foi 1
  expect(mockCallback.mock.calls[1][0]).toBe(1);

  // O valor de retorno da primeira chamada foi 42
  expect(mockCallback.mock.results[0].value).toBe(42);
});
```

### Mocking Modules

```javascript
// Mockar um módulo completo
jest.mock("./someModule");

// Mockar um módulo com implementação específica
jest.mock("./someModule", () => {
  return {
    method1: jest.fn(() => "mocked1"),
    method2: jest.fn(() => "mocked2"),
  };
});

// Importar um módulo mockado
import someModule from "./someModule";

test("teste com módulo mockado", () => {
  // Usar o módulo mockado
  expect(someModule.method1()).toBe("mocked1");
});
```

### Spying com Jest

```javascript
// Observar um método em um objeto
const spy = jest.spyOn(object, "method");

// Usar o método sendo observado
object.method(arg1, arg2);

// Verificar se o método foi chamado
expect(spy).toHaveBeenCalled();
expect(spy).toHaveBeenCalledWith(arg1, arg2);

// Restaurar a implementação original
spy.mockRestore();
```

## 6. Setup e Teardown

### Funções de Setup e Teardown

- `beforeEach`: Executada antes de cada teste em um bloco
- `afterEach`: Executada após cada teste em um bloco
- `beforeAll`: Executada uma vez antes de todos os testes em um bloco
- `afterAll`: Executada uma vez após todos os testes em um bloco

### Escopo

Essas funções respeitam o escopo de `describe`:

```javascript
beforeAll(() => console.log("1 - beforeAll"));
afterAll(() => console.log("1 - afterAll"));
beforeEach(() => console.log("1 - beforeEach"));
afterEach(() => console.log("1 - afterEach"));

test("", () => console.log("1 - test"));

describe("Grupo de testes", () => {
  beforeAll(() => console.log("2 - beforeAll"));
  afterAll(() => console.log("2 - afterAll"));
  beforeEach(() => console.log("2 - beforeEach"));
  afterEach(() => console.log("2 - afterEach"));

  test("", () => console.log("2 - test"));
});

// Ordem de execução:
// 1 - beforeAll
// 1 - beforeEach
// 1 - test
// 1 - afterEach
// 2 - beforeAll
// 1 - beforeEach
// 2 - beforeEach
// 2 - test
// 2 - afterEach
// 1 - afterEach
// 2 - afterAll
// 1 - afterAll
```

### Setup Único por Arquivo

```javascript
// setupFilesAfterEnv do jest.config.js
// Você pode incluir funções globais de configuração

// O arquivo setup.js:
beforeEach(() => {
  // Código a executar antes de cada teste
});
```

## 7. Testes Assíncronos

### Callbacks

```javascript
test("dados carregados assincronamente", (done) => {
  function callback(data) {
    try {
      expect(data).toBe("dados carregados");
      done();
    } catch (error) {
      done(error);
    }
  }

  fetchData(callback);
});
```

### Promises

```javascript
test("a promise resolve com dados", () => {
  return fetchData().then((data) => {
    expect(data).toBe("dados carregados");
  });
});

// Testando rejeições
test("a promise é rejeitada com erro", () => {
  expect.assertions(1); // Garante que a asserção seja chamada
  return fetchData().catch((e) => expect(e).toMatch("erro"));
});

// Com resolves/rejects
test("a promise resolve com dados", () => {
  return expect(fetchData()).resolves.toBe("dados carregados");
});

test("a promise é rejeitada com erro", () => {
  return expect(fetchData()).rejects.toMatch("erro");
});
```

### Async/Await

```javascript
test("dados carregados assincronamente", async () => {
  const data = await fetchData();
  expect(data).toBe("dados carregados");
});

test("a promise é rejeitada com erro", async () => {
  expect.assertions(1);
  try {
    await fetchData();
  } catch (e) {
    expect(e).toMatch("erro");
  }
});

// Com resolves/rejects
test("a promise resolve com dados", async () => {
  await expect(fetchData()).resolves.toBe("dados carregados");
});

test("a promise é rejeitada com erro", async () => {
  await expect(fetchData()).rejects.toMatch("erro");
});
```

## 8. Snapshot Testing

### Criando um Snapshot

```javascript
import renderer from "react-test-renderer";
import Componente from "./Componente";

test("componente renderiza corretamente", () => {
  const tree = renderer.create(<Componente />).toJSON();
  expect(tree).toMatchSnapshot();
});
```

### Atualizando Snapshots

```bash
# Atualizando todos os snapshots
jest --updateSnapshot

# Atualizando snapshots específicos interativamente
jest --watch
# Pressione 'u' para atualizar snapshots falhos
```

### Snapshots Inline

```javascript
test("componente renderiza botão com texto correto", () => {
  const tree = renderer.create(<Button>Clique aqui</Button>).toJSON();
  expect(tree).toMatchInlineSnapshot();
  // Jest preencherá a chamada com o snapshot na primeira execução
});
```

### Propriedades Dinâmicas

```javascript
test("componente com propriedades dinâmicas", () => {
  const tree = renderer.create(<Componente timestamp={Date.now()} />).toJSON();
  expect(tree).toMatchSnapshot({
    props: {
      timestamp: expect.any(Number),
    },
  });
});
```

## 9. Timer Mocks

### Mockando Funções de Tempo

```javascript
// Simulando setTimeout e setInterval
jest.useFakeTimers();

test("temporizador chama a função após 1 segundo", () => {
  const callback = jest.fn();

  // Configura o temporizador
  setTimeout(callback, 1000);

  // No início, o callback não deve ser chamado
  expect(callback).not.toBeCalled();

  // Avança o tempo em 1 segundo
  jest.advanceTimersByTime(1000);

  // Agora o callback deve ter sido chamado
  expect(callback).toBeCalled();
  expect(callback).toHaveBeenCalledTimes(1);
});

// Restaurar temporizadores reais após testes
afterEach(() => {
  jest.useRealTimers();
});
```

### Avançando Temporizadores

```javascript
jest.useFakeTimers();

test("avança temporizadores", () => {
  const callback = jest.fn();

  setTimeout(callback, 100);

  // Avança todos os temporizadores
  jest.runAllTimers();

  expect(callback).toHaveBeenCalledTimes(1);
});

test("avança temporizadores pendentes", () => {
  const callback = jest.fn();

  // Temporizador que cria outro temporizador
  function recursiveTimer() {
    callback();
    setTimeout(recursiveTimer, 100);
  }

  setTimeout(recursiveTimer, 100);

  // Avança apenas temporizadores pendentes
  jest.runOnlyPendingTimers();

  expect(callback).toHaveBeenCalledTimes(1);
});
```

### Controlando Tempo Atual

```javascript
// Mock de Date.now()
const mockDate = new Date(2020, 0, 1);
jest.spyOn(global, "Date").mockImplementation(() => mockDate);

test("utiliza data mockada", () => {
  expect(new Date().getFullYear()).toBe(2020);
});

// Restaurar após uso
global.Date.mockRestore();
```

## 10. Manual Mocking

### Mockando Módulos

Crie um diretório `__mocks__` ao lado do módulo que você quer mockar:

```
├── __mocks__/
│   └── fs.js
├── __tests__/
│   └── file-test.js
└── file.js
```

```javascript
// __mocks__/fs.js
const fs = jest.createMockFromModule("fs");

// Adicionando funcionalidade mockada personalizada
let mockFiles = {};
function __setMockFiles(newMockFiles) {
  mockFiles = { ...newMockFiles };
}

function readFileSync(path) {
  if (mockFiles[path]) {
    return mockFiles[path];
  }
  throw new Error(`File not found: ${path}`);
}

fs.__setMockFiles = __setMockFiles;
fs.readFileSync = readFileSync;

module.exports = fs;
```

### Usando Módulos Mockados

```javascript
// file-test.js
jest.mock("fs");
const fs = require("fs");

test("lê arquivo mockado", () => {
  // Configurar arquivos mockados
  fs.__setMockFiles({
    "/caminho/arquivo.txt": "conteúdo do arquivo",
  });

  // Usar função mockada
  const data = fs.readFileSync("/caminho/arquivo.txt");
  expect(data).toBe("conteúdo do arquivo");
});
```

### Mocking Parcial

```javascript
// Mockar apenas algumas funções de um módulo
jest.mock("./moduloOriginal", () => {
  const moduloOriginal = jest.requireActual("./moduloOriginal");
  return {
    ...moduloOriginal,
    funcaoParaMockar: jest.fn(() => "mockado"),
  };
});
```

## 11. Integração com TypeScript

### Configuração Básica

1. Instalar dependências:

```bash
npm install --save-dev typescript ts-jest @types/jest
```

2. Configurar `jest.config.js`:

```javascript
module.exports = {
  preset: "ts-jest",
  testEnvironment: "node",
  transform: {
    "^.+\\.tsx?$": "ts-jest",
  },
  testRegex: "(/__tests__/.*|(\\.|/)(test|spec))\\.(jsx?|tsx?)$",
  moduleFileExtensions: ["ts", "tsx", "js", "jsx", "json", "node"],
};
```

### Exemplo de Teste em TypeScript

```typescript
// soma.ts
export function soma(a: number, b: number): number {
  return a + b;
}

// soma.test.ts
import { soma } from "./soma";

test("soma 1 + 2 para resultar em 3", () => {
  expect(soma(1, 2)).toBe(3);
});
```

### Tipos para Jest

```typescript
// Usando tipagem para mock functions
const mockFn = jest.fn<ReturnType, Parameters>();

interface User {
  name: string;
  age: number;
}

const mockUser: jest.Mocked<User> = {
  name: jest.fn().mockReturnValue("João"),
  age: jest.fn().mockReturnValue(30),
};
```

## 12. Migração de Outros Frameworks

### De Mocha/Chai para Jest

- Jest usa `test()` ou `it()` como Mocha
- `expect()` substitui várias asserções do Chai
- Conversão de matchers comuns:

| Mocha/Chai                              | Jest                                  |
| --------------------------------------- | ------------------------------------- |
| `expect(value).to.equal(expected)`      | `expect(value).toBe(expected)`        |
| `expect(value).to.deep.equal(expected)` | `expect(value).toEqual(expected)`     |
| `expect(value).to.exist`                | `expect(value).toBeDefined()`         |
| `expect(value).to.be.true`              | `expect(value).toBe(true)`            |
| `expect(value).to.be.a('string')`       | `expect(typeof value).toBe('string')` |
| `expect([]).to.be.empty`                | `expect([]).toHaveLength(0)`          |

### De Jasmine para Jest

- Jest é baseado no Jasmine, então a sintaxe é muito similar
- A maioria dos matchers do Jasmine funciona no Jest
- Funções como `describe`, `it`, `beforeEach` são compatíveis
- Mocks são ligeiramente diferentes:
  - Jasmine: `spyOn(obj, 'method')`
  - Jest: `jest.spyOn(obj, 'method')`

### De Tape/AVA para Jest

- Para testes mais simples, substitua `tape` ou `t.pass` por `test`
- Substitua matchers como `t.equal` por `expect().toBe()`
- Agrupe testes com `describe` para melhor organização

## 13. Configuração Avançada

### Configuração Customizada

```javascript
// jest.config.js completo
module.exports = {
  // Diretório onde o Jest deve buscar por arquivos
  rootDir: ".",

  // Padrão de arquivos de teste
  testMatch: ["**/__tests__/**/*.js?(x)", "**/?(*.)+(spec|test).js?(x)"],

  // Pastas a ignorar
  testPathIgnorePatterns: ["/node_modules/", "/dist/"],

  // Arquivos de setup executados antes dos testes
  setupFiles: ["<rootDir>/jest-setup.js"],

  // Arquivos executados depois que o ambiente de teste é configurado
  setupFilesAfterEnv: ["<rootDir>/jest-setup-after-env.js"],

  // Transformações (processadores de arquivo)
  transform: {
    "^.+\\.jsx?$": "babel-jest",
    "^.+\\.tsx?$": "ts-jest",
  },

  // Módulos a serem mockados automaticamente
  automock: false,

  // Mapeamento de módulos para transformação
  moduleNameMapper: {
    "\\.(css|less|scss|sass)$": "identity-obj-proxy",
    "^@/(.*)$": "<rootDir>/src/$1",
  },

  // Coleta informações de cobertura
  collectCoverage: true,

  // Diretório de relatório de cobertura
  coverageDirectory: "coverage",

  // Padrões para inclusão na cobertura
  collectCoverageFrom: ["src/**/*.{js,jsx,ts,tsx}", "!src/**/*.d.ts"],

  // Limiares de cobertura
  coverageThreshold: {
    global: {
      statements: 80,
      branches: 80,
      functions: 80,
      lines: 80,
    },
  },

  // Ambiente de teste (jsdom, node)
  testEnvironment: "jsdom",

  // Exibe saída detalhada
  verbose: true,

  // Timeout para testes (em ms)
  testTimeout: 10000,

  // Mock para extensões específicas
  moduleFileExtensions: ["js", "jsx", "ts", "tsx", "json"],

  // Reporter personalizado
  reporters: ["default", "jest-junit"],

  // Configurações para o WatchMode
  watchPlugins: ["jest-watch-typeahead/filename", "jest-watch-typeahead/testname"],

  // Configura globalSetup e globalTeardown
  globalSetup: "<rootDir>/setup.js",
  globalTeardown: "<rootDir>/teardown.js",
};
```

### Projetos Múltiplos

```javascript
// jest.config.js para múltiplos projetos
module.exports = {
  projects: [
    {
      displayName: "frontend",
      testMatch: ["<rootDir>/src/frontend/**/*.test.js"],
      setupFiles: ["<rootDir>/setup-frontend.js"],
      testEnvironment: "jsdom",
    },
    {
      displayName: "backend",
      testMatch: ["<rootDir>/src/backend/**/*.test.js"],
      setupFiles: ["<rootDir>/setup-backend.js"],
      testEnvironment: "node",
    },
  ],
};
```

### Configuração Condicional

```javascript
// Configuração baseada em ambiente
const config = {
  // Configuração base...
};

// Adicionar configurações específicas por ambiente
if (process.env.CI) {
  // Configuração CI
  config.bail = true;
  config.verbose = false;
}

module.exports = config;
```

## 14. Práticas Recomendadas

### Organização de Testes

- Mantenha os testes próximos ao código que estão testando
- Use convenções de nomenclatura consistentes (`.test.js` ou `.spec.js`)
- Organize os testes em uma estrutura semelhante ao código fonte

```
src/
├── components/
│   ├── Button.js
│   └── Button.test.js
```

ou

```
src/
├── components/
│   ├── Button.js
└── __tests__/
    └── Button.test.js
```

### Isolamento de Testes

- Evite testes interdependentes
- Restaure os mocks após cada teste
- Use `beforeEach` e `afterEach` para configurar um estado limpo

```javascript
beforeEach(() => {
  // Configurar estado limpo
  localStorage.clear();
  jest.resetAllMocks();
});
```

### Testes Significativos

- Nomeie seus testes claramente descrevendo o comportamento esperado
- Evite asserções múltiplas em um único teste
- Divida em testes menores e específicos:

```javascript
// Evite
test("login funciona", () => {
  // Muitas asserções diferentes...
});

// Prefira
describe("Login", () => {
  test("exibe erro quando usuário não existe", () => {});
  test("exibe erro quando senha está incorreta", () => {});
  test("redireciona para dashboard após login bem-sucedido", () => {});
});
```

### Mocking Eficiente

- Evite mockar tudo - tente testar funcionalidades reais quando possível
- Use spies para verificar interações sem alterar implementação
- Prefira mocks manuais a automáticos para melhor controle

### Cobertura de Código

- Tenha um objetivo razoável (70-80% é frequentemente suficiente)
- Foque na cobertura de caminhos lógicos, não apenas linhas
- Use exceções para código que não precisa de teste (como entradas visuais)

## 15. Recursos Adicionais

### Documentação Oficial

- [Site oficial do Jest](https://jestjs.io/)
- [Documentação do Jest](https://jestjs.io/docs/getting-started)

### Extensões e Ferramentas

- `jest-extended`: Matchers adicionais
- `jest-dom`: Matchers específicos para DOM
- `jest-fetch-mock`: Mock para fetch API
- `jest-axe`: Testes de acessibilidade
- `jest-image-snapshot`: Testes de snapshot visual

### Dicas de Performance

- Use Jest em modo watch durante o desenvolvimento
- Execute testes relevantes primeiro com `--findRelatedTests`
- Utilize `--maxWorkers` para controlar o paralelismo
- Cache os pacotes de transformação com `--cache`

### Estrutura de Teste Recomendada

```javascript
// Estrutura recomendada para testes
describe('Componente/Módulo', () => {
  // Setup e teardown global
  let mockData;

  beforeEach(() => {
    mockData = {...};
  });

  describe('funcionalidade específica', () => {
    // Setup para esta funcionalidade específica
    beforeEach(() => {
      // ...
    });

    test('comportamento esperado 1', () => {
      // Configurar
      const input = ...;

      // Executar
      const result = funcao(input);

      // Verificar
      expect(result).toBe(expected);
    });

    test('comportamento esperado 2', () => {
      // ...
    });
  });

  describe('outra funcionalidade', () => {
    // ...
  });
});
```

### Integração com CI/CD

- **GitHub Actions**: Configure o Jest para executar em pipelines GitHub com:

  ```yaml
  name: Tests
  on: [push, pull_request]
  jobs:
    test:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v3
        - name: Setup Node.js
          uses: actions/setup-node@v3
          with:
            node-version: "18"
            cache: "npm"
        - run: npm ci
        - run: npm test
        - name: Upload coverage
          uses: codecov/codecov-action@v3
          with:
            token: ${{ secrets.CODECOV_TOKEN }}
  ```

- **GitLab CI**: Exemplo de configuração para GitLab CI/CD:

  ```yaml
  image: node:18

  cache:
    paths:
      - node_modules/

  stages:
    - test

  jest:
    stage: test
    script:
      - npm ci
      - npm test -- --ci --coverage
    artifacts:
      paths:
        - coverage/
  ```

- **Jenkins**: Integre com Jenkins utilizando o Jenkinsfile:

  ```groovy
  pipeline {
    agent {
      docker {
        image 'node:18'
      }
    }
    stages {
      stage('Setup') {
        steps {
          sh 'npm ci'
        }
      }
      stage('Test') {
        steps {
          sh 'npm test -- --ci --coverage'
        }
        post {
          always {
            junit 'junit.xml'
            publishHTML target: [
              allowMissing: false,
              alwaysLinkToLastBuild: true,
              keepAll: true,
              reportDir: 'coverage/lcov-report',
              reportFiles: 'index.html',
              reportName: 'Jest Coverage Report'
            ]
          }
        }
      }
    }
  }
  ```

- **Travis CI**: Configuração para Travis CI:

  ```yaml
  language: node_js
  node_js:
    - 18
  cache:
    directories:
      - node_modules
  script:
    - npm test -- --coverage
  after_success:
    - npx codecov
  ```

- **CircleCI**: Exemplo de configuração:

  ```yaml
  version: 2.1
  jobs:
    test:
      docker:
        - image: cimg/node:18.0
      steps:
        - checkout
        - restore_cache:
            keys:
              - v1-dependencies-{{ checksum "package.json" }}
              - v1-dependencies-
        - run: npm ci
        - save_cache:
            paths:
              - node_modules
            key: v1-dependencies-{{ checksum "package.json" }}
        - run: npm test -- --ci --coverage
        - store_artifacts:
            path: coverage
  workflows:
    version: 2
    build_and_test:
      jobs:
        - test
  ```

- **Badges de Status**: Adicione badges de status de testes ao seu README:

  ```markdown
  [![Tests](https://github.com/username/repo/actions/workflows/tests.yml/badge.svg)](https://github.com/username/repo/actions)
  [![codecov](https://codecov.io/gh/username/repo/branch/main/graph/badge.svg)](https://codecov.io/gh/username/repo)
  ```

- **Integrações de Cobertura**: Serviços como Codecov, Coveralls ou SonarQube para visualizar e monitorar a cobertura de testes ao longo do tempo.

- **Pull Request Checks**: Configure verificações automáticas para garantir que todos os testes passem antes de permitir merge de PRs.

## 16. Testes no NestJS

### 16.1 Introdução aos Testes no NestJS

O NestJS inclui suporte nativo para testes com Jest, facilitando a criação de testes unitários, de integração e end-to-end. O framework oferece uma estrutura de teste robusta que se integra perfeitamente com o ecossistema NestJS.

### 16.2 Tipos de Testes no NestJS

#### 16.2.1 Testes Unitários

Testes unitários no NestJS focam em componentes individuais como serviços, controladores e pipes. Eles verificam se cada unidade funciona corretamente de forma isolada.

#### 16.2.2 Testes de Integração

Esses testes verificam as interações entre módulos e componentes, validando se diferentes partes do sistema funcionam corretamente juntas.

#### 16.2.3 Testes End-to-End (E2E)

Testes E2E simulam requisições reais à API e verificam todo o fluxo de execução da aplicação.

### 16.3 Configuração de Testes no NestJS

O CLI do NestJS já gera um ambiente de teste configurado. A estrutura de projeto padrão inclui:

- `jest.config.js` - Configuração principal do Jest
- `test/` - Diretório para testes E2E
- Arquivos `.spec.ts` - Arquivos de teste unitários localizados junto ao código fonte

Exemplo de `jest.config.js` para NestJS:

```javascript
module.exports = {
  moduleFileExtensions: ["js", "json", "ts"],
  rootDir: "src",
  testRegex: ".*\\.spec\\.ts$",
  transform: {
    "^.+\\.(t|j)s$": "ts-jest",
  },
  collectCoverageFrom: ["**/*.(t|j)s"],
  coverageDirectory: "../coverage",
  testEnvironment: "node",
};
```

### 16.4 Testando Controladores

#### 16.4.1 Testes Unitários de Controladores

```typescript
import { Test, TestingModule } from "@nestjs/testing";
import { UsersController } from "./users.controller";
import { UsersService } from "./users.service";

describe("UsersController", () => {
  let controller: UsersController;
  let service: UsersService;

  beforeEach(async () => {
    const mockUsersService = {
      findAll: jest.fn().mockResolvedValue([
        { id: 1, name: "John" },
        { id: 2, name: "Jane" },
      ]),
      findOne: jest.fn().mockImplementation((id) => Promise.resolve({ id, name: `User ${id}` })),
    };

    const module: TestingModule = await Test.createTestingModule({
      controllers: [UsersController],
      providers: [
        {
          provide: UsersService,
          useValue: mockUsersService,
        },
      ],
    }).compile();

    controller = module.get<UsersController>(UsersController);
    service = module.get<UsersService>(UsersService);
  });

  it("should be defined", () => {
    expect(controller).toBeDefined();
  });

  it("should get all users", async () => {
    const result = await controller.findAll();
    expect(result).toHaveLength(2);
    expect(service.findAll).toHaveBeenCalled();
  });

  it("should get user by id", async () => {
    const result = await controller.findOne("1");
    expect(result.id).toBe("1");
    expect(service.findOne).toHaveBeenCalledWith("1");
  });
});
```

### 16.5 Testando Serviços

#### 16.5.1 Testes Unitários de Serviços

```typescript
import { Test, TestingModule } from "@nestjs/testing";
import { UsersService } from "./users.service";
import { getRepositoryToken } from "@nestjs/typeorm";
import { User } from "./entities/user.entity";

describe("UsersService", () => {
  let service: UsersService;
  let mockRepository: any;

  beforeEach(async () => {
    mockRepository = {
      find: jest.fn().mockResolvedValue([
        { id: 1, name: "John" },
        { id: 2, name: "Jane" },
      ]),
      findOne: jest
        .fn()
        .mockImplementation((conditions) =>
          Promise.resolve({ id: conditions.where.id, name: `User ${conditions.where.id}` })
        ),
      save: jest.fn().mockImplementation((user) => Promise.resolve({ id: Date.now(), ...user })),
      delete: jest.fn().mockResolvedValue({ affected: 1 }),
    };

    const module: TestingModule = await Test.createTestingModule({
      providers: [
        UsersService,
        {
          provide: getRepositoryToken(User),
          useValue: mockRepository,
        },
      ],
    }).compile();

    service = module.get<UsersService>(UsersService);
  });

  it("should be defined", () => {
    expect(service).toBeDefined();
  });

  it("should find all users", async () => {
    const users = await service.findAll();
    expect(users).toHaveLength(2);
    expect(mockRepository.find).toHaveBeenCalled();
  });

  it("should create a user", async () => {
    const newUser = { name: "New User", email: "new@example.com" };
    await service.create(newUser);
    expect(mockRepository.save).toHaveBeenCalledWith(newUser);
  });
});
```

### 16.6 Testando Módulos Completos

```typescript
import { Test, TestingModule } from "@nestjs/testing";
import { UsersModule } from "./users.module";
import { UsersService } from "./users.service";
import { getRepositoryToken } from "@nestjs/typeorm";
import { User } from "./entities/user.entity";

describe("UsersModule", () => {
  let module: TestingModule;
  let service: UsersService;

  beforeEach(async () => {
    module = await Test.createTestingModule({
      imports: [UsersModule],
    })
      .overrideProvider(getRepositoryToken(User))
      .useValue({
        find: jest.fn().mockResolvedValue([]),
        findOne: jest.fn().mockResolvedValue(null),
        save: jest.fn().mockResolvedValue({}),
      })
      .compile();

    service = module.get<UsersService>(UsersService);
  });

  it("should be defined", () => {
    expect(module).toBeDefined();
    expect(service).toBeDefined();
  });
});
```

### 16.7 Testes E2E

Os testes E2E no NestJS são armazenados na pasta `test/` por padrão. Eles iniciam uma instância da aplicação e fazem requisições HTTP.

```typescript
// test/users.e2e-spec.ts
import { Test, TestingModule } from "@nestjs/testing";
import { INestApplication } from "@nestjs/common";
import * as request from "supertest";
import { AppModule } from "../src/app.module";

describe("UsersController (e2e)", () => {
  let app: INestApplication;

  beforeEach(async () => {
    const moduleFixture: TestingModule = await Test.createTestingModule({
      imports: [AppModule],
    }).compile();

    app = moduleFixture.createNestApplication();
    await app.init();
  });

  afterAll(async () => {
    await app.close();
  });

  it("/users (GET)", () => {
    return request(app.getHttpServer())
      .get("/users")
      .expect(200)
      .expect((res) => {
        expect(Array.isArray(res.body)).toBeTruthy();
      });
  });

  it("/users/:id (GET)", () => {
    return request(app.getHttpServer())
      .get("/users/1")
      .expect(200)
      .expect((res) => {
        expect(res.body).toHaveProperty("id");
        expect(res.body).toHaveProperty("name");
      });
  });

  it("/users (POST)", () => {
    return request(app.getHttpServer())
      .post("/users")
      .send({ name: "Test User", email: "test@example.com" })
      .expect(201)
      .expect((res) => {
        expect(res.body).toHaveProperty("id");
      });
  });
});
```

### 16.8 Testando Guards, Pipes e Interceptors

#### 16.8.1 Testando Guards

```typescript
import { Test, TestingModule } from "@nestjs/testing";
import { AuthGuard } from "./auth.guard";
import { ExecutionContext } from "@nestjs/common";
import { JwtService } from "@nestjs/jwt";

describe("AuthGuard", () => {
  let guard: AuthGuard;
  let jwtService: JwtService;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        AuthGuard,
        {
          provide: JwtService,
          useValue: {
            verify: jest.fn(),
          },
        },
      ],
    }).compile();

    guard = module.get<AuthGuard>(AuthGuard);
    jwtService = module.get<JwtService>(JwtService);
  });

  it("should be defined", () => {
    expect(guard).toBeDefined();
  });

  it("should allow access with valid token", () => {
    const mockContext = {
      switchToHttp: () => ({
        getRequest: () => ({
          headers: {
            authorization: "Bearer valid-token",
          },
        }),
      }),
    } as unknown as ExecutionContext;

    jest.spyOn(jwtService, "verify").mockReturnValue({ userId: 1 });

    expect(guard.canActivate(mockContext)).toBeTruthy();
    expect(jwtService.verify).toHaveBeenCalledWith("valid-token");
  });
});
```

#### 16.8.2 Testando Pipes

```typescript
import { ValidationPipe } from "@nestjs/common";
import { Test } from "@nestjs/testing";
import { CreateUserDto } from "./dto/create-user.dto";

describe("ValidationPipe", () => {
  let validationPipe: ValidationPipe;

  beforeEach(async () => {
    validationPipe = new ValidationPipe({
      transform: true,
      whitelist: true,
    });
  });

  it("should validate and transform DTO", async () => {
    const dto = { name: "Test User", email: "test@example.com", age: "30" };
    const result = await validationPipe.transform(dto, {
      type: "body",
      metatype: CreateUserDto,
    } as any);

    expect(result).toBeInstanceOf(CreateUserDto);
    expect(result.age).toBe(30); // transformed to number
  });

  it("should throw error with invalid data", async () => {
    const dto = { name: "", email: "invalid-email" };

    await expect(
      validationPipe.transform(dto, {
        type: "body",
        metatype: CreateUserDto,
      } as any)
    ).rejects.toThrow();
  });
});
```

### 16.9 Usando Test Databases

Para testes de integração com banco de dados real:

```typescript
import { Test, TestingModule } from "@nestjs/testing";
import { TypeOrmModule } from "@nestjs/typeorm";
import { UsersModule } from "./users.module";
import { User } from "./entities/user.entity";
import { UsersService } from "./users.service";

describe("UsersService Integration", () => {
  let service: UsersService;
  let module: TestingModule;

  beforeAll(async () => {
    module = await Test.createTestingModule({
      imports: [
        TypeOrmModule.forRoot({
          type: "sqlite",
          database: ":memory:",
          entities: [User],
          synchronize: true,
        }),
        UsersModule,
      ],
    }).compile();

    service = module.get<UsersService>(UsersService);
  });

  afterAll(async () => {
    await module.close();
  });

  it("should create and retrieve a user", async () => {
    const userData = { name: "Test User", email: "test@example.com" };

    const createdUser = await service.create(userData);
    expect(createdUser).toHaveProperty("id");

    const foundUser = await service.findOne(createdUser.id);
    expect(foundUser).toMatchObject(userData);
  });
});
```

### 16.10 Usando Mocks Automáticos

O NestJS permite criar mocks automáticos para facilitar testes:

```typescript
import { Test } from "@nestjs/testing";
import { UsersController } from "./users.controller";
import { UsersService } from "./users.service";

describe("UsersController with auto-mocks", () => {
  let controller: UsersController;
  let service: UsersService;

  beforeEach(async () => {
    const moduleRef = await Test.createTestingModule({
      controllers: [UsersController],
      providers: [UsersService],
    })
      .useMocker((token) => {
        if (token === UsersService) {
          return {
            findAll: jest.fn().mockResolvedValue([
              { id: 1, name: "John" },
              { id: 2, name: "Jane" },
            ]),
            findOne: jest.fn().mockImplementation((id) => Promise.resolve({ id, name: `User ${id}` })),
          };
        }
      })
      .compile();

    controller = moduleRef.get<UsersController>(UsersController);
    service = moduleRef.get<UsersService>(UsersService);
  });

  it("should get all users", async () => {
    const users = await controller.findAll();
    expect(users).toHaveLength(2);
  });
});
```

### 16.11 Dicas e Práticas Recomendadas

1. **Organização de Testes**: Mantenha testes unitários próximos ao código fonte (`*.spec.ts`) e testes E2E em um diretório separado (`/test`).

2. **Mock de Dependências**: Use `useValue` ou `useFactory` para fornecer mocks consistentes para suas dependências.

3. **Configurações de Teste**: Crie um módulo de teste para configurações compartilhadas entre múltiplos testes.

4. **Cache de Compilação**: Use `.compile()` apenas uma vez no nível do `describe` para melhorar o desempenho dos testes.

5. **Isolamento de Banco de Dados**: Utilize bancos em memória como SQLite para testes integrados mais rápidos.

6. **Reset de Estado**: Limpe o estado do banco de dados entre os testes para evitar interferências.

7. **Coverage**: Execute testes com `--coverage` para identificar áreas não testadas.

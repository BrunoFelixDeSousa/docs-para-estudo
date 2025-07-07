# [⬅ Voltar para o índice principal](../../../README.md)

# Mini Documentação React com TypeScript

## Sumário

1. [Introdução ao React com TypeScript](#introdução-ao-react-com-typescript)
2. [Configuração Inicial](#configuração-inicial)
3. [Componentes](#componentes)
   - [Componentes Funcionais](#componentes-funcionais)
   - [Props](#props)
   - [Componentes de Classe (legados)](#componentes-de-classe-legados)
4. [Hooks Básicos](#hooks-básicos)
   - [useState](#usestate)
   - [useEffect](#useeffect)
   - [useContext](#usecontext)
   - [useRef](#useref)
5. [Hooks Adicionais](#hooks-adicionais)
   - [useReducer](#usereducer)
   - [useMemo](#usememo)
   - [useCallback](#usecallback)
   - [useLayoutEffect](#uselayouteffect)
   - [useImperativeHandle](#useimperativehandle)
   - [useId](#useid)
   - [useSyncExternalStore](#usesyncexternalstore)
   - [useTransition](#usetransition)
   - [useDeferredValue](#usedeferredvalue)
6. [Criando Hooks Personalizados](#criando-hooks-personalizados)
7. [Context API](#context-api)
8. [Renderização Condicional e Listas](#renderização-condicional-e-listas)
9. [Manipulação de Eventos](#manipulação-de-eventos)
10. [Formulários](#formulários)
11. [Componentes Estilizados](#componentes-estilizados)
12. [Roteamento com React Router](#roteamento-com-react-router)
13. [Estado Global com Redux](#estado-global-com-redux)
14. [APIs de Integração](#apis-de-integração)
    - [React Suspense](#react-suspense)
    - [Error Boundaries](#error-boundaries)
    - [Lazy Loading](#lazy-loading)
    - [Server Components](#server-components)
15. [Patterns Avançados](#patterns-avançados)
    - [Render Props](#render-props)
    - [High Order Components (HOCs)](#high-order-components-hocs)
    - [Compound Components](#compound-components)
    - [Component Composition](#component-composition)
16. [Otimização de Performance](#otimização-de-performance)
    - [React.memo](#reactmemo)
    - [Virtualização](#virtualização)
    - [Code Splitting](#code-splitting)
17. [Testes](#testes)
    - [Jest e React Testing Library](#jest-e-react-testing-library)
    - [Componente de Teste](#componente-de-teste)

## Introdução ao React com TypeScript

O React é uma biblioteca JavaScript para construção de interfaces de usuário. O TypeScript adiciona tipagem estática ao JavaScript, proporcionando melhor documentação, autocomplete e detecção de erros em tempo de desenvolvimento.

### Vantagens de usar TypeScript com React:

- Autocomplete e IntelliSense aprimorados
- Detecção de erros em tempo de compilação
- Melhor documentação de código
- Props e estados tipados
- Refatoração mais segura

## Configuração Inicial

### Criando um projeto React com TypeScript:

```bash
# Usando Create React App
npx create-react-app meu-app-react --template typescript

# Usando Vite (mais rápido)
npm create vite@latest meu-app-react -- --template react-ts
```

### Estrutura de arquivos básica:

```
/src
  /components
  /hooks
  /contexts
  /types
  /utils
  App.tsx
  index.tsx
```

### Configurações do tsconfig.json:

```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx"
  },
  "include": ["src"]
}
```

## Componentes

### Componentes Funcionais

Componentes funcionais são a forma moderna e recomendada de criar componentes React.

#### Componente Básico:

```tsx
import React from "react";

const Greeting: React.FC = () => {
  return <h1>Olá, mundo!</h1>;
};

export default Greeting;
```

### Props

Props são as propriedades passadas para um componente.

#### Declarando tipos para props:

```tsx
import React from "react";

// Usando interface (recomendado para props)
interface UserCardProps {
  name: string;
  age: number;
  email?: string; // Prop opcional
  onProfileClick: (id: string) => void;
}

// Componente com props tipadas
const UserCard: React.FC<UserCardProps> = ({ name, age, email, onProfileClick }) => {
  return (
    <div onClick={() => onProfileClick("123")}>
      <h2>{name}</h2>
      <p>Idade: {age}</p>
      {email && <p>Email: {email}</p>}
    </div>
  );
};

// Uso do componente
const App: React.FC = () => {
  const handleClick = (id: string) => {
    console.log(`Perfil ${id} clicado`);
  };

  return <UserCard name="João Silva" age={30} email="joao@exemplo.com" onProfileClick={handleClick} />;
};
```

#### Props com children:

```tsx
interface CardProps {
  title: string;
  children: React.ReactNode;
}

const Card: React.FC<CardProps> = ({ title, children }) => {
  return (
    <div className="card">
      <h3>{title}</h3>
      <div className="card-content">{children}</div>
    </div>
  );
};

// Uso
<Card title="Meu Card">
  <p>Este é o conteúdo do card</p>
</Card>;
```

### Componentes de Classe (legados)

Embora os componentes funcionais sejam preferidos, componentes de classe ainda são suportados.

```tsx
import React, { Component } from "react";

interface CounterProps {
  initialValue: number;
}

interface CounterState {
  count: number;
}

class Counter extends Component<CounterProps, CounterState> {
  constructor(props: CounterProps) {
    super(props);
    this.state = {
      count: props.initialValue,
    };
  }

  increment = (): void => {
    this.setState((prevState) => ({
      count: prevState.count + 1,
    }));
  };

  render() {
    return (
      <div>
        <p>Contagem: {this.state.count}</p>
        <button onClick={this.increment}>Incrementar</button>
      </div>
    );
  }
}

// Uso
<Counter initialValue={0} />;
```

## Hooks Básicos

### useState

O useState gerencia o estado em componentes funcionais.

```tsx
import React, { useState } from "react";

interface User {
  name: string;
  age: number;
}

const UserProfile: React.FC = () => {
  // Estado primitivo
  const [isLoggedIn, setIsLoggedIn] = useState<boolean>(false);

  // Estado com objeto tipado
  const [user, setUser] = useState<User>({ name: "João", age: 30 });

  // Estado com tipo union
  const [status, setStatus] = useState<"idle" | "loading" | "success" | "error">("idle");

  // Estado possivelmente undefined
  const [data, setData] = useState<string[] | undefined>(undefined);

  const updateName = () => {
    // Atualização imutável do objeto
    setUser((prevUser) => ({ ...prevUser, name: "Maria" }));
  };

  return (
    <div>
      <p>Nome: {user.name}</p>
      <p>Idade: {user.age}</p>
      <p>Status: {status}</p>
      <button onClick={updateName}>Atualizar Nome</button>
      <button onClick={() => setStatus("loading")}>Carregar</button>
    </div>
  );
};
```

### useEffect

O useEffect gerencia efeitos colaterais como busca de dados, inscrições e modificações manuais do DOM.

```tsx
import React, { useState, useEffect } from "react";

interface Post {
  id: number;
  title: string;
  body: string;
}

const PostList: React.FC = () => {
  const [posts, setPosts] = useState<Post[]>([]);
  const [isLoading, setIsLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);

  // Efeito executado na montagem do componente
  useEffect(() => {
    const fetchPosts = async () => {
      try {
        setIsLoading(true);
        const response = await fetch("https://jsonplaceholder.typicode.com/posts");

        if (!response.ok) {
          throw new Error("Falha ao buscar posts");
        }

        const data: Post[] = await response.json();
        setPosts(data.slice(0, 5));
        setError(null);
      } catch (err) {
        setError(err instanceof Error ? err.message : "Erro desconhecido");
      } finally {
        setIsLoading(false);
      }
    };

    fetchPosts();

    // Função de limpeza (executada na desmontagem)
    return () => {
      console.log("Componente desmontado, limpeza executada");
    };
  }, []); // Array de dependências vazio = executado apenas na montagem

  // Efeito que depende de uma variável
  useEffect(() => {
    document.title = `${posts.length} posts carregados`;
  }, [posts]); // Executado quando posts muda

  if (isLoading) return <div>Carregando...</div>;
  if (error) return <div>Erro: {error}</div>;

  return (
    <div>
      <h1>Posts</h1>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>
            <h3>{post.title}</h3>
            <p>{post.body}</p>
          </li>
        ))}
      </ul>
    </div>
  );
};
```

### useContext

O useContext permite acessar dados do contexto sem componentes consumidores aninhados.

```tsx
// Primeiro, criamos um contexto tipado (em um arquivo separado)
// src/contexts/ThemeContext.tsx
import React, { createContext, useState, useContext } from "react";

interface ThemeContextType {
  isDarkMode: boolean;
  toggleTheme: () => void;
}

// Valor padrão do contexto
const defaultContext: ThemeContextType = {
  isDarkMode: false,
  toggleTheme: () => {},
};

export const ThemeContext = createContext<ThemeContextType>(defaultContext);

// Provider Component
interface ThemeProviderProps {
  children: React.ReactNode;
}

export const ThemeProvider: React.FC<ThemeProviderProps> = ({ children }) => {
  const [isDarkMode, setIsDarkMode] = useState<boolean>(false);

  const toggleTheme = () => {
    setIsDarkMode((prev) => !prev);
  };

  const value: ThemeContextType = {
    isDarkMode,
    toggleTheme,
  };

  return <ThemeContext.Provider value={value}>{children}</ThemeContext.Provider>;
};

// Hook personalizado para facilitar o uso do contexto
export const useTheme = () => useContext(ThemeContext);

// No componente que usa o contexto:
// src/components/ThemedButton.tsx
import React from "react";
import { useTheme } from "../contexts/ThemeContext";

const ThemedButton: React.FC = () => {
  const { isDarkMode, toggleTheme } = useTheme();

  return (
    <button
      style={{
        backgroundColor: isDarkMode ? "#333" : "#fff",
        color: isDarkMode ? "#fff" : "#333",
        padding: "10px",
        border: "1px solid #ccc",
      }}
      onClick={toggleTheme}
    >
      Mudar para modo {isDarkMode ? "claro" : "escuro"}
    </button>
  );
};

// No App.tsx, envolvemos nossa árvore com o provider
import React from "react";
import { ThemeProvider } from "./contexts/ThemeContext";
import ThemedButton from "./components/ThemedButton";

const App: React.FC = () => {
  return (
    <ThemeProvider>
      <div>
        <h1>App com Tema</h1>
        <ThemedButton />
      </div>
    </ThemeProvider>
  );
};
```

### useRef

O useRef cria uma referência mutável que persiste entre renderizações.

```tsx
import React, { useRef, useEffect, useState } from "react";

const TextInputWithFocus: React.FC = () => {
  // Ref para elemento DOM tipado
  const inputRef = useRef<HTMLInputElement>(null);

  // Ref para armazenar um valor que não causa re-renderização quando alterado
  const renderCountRef = useRef<number>(0);

  const [text, setText] = useState<string>("");

  // Foca o input na montagem
  useEffect(() => {
    // current pode ser null, então verificamos
    if (inputRef.current) {
      inputRef.current.focus();
    }
  }, []);

  // Conta renderizações sem causar loop
  useEffect(() => {
    renderCountRef.current += 1;
    console.log(`Componente renderizado ${renderCountRef.current} vezes`);
  });

  return (
    <div>
      <input ref={inputRef} value={text} onChange={(e) => setText(e.target.value)} placeholder="Digite algo..." />
      <button onClick={() => inputRef.current?.focus()}>Focar no Input</button>
      <p>Texto atual: {text}</p>
      <p>Renderizações: {renderCountRef.current}</p>
    </div>
  );
};
```

## Hooks Adicionais

### useReducer

O useReducer é uma alternativa ao useState para lógica de estado mais complexa.

```tsx
import React, { useReducer } from "react";

// Definição dos tipos
type TodoType = {
  id: number;
  text: string;
  completed: boolean;
};

type TodoState = {
  todos: TodoType[];
  nextId: number;
};

// Discriminated union para actions
type TodoAction =
  | { type: "ADD_TODO"; payload: string }
  | { type: "TOGGLE_TODO"; payload: number }
  | { type: "REMOVE_TODO"; payload: number }
  | { type: "CLEAR_COMPLETED" };

// Estado inicial
const initialState: TodoState = {
  todos: [],
  nextId: 1,
};

// Reducer function
const todoReducer = (state: TodoState, action: TodoAction): TodoState => {
  switch (action.type) {
    case "ADD_TODO":
      return {
        ...state,
        todos: [...state.todos, { id: state.nextId, text: action.payload, completed: false }],
        nextId: state.nextId + 1,
      };
    case "TOGGLE_TODO":
      return {
        ...state,
        todos: state.todos.map((todo) => (todo.id === action.payload ? { ...todo, completed: !todo.completed } : todo)),
      };
    case "REMOVE_TODO":
      return {
        ...state,
        todos: state.todos.filter((todo) => todo.id !== action.payload),
      };
    case "CLEAR_COMPLETED":
      return {
        ...state,
        todos: state.todos.filter((todo) => !todo.completed),
      };
    default:
      return state;
  }
};

// Componente usando useReducer
const TodoApp: React.FC = () => {
  const [state, dispatch] = useReducer(todoReducer, initialState);
  const [input, setInput] = React.useState("");

  const handleAddTodo = (e: React.FormEvent) => {
    e.preventDefault();
    if (input.trim()) {
      dispatch({ type: "ADD_TODO", payload: input });
      setInput("");
    }
  };

  return (
    <div>
      <h1>Todo List</h1>
      <form onSubmit={handleAddTodo}>
        <input value={input} onChange={(e) => setInput(e.target.value)} placeholder="Adicionar tarefa" />
        <button type="submit">Adicionar</button>
      </form>
      <ul>
        {state.todos.map((todo) => (
          <li key={todo.id} style={{ textDecoration: todo.completed ? "line-through" : "none" }}>
            <span onClick={() => dispatch({ type: "TOGGLE_TODO", payload: todo.id })}>{todo.text}</span>
            <button onClick={() => dispatch({ type: "REMOVE_TODO", payload: todo.id })}>X</button>
          </li>
        ))}
      </ul>
      <button onClick={() => dispatch({ type: "CLEAR_COMPLETED" })}>Limpar completados</button>
    </div>
  );
};
```

### useMemo

O useMemo memoriza valores computados para evitar recálculos desnecessários.

```tsx
import React, { useState, useMemo } from "react";

interface Product {
  id: number;
  name: string;
  price: number;
  category: string;
}

const ProductList: React.FC = () => {
  const [products, setProducts] = useState<Product[]>([
    { id: 1, name: "Laptop", price: 999.99, category: "Electronics" },
    { id: 2, name: "Headphones", price: 199.99, category: "Electronics" },
    { id: 3, name: "Shirt", price: 29.99, category: "Clothing" },
    { id: 4, name: "Jeans", price: 59.99, category: "Clothing" },
  ]);

  const [searchTerm, setSearchTerm] = useState<string>("");
  const [minPrice, setMinPrice] = useState<number>(0);

  // Valor computado pesado que só recalcula quando as dependências mudam
  const filteredProducts = useMemo(() => {
    console.log("Filtrando produtos..."); // Demonstra quando o cálculo acontece

    return products.filter(
      (product) => product.name.toLowerCase().includes(searchTerm.toLowerCase()) && product.price >= minPrice
    );
  }, [products, searchTerm, minPrice]); // Dependências do cálculo

  // Estatísticas computadas
  const stats = useMemo(() => {
    console.log("Calculando estatísticas...");

    const totalProducts = filteredProducts.length;
    const averagePrice = filteredProducts.reduce((sum, product) => sum + product.price, 0) / totalProducts || 0;
    const categories = [...new Set(filteredProducts.map((p) => p.category))];

    return { totalProducts, averagePrice, categories };
  }, [filteredProducts]);

  return (
    <div>
      <div>
        <input
          type="text"
          placeholder="Buscar produtos..."
          value={searchTerm}
          onChange={(e) => setSearchTerm(e.target.value)}
        />
        <input
          type="number"
          placeholder="Preço mínimo"
          value={minPrice}
          onChange={(e) => setMinPrice(Number(e.target.value))}
        />
      </div>

      <div>
        <p>Total de produtos: {stats.totalProducts}</p>
        <p>Preço médio: ${stats.averagePrice.toFixed(2)}</p>
        <p>Categorias: {stats.categories.join(", ")}</p>
      </div>

      <ul>
        {filteredProducts.map((product) => (
          <li key={product.id}>
            {product.name} - ${product.price} ({product.category})
          </li>
        ))}
      </ul>
    </div>
  );
};
```

### useCallback

O useCallback memoriza funções para evitar recriá-las em cada renderização.

```tsx
import React, { useState, useCallback } from "react";

// Componente filho que renderiza apenas quando necessário
interface ItemProps {
  name: string;
  onDelete: (name: string) => void;
}

// Este componente usa React.memo para evitar re-renderizações desnecessárias
const Item: React.FC<ItemProps> = React.memo(({ name, onDelete }) => {
  console.log(`Item ${name} renderizado`);

  return (
    <li>
      {name}
      <button onClick={() => onDelete(name)}>Deletar</button>
    </li>
  );
});

const ItemList: React.FC = () => {
  const [items, setItems] = useState<string[]>(["Item 1", "Item 2", "Item 3"]);
  const [newItemName, setNewItemName] = useState<string>("");
  const [count, setCount] = useState<number>(0); // Estado não relacionado para demonstração

  // Sem useCallback, esta função seria recriada a cada renderização
  // fazendo com que Item re-renderize desnecessariamente
  const handleDelete = useCallback((name: string) => {
    console.log(`Deletando ${name}`);
    setItems((currentItems) => currentItems.filter((item) => item !== name));
  }, []); // Dependências vazias, função permanece estável

  // Esta função depende de um estado, então incluímos na dependência
  const handleAddItem = useCallback(() => {
    if (newItemName.trim()) {
      setItems((curr) => [...curr, newItemName]);
      setNewItemName("");
    }
  }, [newItemName]);

  return (
    <div>
      <div>
        <input value={newItemName} onChange={(e) => setNewItemName(e.target.value)} placeholder="Nome do novo item" />
        <button onClick={handleAddItem}>Adicionar Item</button>
      </div>

      <button onClick={() => setCount((c) => c + 1)}>Contador: {count} (clique para forçar re-renderização)</button>

      <ul>
        {items.map((item) => (
          <Item key={item} name={item} onDelete={handleDelete} />
        ))}
      </ul>
    </div>
  );
};
```

### useLayoutEffect

Similar ao useEffect, mas dispara de forma síncrona após todas as mutações do DOM.

```tsx
import React, { useState, useRef, useLayoutEffect } from "react";

const TooltipComponent: React.FC = () => {
  const [tooltipVisible, setTooltipVisible] = useState<boolean>(false);
  const buttonRef = useRef<HTMLButtonElement>(null);
  const tooltipRef = useRef<HTMLDivElement>(null);

  // useLayoutEffect é executado sincronamente antes do navegador pintar
  // Útil para leitura/escrita de layout que precisa estar sincronizada
  useLayoutEffect(() => {
    if (!tooltipVisible || !buttonRef.current || !tooltipRef.current) return;

    // Posiciona o tooltip relativamente ao botão
    // Isso ocorre antes do navegador pintar, evitando flash visual
    const buttonRect = buttonRef.current.getBoundingClientRect();

    tooltipRef.current.style.position = "absolute";
    tooltipRef.current.style.top = `${buttonRect.bottom + 5}px`;
    tooltipRef.current.style.left = `${buttonRect.left}px`;
  }, [tooltipVisible]); // Re-executado quando a visibilidade muda

  return (
    <div style={{ padding: "100px" }}>
      <button
        ref={buttonRef}
        onMouseEnter={() => setTooltipVisible(true)}
        onMouseLeave={() => setTooltipVisible(false)}
      >
        Passe o mouse aqui
      </button>

      {tooltipVisible && (
        <div
          ref={tooltipRef}
          style={{
            backgroundColor: "black",
            color: "white",
            padding: "5px",
            borderRadius: "3px",
          }}
        >
          Este é um tooltip posicionado com useLayoutEffect
        </div>
      )}
    </div>
  );
};
```

### useImperativeHandle

Permite personalizar o que é exposto quando um componente é referenciado com `ref`.

```tsx
import React, { useRef, useImperativeHandle, forwardRef } from "react";

// Definindo o tipo para os métodos expostos pelo ref
interface CustomInputHandle {
  focus: () => void;
  clear: () => void;
  getValue: () => string;
}

interface CustomInputProps {
  placeholder?: string;
  defaultValue?: string;
}

// Componente com ref encaminhado e tipado
const CustomInput = forwardRef<CustomInputHandle, CustomInputProps>((props, ref) => {
  const { placeholder = "", defaultValue = "" } = props;
  const inputRef = useRef<HTMLInputElement>(null);
  const [value, setValue] = React.useState(defaultValue);

  // Expõe apenas os métodos desejados, não o elemento DOM inteiro
  useImperativeHandle(
    ref,
    () => ({
      // Método para focar o input
      focus: () => {
        inputRef.current?.focus();
      },
      // Método para limpar o input
      clear: () => {
        if (inputRef.current) {
          setValue("");
          inputRef.current.value = "";
        }
      },
      // Método para obter o valor atual
      getValue: () => {
        return value;
      },
    }),
    [value]
  ); // Dependência para atualizar quando o valor muda

  return (
    <input
      ref={inputRef}
      value={value}
      onChange={(e) => setValue(e.target.value)}
      placeholder={placeholder}
      style={{ padding: "8px", borderRadius: "4px", border: "1px solid #ccc" }}
    />
  );
});

// Componente pai que usa o CustomInput
const FormWithImperative: React.FC = () => {
  // Ref tipada para acessar os métodos expostos
  const inputRef = useRef<CustomInputHandle>(null);

  const handleFocusClick = () => {
    // Chama o método exposto pelo ref
    inputRef.current?.focus();
  };

  const handleClearClick = () => {
    inputRef.current?.clear();
  };

  const handleGetValueClick = () => {
    const value = inputRef.current?.getValue();
    alert(`Valor atual: ${value}`);
  };

  return (
    <div>
      <h3>Form com useImperativeHandle</h3>
      <div style={{ marginBottom: "10px" }}>
        <CustomInput ref={inputRef} placeholder="Digite algo..." defaultValue="Valor inicial" />
      </div>
      <div>
        <button onClick={handleFocusClick}>Focar</button>
        <button onClick={handleClearClick}>Limpar</button>
        <button onClick={handleGetValueClick}>Obter Valor</button>
      </div>
    </div>
  );
};
```

### useId

O useId gera IDs únicos e estáveis para elementos, útil para acessibilidade.

```tsx
import React, { useId } from "react";

interface FormFieldProps {
  label: string;
  type?: "text" | "email" | "password";
  required?: boolean;
}

const FormField: React.FC<FormFieldProps> = ({ label, type = "text", required = false }) => {
  // Gera um ID único para este campo no formulário
  const id = useId();

  // Criamos IDs relacionados para elementos associados
  const labelId = `${id}-label`;
  const inputId = `${id}-input`;
  const errorId = `${id}-error`;

  const [value, setValue] = React.useState<string>("");
  const [isTouched, setIsTouched] = React.useState<boolean>(false);

  const showError = required && isTouched && !value;

  return (
    <div style={{ marginBottom: "16px" }}>
      <label
        id={labelId}
        htmlFor={inputId}
        style={{
          display: "block",
          marginBottom: "4px",
          fontWeight: "bold",
        }}
      >
        {label} {required && <span style={{ color: "red" }}>*</span>}
      </label>

      <input
        id={inputId}
        type={type}
        value={value}
        onChange={(e) => setValue(e.target.value)}
        onBlur={() => setIsTouched(true)}
        aria-labelledby={labelId}
        aria-required={required}
        aria-invalid={showError}
        aria-describedby={showError ? errorId : undefined}
        style={{
          padding: "8px",
          borderRadius: "4px",
          border: showError ? "1px solid red" : "1px solid #ccc",
          width: "100%",
        }}
      />

      {showError && (
        <div
          id={errorId}
          style={{
            color: "red",
            fontSize: "0.8rem",
            marginTop: "4px",
          }}
        >
          Este campo é obrigatório
        </div>
      )}
    </div>
  );
};

// Uso em um formulário
const AccessibleForm: React.FC = () => {
  return (
    <form>
      <h2>Formulário com IDs acessíveis</h2>
      <FormField label="Nome" required />
      <FormField label="Email" type="email" required />
      <FormField label="Senha" type="password" required />
      <FormField label="Bio" />

      <button type="submit">Enviar</button>
    </form>
  );
};
```

### useSyncExternalStore

O useSyncExternalStore sincroniza o estado do React com uma fonte de dados externa.

```tsx
import React, { useSyncExternalStore } from "react";

// Definindo uma store externa simples
const createStore = <T,>(initialState: T) => {
  let state = initialState;
  const listeners = new Set<() => void>();

  const subscribe = (listener: () => void) => {
    listeners.add(listener);
    return () => listeners.delete(listener);
  };

  const getState = () => state;

  const setState = (newState: T) => {
    state = newState;
    listeners.forEach((listener) => listener());
  };

  return { subscribe, getState, setState };
};

// Criando uma store de tema
type Theme = "light" | "dark";
const themeStore = createStore<Theme>("light");

// Hook personalizado que usa useSyncExternalStore
const useThemeStore = () => {
  const theme = useSyncExternalStore(themeStore.subscribe, themeStore.getState);

  const toggleTheme = () => {
    themeStore.setState(theme === "light" ? "dark" : "light");
  };

  return { theme, toggleTheme };
};

// Componente que usa a store externa
const ThemeSwitcher: React.FC = () => {
  const { theme, toggleTheme } = useThemeStore();

  return (
    <div
      style={{
        padding: "20px",
        backgroundColor: theme === "light" ? "#ffffff" : "#333333",
        color: theme === "light" ? "#333333" : "#ffffff",
        transition: "all 0.3s ease",
      }}
    >
      <h2>Tema Atual: {theme}</h2>
      <button onClick={toggleTheme}>Mudar para {theme === "light" ? "escuro" : "claro"}</button>
    </div>
  );
};

// Outro componente que usa a mesma store
const ThemeInfo: React.FC = () => {
  const { theme } = useThemeStore();

  return (
    <div>
      <p>Tema configurado: {theme}</p>
    </div>
  );
};
```

### useTransition

O useTransition permite marcar atualizações de estado como não-urgentes, melhorando a responsividade da UI.

```tsx
import React, { useState, useTransition } from "react";

const SlowList: React.FC = () => {
  const [inputValue, setInputValue] = useState<string>("");
  const [items, setItems] = useState<string[]>([]);
  const [isPending, startTransition] = useTransition();

  // Função que cria uma lista grande
  const generateItems = (count: number, filter: string) => {
    const newItems: string[] = [];
    for (let i = 0; i < count; i++) {
      const item = `Item ${i + 1}`;
      if (item.toLowerCase().includes(filter.toLowerCase())) {
        newItems.push(item);
      }
    }
    return newItems;
  };

  const handleInputChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const value = e.target.value;

    // Atualiza o input imediatamente (alta prioridade)
    setInputValue(value);

    // Marca a atualização da lista como transição não-urgente
    startTransition(() => {
      // Operação pesada dentro de uma transição
      setItems(generateItems(10000, value));
    });
  };

  return (
    <div>
      <input
        type="text"
        value={inputValue}
        onChange={handleInputChange}
        placeholder="Filtre os itens..."
        style={{ padding: "8px", marginBottom: "16px" }}
      />

      {isPending ? (
        <p>Atualizando lista...</p>
      ) : (
        <div>
          <p>Mostrando {items.length} itens</p>
          <ul>
            {items.slice(0, 100).map((item, index) => (
              <li key={index}>{item}</li>
            ))}
            {items.length > 100 && <li>...mais {items.length - 100} itens</li>}
          </ul>
        </div>
      )}
    </div>
  );
};
```

### useDeferredValue

O useDeferredValue permite adiar a atualização de um valor menos crítico.

```tsx
import React, { useState, useDeferredValue } from "react";

interface ListProps {
  input: string;
}

// Componente de lista que simula uma renderização lenta
const SlowList: React.FC<ListProps> = ({ input }) => {
  // Simulando trabalho pesado em cada renderização
  const items = [];
  for (let i = 0; i < 2000; i++) {
    if (i === 0) {
      // Apenas para forçar algum trabalho de renderização
      console.log("Renderizando lista...");
    }
    if (input && i.toString().includes(input)) {
      items.push(<li key={i}>Item {i}</li>);
    }
  }

  return <ul>{items.length > 0 ? items : <li>Nenhum item encontrado</li>}</ul>;
};

// Componente principal usando useDeferredValue
const DeferredValueExample: React.FC = () => {
  const [input, setInput] = useState<string>("");

  // Atrasa a atualização do valor usado para renderizar a lista
  const deferredInput = useDeferredValue(input);

  // Verifica se o valor atual é diferente do valor adiado
  const isStale = input !== deferredInput;

  return (
    <div>
      <input
        value={input}
        onChange={(e) => setInput(e.target.value)}
        placeholder="Digite números para filtrar..."
        style={{ marginBottom: "16px" }}
      />

      <div style={{ opacity: isStale ? 0.5 : 1 }}>
        <p>Mostrando resultados para: {deferredInput || "todos"}</p>
        <SlowList input={deferredInput} />
      </div>
    </div>
  );
};
```

## Criando Hooks Personalizados

Hooks personalizados permitem extrair lógica de componentes em funções reutilizáveis.

```tsx
// src/hooks/useLocalStorage.ts
import { useState, useEffect } from "react";

// Hook personalizado para persistir estado no localStorage
export function useLocalStorage<T>(key: string, initialValue: T): [T, (value: T) => void] {
  // Função para obter o valor inicial
  const getStoredValue = (): T => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(`Erro ao recuperar ${key} do localStorage:`, error);
      return initialValue;
    }
  };

  // Inicializa o estado com o valor do localStorage ou o valor inicial
  const [storedValue, setStoredValue] = useState<T>(getStoredValue);

  // Função para atualizar o valor no estado e no localStorage
  const setValue = (value: T) => {
    try {
      // Permite que o valor seja uma função, como em useState
      const valueToStore = value instanceof Function ? value(storedValue) : value;

      // Salva no estado
      setStoredValue(valueToStore);

      // Salva no localStorage
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.error(`Erro ao salvar ${key} no localStorage:`, error);
    }
  };

  // Sincroniza o estado se o valor do localStorage mudar em outra janela/tab
  useEffect(() => {
    const handleStorageChange = (event: StorageEvent) => {
      if (event.key === key && event.newValue) {
        setStoredValue(JSON.parse(event.newValue));
      }
    };

    window.addEventListener("storage", handleStorageChange);

    return () => {
      window.removeEventListener("storage", handleStorageChange);
    };
  }, [key]);

  return [storedValue, setValue];
}

// src/hooks/useFetch.ts
import { useState, useEffect } from "react";

interface FetchState<T> {
  data: T | null;
  loading: boolean;
  error: Error | null;
}

export function useFetch<T>(url: string, options?: RequestInit): FetchState<T> {
  const [state, setState] = useState<FetchState<T>>({
    data: null,
    loading: true,
    error: null,
  });

  useEffect(() => {
    let isMounted = true;

    const fetchData = async () => {
      setState((prev) => ({ ...prev, loading: true }));

      try {
        const response = await fetch(url, options);

        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }

        const data = await response.json();

        if (isMounted) {
          setState({
            data,
            loading: false,
            error: null,
          });
        }
      } catch (error) {
        if (isMounted) {
          setState({
            data: null,
            loading: false,
            error: error instanceof Error ? error : new Error("Unknown error"),
          });
        }
      }
    };

    fetchData();

    return () => {
      isMounted = false;
    };
  }, [url, JSON.stringify(options)]);

  return state;
}

// src/hooks/useMediaQuery.ts
import { useState, useEffect } from "react";

export function useMediaQuery(query: string): boolean {
  // Inicializa o estado com o resultado da consulta de mídia
  const [matches, setMatches] = useState<boolean>(() => {
    // Verifica se estamos no lado do cliente
    if (typeof window !== "undefined") {
      return window.matchMedia(query).matches;
    }
    return false;
  });

  useEffect(() => {
    // Verifica se estamos no lado do cliente
    if (typeof window === "undefined") return;

    const mediaQuery = window.matchMedia(query);

    // Define o estado inicial
    setMatches(mediaQuery.matches);

    // Define o listener para atualizações
    const handler = (event: MediaQueryListEvent) => {
      setMatches(event.matches);
    };

    // Adiciona o listener
    mediaQuery.addEventListener("change", handler);

    // Cleanup
    return () => {
      mediaQuery.removeEventListener("change", handler);
    };
  }, [query]);

  return matches;
}

// Exemplo de uso dos hooks personalizados
import React from "react";
import { useLocalStorage } from "../hooks/useLocalStorage";
import { useFetch } from "../hooks/useFetch";
import { useMediaQuery } from "../hooks/useMediaQuery";

interface User {
  id: number;
  name: string;
  email: string;
}

const CustomHooksExample: React.FC = () => {
  // Usando useLocalStorage
  const [username, setUsername] = useLocalStorage<string>("username", "");

  // Usando useFetch
  const { data, loading, error } = useFetch<User[]>("https://jsonplaceholder.typicode.com/users");

  // Usando useMediaQuery
  const isMobile = useMediaQuery("(max-width: 768px)");

  return (
    <div>
      <h2>Exemplo de Hooks Personalizados</h2>

      <div>
        <h3>useLocalStorage</h3>
        <input
          value={username}
          onChange={(e) => setUsername(e.target.value)}
          placeholder="Digite seu nome de usuário"
        />
        <p>Valor salvo: {username || "Nenhum"}</p>
      </div>

      <div>
        <h3>useFetch</h3>
        {loading && <p>Carregando...</p>}
        {error && <p>Erro: {error.message}</p>}
        {data && (
          <ul>
            {data.slice(0, 3).map((user) => (
              <li key={user.id}>
                {user.name} - {user.email}
              </li>
            ))}
          </ul>
        )}
      </div>

      <div>
        <h3>useMediaQuery</h3>
        <p>Seu dispositivo é: {isMobile ? "Mobile" : "Desktop"}</p>
      </div>
    </div>
  );
};
```

## Context API

O Context API permite compartilhar dados entre componentes sem passá-los explicitamente como props.

```tsx
// src/contexts/AppContext.tsx
import React, { createContext, useContext, useState, useEffect } from "react";

// Definindo o tipo do contexto
interface AppContextType {
  user: User | null;
  isAuthenticated: boolean;
  login: (username: string, password: string) => Promise<boolean>;
  logout: () => void;
  theme: "light" | "dark";
  toggleTheme: () => void;
}

interface User {
  id: number;
  username: string;
  email: string;
}

// Criando o contexto com um valor padrão
const AppContext = createContext<AppContextType | undefined>(undefined);

// Hook personalizado para usar o contexto
export const useAppContext = () => {
  const context = useContext(AppContext);
  if (context === undefined) {
    throw new Error("useAppContext deve ser usado dentro de um AppProvider");
  }
  return context;
};

// Props do provider
interface AppProviderProps {
  children: React.ReactNode;
}

// Provider do contexto
export const AppProvider: React.FC<AppProviderProps> = ({ children }) => {
  const [user, setUser] = useState<User | null>(null);
  const [theme, setTheme] = useState<"light" | "dark">("light");

  // Carrega o usuário e o tema do localStorage na inicialização
  useEffect(() => {
    const storedUser = localStorage.getItem("user");
    const storedTheme = localStorage.getItem("theme");

    if (storedUser) {
      setUser(JSON.parse(storedUser));
    }

    if (storedTheme === "dark" || storedTheme === "light") {
      setTheme(storedTheme);
    }
  }, []);

  // Atualiza o localStorage quando o tema muda
  useEffect(() => {
    localStorage.setItem("theme", theme);
    // Aplica a classe de tema no documento
    document.documentElement.setAttribute("data-theme", theme);
  }, [theme]);

  // Função de login simulada
  const login = async (username: string, password: string): Promise<boolean> => {
    try {
      // Simulando uma chamada de API
      await new Promise((resolve) => setTimeout(resolve, 1000));

      // Usuário mockado para demonstração
      const mockUser: User = {
        id: 1,
        username,
        email: `${username}@example.com`,
      };

      setUser(mockUser);
      localStorage.setItem("user", JSON.stringify(mockUser));
      return true;
    } catch (error) {
      console.error("Erro ao fazer login:", error);
      return false;
    }
  };

  // Função de logout
  const logout = () => {
    setUser(null);
    localStorage.removeItem("user");
  };

  // Função para alternar o tema
  const toggleTheme = () => {
    setTheme((current) => (current === "light" ? "dark" : "light"));
  };

  // Montar o valor do contexto
  const contextValue: AppContextType = {
    user,
    isAuthenticated: !!user,
    login,
    logout,
    theme,
    toggleTheme,
  };

  return <AppContext.Provider value={contextValue}>{children}</AppContext.Provider>;
};

// Uso em componentes:
// src/components/Header.tsx
import React from "react";
import { useAppContext } from "../contexts/AppContext";

const Header: React.FC = () => {
  const { user, isAuthenticated, logout, theme, toggleTheme } = useAppContext();

  return (
    <header className={`header ${theme}`}>
      <h1>Meu App</h1>

      <div className="theme-toggle">
        <button onClick={toggleTheme}>{theme === "light" ? "🌙" : "☀️"}</button>
      </div>

      <div className="user-section">
        {isAuthenticated ? (
          <>
            <span>Olá, {user?.username}</span>
            <button onClick={logout}>Sair</button>
          </>
        ) : (
          <span>Não autenticado</span>
        )}
      </div>
    </header>
  );
};

// src/App.tsx
import React from "react";
import { AppProvider } from "./contexts/AppContext";
import Header from "./components/Header";
import MainContent from "./components/MainContent";
import Footer from "./components/Footer";

const App: React.FC = () => {
  return (
    <AppProvider>
      <div className="app">
        <Header />
        <MainContent />
        <Footer />
      </div>
    </AppProvider>
  );
};
```

## Renderização Condicional e Listas

```tsx
import React, { useState } from "react";

interface Task {
  id: number;
  title: string;
  completed: boolean;
}

const TaskManagement: React.FC = () => {
  const [tasks, setTasks] = useState<Task[]>([
    { id: 1, title: "Aprender React", completed: true },
    { id: 2, title: "Estudar TypeScript", completed: false },
    { id: 3, title: "Criar um projeto", completed: false },
  ]);

  const [filter, setFilter] = useState<"all" | "active" | "completed">("all");
  const [newTaskTitle, setNewTaskTitle] = useState<string>("");

  // Filtra as tarefas com base no filtro selecionado
  const filteredTasks = tasks.filter((task) => {
    if (filter === "active") return !task.completed;
    if (filter === "completed") return task.completed;
    return true; // 'all'
  });

  // Adiciona uma nova tarefa
  const addTask = (e: React.FormEvent) => {
    e.preventDefault();

    if (!newTaskTitle.trim()) return;

    const newTask: Task = {
      id: Date.now(),
      title: newTaskTitle,
      completed: false,
    };

    setTasks([...tasks, newTask]);
    setNewTaskTitle("");
  };

  // Alterna o estado da tarefa
  const toggleTask = (id: number) => {
    setTasks(tasks.map((task) => (task.id === id ? { ...task, completed: !task.completed } : task)));
  };

  // Remove uma tarefa
  const removeTask = (id: number) => {
    setTasks(tasks.filter((task) => task.id !== id));
  };

  return (
    <div className="task-manager">
      <h1>Gerenciador de Tarefas</h1>

      {/* Formulário para adicionar tarefas */}
      <form onSubmit={addTask}>
        <input
          type="text"
          value={newTaskTitle}
          onChange={(e) => setNewTaskTitle(e.target.value)}
          placeholder="Adicionar nova tarefa"
        />
        <button type="submit">Adicionar</button>
      </form>

      {/* Filtros */}
      <div className="filters">
        <button onClick={() => setFilter("all")} disabled={filter === "all"}>
          Todas
        </button>
        <button onClick={() => setFilter("active")} disabled={filter === "active"}>
          Ativas
        </button>
        <button onClick={() => setFilter("completed")} disabled={filter === "completed"}>
          Concluídas
        </button>
      </div>

      {/* Lista de tarefas */}
      {filteredTasks.length > 0 ? (
        <ul className="task-list">
          {filteredTasks.map((task) => (
            <li key={task.id} className={task.completed ? "completed" : ""}>
              <input type="checkbox" checked={task.completed} onChange={() => toggleTask(task.id)} />
              <span>{task.title}</span>
              <button onClick={() => removeTask(task.id)}>Remover</button>
            </li>
          ))}
        </ul>
      ) : (
        <p>Nenhuma tarefa encontrada.</p>
      )}

      {/* Estatísticas */}
      <div className="stats">
        <p>
          {tasks.filter((t) => !t.completed).length} tarefas pendentes, {tasks.filter((t) => t.completed).length}{" "}
          concluídas
        </p>

        {/* Renderização condicional baseada em uma condição booleana */}
        {tasks.some((t) => t.completed) && (
          <button onClick={() => setTasks(tasks.filter((t) => !t.completed))}>Limpar concluídas</button>
        )}
      </div>
    </div>
  );
};
```

## Manipulação de Eventos

```tsx
import React, { useState, useRef } from "react";

// Tipos personalizados para eventos
interface DragState {
  isDragging: boolean;
  origin: { x: number; y: number } | null;
  translation: { x: number; y: number };
}

const EventHandlingDemo: React.FC = () => {
  // Form state
  const [formData, setFormData] = useState({
    name: "",
    email: "",
    message: "",
  });

  // Drag state
  const [dragState, setDragState] = useState<DragState>({
    isDragging: false,
    origin: null,
    translation: { x: 0, y: 0 },
  });

  // Elementos DOM refs
  const boxRef = useRef<HTMLDivElement>(null);
  const fileInputRef = useRef<HTMLInputElement>(null);

  // Form event handlers
  const handleInputChange = (e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>) => {
    const { name, value } = e.target;
    setFormData({
      ...formData,
      [name]: value,
    });
  };

  const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    console.log("Form submitted:", formData);
    alert(`Formulário enviado!\nNome: ${formData.name}\nEmail: ${formData.email}\nMensagem: ${formData.message}`);
  };

  // File input handlers
  const handleFileButtonClick = () => {
    fileInputRef.current?.click();
  };

  const handleFileChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const files = e.target.files;
    if (files && files.length > 0) {
      alert(`Arquivo selecionado: ${files[0].name}`);
    }
  };

  // Mouse and keyboard handlers
  const handleMouseEnter = (e: React.MouseEvent<HTMLDivElement>) => {
    if (boxRef.current) {
      boxRef.current.style.backgroundColor = "#4a90e2";
    }
  };

  const handleMouseLeave = (e: React.MouseEvent<HTMLDivElement>) => {
    if (boxRef.current) {
      boxRef.current.style.backgroundColor = "#60a5fa";
    }
  };

  const handleKeyDown = (e: React.KeyboardEvent<HTMLInputElement>) => {
    if (e.key === "Enter") {
      alert("Enter pressionado!");
    }
  };

  // Drag handlers
  const handleMouseDown = (e: React.MouseEvent<HTMLDivElement>) => {
    if (boxRef.current) {
      setDragState({
        isDragging: true,
        origin: { x: e.clientX, y: e.clientY },
        translation: { x: parseInt(boxRef.current.style.left || "0"), y: parseInt(boxRef.current.style.top || "0") },
      });
    }
  };

  const handleMouseMove = (e: React.MouseEvent<HTMLDivElement>) => {
    if (dragState.isDragging && dragState.origin) {
      const deltaX = e.clientX - dragState.origin.x;
      const deltaY = e.clientY - dragState.origin.y;

      const newX = dragState.translation.x + deltaX;
      const newY = dragState.translation.y + deltaY;

      if (boxRef.current) {
        boxRef.current.style.left = `${newX}px`;
        boxRef.current.style.top = `${newY}px`;
      }
    }
  };

  const handleMouseUp = () => {
    if (dragState.isDragging) {
      setDragState({
        ...dragState,
        isDragging: false,
      });
    }
  };

  return (
    <div className="events-demo">
      <h1>Manipulação de Eventos</h1>

      {/* Formulário com vários eventos */}
      <section>
        <h2>Formulário</h2>
        <form onSubmit={handleSubmit}>
          <div>
            <label htmlFor="name">Nome:</label>
            <input
              type="text"
              id="name"
              name="name"
              value={formData.name}
              onChange={handleInputChange}
              onKeyDown={handleKeyDown}
              placeholder="Seu nome"
            />
          </div>

          <div>
            <label htmlFor="email">Email:</label>
            <input
              type="email"
              id="email"
              name="email"
              value={formData.email}
              onChange={handleInputChange}
              placeholder="seu@email.com"
            />
          </div>

          <div>
            <label htmlFor="message">Mensagem:</label>
            <textarea
              id="message"
              name="message"
              value={formData.message}
              onChange={handleInputChange}
              placeholder="Sua mensagem"
              rows={4}
            />
          </div>

          <button type="submit">Enviar</button>
        </form>
      </section>

      {/* Upload de arquivo customizado */}
      <section>
        <h2>Upload de Arquivo</h2>
        <input type="file" ref={fileInputRef} onChange={handleFileChange} style={{ display: "none" }} />
        <button onClick={handleFileButtonClick}>Selecionar Arquivo</button>
      </section>

      {/* Elemento arrastável */}
      <section>
        <h2>Elemento Arrastável</h2>
        <div
          ref={boxRef}
          style={{
            width: "100px",
            height: "100px",
            backgroundColor: "#60a5fa",
            position: "relative",
            cursor: "grab",
            userSelect: "none",
            display: "flex",
            alignItems: "center",
            justifyContent: "center",
            color: "white",
          }}
          onMouseEnter={handleMouseEnter}
          onMouseLeave={handleMouseLeave}
          onMouseDown={handleMouseDown}
          onMouseMove={handleMouseMove}
          onMouseUp={handleMouseUp}
        >
          Arraste-me
        </div>
      </section>
    </div>
  );
};
```

## Formulários

### Formulários Controlados

Em formulários controlados, os valores dos elementos são controlados pelo estado do React.

```tsx
import React, { useState, ChangeEvent, FormEvent } from "react";

interface FormData {
  username: string;
  email: string;
  password: string;
}

const ControlledForm: React.FC = () => {
  const [formData, setFormData] = useState<FormData>({
    username: "",
    email: "",
    password: "",
  });

  const [errors, setErrors] = useState<Partial<FormData>>({});

  const handleChange = (e: ChangeEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    setFormData({
      ...formData,
      [name]: value,
    });
  };

  const validate = (): boolean => {
    const newErrors: Partial<FormData> = {};

    if (!formData.username) newErrors.username = "Nome de usuário é obrigatório";
    if (!formData.email) {
      newErrors.email = "Email é obrigatório";
    } else if (!/\S+@\S+\.\S+/.test(formData.email)) {
      newErrors.email = "Email inválido";
    }
    if (!formData.password) {
      newErrors.password = "Senha é obrigatória";
    } else if (formData.password.length < 6) {
      newErrors.password = "Senha deve ter pelo menos 6 caracteres";
    }

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    if (validate()) {
      console.log("Dados do formulário:", formData);
      // Enviar dados para o servidor
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="username">Nome de Usuário:</label>
        <input id="username" type="text" name="username" value={formData.username} onChange={handleChange} />
        {errors.username && <span className="error">{errors.username}</span>}
      </div>

      <div>
        <label htmlFor="email">Email:</label>
        <input id="email" type="email" name="email" value={formData.email} onChange={handleChange} />
        {errors.email && <span className="error">{errors.email}</span>}
      </div>

      <div>
        <label htmlFor="password">Senha:</label>
        <input id="password" type="password" name="password" value={formData.password} onChange={handleChange} />
        {errors.password && <span className="error">{errors.password}</span>}
      </div>

      <button type="submit">Cadastrar</button>
    </form>
  );
};

export default ControlledForm;
```

### Formulários Não Controlados

Formulários não controlados usam refs para acessar valores do DOM diretamente.

```tsx
import React, { useRef, FormEvent } from "react";

const UncontrolledForm: React.FC = () => {
  const usernameRef = useRef<HTMLInputElement>(null);
  const emailRef = useRef<HTMLInputElement>(null);
  const passwordRef = useRef<HTMLInputElement>(null);

  const handleSubmit = (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();

    const formData = {
      username: usernameRef.current?.value,
      email: emailRef.current?.value,
      password: passwordRef.current?.value,
    };

    console.log("Dados do formulário:", formData);
    // Enviar dados para o servidor
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="username">Nome de Usuário:</label>
        <input id="username" type="text" name="username" ref={usernameRef} defaultValue="" />
      </div>

      <div>
        <label htmlFor="email">Email:</label>
        <input id="email" type="email" name="email" ref={emailRef} defaultValue="" />
      </div>

      <div>
        <label htmlFor="password">Senha:</label>
        <input id="password" type="password" name="password" ref={passwordRef} defaultValue="" />
      </div>

      <button type="submit">Cadastrar</button>
    </form>
  );
};

export default UncontrolledForm;
```

### Bibliotecas de Formulários

#### Formik

Formik é uma biblioteca popular para gerenciar formulários no React.

```tsx
import React from "react";
import { Formik, Form, Field, ErrorMessage, FormikHelpers } from "formik";
import * as Yup from "yup";

interface FormValues {
  username: string;
  email: string;
  password: string;
}

const validationSchema = Yup.object({
  username: Yup.string().required("Nome de usuário é obrigatório"),
  email: Yup.string().email("Email inválido").required("Email é obrigatório"),
  password: Yup.string().min(6, "Senha deve ter pelo menos 6 caracteres").required("Senha é obrigatória"),
});

const FormikForm: React.FC = () => {
  const initialValues: FormValues = {
    username: "",
    email: "",
    password: "",
  };

  const handleSubmit = (values: FormValues, { setSubmitting }: FormikHelpers<FormValues>) => {
    setTimeout(() => {
      console.log("Dados do formulário:", values);
      setSubmitting(false);
    }, 500);
  };

  return (
    <Formik initialValues={initialValues} validationSchema={validationSchema} onSubmit={handleSubmit}>
      {({ isSubmitting }) => (
        <Form>
          <div>
            <label htmlFor="username">Nome de Usuário:</label>
            <Field type="text" name="username" id="username" />
            <ErrorMessage name="username" component="div" className="error" />
          </div>

          <div>
            <label htmlFor="email">Email:</label>
            <Field type="email" name="email" id="email" />
            <ErrorMessage name="email" component="div" className="error" />
          </div>

          <div>
            <label htmlFor="password">Senha:</label>
            <Field type="password" name="password" id="password" />
            <ErrorMessage name="password" component="div" className="error" />
          </div>

          <button type="submit" disabled={isSubmitting}>
            {isSubmitting ? "Enviando..." : "Cadastrar"}
          </button>
        </Form>
      )}
    </Formik>
  );
};

export default FormikForm;
```

#### React Hook Form

React Hook Form é uma biblioteca mais leve e eficiente para formulários.

```tsx
import React from "react";
import { useForm, SubmitHandler } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

// Definindo o esquema de validação com Zod
const formSchema = z.object({
  username: z.string().min(1, { message: "Nome de usuário é obrigatório" }),
  email: z.string().email({ message: "Email inválido" }),
  password: z.string().min(6, { message: "Senha deve ter pelo menos 6 caracteres" }),
});

// Tipo inferido do esquema
type FormValues = z.infer<typeof formSchema>;

const HookForm: React.FC = () => {
  const {
    register,
    handleSubmit,
    formState: { errors, isSubmitting },
  } = useForm<FormValues>({
    resolver: zodResolver(formSchema),
  });

  const onSubmit: SubmitHandler<FormValues> = (data) => {
    console.log("Dados do formulário:", data);
    // Enviar dados para o servidor
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <div>
        <label htmlFor="username">Nome de Usuário:</label>
        <input id="username" type="text" {...register("username")} />
        {errors.username && <span className="error">{errors.username.message}</span>}
      </div>

      <div>
        <label htmlFor="email">Email:</label>
        <input id="email" type="email" {...register("email")} />
        {errors.email && <span className="error">{errors.email.message}</span>}
      </div>

      <div>
        <label htmlFor="password">Senha:</label>
        <input id="password" type="password" {...register("password")} />
        {errors.password && <span className="error">{errors.password.message}</span>}
      </div>

      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? "Enviando..." : "Cadastrar"}
      </button>
    </form>
  );
};

export default HookForm;
```

### Inputs Personalizados

Criando componentes de input personalizados e reutilizáveis com TypeScript.

```tsx
import React, { InputHTMLAttributes, forwardRef } from "react";

interface InputProps extends InputHTMLAttributes<HTMLInputElement> {
  label: string;
  error?: string;
}

export const Input = forwardRef<HTMLInputElement, InputProps>(({ label, error, id, ...rest }, ref) => {
  return (
    <div className="form-group">
      <label htmlFor={id}>{label}</label>
      <input ref={ref} id={id} className={`form-control ${error ? "is-invalid" : ""}`} {...rest} />
      {error && <div className="invalid-feedback">{error}</div>}
    </div>
  );
});

Input.displayName = "Input";

// Uso com React Hook Form
import { useForm, Controller } from "react-hook-form";

interface FormData {
  name: string;
  email: string;
}

const CustomInputForm: React.FC = () => {
  const {
    control,
    handleSubmit,
    formState: { errors },
  } = useForm<FormData>();

  const onSubmit = (data: FormData) => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Controller
        name="name"
        control={control}
        rules={{ required: "Nome é obrigatório" }}
        render={({ field }) => <Input label="Nome" id="name" error={errors.name?.message} {...field} />}
      />

      <Controller
        name="email"
        control={control}
        rules={{
          required: "Email é obrigatório",
          pattern: {
            value: /\S+@\S+\.\S+/,
            message: "Email inválido",
          },
        }}
        render={({ field }) => <Input label="Email" id="email" type="email" error={errors.email?.message} {...field} />}
      />

      <button type="submit">Enviar</button>
    </form>
  );
};
```

## Componentes Estilizados

### CSS Modules

CSS Modules permite escopar os estilos localmente para componentes específicos.

```tsx
// styles.module.css
.button {
  background-color: #007bff;
  color: white;
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.primary {
  background-color: #007bff;
}

.secondary {
  background-color: #6c757d;
}

.danger {
  background-color: #dc3545;
}

// Button.tsx
import React from 'react';
import styles from './styles.module.css';

type ButtonVariant = 'primary' | 'secondary' | 'danger';

interface ButtonProps {
  variant?: ButtonVariant;
  children: React.ReactNode;
  onClick?: () => void;
}

const Button: React.FC<ButtonProps> = ({
  variant = 'primary',
  children,
  onClick
}) => {
  return (
    <button
      className={`${styles.button} ${styles[variant]}`}
      onClick={onClick}
    >
      {children}
    </button>
  );
};

export default Button;
```

### Styled Components

Styled Components permite escrever CSS-in-JS para estilizar componentes React.

```tsx
import React from "react";
import styled, { css } from "styled-components";

type ButtonVariant = "primary" | "secondary" | "danger";

interface ButtonProps {
  variant?: ButtonVariant;
  outlined?: boolean;
  children: React.ReactNode;
  onClick?: () => void;
}

// Definindo os estilos com base nas props
const StyledButton = styled.button<{ variant: ButtonVariant; outlined: boolean }>`
  padding: 10px 15px;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.3s ease;

  ${({ variant, outlined }) => {
    const colors = {
      primary: "#007bff",
      secondary: "#6c757d",
      danger: "#dc3545",
    };

    if (outlined) {
      return css`
        background-color: transparent;
        border: 2px solid ${colors[variant]};
        color: ${colors[variant]};

        &:hover {
          background-color: ${colors[variant]};
          color: white;
        }
      `;
    }

    return css`
      background-color: ${colors[variant]};
      border: 2px solid ${colors[variant]};
      color: white;

      &:hover {
        opacity: 0.8;
      }
    `;
  }}
`;

const Button: React.FC<ButtonProps> = ({ variant = "primary", outlined = false, children, onClick }) => {
  return (
    <StyledButton variant={variant} outlined={outlined} onClick={onClick}>
      {children}
    </StyledButton>
  );
};

export default Button;
```

### Emotion

Emotion é outra biblioteca popular de CSS-in-JS.

````tsx
## Componentes Estilizados (continuação)

### Emotion

Emotion é outra biblioteca popular de CSS-in-JS.

```tsx
/** @jsx jsx */
import { jsx, css } from '@emotion/react';
import styled from '@emotion/styled';

type ButtonVariant = 'primary' | 'secondary' | 'danger';

interface ButtonProps {
  variant?: ButtonVariant;
  outlined?: boolean;
  children: React.ReactNode;
  onClick?: () => void;
}

// Definindo estilos usando o objeto theme
const buttonStyles = (variant: ButtonVariant, outlined: boolean) => {
  const colors = {
    primary: '#007bff',
    secondary: '#6c757d',
    danger: '#dc3545',
  };

  const baseStyles = css`
    padding: 10px 15px;
    border-radius: 4px;
    font-size: 16px;
    cursor: pointer;
    transition: all 0.3s ease;
  `;

  if (outlined) {
    return css`
      ${baseStyles}
      background-color: transparent;
      border: 2px solid ${colors[variant]};
      color: ${colors[variant]};
      &:hover {
        background-color: ${colors[variant]};
        color: white;
      }
    `;
  }

  return css`
    ${baseStyles}
    background-color: ${colors[variant]};
    border: 2px solid ${colors[variant]};
    color: white;
    &:hover {
      opacity: 0.8;
    }
  `;
};

const Button: React.FC<ButtonProps> = ({
  variant = 'primary',
  outlined = false,
  children,
  onClick,
}) => {
  return (
    <button
      css={buttonStyles(variant, outlined)}
      onClick={onClick}
    >
      {children}
    </button>
  );
};

// Alternativa usando styled do Emotion
const StyledButton = styled.button<{ variant: ButtonVariant; outlined: boolean }>`
  padding: 10px 15px;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.3s ease;

  ${({ variant, outlined }) => {
    const colors = {
      primary: '#007bff',
      secondary: '#6c757d',
      danger: '#dc3545',
    };

    if (outlined) {
      return `
        background-color: transparent;
        border: 2px solid ${colors[variant]};
        color: ${colors[variant]};
        &:hover {
          background-color: ${colors[variant]};
          color: white;
        }
      `;
    }

    return `
      background-color: ${colors[variant]};
      border: 2px solid ${colors[variant]};
      color: white;
      &:hover {
        opacity: 0.8;
      }
    `;
  }}
`;

export default Button;
````

### Tailwind CSS

Tailwind CSS é uma abordagem de "utility-first" para estilização.

```tsx
// Primeiro, instale o Tailwind CSS e configure-o no seu projeto
// Exemplo com TypeScript e React

import React from "react";

type ButtonVariant = "primary" | "secondary" | "danger";

interface ButtonProps {
  variant?: ButtonVariant;
  outlined?: boolean;
  children: React.ReactNode;
  onClick?: () => void;
}

const Button: React.FC<ButtonProps> = ({ variant = "primary", outlined = false, children, onClick }) => {
  // Classes condicionais baseadas nas props
  const getButtonClasses = (): string => {
    const baseClasses = "px-4 py-2 rounded font-medium transition-colors duration-300 focus:outline-none";

    const variantClasses = {
      primary: outlined
        ? "bg-transparent border-2 border-blue-500 text-blue-500 hover:bg-blue-500 hover:text-white"
        : "bg-blue-500 border-2 border-blue-500 text-white hover:bg-blue-600 hover:border-blue-600",
      secondary: outlined
        ? "bg-transparent border-2 border-gray-500 text-gray-500 hover:bg-gray-500 hover:text-white"
        : "bg-gray-500 border-2 border-gray-500 text-white hover:bg-gray-600 hover:border-gray-600",
      danger: outlined
        ? "bg-transparent border-2 border-red-500 text-red-500 hover:bg-red-500 hover:text-white"
        : "bg-red-500 border-2 border-red-500 text-white hover:bg-red-600 hover:border-red-600",
    };

    return `${baseClasses} ${variantClasses[variant]}`;
  };

  return (
    <button className={getButtonClasses()} onClick={onClick}>
      {children}
    </button>
  );
};

export default Button;
```

### Typed Theme Provider

Criando um provedor de tema com tipos seguros.

```tsx
// theme.ts
export interface Theme {
  colors: {
    primary: string;
    secondary: string;
    danger: string;
    background: string;
    text: string;
  };
  spacing: {
    xs: string;
    sm: string;
    md: string;
    lg: string;
    xl: string;
  };
  borderRadius: {
    small: string;
    medium: string;
    large: string;
    round: string;
  };
  typography: {
    fontFamily: string;
    fontSize: {
      small: string;
      medium: string;
      large: string;
      xl: string;
    };
  };
}

export const lightTheme: Theme = {
  colors: {
    primary: "#007bff",
    secondary: "#6c757d",
    danger: "#dc3545",
    background: "#ffffff",
    text: "#212529",
  },
  spacing: {
    xs: "4px",
    sm: "8px",
    md: "16px",
    lg: "24px",
    xl: "32px",
  },
  borderRadius: {
    small: "2px",
    medium: "4px",
    large: "8px",
    round: "50%",
  },
  typography: {
    fontFamily: "'Segoe UI', Roboto, sans-serif",
    fontSize: {
      small: "12px",
      medium: "16px",
      large: "20px",
      xl: "24px",
    },
  },
};

export const darkTheme: Theme = {
  ...lightTheme,
  colors: {
    ...lightTheme.colors,
    background: "#121212",
    text: "#e0e0e0",
    primary: "#90caf9",
  },
};

// ThemeProvider.tsx
import React, { createContext, useContext, useState, ReactNode } from "react";
import { ThemeProvider as StyledThemeProvider } from "styled-components";
import { lightTheme, darkTheme, Theme } from "./theme";

interface ThemeContextType {
  theme: Theme;
  toggleTheme: () => void;
  isDarkMode: boolean;
}

const ThemeContext = createContext<ThemeContextType | undefined>(undefined);

interface ThemeProviderProps {
  children: ReactNode;
}

export const ThemeProvider: React.FC<ThemeProviderProps> = ({ children }) => {
  const [isDarkMode, setIsDarkMode] = useState(false);
  const theme = isDarkMode ? darkTheme : lightTheme;

  const toggleTheme = () => {
    setIsDarkMode(!isDarkMode);
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme, isDarkMode }}>
      <StyledThemeProvider theme={theme}>{children}</StyledThemeProvider>
    </ThemeContext.Provider>
  );
};

export const useTheme = (): ThemeContextType => {
  const context = useContext(ThemeContext);
  if (context === undefined) {
    throw new Error("useTheme must be used within a ThemeProvider");
  }
  return context;
};

// Exemplo de uso:
const ThemedButton = styled.button`
  background-color: ${(props) => props.theme.colors.primary};
  color: white;
  padding: ${(props) => props.theme.spacing.sm} ${(props) => props.theme.spacing.md};
  border-radius: ${(props) => props.theme.borderRadius.medium};
  font-family: ${(props) => props.theme.typography.fontFamily};
  font-size: ${(props) => props.theme.typography.fontSize.medium};
`;
```

## 12. Roteamento com React Router

### 12.1. Configuração Básica

Exemplo de configuração do React Router com TypeScript.

```tsx
// App.tsx
import React from "react";
import { BrowserRouter, Routes, Route, Link } from "react-router-dom";
import Home from "./pages/Home";
import About from "./pages/About";
import Users from "./pages/Users";
import UserDetails from "./pages/UserDetails";
import NotFound from "./pages/NotFound";

const App: React.FC = () => {
  return (
    <BrowserRouter>
      <nav>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/about">About</Link>
          </li>
          <li>
            <Link to="/users">Users</Link>
          </li>
        </ul>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/users" element={<Users />} />
        <Route path="/users/:userId" element={<UserDetails />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
};

export default App;
```

### 12.2. Hooks do React Router

Utilizando os hooks do React Router com TypeScript.

```tsx
// UserDetails.tsx
import React, { useEffect, useState } from "react";
import { useParams, useNavigate, useLocation } from "react-router-dom";

interface UserData {
  id: number;
  name: string;
  email: string;
  // outros campos
}

const UserDetails: React.FC = () => {
  const { userId } = useParams<{ userId: string }>();
  const navigate = useNavigate();
  const location = useLocation();
  const [user, setUser] = useState<UserData | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const fetchUser = async () => {
      try {
        // Simulando uma chamada API
        const response = await fetch(`https://api.example.com/users/${userId}`);
        if (!response.ok) {
          throw new Error("User not found");
        }
        const userData: UserData = await response.json();
        setUser(userData);
      } catch (error) {
        console.error("Failed to fetch user:", error);
        navigate("/users", { replace: true });
      } finally {
        setLoading(false);
      }
    };

    fetchUser();
  }, [userId, navigate]);

  if (loading) {
    return <div>Loading...</div>;
  }

  if (!user) {
    return <div>User not found</div>;
  }

  return (
    <div>
      <h2>User Details</h2>
      <p>ID: {user.id}</p>
      <p>Name: {user.name}</p>
      <p>Email: {user.email}</p>
      <button onClick={() => navigate(-1)}>Go Back</button>
    </div>
  );
};

export default UserDetails;
```

### 12.3. Rotas Protegidas

Implementando rotas protegidas com autenticação.

```tsx
// auth.ts
interface User {
  id: string;
  name: string;
  email: string;
  roles: string[];
}

interface AuthContextType {
  user: User | null;
  login: (credentials: { email: string; password: string }) => Promise<void>;
  logout: () => void;
  isAuthenticated: boolean;
  hasRole: (role: string) => boolean;
}

// AuthContext.tsx
import React, { createContext, useState, useContext, ReactNode } from 'react';

const AuthContext = createContext<AuthContextType | undefined>(undefined);

interface AuthProviderProps {
  children: ReactNode;
}

export const AuthProvider: React.FC<AuthProviderProps> = ({ children }) => {
  const [user, setUser] = useState<User | null>(null);

  const login = async (credentials: { email: string; password: string }) => {
    try {
      // Simulando chamada API de login
      const response = await fetch('https://api.example.com/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(credentials),
      });

      if (!response.ok) {
        throw new Error('Login failed');
      }

      const userData: User = await response.json();
      setUser(userData);
      localStorage.setItem('auth_token', 'fake-jwt-token');
    } catch (error) {
      console.error('Login error:', error);
      throw error;
    }
  };

  const logout = () => {
    setUser(null);
    localStorage.removeItem('auth_token');
  };

  const hasRole = (role: string): boolean => {
    return user?.roles.includes(role) || false;
  };

  const value = {
    user,
    login,
    logout,
    isAuthenticated: !!user,
    hasRole,
  };

  return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
};

export const useAuth = (): AuthContextType => {
  const context = useContext(AuthContext);
  if (context === undefined) {
    throw new Error('useAuth must be used within an AuthProvider');
  }
  return context;
};

// ProtectedRoute.tsx
import React from 'react';
import { Navigate, useLocation, Outlet } from 'react-router-dom';
import { useAuth } from './AuthContext';

interface ProtectedRouteProps {
  requiredRole?: string;
}

const ProtectedRoute: React.FC<ProtectedRouteProps> = ({ requiredRole }) => {
  const { isAuthenticated, hasRole } = useAuth();
  const location = useLocation();

  if (!isAuthenticated) {
    return <Navigate to="/login" state={{ from: location }} replace />;
  }

  if (requiredRole && !hasRole(requiredRole)) {
    return <Navigate to="/unauthorized" replace />;
  }

  return <Outlet />;
};

export default ProtectedRoute;

// Uso no App.tsx
import { AuthProvider } from './AuthContext';
import ProtectedRoute from './ProtectedRoute';
import Login from './pages/Login';
import Dashboard from './pages/Dashboard';
import AdminPanel from './pages/AdminPanel';
import Unauthorized from './pages/Unauthorized';

const App: React.FC = () => {
  return (
    <AuthProvider>
      <BrowserRouter>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/login" element={<Login />} />
          <Route path="/unauthorized" element={<Unauthorized />} />

          {/* Rotas protegidas

```

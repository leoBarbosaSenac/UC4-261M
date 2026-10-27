# Herança em TypeScript


## 1. 📌 O que é Herança?
**Herança** é o mecanismo que permite criar novas classes (subclasses) a partir de uma classe já existente (superclasse).
Basicamente, temos uma classe "pai" que, a partir dela, temos classes "filhas", que herdam características dela.

- **Superclasse (pai):** contém atributos e métodos comuns.  
- **Subclasse (filho):** herda tudo da superclasse e pode adicionar seus próprios comportamentos.  

👉 Isso evita **duplicação de código** e melhora a **organização**.

---

## 2. 📝 Estrutura de Arquivos

Vamos criar as seguintes classes:

```

src/
├── Animal.ts
├── Mamifero.ts
├── Reptil.ts
└── index.ts   (arquivo principal para testes)

````

---

## 3. 🐾 Código das Classes

### 📄 Animal.ts
```ts
// Classe base (superclasse)
export class Animal {
  nome: string;
  idade: number;

  constructor(nome: string, idade: number) {
    this.nome = nome;
    this.idade = idade;
  }

  mover(): void {
    console.log(`${this.nome} está se movendo...`);
  }
}
````

---

### 📄 Mamifero.ts

```ts
import { Animal } from "./Animal";

// Subclasse Mamífero
export class Mamifero extends Animal {
  temPelos: boolean;

  constructor(nome: string, idade: number, temPelos: boolean) {
    super(nome, idade); // chama o construtor da classe pai
    this.temPelos = temPelos;
  }

  amamentar(): void {
    console.log(`${this.nome} está amamentando seus filhotes.`);
  }
}
```

---

### 📄 Reptil.ts

```ts
import { Animal } from "./Animal";

// Subclasse Réptil
export class Reptil extends Animal {
  tipoEscama: string;

  constructor(nome: string, idade: number, tipoEscama: string) {
    super(nome, idade);
    this.tipoEscama = tipoEscama;
  }

  botarOvo(): void {
    console.log(`${this.nome} botou um ovo.`);
  }
}
```

---

### 📄 index.ts

```ts
import { Mamifero } from "./Mamifero";
import { Reptil } from "./Reptil";

// Criando objetos
const cachorro = new Mamifero("Rex", 5, true);
const cobra = new Reptil("Naja", 2, "escamas lisas");

cachorro.mover();      // herdado da classe Animal
cachorro.amamentar();  // método da classe Mamifero

cobra.mover();         // herdado da classe Animal
cobra.botarOvo();      // método da classe Reptil
```

---

## 4. 🚀 O que aprendemos?

* Criamos uma **superclasse Animal** com atributos e métodos comuns.
* Criamos subclasses (`Mamifero` e `Reptil`) que **herdam** de `Animal`.
* Cada subclasse adicionou **comportamentos próprios**.

---

# Exercícios — Herança em TypeScript

## Exercício 1: Criando uma subclasse

Crie uma classe `Crocodilo` que herde da classe `Reptil`.

A classe `Crocodilo` deve:

- Possuir um método `nadar()`, que exiba uma mensagem informando que o crocodilo está nadando.
- Utilizar os atributos e métodos herdados das classes `Reptil` e `Animal`.

No arquivo `index.ts`:

1. Crie um objeto da classe `Crocodilo`.
2. Chame o método `mover()`, herdado de `Animal`.
3. Chame o método `botarOvo()`, herdado de `Reptil`.
4. Chame o método `nadar()`, definido na própria classe `Crocodilo`.

Observe como uma classe pode herdar características de outra classe que também utiliza herança.

---

## Exercício 2: Organizando uma hierarquia de animais

Vamos ampliar nosso sistema de animais utilizando herança.

### Parte 1 — Criando as classes

Crie as seguintes classes:

- `Ave`: deve herdar de `Animal`.
- `Cachorro`: deve herdar de `Mamifero`.
- `Coruja`: deve herdar de `Ave`.

Cada classe deve possuir seu próprio construtor, recebendo os parâmetros necessários e utilizando `super()` para chamar o construtor da classe pai.

### Parte 2 — Criando um método comum

Adicione o método `comer()` à classe `Animal`.

Esse método deve exibir uma mensagem informando que o animal está comendo.

Como `Cachorro`, `Coruja` e `Crocodilo` herdam, direta ou indiretamente, de `Animal`, todos poderão utilizar esse método sem precisar implementá-lo novamente.

### Parte 3 — Criando um array de animais

No arquivo `index.ts`:

1. Crie um objeto da classe `Cachorro`.
2. Crie um objeto da classe `Coruja`.
3. Crie um objeto da classe `Crocodilo`.
4. Armazene os três objetos em um único array do tipo `Animal[]`.
5. Utilize um laço de repetição para percorrer o array.
6. Para cada animal, chame o método `comer()`.

### Parte 4 — Refletindo sobre a herança

Responda:

1. Por que foi possível armazenar objetos de classes diferentes no mesmo array?
2. Por que todos os objetos conseguem utilizar o método `comer()`?
3. Qual é a vantagem de implementar esse método apenas uma vez na classe `Animal`?

## 📚 Resumo

* **Herança** = reutilizar código criando uma hierarquia de classes.
* Em TypeScript usamos `extends` para indicar que uma classe herda de outra.
* Subclasses herdam atributos/métodos da superclasse, mas também podem ter **novos comportamentos**.

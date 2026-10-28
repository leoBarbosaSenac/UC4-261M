# Modificador `protected` em TypeScript

## O que é `protected`?

Em **TypeScript**, o modificador `protected` é usado em **Propriedades** e **Métodos** de uma classe. Ele funciona de forma intermediária entre `private` e `public`:

- **public** → acessível de qualquer lugar.
- **private** → acessível **somente dentro da própria classe**.
- **protected** → acessível **dentro da própria classe e em classes que herdam dela**.

Ou seja, `protected` permite **herança segura**, permitindo que subclasses utilizem os membros da classe pai sem expô-los publicamente.

---

## Sintaxe

```typescript
class Animal {
    protected nome: string;

    constructor(nome: string) {
        this.nome = nome;
    }

    protected fazerSom(): void {
        console.log(`${this.nome} está fazendo um som.`);
    }
}

class Cachorro extends Animal {
    latir() {
        console.log(`${this.nome} está latindo.`); // Acesso permitido porque é protected
        this.fazerSom(); // Também permitido
    }
}

const dog = new Cachorro("Rex");
dog.latir();          // Funciona
// dog.nome;          // ERRO! Não é permitido acessar fora da classe ou herança
// dog.fazerSom();    // ERRO! Não é permitido acessar fora da classe ou herança
````

---

## Diferença entre `protected` e `private`

| Modificador | Acesso dentro da classe | Acesso fora da classe | Acesso em subclasses |
| ----------- | ----------------------- | --------------------- | -------------------- |
| public      | ✅                       | ✅                     | ✅                    |
| private     | ✅                       | ❌                     | ❌                    |
| protected   | ✅                       | ❌                     | ✅                    |

**Resumo:**
`protected` é útil quando você quer **proteger informações** da classe para que **só subclasses possam acessar**, sem permitir que qualquer código externo interfira diretamente.

---

## Exemplo prático

Imagine que estamos criando um jogo com personagens:

```typescript
export class Personagem {
    protected vida: number;

    constructor(vida: number) {
        this.vida = vida;
    }

    // Método protected: só pode ser chamado dentro da classe ou subclasses
    protected sofrerDano(valor: number) {
        this.vida -= valor;
        console.log(`${this.constructor.name} sofreu ${valor} de dano. Vida atual: ${this.vida}`);
    }

    // Método público para permitir que outras instâncias recebam dano sem expor o protected
    public receberDano(valor: number) {
        this.sofrerDano(valor);
    }
}

import { Personagem } from "./Personagem";

export class Guerreiro extends Personagem {
    constructor(vida: number) {
        super(vida);
    }

    // Método público que chama o protected de outra instância via método público
    atacar(inimigo: Personagem) {
        console.log(`${this.constructor.name} atacando ${inimigo.constructor.name}...`);
        inimigo.receberDano(10); // usa o método público, que chama o protected internamente
    }

    // Método que acessa diretamente o protected da própria instância
    treinar() {
        console.log(`${this.constructor.name} treinando...`);
        this.sofrerDano(5); // ✅ permitido: acessando protected da própria instância
    }
}

const heroi = new Guerreiro(100);
const vilao = new Guerreiro(80);

heroi.atacar(vilao);  // Atacando outro personagem (usa método público que chama protected)
heroi.treinar();       // Acessando protected na própria instância
```

---

## Exercícios para praticar

1. Crie uma classe `Veiculo` com uma propriedade `protected velocidade`.
   Crie uma subclasse `Carro` que possa acessar e modificar `velocidade` através de um método chamado `acelerar`.
   Instancie o objeto Carro e chame o método `acelerar`.

2. Crie uma classe `ContaBancaria` com um saldo `protected`. Dentro desta classe, crie também um método `atualizarSaldo` que deve ser protected (sim, podemos encapsular métodos também).
   Este método deve receber um valor como parâmetro e acrescenta este valor ao atributo `saldo`.
   Crie depois uma subclasse `ContaPoupanca` e, nela, crie um método público chamado `aplicarJuros`. Este método deve chamar o método atualizarSaldo e acrescentar os juros.
   Em index, instancie ContaPoupanca e tente acessar os dois métodos para ver o que acontece.
   Tente acessar o saldo diretamente fora das classes e veja o que acontece.

---

> **Dica:**
> Use `protected` sempre que você quer **esconder informações do mundo externo**, mas ainda permitir que **classes derivadas manipulem esses dados**. Isso ajuda a manter o **encapsulamento**, que é um dos pilares da POO.

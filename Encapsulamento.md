# Encapsulamento e modificadores de acesso em TypeScript

## 1. O que é encapsulamento?

O **encapsulamento** é um dos pilares da Programação Orientada a Objetos
(POO). Ele consiste em organizar os dados e os comportamentos de um
objeto dentro de uma classe e controlar como outras partes do programa
podem acessá-los.

Imagine uma conta bancária. O saldo faz parte dos dados internos da
conta. Não seria adequado permitir que qualquer parte do programa
alterasse esse saldo livremente. Em vez disso, a classe pode oferecer
métodos, como `depositar`, `sacar` e `consultarSaldo`, para controlar
como essas operações acontecem.

Assim, o encapsulamento ajuda a:

-   **Proteger os dados:** evita alterações diretas e inadequadas.
-   **Controlar o acesso:** define quais informações e operações ficam
    disponíveis para outras partes do programa.
-   **Organizar o código:** reúne os dados e os métodos relacionados
    dentro da mesma classe.
-   **Facilitar a manutenção:** permite mudar detalhes internos sem
    necessariamente alterar a forma como outras partes do programa
    utilizam a classe.

### Exemplo sem encapsulamento

``` typescript
class ContaBancaria {
    saldo: number;

    constructor(saldoInicial: number) {
        this.saldo = saldoInicial;
    }
}

const conta = new ContaBancaria(100);

conta.saldo = -500; // O saldo pode ser alterado diretamente
console.log(conta.saldo);
```

Nesse exemplo, o código externo consegue alterar o saldo diretamente,
inclusive para um valor negativo. A classe não controla como essa
alteração acontece.

### Exemplo com encapsulamento

``` typescript
class ContaBancaria {
    private saldo: number;

    constructor(saldoInicial: number) {
        this.saldo = saldoInicial;
    }

    public consultarSaldo(): void {
        console.log(`Saldo: R$ ${this.saldo}`);
    }

    public depositar(valor: number): void {
        if (valor > 0) {
            this.saldo += valor;
        }
    }
}

const conta = new ContaBancaria(100);

conta.depositar(50);
conta.consultarSaldo();

// conta.saldo = -500; // ERRO: saldo é private
```

Agora, o saldo é `private`, portanto não pode ser acessado diretamente
fora da classe. O método `depositar` também verifica se o valor é
positivo antes de alterar o saldo.

O encapsulamento não significa esconder tudo. Significa **controlar o
que pode ser acessado e de que maneira**. Uma classe pode manter alguns
dados internos e, ao mesmo tempo, disponibilizar métodos públicos para
que outras partes do programa realizem operações permitidas.

------------------------------------------------------------------------

## 2. Modificadores de acesso

Em TypeScript, os modificadores de acesso ajudam a implementar o
encapsulamento, definindo onde as propriedades e os métodos de uma
classe podem ser utilizados.

Os três modificadores mais comuns são:

-   **`public`**: permite o acesso de qualquer lugar. É o modificador
    padrão quando nenhum outro é informado.
-   **`private`**: permite o acesso somente dentro da própria classe.
-   **`protected`**: permite o acesso dentro da própria classe e nas
    classes que herdam dela.

------------------------------------------------------------------------

## 3. Modificador `public`

O modificador `public` permite que uma propriedade ou um método seja
acessado de dentro da classe, de classes filhas e também de fora da
classe, por meio de um objeto.

Como `public` é o padrão em TypeScript, não é obrigatório escrevê-lo
explicitamente.

### Exemplo

``` typescript
class Pessoa {
    public nome: string;

    constructor(nome: string) {
        this.nome = nome;
    }

    public apresentar(): void {
        console.log(`Olá! Meu nome é ${this.nome}.`);
    }
}

const pessoa = new Pessoa("Ana");

console.log(pessoa.nome); // Acesso permitido
pessoa.apresentar();      // Acesso permitido
```

Neste exemplo, tanto a propriedade `nome` quanto o método `apresentar()`
são públicos. Por isso, podem ser acessados diretamente pelo objeto
`pessoa`.

### Quando usar `public`?

Use `public` quando uma informação ou ação deve estar disponível para o
código que utiliza a classe, como métodos para apresentar dados ou
executar ações.

------------------------------------------------------------------------

## 4. Modificador `private`

O modificador `private` restringe o acesso a uma propriedade ou método à
própria classe em que foi declarado. O código externo e as subclasses
não podem acessar diretamente esse membro.

### Exemplo

``` typescript
class ContaBancaria {
    private saldo: number;

    constructor(saldoInicial: number) {
        this.saldo = saldoInicial;
    }

    public consultarSaldo(): void {
        console.log(`Saldo: R$ ${this.saldo}`);
    }

    public depositar(valor: number): void {
        if (valor > 0) {
            this.saldo += valor;
        }
    }
}

const conta = new ContaBancaria(100);

conta.consultarSaldo(); // Acesso permitido
conta.depositar(50);    // Acesso permitido

// console.log(conta.saldo); // ERRO: saldo é private
```

A propriedade `saldo` é privada. Por isso, não pode ser acessada
diretamente fora da classe. Para consultar ou alterar o saldo, usamos os
métodos públicos que a própria classe disponibiliza.

### E em uma subclasse?

Uma classe que herda de `ContaBancaria` também não pode acessar
diretamente a propriedade `saldo`, pois ela é `private`. Se uma
subclasse precisar trabalhar com esse dado, podemos disponibilizar
métodos públicos ou protegidos adequados.

### Quando usar `private`?

Use `private` para impedir o acesso direto a detalhes internos da
classe. Isso ajuda a proteger os dados e a controlar como eles são
consultados ou modificados.

------------------------------------------------------------------------

## 5. Modificador `protected`

O modificador `protected` permite o acesso a uma propriedade ou método
dentro da própria classe e nas classes que herdam dela. Entretanto, ele
não permite o acesso direto por meio de um objeto fora dessas classes.

Ou seja, `protected` fica entre `private` e `public` em relação ao
acesso.

### Exemplo

``` typescript
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
    public latir(): void {
        console.log(`${this.nome} está latindo.`);
        this.fazerSom();
    }
}

const cachorro = new Cachorro("Rex");

cachorro.latir(); // Funciona

// console.log(cachorro.nome); // ERRO: nome é protected
// cachorro.fazerSom();       // ERRO: fazerSom é protected
```

A classe `Cachorro` herda de `Animal`. Por isso, consegue acessar `nome`
e `fazerSom()`, que são `protected`. Já o código de fora não pode
acessar esses membros diretamente.

### Exemplo prático: personagens de um jogo

``` typescript
class Personagem {
    protected vida: number;

    constructor(vida: number) {
        this.vida = vida;
    }

    protected sofrerDano(valor: number): void {
        this.vida -= valor;
        console.log(`Vida atual: ${this.vida}`);
    }

    public receberDano(valor: number): void {
        this.sofrerDano(valor);
    }
}

class Guerreiro extends Personagem {
    public treinar(): void {
        console.log("O guerreiro está treinando.");
        this.sofrerDano(5);
    }
}

const heroi = new Guerreiro(100);

heroi.treinar();       // Funciona
heroi.receberDano(10); // Funciona

// heroi.vida;          // ERRO: vida é protected
// heroi.sofrerDano(5); // ERRO: sofrerDano é protected
```

A classe `Guerreiro` consegue usar `vida` e `sofrerDano()` por herança.
O código externo, por outro lado, utiliza o método público
`receberDano()` para solicitar a ação sem acessar diretamente o método
protegido.

### Quando usar `protected`?

Use `protected` quando uma propriedade ou método deve ser utilizado pela
própria classe e por suas subclasses, mas não deve ficar disponível
diretamente para o restante do código.

------------------------------------------------------------------------

## 6. Comparando os modificadores

| Modificador | Dentro da própria classe | Fora da classe, por um objeto | Em subclasses |
|---|---|---|---|
| `public` | Sim | Sim | Sim |
| `private` | Sim | Não | Não |
| `protected` | Sim | Não | Sim |

**Importante:** esses modificadores controlam o acesso durante a
verificação de tipos do TypeScript. Os modificadores `private` e
`protected` tradicionais do TypeScript não oferecem, por si só, o mesmo
tipo de privacidade em tempo de execução que o recurso JavaScript
`#campo`.

------------------------------------------------------------------------

## 7. Exercícios para praticar

### Exercício 1 --- `public`

1.  Crie uma classe `Livro` com uma propriedade pública chamada
    `titulo`.
2.  Crie um método público chamado `exibirTitulo` que mostre o título no
    console.
3.  Instancie um objeto `Livro`.
4.  Acesse a propriedade `titulo` e chame o método `exibirTitulo` por
    meio do objeto.

### Exercício 2 --- `private`

1.  Crie uma classe `ContaBancaria` com uma propriedade privada chamada
    `saldo`.
2.  No construtor, receba um valor inicial para o saldo.
3.  Crie um método público chamado `consultarSaldo` para mostrar o
    saldo.
4.  Crie um método público chamado `depositar` que receba um valor e o
    acrescente ao saldo, desde que seja maior que zero.
5.  Instancie uma conta e teste os métodos.
6.  Tente acessar `saldo` diretamente pelo objeto. Observe o erro
    indicado pelo TypeScript.

### Exercício 3 --- `protected`

1.  Crie uma classe `Veiculo` com uma propriedade protegida chamada
    `velocidade`, iniciada em zero.
2.  Crie uma subclasse `Carro` que herde de `Veiculo`.
3.  Na classe `Carro`, crie um método público chamado `acelerar` que
    acrescente um valor à velocidade.
4.  Crie também um método público chamado `exibirVelocidade` para
    mostrar a velocidade atual.
5.  Instancie um `Carro`, acelere e exiba sua velocidade.
6.  Tente acessar `velocidade` diretamente pelo objeto e observe o erro
    indicado pelo TypeScript.

### Exercício 4 --- Comparando `private` e `protected`

1.  Crie uma classe `Conta` com uma propriedade `protected saldo` e um
    método `protected atualizarSaldo(valor: number)` que acrescente o
    valor recebido ao saldo.
2.  Crie uma subclasse `ContaPoupanca` com um método público
    `aplicarJuros` que utilize `atualizarSaldo` para acrescentar os
    juros.
3.  Crie um método público para consultar o saldo.
4.  Instancie `ContaPoupanca` e teste seus métodos.
5.  Tente acessar `saldo` e `atualizarSaldo` diretamente pelo objeto.
    Observe o erro indicado pelo TypeScript.

------------------------------------------------------------------------

## 8. Resumo

-   **Encapsulamento:** organiza os dados e comportamentos de uma classe
    e controla como eles são acessados.
-   **`public`**: pode ser acessado de qualquer lugar.
-   **`private`**: só pode ser acessado dentro da classe em que foi
    declarado.
-   **`protected`**: pode ser acessado dentro da classe em que foi
    declarado e nas subclasses.

Escolher o modificador adequado ajuda a organizar as classes, proteger
seus dados e definir quais operações ficam disponíveis para outras
partes do programa.

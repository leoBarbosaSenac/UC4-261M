# Tutorial: Criando um Projeto TypeScript

## 1. Pré-requisitos

Instale o [Node.js](https://nodejs.org), que inclui o npm, usado para gerenciar as dependências do projeto.

## 2. Criar o projeto

No terminal do VS Code, execute os comandos:

```bash
mkdir projeto-typescript
cd projeto-typescript
npm init -y
```

- `mkdir projeto-typescript`: cria a pasta do projeto.
- `cd projeto-typescript`: entra na pasta criada.
- `npm init -y`: cria o arquivo `package.json` com as configurações iniciais.

## 3. Instalar o TypeScript

Instale o TypeScript como dependência de desenvolvimento:

```bash
npm install typescript@^6 -D
```

## 4. Criar o arquivo de configuração

Gere o arquivo `tsconfig.json`:

```bash
npx tsc --init
```

Esse arquivo define as opções usadas pelo compilador TypeScript.

## 5. Configurar o `tsconfig.json`

Use a configuração abaixo para manter os arquivos TypeScript em `src/` e gerar os arquivos JavaScript em `dist/`:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "CommonJS",
    "rootDir": "./src",
    "outDir": "./dist",
    "strict": true,
    "esModuleInterop": true
  },
  "include": ["src/**/*.ts"],
  "exclude": ["node_modules", "dist"]
}
```

- `target`: versão do JavaScript gerado.
- `module`: sistema de módulos utilizado.
- `rootDir`: pasta do código-fonte TypeScript.
- `outDir`: pasta em que os arquivos JavaScript compilados serão salvos.
- `strict`: ativa verificações mais rigorosas de tipos.
- `include`: define quais arquivos TypeScript serão compilados.
- `exclude`: define pastas que não devem ser consideradas na compilação.

## 6. Criar o primeiro arquivo TypeScript

Crie a pasta `src` e, dentro dela, o arquivo `index.ts`:

```typescript
let nome: string = "Daniel";

console.log("Olá, " + nome);
```

Estrutura esperada do projeto:

```text
projeto-typescript/
├── src/
│   └── index.ts
├── package.json
├── package-lock.json
├── tsconfig.json
└── node_modules/
```

## 7. Compilar e executar

Na raiz do projeto, compile os arquivos TypeScript:

```bash
npx tsc
```

O JavaScript gerado será salvo na pasta `dist/`. Para executar o programa:

```bash
node dist/index.js
```

A saída esperada é:

```text
Olá, Daniel
```

# Módulo 7: Modularização - Funções e Procedimentos

### 1. Conceito de Modularização: "Dividir para Conquistar"

#### Motivação

Até agora, escrevemos todos os nossos algoritmos em um único bloco de código, dentro do `inicio...fimalgoritmo`. Para programas pequenos, isso funciona. Mas imagine um sistema complexo como um editor de texto. Haveria código para abrir arquivos, salvar, formatar texto, verificar ortografia, imprimir... Colocar tudo em um único lugar seria um caos!

A **Modularização** é a técnica de quebrar um sistema grande em partes menores e independentes, chamadas **módulos** ou **sub-rotinas**. Cada módulo tem uma responsabilidade específica e bem definida. Em Portugol, implementamos isso através de **Procedimentos** e **Funções**.

**Benefícios:**

- **Legibilidade:** É mais fácil entender um programa dividido em pequenas partes lógicas (`leia_dados()`, `calcule_media()`, `exiba_resultado()`) do que um bloco gigante de código.
- **Reutilização:** Se você cria uma rotina para calcular o IMC, pode chamá-la em várias partes do seu programa (ou em outros programas) sem precisar reescrever o código.
- **Manutenção:** Se houver um erro no cálculo da média, você sabe exatamente onde procurar: no módulo responsável por isso, sem precisar mexer no resto do sistema.

## 2. Procedimentos (Sub-rotinas)

Um **Procedimento** é um bloco de código nomeado que realiza uma tarefa específica, mas **não retorna um valor** para quem o chamou. Ele simplesmente _faz_ alguma coisa.

#### Motivação

Pense em tarefas que se repetem, como exibir um cabeçalho ou um menu. Não precisamos que essa ação nos "devolva" um resultado, apenas que ela seja executada.

#### Criação e Chamada

```portugol
// A DEFINIÇÃO do procedimento é feita FORA do bloco principal
procedimento ExibirCabecalho()
// Variáveis declaradas aqui dentro são LOCAIS a este procedimento
inicio
   escrevaL("====================================")
   escrevaL("===     SISTEMA ACADÊMICO      ===")
   escrevaL("====================================")
fimprocedimento

// O ALGORITMO PRINCIPAL
algoritmo "UsoDeProcedimento"
inicio
   // A CHAMADA do procedimento é feita DENTRO do bloco principal
   ExibirCabecalho()

   escrevaL("Bem-vindo ao sistema!")
   escrevaL("Por favor, faça seu login.")

   // Podemos reutilizar o procedimento quantas vezes quisermos
   ExibirCabecalho()
fimalgoritmo
```

_O procedimento `ExibirCabecalho` é como uma nova instrução que nós mesmos criamos._

## 3. Funções

Uma **Função** é muito parecida com um procedimento, mas com uma diferença crucial: ela **sempre retorna um valor** de um tipo específico (`INTEIRO`, `REAL`, `LOGICO`, `CARACTERE`) para o local onde foi chamada.

#### Motivação

Quando queremos calcular algo (uma soma, uma média, uma área) e precisamos **usar esse resultado** em outra parte do código. A função realiza o cálculo e "entrega" a resposta.

#### Criação e Chamada

```portugol
// A DEFINIÇÃO da função especifica o tipo de dado que ela retorna
funcao Somar(a: real; b: real) : real
// 'a' e 'b' são parâmetros: valores que a função recebe para trabalhar
inicio
   // A palavra-chave 'retorne' envia o resultado de volta
   retorne a + b
fimfuncao

// O ALGORITMO PRINCIPAL
algoritmo "UsoDeFuncao"
var
   resultado_soma: real
inicio
   escrevaL("Vamos usar nossa função de soma.")

   // CHAMAMOS a função e armazenamos o valor retornado em uma variável
   resultado_soma <- Somar(10, 5)

   escrevaL("O resultado de 10 + 5 é: ", resultado_soma)

   // Podemos usar o retorno diretamente em outras expressões
   escrevaL("O dobro do resultado é: ", Somar(10, 5) * 2)
fimalgoritmo
```

## 4. Passagem de Parâmetros

Parâmetros são os canais de comunicação para enviar dados para dentro de um procedimento ou função. Existem duas formas de fazer isso:

### 4.1 Por Valor

- **Como funciona:** Uma **cópia** do valor da variável é enviada para a sub-rotina.
- **Efeito:** Qualquer alteração feita no parâmetro **dentro** da sub-rotina **NÃO AFETA** a variável original fora dela. É o modo padrão para tipos primitivos (`INTEIRO`, `REAL`, etc.).
- **Analogia:** Você tira uma fotocópia de um documento e entrega para alguém. O que a pessoa riscar na cópia não afeta seu documento original.

### 4.2 Por Referência

- **Como funciona:** O **endereço de memória** da variável original é enviado para a sub-rotina. Não é uma cópia, é um "atalho" para o original.
- **Efeito:** Qualquer alteração feita no parâmetro **dentro** da sub-rotina **AFETA DIRETAMENTE** a variável original.
- **Analogia:** Você entrega a chave da sua casa para alguém. A pessoa pode entrar e mudar os móveis de lugar.
- **Importante:** No VisualG, **vetores e matrizes são SEMPRE passados por referência**.

#### Exemplo Prático - Valor vs. Referência

```portugol
procedimento AlteraDados(var n: inteiro; v: vetor[1..1] de inteiro)
inicio
   escrevaL("DENTRO DO PROCEDIMENTO (antes): n=", n, ", v[1]=", v[1])
   n <- n * 2      // Altera a cópia do valor
   v[1] <- v[1] * 2 // Altera o vetor original
   escrevaL("DENTRO DO PROCEDIMENTO (depois): n=", n, ", v[1]=", v[1])
fimprocedimento

algoritmo "ValorVsReferencia"
var
   numero: inteiro
   vet: vetor[1..1] de inteiro
inicio
   numero <- 10
   vet[1] <- 10

   escrevaL("FORA DO PROCEDIMENTO (antes): numero=", numero, ", vet[1]=", vet[1])
   escrevaL("--- Chamando o procedimento ---")
   AlteraDados(numero, vet)
   escrevaL("--- Voltando do procedimento ---")
   escrevaL("FORA DO PROCEDIMENTO (depois): numero=", numero, ", vet[1]=", vet[1])
fimalgoritmo
```

**Resultado Esperado:**

```
FORA DO PROCEDIMENTO (antes): numero=10, vet[1]=10
--- Chamando o procedimento ---
DENTRO DO PROCEDIMENTO (antes): n=10, v[1]=10
DENTRO DO PROCEDIMENTO (depois): n=20, v[1]=20
--- Voltando do procedimento ---
FORA DO PROCEDIMENTO (depois): numero=10, vet[1]=20 // Note que 'numero' não mudou, mas 'vet' sim!
```

## 5. Escopo de Variáveis: Locais vs. Globais

O **escopo** define onde uma variável é visível e pode ser utilizada.

- **Variáveis Globais:** São declaradas na seção `var` principal do algoritmo. Elas são "públicas" e podem ser acessadas e modificadas de qualquer lugar, inclusive dentro de procedimentos e funções.

  - **Prática Ruim:** O uso excessivo de variáveis globais deve ser evitado! Dificulta o rastreamento de quem altera um valor e torna os módulos dependentes do contexto externo. A comunicação via parâmetros é muito mais segura e limpa.

- **Variáveis Locais:** São declaradas **dentro** de um procedimento ou função. Elas são "privadas" e **só existem** enquanto aquela sub-rotina está sendo executada. Ao final da execução, elas são destruídas.

#### Exemplo Prático de Escopo

```portugol
var
   x_global: inteiro // Variável Global

procedimento TesteEscopo()
var
   x_local: inteiro // Variável Local
inicio
   x_global <- 20 // Modifica a variável global
   x_local <- 50  // Cria e modifica a variável local
   escrevaL("Dentro do procedimento: x_global=", x_global, ", x_local=", x_local)
fimprocedimento

algoritmo "EscopoDeVariaveis"
inicio
   x_global <- 10 // Inicializa a variável global
   escrevaL("No início: x_global=", x_global)

   TesteEscopo()

   escrevaL("No fim: x_global=", x_global) // O valor foi alterado pelo procedimento
   // A linha abaixo causaria um ERRO, pois x_local não existe aqui fora.
   // escrevaL("Tentando acessar x_local: ", x_local)
fimalgoritmo
```

## 6. Exercícios Práticos

1.  **Menu com Procedimento**: Crie um `procedimento` chamado `MostrarMenu()` que exiba na tela 3 opções de um programa (ex: 1. Cadastrar, 2. Listar, 3. Sair). No algoritmo principal, chame este procedimento para exibir o menu ao usuário.

2.  **Função de Maioridade**: Crie uma `funcao` chamada `EhMaiorDeIdade(idade: inteiro) : logico`. Ela deve receber uma idade e retornar `VERDADEIRO` se a idade for >= 18 e `FALSO` caso contrário. No programa principal, peça a idade do usuário, chame a função e exiba uma mensagem apropriada com base no retorno.

3.  **Desafio - Média da Turma Modularizada**: Refaça o algoritmo de calcular a média da turma de 5 alunos usando modularização:
    a. Crie um `procedimento` chamado `LerNotas(var notas_turma: vetor[1..5] de real)`. Este procedimento deve receber o vetor (por referência) e preenchê-lo com as notas digitadas pelo usuário.
    b. Crie uma `funcao` chamada `CalcularMedia(notas_turma: vetor[1..5] de real) : real`. Ela deve receber o vetor preenchido, calcular a média e retorná-la.
    c. No algoritmo principal, declare o vetor, chame o procedimento para ler as notas, chame a função para calcular a média e, por fim, exiba o resultado.

## 7. Aplicações Reais

- **Bibliotecas de Software**: Em programação, usamos constantemente bibliotecas de funções e procedimentos prontos. `Math.sqrt()` (raiz quadrada), `print()` (escrever na tela), `sort()` (ordenar) são todas sub-rotinas que outros programadores criaram para nós reutilizarmos.
- **Desenvolvimento de Jogos**: Um motor de jogo (como Unity ou Unreal) oferece milhares de funções e procedimentos para lidar com física (`AplicarForca()`), gráficos (`DesenharObjeto()`) e som (`TocarAudio()`), permitindo que os desenvolvedores se concentrem na lógica do jogo.
- **Aplicações Web**: Cada botão em uma página pode chamar uma função JavaScript diferente para validar um formulário, enviar dados para um servidor ou exibir uma animação.

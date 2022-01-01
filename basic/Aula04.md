# Aula 4 - Variaveis

Como dito na [Aula 2](Aula02.md) no Rust os dados possuem tipos, mas não fique assustado o Rust da uma mãozinha brava cuidando dos tipos para você, mas sempre bom ter eles na cabeça.

Para declararmos uma variavel usamos a palavra reservada `let`

```rs
// atribuindo variaveis com tipo implicito
let i = 4; // Por ser um numero inteiro o rust vai pressupor que seja um i32

// Atribuindo variaveis com tipo explicito
let j: u8 = 3; // usando o tipo : depois do nome da variavel
let k = 0_f32; // usando o tipo no final de um numero (so funciona nos numeros) (obs: não precisa do _ mas é recomendado para almentar a legibilidade)
let l = 0 as usize; // usando o `as` (obs: `as` pode ser usado tambem para dar parse em tipos simples, exemplo abaixo)

// Parse com `as`
let a: usize = 3;
let b: u32 = a as u32; // no caso nem precisaria do : coloquei para ficar mais explicito o parse
```

Bem criado a variavel bora usar ela em um `println!`

```rs
let i = 3;
println!("i = {}", i);
```

Mas não é so de valores constantes que vive um programa, podemos em rust assim como outras langs usar expressões matematicas

```rs
let soma = 1 + 3;
let subtracao = 3 - 2;
let divisao = 10 / 5;
let multiplicação = 3 * 3;
let operador_mod = 3 % 5;

// Demais operadores vc pode acessar pontilhando o tipo inteiro e float 
let potencia = 4.pow(3);
```

## O problema dos Mutaveis
Se você brincou com as variaveis você pode ter se deparado com isso:
```rs
let i = 2;
let j = 3;
i = j + 2; // Erro
```
Bem isso ocorre porque as variaveis do rust são imutaveis por padrão. Mas então como resolver????

Rust tem duas abordagens possiveis, abordagem mutavel e imutavel. A mutavel você já conhece que seria a reatribuição do valor, mas com um detalhe no rust você precisa usar o `mut`para mudar o tipo da variavel para mutavel

```rs
let mut a = 2;
a = 5; // Sem Erros \o/
```

Ainda sim essa abordagem não é a mais adequada para alguns caso, devendo sempre que possivel preferir pela abordagem imutavel, no nosso caso seria adicionando uma nova variavel para recever o valor.

Mas e o caso de precisar realmente de substituir o valor pois não quer manter o valor anterior e não quer criar um novo nome de variavel. Nesse caso temos o shadow var, que é a tecnica de sobrescrever uma declaração:

```rs
let a = 3;
let a = 4; // Shadow, a original morre e da espaço ao novo a

println!("{}", a); // 4
``
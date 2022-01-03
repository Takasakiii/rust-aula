# Aula 4 - Variáveis

Como dito na [Aula 2](Aula02.md), no Rust os dados possuem tipos, mas não fique assustado, o Rust da uma mãozinha brava cuidando dos tipos para você, mas sempre bom ter eles na cabeça.

Para declararmos uma variável usamos a palavra reservada `let`:

```rs
// Atribuindo variáveis com tipo implícito:
let i = 4; // Por ser um número inteiro, o Rust vai pressupor que seja um i32.

// Atribuindo variáveis com tipo explicito:
let j: u8 = 3; // Usando o tipo com : depois do nome da variável
let k = 0_f32; // Usando o tipo no final de um número (só funciona nos números) (obs: não precisa do _, mas é recomendado para aumentar a legibilidade).
let l = 0 as usize; // Usando o `as` (obs: `as` pode ser usado também para dar parse em tipos simples, exemplo abaixo).

// Parse com `as`:
let a: usize = 3;
let b: u32 = a as u32; // No caso nem precisaria do `:`, coloquei para ficar mais explicito o parse.
```

Bem, criado a variável, bora usar ela em um `println!`

```rs
let i = 3;
println!("i = {}", i);
```

Mas não é so de valores constantes que se vive um programa, podemos em Rust, assim como outras langs, usar expressões matemáticas:

```rs
let soma = 1 + 3;
let subtracao = 3 - 2;
let divisao = 10 / 5;
let multiplicação = 3 * 3;
let operador_mod = 3 % 5;

// Demais operadores vc pode acessar pontilhando os tipos inteiro e float:
let potencia = 4.pow(3);
```

## O problema dos Mutáveis
Se você brincou com as variáveis você pode ter se deparado com isso:
```rs
let i = 2;
let j = 3;
i = j + 2; // Erro
```

Bem, isso ocorre porque as variáveis do Rust são imutáveis por padrão. Mas então, como resolver????

Rust tem duas abordagens possíveis, abordagem mutável e imutável. A mutável você já conhece, que seria a reatribuição do valor, mas com um detalhe: no Rust você precisa usar o `mut`para mudar o tipo da variável para mutável:

```rs
let mut a = 2;
a = 5; // Sem Erros \o/
```

Ainda sim essa abordagem não é a mais adequada para alguns casos, devendo sempre que possível preferir pela abordagem imutável, que no nosso caso seria adicionando uma nova variável para receber o valor.

Mas e no caso de precisar realmente substituir o valor, pois não se quer manter o valor anterior e não se quer criar um novo nome de variável? Nesse caso, temos o shadow var, que é a técnica de sobrescrever uma declaração:

```rs
let a = 3;
let a = 4; // Shadow, a original morre e da espaço ao novo a.

println!("{}", a); // 4
```

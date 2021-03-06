# Referências, Lifetimes e Empréstimos

A partir de agora, precisamos entrar em um assunto um pouco mais técnico para continuarmos, mas que nos dará suporte quando começarmos a usar tipos complexos.

## Referências
Referências são variáveis que possuem em seu interior o endereço de outra variável, mas para que isso??

Bem, atualmente estamos mexendo em variáveis numéricas, em que seu tamanho é pequeno o suficiente para que uma cópia dessa variável seja algo trivial para o computador, mas, quando temos strings, vetores, structs, etc, isso deixa de ser trivial, ainda mais se você tiver aquele vetor de 1000 posições...

Se você estiver se perguntando como isso ocorre ou o que está acontecendo, bem, vou explicar:

Na sua memória RAM, você possui vários segmentos onde serão guardados os dados (aproximadamente 1 segmento = 1 byte), e, para a CPU fazer acesso a esses segmentos, eles são endereçados usando um número sequencial para referenciar a cada um deles. Então uma referência é uma variável que contém o endereço da variável que ela representa.

Então, como declaramos uma referência no Rust:

```rs
let a = 3; // Variável original.
let ref_a = &a; // Variável contendo a referência de a.

println!("A = {}, referência de A = {}", a, ref_a); // 3 e 3
```

Tá, mas a gente não está só limitado a leitura, referências também podem ser usadas para escrita:

```rs
let mut a = 5;
let b = &mut a;

*b += 10; // Forma reduzida para b = b + 10, * é para falar pro Rust explicitamente que estamos mexendo no valor da variável a, e não no valor de b.
println!("A = {}", a); // A = 15
```

## Lifetimes
A partir do momento que você tem referências você ganha alguns probleminhas novos, já que o Rust vai te ajudar com o sistema de lifetime.

O problema mais comum é: "não tenho mais a variável original e vou acessar ou mutar minha referência":

```rs
let b = { // Isso é um bloco, você pode usa-los para delimitar partes de um código, nesse caso estou usando para rodar um código isolado e o resultado do código será atribuído na variável b.
    let a = 4;
    &a // Vai passar a referência de a para b.
    // Fim do bloco, a não existe mais.
};
```

Por mais que seja um exemplo simples, é muito mais comum que você imagina, bem, langs como C e C++ permitem que a atribuição acima seja feita, mas, quando você usa a variável, toma o famoso `segmentation fault (core dumped)`. Mas, para evitar isso, o compilador do Rust lê cada segmento de código e define lifetimes `'a`, `'b`, `'c`, ..., etc, como no exemplo abaixo, que mostra como ele leria as lifetimes do nosso código:

```rs
// Primeira lifetime 'a.
let b = {
    // Bloco cria a lifetime 'b.
    let a = 4; // Variável está vinculado a 'b.
    &a // Nesse momento, o Rust avalia a lifetime do a (verificando que ele nasceu no 'b) e que ele está tentando mandar uma referência para o 'a e assim não permitindo a compilação.
};
```

Além de blocos, Rust também avalia lifetimes dos dados internos de structs, vetores e etc.

## Empréstimos (borrow)
Outro erro comum que a gente acaba fazendo sem querer quando tem possibilidade de referências são inconsistências de dados quando usamos referências mutáveis.

Por exemplo: tentar criar duas referências mutáveis para a mesma variável ao mesmo tempo, ler uma variável enquanto está mutando ela, e até mesmo mexer na variável original enquanto possui empréstimos que podem causar problemas. Enfim, se o compilador estiver falando de borrow, verifique suas referências para ver se não está tentando fazer algo que você não queria.

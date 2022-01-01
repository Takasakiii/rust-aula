# Referências, Timelifes e Emprestimos

A partir de agora, precisamos entrar em um assunto um pouco mais técnico para continuarmos, mas que nos dará suporte quando começarmos a usar tipos complexos.

## Referências
Referências são variáveis que possuem em seu interior o endereço de outra variável, mas para que isso??

Bem, atualmente estamos mechendo em variaveis numéricas, que seu tamanho é pequeno suficiente para que uma cópia dessa variável seja algo trivial para o computador, quando temos strings, vetores, structs, etc., isso deixa de ser trivial, ainda mais se você tiver aquele vetor de 1000 posições....

Se você tiver se perguntando como isso ocorre ou o que tá acontecendo, bem vou explicar:

Na sua memória RAM, você possui varios segmentos onde serão guardadado os dados (aproximadamente 1 segmento = 1 byte), e, para a CPU fazer acesso a esses segmentos, eles são endereçados usando um número sequencial para referenciar cada um deles. Então uma referência é uma variavel que contém esse endereço da variável que ela representa.

Então como declaramos uma referência no Rust:
```rs
let a = 3; // variável original
let ref_a = &a; // variável contendo referência de a

println!("A = {}, referência de A = {}", a, ref_a); // 3 e 3
```

Tá, mas a gente não está só limitado a leitura, referências também podem ser usadas para escrita:

```rs
let mut a = 5;
let b = &mut a;

*b += 10; // Forma reduzida para b = b + 10, * é para falar pro rust explicitamente que estamos mechendo no valor da variável a e não no valor de b;
println!("A = {}", a); // A = 15
```

## Lifetimes
A partir do momento que você tem referências você ganha alguns probleminhas novos, já que o Rust vai te ajudar com o sistema de lifetime.

O problema mais comum é: "não tenho mais a variavel original e vou acessar ou mutar minha referência":

```rs
let b = { // isso é um bloco, você pode usa-los para delimitar partes de um codigo, nesse caso estou usando para rodar um codigo isolado e o resultado do codigo será atribuido na variável b
    let a = 4;
    &a // vai passar a referência de a para o b
    // fim do bloco, a não existe mais
};
```
Por mais que seja um exemplo simples, é muito mais comum que você imagina, bem, lang como C e C++ permitem que a atribuição acima seja feita, mas quando você usa a variável toma o famoso `segmentation fault (core dumped)`. Mas para evitar isso, o compilador do Rust lê cada segmento de codigo e define lifetimes `'a`, `'b`, `'c`, ..... etc., como no exemplo abaixo, que mostra como ele leria as lifetimes do nosso código:

```rs
// primeira lifetime 'a
let b = {
    // bloco cria a lifetime 'b
    let a = 4; // variável está vinculado a 'b
    &a // Nesse momento, o Rust avalia a lifetime do a (verificando que ele nasceu no 'b) e que ele está tentando mandar uma referência para o 'a e assim não permitindo a compilação
};
```

Alem de blocos, Rust tambem avalia lifetimes dos dados internos de structs, vetores e etc.

## Empréstimos (borrow)
Outro erro comum que a gente acaba fazendo sem querer quando tem possibilidade de referências são inconsistencias de dados quando usamos referência mutáveis.

Por exemplo: tentar criar duas referências mutáveis para a mesma variável ao mesmo tempo, ler uma variável enquanto está mutando ela, e até mesmo mexer na variável original enquanto possui empréstimos que podem causar problemas. Enfim, se o compilador estiver falando de borrow, verifique suas referências para ver se não está tentando fazer algo que você não queria.
# Referencias, Timelifes e Emprestimos

A partir de agora precisamos entrar para continuarmos e um assunto um pouco mais tecnico, mas que nos dará suporte quando começarmos a usar tipos complexos.

## Referencias
Referencias são variaveis que possuem em seu interior o endereço de outra variavel, mas para que isso??

Bem atualmente estamos mechendo em variaveis numericas, que seu tamanho é pequeno suficiente para que uma copia dessa variavel seja algo trivial para o computador, quando temos strings, vetores, structs, etc isso deixa de ser trivial, ainda mais se você tiver aquele vetor de 1000 posições....

Se você tiver se perguntando como isso ocorre ou que ta acontecendo, bem vou explicar:

Na sua memoria RAM você possui varios segmentos onde serão guardadado os dados (aproximadamente 1 segmento = 1 byte) e para a CPU fazer acesso a esses segmentos eles são endereçados usando um numero sequencial para referenciar cada um deles. Então uma referencia é uma variavel que contem esse endereço da variavel que ela representa.

Então como declaramos uma referencia no Rust:
```rs
let a = 3; // variavel original
let ref_a = &a; // variavel contendo referencia de a

println!("A = {}, referencia de A = {}", a, ref_a); // 3 e 3
```

Ta mais a gente não está só limitado a leitura, tambem referencias podem serem usadas para escrita:

```rs
let mut a = 5;
let b = &mut a;

*b += 10; // Forma reduzida para b = b + 10, * é para falar pro rust explicitamente que estamos mechendo no valor da variavel a e não no valor de b;
println!("A = {}", a); // A = 15
```

## Lifetimes
A partir do momento que você tem referencias você ganha alguns probleminhas novos que o rust vai te ajudar com o sistema de lifetime.

O problema mais comum é não tenho mais a variavel original e vou acessar ou mutar minha referencia:
```rs

let b = { // isso é um bloco, você pode usa-los para delimitar partes de um codigo, nesse caso estou usando para rodar um codigo isolado e o resultado do codigo será atribuido na variavel b
    let a = 4;
    &a // vai passar a referencia de a para o b
    // fim do bloco a não existe mais
};
```
Por mais que seja um exemplo simples, é muito mais comum que você imagina, bem lang como C / C++ permitem que a atribuição acima seja feita e quando você usa a variavel toma o famoso `segmentation error (core dumped)`. Mas para evitar isso o compilador do rust le cada segmento de codigo e define lifetimes `'a`, `'b`, `'c`, ..... etc, exemplo como ele leria as lifetime do nosso codigo:

```rs
// primeira lifetime 'a
let b = {
    // bloco cria a lifetime 'b
    let a = 4; // variavel está vinculado a 'b
    &a // Nesse momento rust avalia a lifetime do a (verificando que ele nasceu no 'b) e que ele está tentando mandar uma referencia para o 'a e assim não permitindo a compilação
};
```

Alem de blocos, rust tambem avalia lifetimes dos dados internos de structs, vetores e etc.

## Emprestimos (borrow)
Outro erro comuns que a gente acaba fazendo sem querer quando tem possibilidade de referencias são inconsistencias de dados quando usamos referencia mutaveis

Por exemplo tentar criar duas referencias mutaveis para a mesma variavel ao mesmo tempo, ler uma variavel enquanto esta mutando ela, e até mesmo mecher na variavel original enquanto possui emprestimos que podem causar problemas. Enfim se o compilador estiver falando de borrow verifique suas referencias para ver se não está tentando fazer algo que você não queria.
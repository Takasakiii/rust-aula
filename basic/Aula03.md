# Estrutura de Código Basica

Bem nosso arquivo principal é o `src/main.rs` e nele temos nossa função principal a `main`

```rs
fn main() { // Essa é a função main, ela precisa estar no arquivo main.rs para que ela defina o ponto de inicio do nosso programa
    println!("Hello World!"); // E aqui vemos o println que é o macro responsavel pelo stdout (saida de console). Bem não se preocupem de saberem que é um macro, apenas posso dizer inicialmente que é uma função especial.
}
```

Claro como deve imaginar alterar o `Hello World!` vai alterar a frase que seu programa mostra no console quando é iniciado, alem de poder por texto fixo o `println!`tambem aceita frags de formatação (no rust chamamos placeholders, espaçadores, debug flag, etc de flag de formatação) como por exemplo:

```rs
println!("Meu nome é {}, tenho {} anos", "Jorge", 13);

// Lembre-se cada {} que adicionar deve ter um argumento correspondente no println!
```

Algumas flags: 

```
{} - Display flag, chama a trait display daquele tipo (falarei mais disso depois) como se fosse o .toString() das outras langs

{:?} - Debug flag, chama a trait Debug daquele tipo, ele vai mostrar informações extras para fazer a depuração daquele tipo

{:#?} - Debug with Format, mostra o Debug do elemento porem formatado para melhor visualização

{:04} - Adiciona 0 a esqueda em um numero

{:^4} - Centraliza um Display em 4 espaçamentos

```
Coloquei apenas algumas flags mais usadas, mas se precisarem de alguma mais especifica pode consultar a [documentação](https://doc.rust-lang.org/std/fmt/).




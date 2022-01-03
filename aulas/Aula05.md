# Aula 5 - If e Match

## If
Como em grande maioria das linguagens de programação, Rust também conta com `if`, que funciona praticamente igual a todas as linguagens com base C:

```rs
// Nos ifs do Rust os parenteses são opcionais, assim como no Python
if idade > 18 {
    println!("Maior de idade")
} else {
    println!("Menor de idade")
}
```

Os comparadores do Rust são:
 - `==` - Igualdade 
 - `>` - Maior que
 - `<` - Menor que
 - `>=` - Maior ou Igual
 - `<=` - Menor ou Igual 

E também há os operadores lógicos:
 - `&&` - E (And)
 - `||` - Ou (Or)
 - `!` - Negação (Not)

## Match
Match é uma estrutura usada do sistema de pattern matching (aprofundaremos mais sobre o pattern matching no futuro) do Rust para efetuar comparações de forma mais simples:

```rs
match idade {
    0..18 => println!("Menor de idade"), // 0..18 se referencia como um range. Ranges representam um intervalo numérico, nesse caso indo do 0 até o 17.
    18 => println!("Completou 18!!!!"),
    idade @ 19..=100 => println!("{} ano(s) maior que 18", idade - 18), // O range, nesse caso, por conter o =, ele inclui no intervalo o número 100.
    // O operador @ vai colocar na variável idade qual dos valores que deu match com o range.
    idade if idade < 0 => println!("Idade é menor que 0"), // Você ainda pode usar o if como um guard da condição para deixar ela mais específica
    _ => println!("Velho") // O _ fala para o Rust que não precisamos do valor.
    // No Rust a última rota de um match, que recebe apenas uma variável ou _, é chamada de default, pois tudo que não se enquadrar nos anteriores cairão aqui.
}
```

> Observação: Ao usar o `match`, o Rust vai pedir que todas as variantes possíveis para aquela variável tenha uma rota válida, por isso a importância da rota default.

## Usando o If ou Match para retornar valores
No Rust podemos usar o If ou Match para obtermos um valor já tratado sem o uso de variáveis auxiliares, quase que como um operador ternário

```rs
let condicional = true;
let valor = if condicional {
    // O valor que deseja retornar precisa estar na última linha do bloco, sem o `;` no final.
    20
} else {
    // Quando usamos o `if` para retorno precisamos sempre fornecer sempre um else.
    30
};

// Ifs e Matches com retorno usam `;` no final.
```

# Aula 11 - Empréstimos de Tipos Complexos

Como dito nas aulas anteriores, tipos complexos muitas vezes não podem ser simplesmente copiados como os tipos básicos. Se tentarmos reatribuir uma variável complexa a uma nova variável ou passar uma variável complexa para uma função, o Rust vai realizar um `move`. Alguns tipos, no entanto, mesmo sendo complexos podem ser copiados (comentaremos mais sobre isso quando falarmos de `traits`), mas, mesmo que possam ser copiados, devem ser evitados, por essa copia ainda ter um custo significativo.


Exemplo de `move`:

```rs
let nome_a = "Pedro André Pepperoni".to_owned();
let nome_b = nome_a; // Para o Rust, você definiu que agora o conteúdo de nome_a deve ficar em nome_b.

println!("Nome A = {}\nNome B = {}", nome_a, nome_b); // Ocorre um erro, pois o nome_a, por conta do `move`, perde seu conteúdo para nome_b
```

Pera, então o Rust move o conteúdo de uma variável a outra??? Isso não é pesado???? Bem, na realidade o Rust não move nenhum dado, apenas o interpreta na hora da compilação, para dar a possibilidade de poder passar o conteúdo de uma variável para outra.

Outra forma de `move` é de elementos internos de estruturas complexas, como mover elementos internos de um `Array` para uma variável.

```rs
let arr = ["a".to_owned(), "b".to_owned(), "c".to_owned()];

let b = arr[0]; // Erro, ao fazer isso você está tentando mover um item do array para fora, pois como o array não é volátil, não poderia mover itens para fora dele.
```

> Foi usado tipo `String` ao invés de `str`, pois a `str` é uma referência, e referências podem ser copiadas, por serem apenas um endereço.

## Usando referências para empréstimos de dados complexos (borrow)

Você pode usar referências para manipular de forma mais controlada seus tipos complexos, fazendo um empréstimo do conteúdo da variável sem tomá-la.

Os empréstimos podem ser usados para leitura de dados, compartilhamento da estrutura de dados com varias funções, alterar elementos de forma controlada, etc.

Mas elas não resolvem tudo, elas não substituem a variável original em casos de funções de consumo ou para manter valores quando a variável original deixa de existir.

```rs
// Consertando os exemplos vistos anteriormente.

// Exemplo 1:
let nome_a = "Pedro André Pepperoni".to_owned();
let nome_b = &nome_a; // Pegamos a referência de nome_a.

println!("Nome A = {}\nNome B = {}", nome_a, nome_b);

// Exemplo 2:
let arr = ["a".to_owned(), "b".to_owned(), "c".to_owned()];

let b = &arr[0];
```

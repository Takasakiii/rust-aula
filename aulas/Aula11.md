# Aula 11 - Emprestimos de Tipos Complexos

Como dito nas aulas anteriores tipos complexos muitas vezes não podem ser simplesmente copiados como os tipos basicos, caso tentarmos reatribuir uma variavel complexa a uma nova variavel ou passar uma variavel complexa para uma função o rust vai realizar um `move` (alguns tipos no entanto mesmo complexo podem ser copiados, comentaremos mais sobre isso quando falarmos de `traits`, mas mesmo que possam ser copiados devem ser evitados por essa copia ainda ter um custo significativo).


Exemplo de `move`:
```rs
let nome_a = "Pedro André Peperone".to_owned();
let nome_b = nome_a; // Para o rust você definiu que agora o conteudo de nome_a deve ficar em nome_b

println!("Nome A = {}\nNome B = {}", nome_a, nome_b); // Ocorre um erro pois o nome_a por conta do `move` perde seu conteudo para nome_b
```

Pera então o Rust move o conteudo de uma variavel a outro??? Isso não é pesado???? Bem na realidade o Rust não move nenhum dado apenas interpreta na hora da compição para dar a possibilidade de poder passar conteudo de uma variavel para outra.

Outra forma de `move` é de elementos internos de estruturas complexas como mover elementos internos de um `Array` para uma variavel.

```rs
let arr = ["a".to_owned(), "b".to_owned(), "c".to_owned()];

let b = arr[0]; // Erro, ao fazer isso você esta tentando mover um item do array para fora, como o array não é volátio não poderia mover itens para fora dele
```

> Foi usado tipo `String` ao invez de `str` pois a `str` é uma referencia e referencias podem ser copiadas por serem apenas um endereço.

## Usando referencias para emprestimos de dados complexos (borrow)

Você pode usar referencias para manipular de forma mais controlada seus tipos complexos, fazendo um emprestimo do conteudo da variavel sem toma-la.

Os emprestimos podem ser usados para leitura de dados, compartilhamento da estrutura de dados com varias funções, alterar elementos de forma controlada, etc.

Mas elas não resolvem tudo, elas não substituem a variavel original em casos de funções de consumo ou para manter valores quando a variavel original deixa de existir.

```rs
// Consertando os exemplos vistos anteriormente
// Exemplo 1
let nome_a = "Pedro André Peperone".to_owned();
let nome_b = &nome_a; // Pegamos a referencia de nome_a

println!("Nome A = {}\nNome B = {}", nome_a, nome_b);

// Exemplo 2

let arr = ["a".to_owned(), "b".to_owned(), "c".to_owned()];

let b = &arr[0];
```

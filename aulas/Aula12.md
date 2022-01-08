# Aula 12 - Tuplas e Structs

## Tuplas

Tupla é uma estrutura de dados relativamente simples, se assemelha ao `Array`, porém seus elementos podem ter tipos heterogêneos (ou seja, cada elemento pode ter um tipo diferente), e são usadas principalmente em retornos para funções de retornos múltiplos.

```rs
let my_tuple = ("abacaxi", 12, [1, 2, 4]);

let _arr = &my_tuple.2; // Usamos .index para acessar as diferentes posições da tupla.
```

Outra forma comum de acesso a elementos da tuplas é com o pattern matching, desconstruindo a tupla em variáveis independentes:

```rs
let my_tuple = ("abacaxi", 12, [1, 2, 4]);
let (_fruta, _idade, _arr) = my_tuple; // Desde que ambos os lados sejam equivalentes, o pattern match permitirá a atribuição.
```

## Structs

`struct`s são estruturas de dados heterogêneas fixas (onde cada `struct` aceita somente o mesmo número de elementos e o mesmo tipo de dados para as mesmas posições). Elas podem ter 3 variações: a struct vazia, a struct de tupla e a struct "normal".

```rs
struct MyEmptyStruct; // Elas são geralmente usadas para implementar traits ou serem usadas como um tipo Vazio.

struct MyTupleStruct(u8, String, MyEmptyStruct); // Funciona praticamente como uma tupla, porém podendo ter permissões (falaremos mais sobre isso na aula de Módulos).

struct MyNormalStruct {
    nome: String,
    idade: u8,
    is_man: bool
}
// Os campos internos são acessado pelo . e o nome do campo.
```

Assim como as tuplas, structs também podem ser desconstruídas com pattern matching:

```rs
let my_struct = MyNormalStruct {
    nome: "Brunin".to_owned(),
    idade: 20,
    is_man: true
};

let MyNormalStruct {
    nome,
    idade,
    is_man
} = my_struct;
```

> Sempre que desconstruirmos uma struct seu tipo deve ser explicitado.

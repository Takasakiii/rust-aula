# Aula 12 - Tuplas e Structs

## Tuplas
Tuplas é uma estrutura de dados relativamente simples, se assemelha ao `Array`, porem seus elementos pode ter tipos heterogeneos (ou seja cada elemento pode ter um tipo diferente), são usadas principalmente em retornos para funções de retornos multiplos.

```rs
let my_tuple = ("abacaxi", 12, [1, 2, 4]);

let _arr = &my_tuple.2; // Usamos .index para acessar as diferentes posições da tupla
```

Outra forma comum de acesso a elementos da tuplas é com o match-pattern, desconstruindo a tupla em variaveis independentes

```rs
let my_tuple = ("abacaxi", 12, [1, 2, 4]);
let (_fruta, _idade, _arr) = my_tuple; // Des que ambos os lados sejam equivalentes o patern match permitirá a atribuição
```

## Structs
`struct`s são estruturas de dados heterogeneas fixas (onde cada `struct` aceita somente o mesmo numero de elementos, o mesmo tipo de dados para as mesmas posições). Elas podem ter 3 variações a struct vazia, a struct de tupla e a struct "normal".

```rs
struct MyEmptyStrct; // Elas são geralmente usadas para implementar traits ou serem usadas como tipo Vazio

struct MyTupleStruct(u8, String, MyEmptyStrct); // Funcioname praticamente como uma tupla porem podendo ter permissoes (falaremos mais sobre isso na aula de Modulos)

struct MyNormalStruct {
    nome: String,
    idade: u8,
    is_man: bool
}
// Os campos internos são acessado pelo . e o nome do campo
```

Assim como as tuplas structs tambem podem ser descontruidas com patern match

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
> Sempre que desconstruimos uma struct seu tipo deve ser explicitado


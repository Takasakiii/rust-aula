# Aula 9 - Arrays

Array é o tipo mais simples dos tipos complexos, permitem armazenar uma quantidade de vários elementos de um mesmo tipo, porem seu tamanho é fixo, os elementos internos são acessados a partir de um index (que corresponde ao deslocamento na memoria a partir do primeiro elemento).

```rs
// Declaração implícita:
let my_arr = [1, 2, 3, 4];

// Declaração explícita:
let my_arr: [u8; 4] = [1, 2, 3, 4]


// Acessando um item:
println!("{}", my_arr[2]) // 3
```

Rust também oferece um tipo de criação de arrays para arrays em que todos os elementos são iguais:

```rs
let my_big_array = [10; 1000] // Dessa forma o Rust criará um array de 1000 posições, onde todas as posições terão o número 10.
```

Aprendemos a definir um array e acessar elemento por elemento de um array. Vamos então aprender a iterar sobre seus valores:

## Iterando de Modo Imperativo
Ou seja, usando o for ou outras estruturas de repetição:

```rs
let arr = [1, 2, 3, 4, 5, 6];
// while
let mut i = 0;
while i < arr.len() {
    println!("{}", arr[i])
}

// for
for i in arr.into_iter() { // into_iter retorna o iterador do array, mas você pode achar sintaxe com o iter implícito, exemplo abaixo:
    println!("{}", i);
}

// for com into_iter() implícito:
for i in arr { 
    println!("{}", i);
}

// Também podemos usar um iter mutável para poder alterar os dados do array:
let mut arr = [1, 2, 3, 4];

for i in arr.iter_mut() {
    *i += 2; // adiciona 2 a cada elemento, como i é uma referencia mutável se usa o * para explicitar que a alteração é no elemento que ele está representando.
}

// E também tem a versão implícita:
for i in &mut arr {
    *i += 10;
}
```

## iter(), into_iter() e iter_mut()
Bem como vocês viram acima, o Rust tem várias forma de iteradores, nos exemplos acima vimos um exemplo de `into_iter` e de `iter_mut`, mas qual é a relação deles com o `iter`?

Estamos atualmente usando números como elementos dos nossos arrays, como disse na [Aula 07](Aula07.md), números são extremamente simples e copiar eles é trivial para a CPU, mas imagine strings ou outros arrays, esses elementos são complexos e o rust não pode ficar simplesmente copiando, pois isso teria muito custo de CPU, então o rust `move` esses elementos, e a gente vai conversar sobre mover valores mais adiante. 

Para isso usamos referências, mas, se você usou o `into_iter` de forma implícita ou explícita, percebeu que o valor que o for te entrega não é uma referencia, e sim uma cópia. Nesse caso, usamos o `iter`, que nos retorna uma referência. 

```rs
let arr = [[1, 2, 3], [4, 5, 6], [7, 8, 9]];

// Forma explícita:
for sub_arr in arr.iter() {
    for i in sub_arr {
        println!("{}", i);
    }
}

// Forma implícita:
for sub_arr in &arr {
    for i in sub_arr {
        println!("{}", i);
    }
}
```

E o `iter_mut`? Bem, ele é como o nome diz um iter que retorna uma referência, porém essa referência é mutável, permitindo você mudar o valor do elemento iterado.

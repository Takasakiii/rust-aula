# Aula 9 - Arrays

Array é o tipo mais simples dos tipos complexos, permitem armazenar uma quantidade varios de elementos de um mesmo tipo, porem seu tamanho é fixo, os elementos internos são acessados a partir de um index (que corresponde ao deslocamento na memoria a partir do primeiro elemento).

```rs
// declaração implicita
let my_arr = [1, 2, 3, 4];

// declaração explicita
let my_arr: [u8; 4] = [1, 2, 3, 4]


// Acessando um item
println!("{}", my_arr[2]) // 3
```

Rust tambem oferece um tipo de criação de arrays para arrays que todos os elementos são iguais

```rs
let my_big_array = [10; 1000] // Dessa forma o rust cria-ra um array de 1000 posições onde todas as posições terá o numero 10
```

Aprendemos a definir um array e acessar elemento por elemento de um array. Vamos então aprender a iterar seus valores

## Iterando de Modo Iperativo
Ou seja usando o for ou outras estruturas de repetição
```rs
let arr = [1, 2, 3, 4, 5, 6]
// while
let mut i = 0;
while i < arr.len() {
    println!("{}", arr[i])
}

// for
for i in arr.into_iter() { // into_iter retorna o iterador do array, mas você pode achar sintaxe com o iter implicito, exemplo abaixo
    println!("{}", i);
}

// for into_iter() implicito
for i in arr { 
    println!("{}", i);
}

// E tambem podemos usar um iter mutavel para poder alterar os dados do array
let mut arr = [1, 2, 3, 4];

for i in arr.iter_mut() {
    *i += 2; // adiciona 2 a cada elemento, como i é uma referencia mutavel se usa o * para explicitar que a alteração é no elemento que ele está representando.
}

// E tambem tem a vesão implicita
for i in &mut arr {
    *i += 10;
}
```

## iter(), into_iter() e iter_mut()
Bem como vocês viram acima o rust tem varias forma de iteradores, nos exemplos de cima vimos um exemplo de `into_iter` e de `iter_mut` mas qual é a relação deles com o `iter`?

Estamos atualmente usando numeros como elementos dos nossos arrays, como disse na [Aula 07](Aula07.md) numeros são extremamente simples e copiar eles é trivial para a CPU, mas imagine strings ou outros arrays, esses elementos são complexos e o rust não pode ficar simplesmente copiando, pois isso teria muito custo de CPU, então o rust `move` esses elementos, e a gente vai conversar sobre mover valores mais adiante. 

Para isso usamos referencias, mas se você usou o `into_iter` de forma inplicita ou explicita percebeu que o valor que o for te entrega não é uma referencia e sim uma cópia, nesse caso usamos o `iter` que nos retorna uma referencia. 

```rs
let arr = [[1, 2, 3], [4, 5, 6], [7, 8, 9]];

// Forma explicita
for sub_arr in arr.iter() {
    for i in sub_arr {
        println!("{}", i);
    }
}

// Forma inplicita
for sub_arr in &arr {
    for i in sub_arr {
        println!("{}", i);
    }
}
```

E o `iter_mut`? Bem ele é como o nome diz um iter (que retorna uma referencia) porem essa referencia é mutavel, permitindo você mudar o valor do elemento iterado.


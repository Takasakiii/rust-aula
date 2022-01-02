# Aula 6 - Loops

No Rust temos 3 estruturas para fazermos loops imperativos (que são as meneiras classicas de realizar uma repetição), o `while`, `for` e o `loop`

## While
`while` é o mais padrão, ele usa uma condicional (ou boolean) lógica e enquanto a mesma for verdadeira ele inicia e repete o bloco.

```rs
let mut i = 0;
while i < 10 {
    println!("{}", i);
    i += 1; // i+=1 é uma redução de i = i + 1
}
```

## For
`for` do Rust semelha-se a um `foreach` de outras linguagens ou até mesmo do `for` do python.

Ele precisa de um iterador e vai retornar o elemento de acordo com a quantidade de repetição (Vamos ver mais sobre iteradores na Aula 9)
```rs
// O i é o elemento atual que o for obteve do nosso iterador (no caso o range)
// Ranges pode ser usados como iteradores permitindo tanto usar eles em um for tanto usar eles em funções que precisam de iteradores (Veremos mais na Aula 9)
for i in 0..10 {
    println!("{}", i);
}
```


## Loop
`loop` é uma estrutura do Rust para repetição infinita/indeterminada, como se fosse um `while true {}` mas com algumas funcionalidades extras.

```rs
// Transformação dos exemplos anteriores para loop
let mut i = 0;
loop {
    if i >= 10 {
        break; // break faz com que qualquer estrutura de repetição seja parada definitivamente
        // Outro comando que pode ser usado com estruras de repetição é o `continue` que permite interromper o ciclo da estrutura e dar um skip para a proxima iteração
    }

    println!("{}", i);
    i += 1;
}
```

O `loop` quando é finalizado com um `break` ele pode retornar um valor (assim como um `return` em uma função)

```rs
let mut i = 0;
let decimo_valor = loop {
    if i >= 10 {
        break i;
    }

    i += 1;
};
```

## Label Nested Loops
As vezes podemos ter situações que trabalhamos com varias estruturas de repetição alinhadas e queremos usar o `break` ou `continue`, dependendo de qual dos loops alinhados você quer aplicar o efeito isso pode ser problematico. Para isso no Rust podemos dar apelidos aos nossos loops e usar eles nos `break` e `continue`.

```rs
// Usando ' + nome + : dei um nome para o meu for
'my_external_for: for i in 0..100 {
    let mut j = 3;
    'my_internal_loop: loop {
        if j > 10 {
            // E posso passar o nome para o break para ele interromper o loop que eu desejo
            break 'my_external_for;
        }
        j += 2;
    }
}
```
# Aula 18 - Unsafe

Como dito na [Aula15](Aula15.md) `static` podem ser mutáveis com `unsafe`, mas o que é `unsafe`???

No rust temos um compilador muito protetor, ele não deixa você fazer nada que daria problema com seu programa, te protegendo de referencias invalidas e race condition.

Mas ainda ele é uma linguagem de sistema, e não pode proibir de você acessar elementos mais raw da programação de sistemas.

Com `unsafe` você poderá:

- Desreferenciar ponteiros
- Executar funções unsafe
- implementar traits unsafe
- usar `static` mutável
- Acessa campos em `union`

## Raw Pointers

`Raw Pointer` é o ponteiro clássico da computação, o mesmo encontrado em linguagens como C e C++, ele salva apenas o endereço da variável que referencia sem validar `borrow`.

São usados normalmente para fazer iterabilidade com ponteiros C e chamadas raw com endereços de hardware, e também usados para uma otimização fina em códigos rust quando o código em questão realmente não necessite das checagens adicionais para se garantir safe.

Raw Pointers possuem duas versões uma mutável e outra imutável seguindo a regra da referencia comum.

```rs
unsafe {
    let mut n1 = 3;
    let immutable_pointer: *const i32 = &n1;
    let mutable_pointer: *mut i32 = &mut n1;

    *mutable_pointer += 1;
    println!("{}", *immutable_pointer);

    // Se fizermos o mesmo código com referencias ocorrerá erro de compilação por estar usando ao mesmo tempo referencia mutável e imutável
}
```

> O uso inadequado de Raw Pointer pode gerar acesso indevidos a locais de memoria invalido, quando ocorrer o sistema irá forçar o encerramento do processo e emitirá o erro `segmentation fault (core dumped)`

> Além de não ser afetado por borrow Raw Pointer não possui lifetime (se comportando como um `'static` para o compilador) fazendo que possa ter ponteiros apontando para variáveis inexistentes

## Static Mutable

Static Mutable também é uma feature que necessita ser usada em modo `unsafe` pois a mesma pode gerar race condition (quando uma variável é alterada em paralelo causando problemas de consistência em seu dado).

```rs
unsafe fn counter() -> u32 {
    // Static pode ser colocada tanto no escopo global tanto dentro de funções, ele sera apenas na primeira execução definido para 0
    static mut COUNTER: u32 = 0;
    COUNTER += 1;
    COUNTER
}

fn main() {
    // Main não pode ser função unsafe, por conta disso usamos blocos unsafe
    unsafe {
        loop {
            println!("{}", counter());
        }
    }
}
```

> Como não estamos mexendo com códigos assíncronos usar static mut não é problemático

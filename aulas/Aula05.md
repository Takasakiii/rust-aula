# Aula 5 - If e Match

## If
Como em grande maioria das linguagens de programação rust tambem conta com if, que funciona praticamente igual a todas as linguagens com base C
```rs
// Os ifs do rust os parenteses são opcionais assim como no python
if idade > 18 {
    println!("Maior de idade")
} else {
    println!("Menor de idade")
}
```
Os comparadores do Rust são  
 - `==` - Igualdade 
 - `>` - Maior que
 - `<` - Menor que
 - `>=` - Maior Igual
 - `<=` - Menor Igual 

E tambem os operadores lógicos:
 - `&&` - E (And)
 - `||` - Ou (Or)
 - `!` - Negação (Not)

## Match
Match é uma estrutura que usa do sistema de match-patern(aprofundaremos mais sobre o match-patern no futuro) do rust para efeturar comparações de forma mais simples.
```rs
match idade {
    0..18 => println!("Menor de idade"), // 0..18 se referencia como um range. Ranges representam um intervalo numerico, nesse caso indo do 0 até 17
    18 => println!("Completou 18!!!!"),
    idade @ 19..=100 => println!("{} ano(s) maior que 18", idade - 18), // O range nesse caso por conter o = ele inclui no intervalo o numero 100
    // O operador @ vai colocar na variavel idade qual dos valores que deu match com o range
    idade if idade < 0 => println!("Idade é menor que 0"), // Você ainda pode usar o if como um guard da condição para deixar ela mais especifica
    _ => println!("Velho") // O _ fala para o rust que não precisamos do valor.
    // No rust a ultima rota de um match que recebe apenas uma variavel ou _ chamamos de default pois tudo que não se enquadrar nos anteriores cairão aqui
}
```
> Observação: Ao usar o match o Rust vai pedir que todas as variantes possiveis para aquela variavel tenha uma rota valida, por isso a importancia da rota default.

## Usando o if ou match para retornar valores
No Rust podemos usar o If ou Match para obtermos um valor ja tratado sem uso de variaveis auxiliares, quase como um operador ternario

```rs
let condicional = true;
let valor = if condicional {
    // O valor que deseja retornar a variavel precisa estar na ultima linha do bloco sem o ;
    20
} else {
    // Quando usamos o if para retorno precisamos sempre fornecer um else
    30
};

// Ifs e match para retorno usam ; no final
```
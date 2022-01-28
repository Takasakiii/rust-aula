# Aula 20 - Derives

Derives em sua verão resumida são `traits` que podem ser implementadas a partir de um `macro`, fazendo assim uma maior ergonomia para sua implementação em seus tipos.

```rs
// Usamos o macro derive para implementar uma trait compatível com derive
// A trait Debug é responsável pelo placeholder {:?} que usamos nos println! ou format!
// Para múltiplos derive usa-se , como separador. Ex: #[derive(Debug, Clone)]
#[derive(Debug)]
struct Familia{
    sobrenome: String,
    geracoes: u8
}
```

Bem simples, então aproveitarei para explicar os principais traits que podem ser usadas com `derive`:

- Debug: Adiciona a exibição "Debug" de um tipo que poderá ser usado no placeholder `{:?}`
- Default: Adiciona um inicializador (o `default()`) para o tipo usando os valores padrão (chamando recursivamente o `default()` dos subtipos), fazendo assim um construtor simplificado para seus tipos
- Clone: Adiciona métodos para chamar recursivamente o método `clone` dos subtipos, que adicionaram regras para duplicar a variável (obs: o método clone deve ser usado com cautela em variáveis grandes)
- Copy: Assim como os demais herda o método de Copy dos subtipos, Copy permite uma clonagem mais natural com `memcopy` sendo mais eficiente que o `Clone` (obs: Para implementar `Copy` a `Clone` também deve ser implementada) (obs2: Nem todos os tipos tem suporte a `Copy`, ficando restrito a tipos bases)
- PartialEq: Permite a implementação do comparador para realizar comparações menos restritiva quando a precisão não é importante.
- Eq: Comparação restritiva quando os dados comparados devem ser plenamente iguais ([mais informações aqui](https://users.rust-lang.org/t/what-is-the-difference-between-eq-and-partialeq/15751/2)), requerendo ser implementado junto do `PartialEq`
- PartialOrd: Assim como o `PartialEq` porem para fins de ordenação e comparação de maior e menor.
- Ord: Ordenação restritiva funcionando como o `Eq` porem para comparações de ordenação
- Hash: Adiciona métodos para geração de hash para os tipos.

Além dos derives do próprio rust muitas libs adicionam seus próprios derives, ficará para as aulas futuras abortar mais sobre sua criação pois dependemos da aula de macro para entendermos melhor.

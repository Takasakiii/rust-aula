# Aula 13 - Modules e Controles de Acesso

Até então estamos usando o mesmo arquivo e contexto para todo nosso código Rust, mas isso é ruim ainda mais quando temos projetos maiores, ou queremos novos contextos para controle ao acesso de structs, types, funções, etc

Para enão podermos criar novos contextos podemos usar o `mod` block.
```rs
mod operacoes {
    pub 
}
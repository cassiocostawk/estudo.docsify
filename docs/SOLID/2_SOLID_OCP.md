# 📍 OCP
`Dependency Inversion Principle`<br>
`Princípio da Inversão de Dependência`

Se preocupa com a instabilidade das classes.

O princípio do aberto/fechado estabelece que "entidades de software devem ser abertas para extensão, mas fechadas para modificação"; isto é, a entidade pode permitir que o seu comportamento seja estendido sem modificar seu código-fonte.

Mantenha a aplicação fechada para mudanças, mas aberta para extensões. 
Ao invés de mudar a classe, criei uma nova. 
As duas vão ficar estáveis.

Minimize a alteração de código que está pronto (fechado) mas garanta que seu projeto continue estensível (aberto); esse é o terceiro princípio S.O.L.I.D. e é chamado de `Open/Closed Principle`.

O padrão `Decorator` é utilizado para aplicar o `OCP` na prática: uma nova classe é criada que apenas decora uma existente
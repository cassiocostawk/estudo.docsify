# 🧀💨 Code Smells 
*`Fedores de Código`*

---
## 🔹 Inchaços `Bloaters`
### 💨 Código Repetido
Código repetido no método, classe, unit, projeto...

### 💨 Método Longo
`Long Method`
Um método contém muitas linhas de código. Geralmente, qualquer método com mais de dez linhas deve fazer você começar a fazer perguntas.

### 💨 Classe Grande
`Large Class`
Uma classe contém muitos campos/métodos/linhas de código.

### 💨 Obsessão Primitiva
`Primitive Obsession`
+ Uso de primitivos em vez de pequenos objetos para tarefas simples (como moeda, intervalos, strings especiais para números de telefone, etc.)
<br>
+ Uso de constantes para informações de codificação 
  (como uma constante `USER_ADMIN_ROLE = 1` para se referir a usuários com direitos de administrador).
<br>
+ Uso de constantes de string como nomes de campo para uso em matrizes de dados.

### 💨 Lista de parâmetros longa
`Long Parameter List`
Mais de três ou quatro parâmetros para um método.

### 💨 Agrupamentos de dados
`Data Clumps`
Às vezes, partes diferentes do código contêm grupos idênticos de variáveis ​​(como parâmetros para conexão com um banco de dados). 
Esses aglomerados devem ser transformados em suas próprias classes.

---
## 🔹 Abusadores de Orientação a Objetos `Object-Orientation Abusers`
### 💨 Classes alternativas com interfaces diferentes
`Alternative Classes with Different Interfaces`
Duas classes executam funções idênticas, mas têm nomes de métodos diferentes.

### 💨 Doação Recusada
`Refused Bequest`
Se uma subclasse usa apenas alguns dos métodos e propriedades herdados de seus pais, a hierarquia fica desequilibrada. Os métodos desnecessários podem simplesmente não ser usados ​​ou ser redefinidos e gerar exceções.

Se a herança não fizer sentido e a subclasse realmente não tiver nada em comum com a superclasse, elimine a herança em favor de Substituir herança por delegação.

### 💨 Alternar declarações 
`Switch Statements`
Você tem um operador `switch` complexo ou uma sequência de instruções de `if`.

### 💨 Campo Temporário
`Temporary Field`
s campos temporários obtêm seus valores (e, portanto, são necessários para os objetos) apenas em determinadas circunstâncias. Fora dessas circunstâncias, eles estão vazios.

Muitas vezes, os campos temporários são criados para uso em um algoritmo que requer uma grande quantidade de entradas. Então, ao invés de criar um grande número de parâmetros no método, o programador decide criar campos para esses dados na classe. Esses campos são usados ​​apenas no algoritmo e não são usados ​​no resto do tempo.

Esse tipo de código é difícil de entender. Você espera ver dados em campos de objetos, mas por algum motivo eles estão quase sempre vazios.

---
## 🔹 Prevenção de Mudanças `Change Preventers`
Esses cheiros significam que, se você precisar alterar algo em um local do seu código, também precisará fazer muitas alterações em outros locais. O desenvolvimento de programas torna-se muito mais complicado e caro como resultado.

### 💨 Mudança divergente `Divergent Change`
Você precisa alterar muitos métodos não relacionados ao fazer alterações em uma classe. Por exemplo, ao adicionar um novo tipo de produto, você precisa alterar os métodos para localizar, exibir e solicitar produtos.

### 💨 Cirurgia de espingarda `Shotgun Surgery`
Fazer qualquer modificação requer que você faça muitas pequenas alterações em muitas classes diferentes.

### 💨 Hierarquias de herança paralela `Parallel Inheritance Hierarchies`
Sempre que você cria uma subclasse para uma classe, você precisa criar uma subclasse para outra classe.

---
## 🔹 Dispensáveis `Dispensables`
Um dispensável é algo inútil e desnecessário cuja ausência tornaria o código mais limpo, mais eficiente e mais fácil de entender.

### 💨 Comentários `Comments`
Um método é preenchido com comentários explicativos.

### 💨 Código duplicado `Duplicate Code`
Dois fragmentos de código parecem quase idênticos.

### 💨 Classe preguiçosa `Lazy Class`
Compreender e manter as aulas sempre custa tempo e dinheiro. Portanto, se uma aula não fizer o suficiente para chamar sua atenção, ela deve ser excluída.

### 💨 Classe de dados `Data Class`
Uma classe de dados refere-se a uma classe que contém apenas campos e métodos brutos para acessá-los (getters e setters). Estes são simplesmente contêineres para dados usados ​​por outras classes. Essas classes não contêm nenhuma funcionalidade adicional e não podem operar independentemente nos dados que possuem.

### 💨 Código morto `Dead Code`
Uma variável, parâmetro, campo, método ou classe não é mais usada (geralmente porque está obsoleta).

### 💨 Generalidade especulativa `Speculative Generality`
Há uma classe, método, campo ou parâmetro não utilizado.

---
## 🔹 Acopladores `Couplers`
Todos os cheiros neste grupo contribuem para o acoplamento excessivo entre classes ou mostram o que acontece se o acoplamento for substituído por delegação excessiva.

### 💨 Inveja do recurso `Feature Envy`
Um método acessa os dados de outro objeto mais do que seus próprios dados.

### 💨 Intimidade Inapropriada `Inappropriate Intimacy`
Uma classe usa os campos e métodos internos de outra classe.

### 💨 Cadeias de mensagens `Message Chains`
No código você vê uma série de chamadas que se assemelham `$a->b()->c()->d()`

### 💨 Intermediário `Middle Man`
Se uma classe executa apenas uma ação, delegando trabalho para outra classe, por que ela existe?
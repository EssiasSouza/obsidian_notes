Portanto, agora veremos como podemos gravar os dados no banco de dados e também

entenderemos alguns serviços inteligentes, que são entidades de gravação no armazenamento de dados.

Portanto, voltarei ao meu designer e, dentro da

construção, criarei um novo modelo de processo.

Portanto, isso foi apenas para fins de teste, que criamos anteriormente.

Agora, criarei mais um novo modelo de processo e usarei a convenção de nomenclatura de acordo.

Portanto, o que preciso fazer é escrever os detalhes da despesa para que eu possa mencioná-la.

Certo.

Despesas.

E o criará por padrão Security.

E isso é tudo o que tenho.

Meu modelo de processo vazio.

E então eu posso descobrir aqui.

Portanto, essa é minha opção de pesquisa.

Posso pesquisar qualquer serviço inteligente que seja necessário.

Então, o meu requisito é que eu queria escrever esse formulário no banco de dados,

certo? Então, antes de tudo, preciso indicar a entidade do armazenamento de dados, certo?

Portanto, preciso usar uma constante que apontará para a entidade do armazenamento de dados.

Portanto, não vimos a constante.

Então, vamos criar uma constante.

Primeiro, preciso clicar em novo e depois em constante.

Portanto, as constantes no Appian são de diferentes tipos.

Por exemplo, se eu quiser usar algum item de texto em todo esse ambiente, em

qualquer lugar posso colocar um texto e criar uma constante para esse texto e usá-lo.

Portanto, esses são os tipos disponíveis para uma constante.

Pode ser de entidade de armazenamento de dados, data constante, data, documento com data e hora, documento ou pasta.

Portanto, para indicar uma pasta, também precisamos de uma constante para indicar o modelo do processo.

Precisamos de um relatório de processo constante.

Precisamos de uma constante ou um relatório ou qualquer outra coisa, o que quer que seja mencionado aqui,

certo? As variáveis ou talvez algum item de texto ou qualquer outro item inteiro e booleano e.

Tudo bem.

Portanto, meu requisito é criar uma constante para minha entidade de armazenamento de

dados para que eu possa gravar os dados no banco de dados.

Então, o que eu preciso é de um DSC e, como não é permitido

o uso de espaço, preciso usar sublinhados e colocarei tudo em letras maiúsculas.

Então, ele e o nome da minha tabela, que é let's find out at expense data, eu acho.

Sim, dados de despesas e, em seguida, tabela para que eu

possa colocar dentro da descrição como o nome da tabela.

Aponta para.

E o nome da tabela e o tipo de conteúdo são entidades de armazenamento de dados.

É isso que eu quero.

Preciso selecionar o armazenamento de dados, que, no meu caso, é o itI

e há apenas uma única entidade de armazenamento de dados que criamos anteriormente.

Isso é tudo o que posso salvar dentro da pasta constante.

No momento, estou usando uma única pasta, mas, se quisermos, também podemos criar uma pasta separada.

Portanto, agora minha constante de armazenamento de dados está pronta.

Agora preciso fazer a configuração no modelo de processo.

Portanto, antes de mais nada, preciso atribuir essa tarefa ou podemos dizer que preciso

de um formulário para ver esse formulário de despesas específico, aquele que criamos anteriormente.

Precisamos que esse formulário seja exibido.

Então, como podemos fazer isso, porque no Appian, sempre que usarmos essa

tarefa de entrada do usuário, ela será apenas uma tarefa e precisaremos

atribuí-la a alguém, certo? Por exemplo, precisamos mostrar algum formulário, talvez em

um evento de clique de botão, ou talvez precisemos atribuí-lo a alguém.

Em seguida, precisamos definir o responsável por essa tarefa de entrada do usuário.

E sempre que eu disser tarefa de entrada do usuário, isso significa apenas tarefa.

E dentro dessa tarefa de entrada do usuário, usaremos as interfaces à direita para mostrar o formulário que criamos.

Portanto, agora preciso arrastar e soltar essa tarefa de entrada do usuário.

É uma tarefa humana, certo? Você pode ver que no item de fluxo

de trabalho também há uma única tarefa humana, que é a tarefa

de entrada do usuário, e anexaremos uma interface dentro dela, e essa

interface ficará visível para a pessoa, quem quer que seja o responsável.

Certo.

E preciso clicar dentro disso e entrar no formulário.

Dentro do formulário, preciso selecionar o formulário.

Portanto, meu formulário é um novo formulário de despesas.

Então, novamente, esse pop-up está chegando.

Então, por que esse pop-up está aparecendo? Porque, se

você se lembrar, sempre que selecionarmos o processo

e iniciarmos o formulário, esse formulário também aparecerá.

Mas isso é um pouco diferente, porque ele não mapeará

diretamente a entrada da função com as variáveis do processo.

Primeiro, ele mapeará as entradas de função com as entradas de nó e,

em seguida, as entradas de nó serão mapeadas para a variável de processo.

Portanto, essa é a diferença.

E isso nós precisamos entender, certo? Então, você pode ver que estamos mapeando

as entradas de função com as entradas de nó e o que são

as entradas de nó? As entradas de nó são as entradas que usaremos

somente dentro dos nós e que não podemos usar fora dos nós, certo.

Então, precisamos fazer isso e eu preciso clicar em.

Sim.

E isso é tudo.
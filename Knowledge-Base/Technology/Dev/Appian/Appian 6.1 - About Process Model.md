Portanto, nesta seção, aprenderemos sobre o modelo de processo e, em várias palestras, veremos

como podemos entender o modelo de processo e como podemos entender os serviços

inteligentes, como podemos usar os serviços inteligentes e como podemos depurar as coisas.

Então, vamos começar criando um modelo de processo, um novo

modelo de processo, e só precisamos clicar nesse novo objeto.

E aqui tenho novamente duas opções: criar do zero e duplicar o modelo de processo existente.

Portanto, novamente, se já tivermos algum modelo de processo que desejamos reutilizar, poderemos fazê-lo.

Caso contrário, podemos criar do zero.

E a convenção de nomenclatura é muito simples.

Também podemos dar espaços porque o modelo de processo não pode ser acessado diretamente.

Precisamos de uma constante para acessar nosso modelo de

processo, certo? Então, vamos adicionar esse modelo de processo.

Portanto, este é meu primeiro modelo de processo.

Portanto, colocarei apenas esse nome e darei a descrição como teste e

poderei armazenar o modelo de processo na pasta de modelos de processo.

Agora preciso criá-lo e dar a permissão.

Portanto, podemos modificar um modelo de processo no modelador de processos e podemos

abrir vários modelos de processos ao mesmo tempo e modificar cada um deles.

E, depois de concluir tudo, podemos salvar e publicar o modelo do processo.

Como você pode ver, este é um modelo de processo vazio com apenas um

nó inicial e um nó final, certo? Portanto, esses são chamados de nós.

E, neste momento, esses dois nós estão conectados um ao outro

e não há nada entre eles nessa linha, certo? Você

pode arrastar e soltar esses nós e colocá-los onde quiser,

certo? Então, vamos entender o modelo de processo primeiro, porque

há muitas coisas nesse modelo de processo que podemos contornar.

Você pode ver que essa é a exibição de lista e há algumas opções que não estão disponíveis na exibição de lista.

Portanto, sempre trabalhamos na visualização do designer.

Na visualização do designer, teremos tudo, como instâncias de processo e exceções

de atividade e todas essas coisas, certo? E esse pequeno ícone,

como você pode ver, são as propriedades desse modelo de processo.

Então, vamos clicar nesse ícone e entender as propriedades desse modelo de processo.

Portanto, você pode ver que estamos na guia geral e que ela

foi criada nesta data e modificada pela última vez nesta data.

E podemos escolher os idiomas que quisermos e podemos definir as configurações.

Portanto, aqui temos o ID do modelo do processo e o ID do modelo do processo.

Portanto, é o ID exclusivo de cada modelo de processo nesse ambiente.

E esse ID também é um ID exclusivo, mas é um ID

alfanumérico longo, certo? Podemos usar onde for necessário, como se quiséssemos pesquisar

esse objeto, então podemos usar o ID e também o Uuid.

Certo? Depois disso, temos o nome do modelo

do processo, o nome estático que fornecemos aqui.

Sempre que criarmos o modelo do processo, a descrição e o nome de exibição do processo.

Isso é importante porque temos dois tipos de nome aqui.

Você pode ver que temos o nome do modelo do processo e o nome de exibição do processo.

Então, qual é a diferença entre os

dois? É algo básico, certo? Nas entrevistas,

você também será questionado sobre essa questão.

A diferença é que esse nome de exibição do processo é o nome

da instância do processo, que você verá na guia de monitoramento, certo? Portanto,

sempre que um modelo de processo for acionado, como podemos dar uma olhada

nas instâncias, precisamos entrar no monitoramento e verificar a atividade do processo.

Portanto, você pode ver que ele está vazio.

Assim, sempre que eu acionar qualquer modelo de processo em meu aplicativo, poderei ver essa

instância aqui e poderemos ver o processo com erros e muitas coisas que poderemos descobrir.

Portanto, veremos isso mais adiante.

Atualmente, ela está vazia.

Vamos voltar ao aplicativo.

Temos o nome de exibição do processo, que é dinâmico.

Posso usar o editor de expressões e colocar o que eu quiser aqui.

Esse é o editor de expressões.

O e-mail prioritário, anexo, zonas públicas, eventos públicos, fuso horário.

Portanto, posso selecionar o fuso horário que eu quiser.

Os detalhes serão divulgados de acordo com esse fuso horário.

Criado por e atualizado por.

E depois dessas informações básicas, temos a variável.

Portanto, esse é o local onde teremos todas as variáveis de processo,

certo? E como podemos acessar a variável de processo usando o banco.

Então, vamos tentar criar uma variável de processo.

Portanto, chamarei isso de teste.

Variável de teste e o tipo que podemos selecionar.

Atualmente, o texto está selecionado, mas posso selecionar qualquer coisa.

Certo? Também posso selecionar meu

tipo de dados personalizado.

Anteriormente, criamos um tipo de dados personalizado, portanto, tentarei selecioná-lo.

Portanto, esses são os dados de despesas, certo? E o

valor, se quisermos fornecer algum valor padrão, podemos fornecê-lo aqui.

Caso contrário, não é necessário.

E então podemos tornar essa variável um parâmetro, conforme necessário.

Portanto, você pode ver que, se eu fizer isso como um parâmetro, esse requerimento será ativado porque,

quando passarmos algum parâmetro de fora desse modelo de processo, também poderemos fazê-lo como um requerimento.

Portanto, se o usuário não passar o parâmetro, o modelo de processo apresentará

o erro e podemos tornar essa variável múltipla e também podemos ocultá-la.

Mas não agora.

Se o parâmetro, a variável não for um parâmetro, poderemos ocultar

esse parâmetro, ocultar essa variável de processo do processo pai.

Portanto, entenderemos esse conceito de processo pai e filho, subprocesso, mais adiante.

Vamos remover essas coisas e seguir em frente.

Estarei criando essa variável de processo e posso excluí-la.

Posso ver todas as configurações aqui e posso adicionar várias variáveis de processo.

E sempre que criarmos um modelo de processo, se

não o estivermos usando, é melhor excluí-lo, certo? A

melhor prática é criar apenas os elementos que estamos

usando e não criar variáveis de processo inutilmente.

Portanto, essa é a minha lista de variáveis.

E então temos o processo para iniciar o formulário.

Portanto, isso é bastante interessante porque se trata de uma forma nesse local.

Podemos anexar um formulário próprio, certo, para iniciar o processo.

Portanto, você pode ver que atualmente o nó inicial está vazio.

É um círculo vazio.

Mas quando tento adicionar uma interface aqui, vamos fazer uma coisa.

Vamos adicionar a interface que criamos para o nosso aplicativo.

Então, se eu voltar para a construção.

E temos essa interface, certo? Assim, posso selecionar

a interface aqui no novo formulário de despesas.

E recebi um pop-up.

A janela pop-up está dizendo: você deseja criar automaticamente parâmetros de processo para

corresponder às entradas da interface? Então, sempre que eu clicar em sim, se

eu tiver um processo dentro dessa interface e tiver algumas entradas de regras,

essas entradas de regras serão mapeadas automaticamente com as variáveis do processo.

E, atualmente, não tenho nenhuma variável de processo semelhante às entradas da regra.

Portanto, ele será criado automaticamente.

Então, você pode ver que, se eu clicar em sim, essas são as entradas de regras provenientes da

interface, certo? E essas são as variáveis de processo que foram criadas automaticamente porque eu as selecionei.

Sim.

E se eu for para a lista de variáveis, você poderá ver que todas essas variáveis estão aparecendo automaticamente.

Certo?

Cancelar.

Ele não estava lá anteriormente.

Portanto, este é o meu processo.

Formulário inicial.

Posso selecioná-lo se quiser que ele inicie esse processo com um formulário.

Veremos isso quando fizermos a depuração.

E se eu tentar clicar.

Sim.

Está bem.

E você pode ver agora que esse nó inicial tem algum tipo de formulário no meio.

É o formulário de início do processo, certo? E depois

do formulário de início do processo, temos o prazo.

Deadline é aquele em que, se quisermos que ele defina algum

prazo para esse processo, modelar a instância do modelo de

processo, podemos ativar esse prazo, certo? E aqui está a

expressão, certo? Podemos adicionar os minutos que forem necessários ou

as horas que forem necessárias, podemos selecionar nesse menu suspenso.

E temos alertas.

Basicamente, se essa instância do processo estiver tendo algum problema

ou algo relacionado ao erro, ele alertará automaticamente o usuário.

Podemos selecionar o tipo de alerta que quisermos.

Ou queremos selecionar o usuário, ou queremos que ele notifique algum tipo de grupo ou usuário, ou queremos

que ele crie alguma expressão para que seja alertado automaticamente de acordo com o que eu escolher.

E o último é o gerenciamento de dados.

Isso também é muito importante porque sempre que a instância for

criada no Appian, ela ocupará um pouco de memória, certo?

Portanto, não precisamos usar Leslie ou gravar esse armazenamento ou

preencher totalmente esse armazenamento em nuvem com todas as instâncias.

Portanto, estaremos removendo ou arquivando as instâncias do processo, certo? Portanto,

podemos definir alguns critérios de acordo com o aplicativo, que este

será arquivado após sete dias ou talvez três, se necessário.

Certo.

Portanto, qualquer que seja a decisão do administrador do aplicativo, podemos escolhê-la e ela será arquivada.

Ou, se eu quiser excluir esse processo, podemos escolher essa opção.

Certo? Ou o sistema, por

padrão, será de sete dias.

Arquivamento Portanto, essas são todas as propriedades desse processo.

Variáveis do modelo, processo, formulário inicial, alerta de prazo e gerenciamento de dados.

Portanto, depois de fazer toda a configuração das propriedades, projetaremos o

modelo do processo e, no momento, escolhemos um formulário inicial.

Então, por exemplo, meu modelo de processo está completo agora,

certo? Então, o que eu preciso fazer depois disso

é salvá-lo, certo? E durante a depuração, podemos salvá-lo.

Mas se quisermos que as alterações sejam refletidas em

todo o ambiente, precisaremos publicá-las, salvá-las e publicá-las.

Portanto, isso criará uma nova versão do modelo de processo.

Sempre que o publicarmos, ele criará uma nova versão.

Portanto, no modelo de processo, também podemos ver as versões

acessando essa opção e podemos ver as versões anteriores.

Podemos carregar, excluir ou salvar uma nova versão com isso.

Portanto, essas são as propriedades básicas de um modelo de processo e vamos ver como depurar um modelo de processo.

Então, eu só preciso entrar no arquivo e iniciar o processo de depuração.

Isso iniciará a depuração desse formulário.

Certo.

E você pode ver que isso está completo porque não há nada no meio.

Houve um formulário que apareceu.

Eu o cancelei, portanto, isso é tudo o que fiz aqui.

E essa é a depuração, está no monitoramento e essa é uma maneira de ver o monitoramento.

Também posso entrar na guia de monitoramento novamente e aqui você pode ver que isso foi concluído,

essa instância, posso clicar nela novamente e ela me levará ao monitoramento ou a outra maneira.

Posso entrar na lista de instâncias do processo e ver qualquer que seja a lista de instâncias do processo.

Posso selecionar todas as instâncias do processo e há um status concluído.

Começou com esse horário, isso e mais nada.

Portanto, é assim que podemos depurá-lo e podemos usar essas

opções, como instâncias de processo, propriedades e outras opções.

O que você quiser escolher.

Por exemplo, queríamos descrever esse modelo de processo, algum texto que queríamos colocar nesse design.

Então, podemos usar essa anotação, podemos colocar a anotação também aqui,

digamos, teste, certo? Então, isso é apenas para descrever as anotações.

Se você quiser descrever algo relacionado a notas, podemos fazer isso.

E também posso anexar esses itens.

Por exemplo, gostaria de anexar este.

Assim, podemos entender facilmente qual é o significado desse nó, certo? E

eu só preciso arrastar e soltar na linha para anexar elementos.

Certo.

E este aqui, e preciso clicar no shift.

Então, terei essa opção e poderei conectar esses itens.

Portanto, se eu tiver uma tarefa de script aqui, posso conectá-la assim: pressionando

shift e soltando o mouse, pressionando shift novamente e soltando o cursor.

Não tem problema.

Portanto, é assim que podemos projetar e usar essas funcionalidades de arrastar e soltar. Vamos entrar no

modelo de processo na próxima aula, onde veremos como podemos gravar os dados no banco de dados.

E também entendemos esses itens de fluxo de

trabalho, certo? E os serviços inteligentes de automação.

Então, vamos ver essa coisa dentro da próxima palestra.

- Não concluído
    
    Parar
    
- Não concluído
    
    Reproduzir
    
- Não concluído
    
    Reproduzir
    
- Não concluído
    
    Reproduzir
    
- Não concluído
    
    Iniciar
    

All About Appian Developement, Organization Ready Developer in 3 Hours

Classificação: 4,2 de 54,2

1.322 classificações

5.975

Alunos

4,5 horas

Total

Última atualização em julho de 2025

Inglês

Português [Automático], Inglês [Automático],
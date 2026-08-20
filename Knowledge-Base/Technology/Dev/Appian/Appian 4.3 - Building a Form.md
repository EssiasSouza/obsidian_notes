Nesta palestra, discutiremos as interfaces.

Portanto, o que fiz foi criar um aplicativo chamado Expense Tracker.

Então, por que criamos esse aplicativo? Porque,

nesta série de palestras, usarei esse aplicativo

e o completaremos até a última palestra.

Portanto, para esta palestra, criaremos uma interface.

E a agenda desse aplicativo é sempre que um usuário chega e o que ele está

fazendo, o usuário está diariamente abrindo esse aplicativo e vai para o site do aplicativo e

verá algum formulário, alguma página inicial e o formulário será algo relacionado a detalhes de despesas.

Assim, sempre que o usuário fizer alguma despesa, ele abrirá esse aplicativo e

colocará todos os detalhes das despesas e, em seguida, enviará o formulário.

Portanto, para isso, criaremos a interface e teremos alguns campos, como

detalhes de despesas, qualquer despesa que esteja ocorrendo, por que essa

despesa está ocorrendo e o valor da despesa, certo? Portanto, criaremos

a interface de acordo com isso e vamos criar a interface.

Portanto, tenho duas opções.

Criarei a interface do zero e já temos a convenção de nomenclatura.

Depois disso, temos o prefixo do aplicativo.

Eu queria usar esse formulário para detalhes de despesas, então o nomearei como.

Ou novo formulário de despesas.

Certo.

Portanto, esse formulário será necessário para enviar uma nova despesa.

Quanto aos detalhes, posso dizer o seguinte.

Formulário usado para adicionar novas despesas.

E isso será armazenado dentro dessa regra e restrição.

E se você não quiser isso, porque a interface também é uma

regra, também podemos criar uma nova pasta de regras novamente para interfaces.

Portanto, sempre que estivermos trabalhando em aplicativos grandes ou

complexos, onde haverá muitas interfaces, constantes e regras.

Portanto, criaremos pastas individuais para interfaces, regras e restrições.

Não usaremos uma única pasta de regras para armazenar tudo,

certo? Portanto, criei essa interface e a nomearei como interface.

A descrição é a seguinte.

Todas as interfaces para ele e nenhuma pasta principal.

Portanto, criarei essa interface.

Vamos clicar, ele está sendo carregado e temos o formulário vazio aqui.

Portanto, agora quero usar esse layout de formulário porque haverá alguns campos e vamos usar essas colunas.

Mais algumas colunas e vamos ver quais são os outros campos que precisamos para usar algumas entradas.

Então, em primeiro lugar, o que farei é usar esse campo de data.

Portanto, mostrarei a data em um formato somente leitura.

Certo.

E então usarei esse número inteiro.

Já temos a data.

Está bem.

E nós podemos.

Para esse campo inteiro, o valor que estamos gastando com essa despesa de dívida.

E então o encontro está ótimo.

Vamos colocar o nome de usuário aqui, o nome de usuário conectado, a pessoa que

enviará o formulário e a descrição da despesa para que eu possa usar o parágrafo.

Certo.

Portanto, tenho quatro campos.

A primeira é a data.

Isso também será somente leitura.

Isso também é somente leitura e isso não é somente leitura.

Esse é um campo inteiro.

O valor da despesa e este é um campo de parágrafo.

Certo.

E vamos excluir esses dois.

Isso não é necessário.

Portanto, estou pensando se há algum outro campo ao qual possamos adicionar o valor

e a descrição, mas acho que isso é tudo, é tudo para esse formulário.

Vamos entrar no modo de expressão agora, pois acabamos de arrastar e soltar essas coisas.

Vamos modificar essas coisas e vamos modificar essa data.

Portanto, eu o farei.

Dê o nome da data de hoje.

Certo.

Portanto, essa é a posição do rótulo acima dele.

Está tudo bem.

No momento, estou mudando o rótulo de todos os campos.

Portanto, este é.

Usuário conectado.

E esse é o valor da minha despesa.

E detalhes.

E mais um campo que posso usar.

Se eu entrar no design, posso usar um menu suspenso para mostrar a categoria da despesa.

Então, onde está meu menu suspenso? Estou colocando o menu suspenso aqui

e vou arrastá-lo e soltá-lo nesse layout de coluna e nesses detalhes.

Posso colocá-lo aqui.

E este abaixo desse valor de despesa.

Então, vamos modificar esse menu suspenso.

Portanto, o que farei.

Vou renomear esse menu suspenso para categoria de despesas.

Certo.

Porque haverá uma determinada categoria.

E esses são os valores e rótulos do meu menu

suspenso, certo? Esses são os rótulos e os valores.

Portanto, também entenderemos como essas configurações estão ocorrendo.

Antes de mais nada, vamos para a data de hoje e também vamos

renomear esse formulário e eu o renomearei para submit expense (enviar despesas),

porque enviaremos esse formulário com as despesas, independentemente do que estivermos escrevendo.

E então salvarei isso no arquivo.

Posso criar entradas de regras.

Portanto, o que farei é criar entradas de regras.

Mas para esta data, a data de hoje, não queremos ler essa

entrada de regra específica porque não vamos armazenar essa data de hoje.

Ou podemos.

Mas, no momento, só queríamos ver esses dois campos como somente leitura.

Portanto, sempre que eu precisar colocar alguns valores, posso usar o parâmetro values.

Posso usar o valor porque esses valores são diretamente codificados.

Certo? Eles não

são dinâmicos.

Esses são alguns valores estáticos e você pode ver automaticamente que a data está chegando.

Mas quero que isso seja somente leitura, portanto, usarei o parâmetro somente leitura e passarei.

Verdadeiro.

Certo.

E vírgula.

Portanto, isso é feito como um usuário somente leitura e conectado.

Eu também quero o mesmo.

Portanto, usarei o valor e, no valor, usarei a função de usuário conectado, que me fornecerá os

detalhes do usuário conectado e, em seguida, farei com que isso seja somente leitura, somente leitura, verdadeiro.

Portanto, estes são os detalhes do meu usuário conectado e, em seguida, a despesa.

Aqui vem a parte em que precisamos salvar os detalhes.

Portanto, sempre que precisarmos salvar alguns detalhes, podemos criar uma regra de entradas e colocarei o valor.

E isso é um número inteiro.

Posso nomear esse número inteiro.

Vamos criar.

Também quero criar mais uma entrada de regra para detalhes.

E isso é do tipo string.

Assim, posso usar a criação de texto e mais uma para o menu suspenso, que é a categoria.

Isso é tudo.

Queremos apenas três entradas de linha.

Então, por que essas entradas de regras que criei? Porque eu queria salvar os

detalhes dentro das entradas de regras e os detalhes serão salvos dentro delas.

Esse é o valor da despesa.

Ele o salvará dentro do valor.

E, no momento, estou guardando os detalhes.

Por exemplo, se eu tentar escrever algo aqui, você poderá ver que o valor

está sendo salvo dentro da quantidade, mas o valor está desaparecendo daqui porque não

estamos usando o parâmetro de valor e o parâmetro de valor está em branco.

Faremos a mesma coisa para que você possa ver o valor aqui e também para

salvar algum valor dentro desse banco, precisamos usar value e save em dois parâmetros.

Certo? E esse salvamento em é uma

matriz para que possamos salvar várias coisas.

Certo? Por exemplo,

este valor.

Quero que ele salve isso dentro dessa entrada de regra e também em outra entrada de regra.

Portanto, a variável local permite que eu coloque uma vírgula aqui e faça a mesma coisa.

Posso mencionar a variável local ou usar uma função de salvamento do banco,

certo? Essa função é usada para salvar o valor em um destino específico.

Falaremos sobre isso mais tarde.

No momento, farei a configuração para os outros itens.

Também nossos dados bancários e valor.

E a mesma coisa preciso fazer para o menu suspenso.

Portanto, no momento, esses valores padrão estão chegando.

Eu não quero isso, então eu vou.

Renomeie-o para it e depois para home e depois ou não para gadgets.

Despesas com gadgets.

Despesas domésticas ou despesas de escritório.

Ou podemos dizer que estamos aprendendo essas três categorias e faremos o mesmo com meus valores de escolha.

E agora preciso salvar os valores em nosso banco.

Categoria e também precisa fazer a mesma coisa em valores para mostrar o valor.

Sempre que estamos selecionando algo, a mesma coisa acontece com as categorias

bang e o espaço reservado também está lá, selecione um valor.

Então, vamos ver o formulário para que você possa ver.

Vamos entrar no modo de visualização.

Temos a data de hoje, temos o usuário conectado, temos alguma despesa, seja qual for

a despesa que quisermos escrever aqui, você pode ver que é um número inteiro.

Portanto, estamos obtendo esse limite e podemos usar um texto se quisermos que ele escreva um pouco mais.

Certo? Ou não queremos

nenhum tipo de limite.

E esta é uma categoria, este é um menu suspenso.

Portanto, isso está acontecendo dessa forma.

Você pode ver aqui que estamos recebendo tudo, seja o que for.

Estamos escrevendo alguns detalhes, certo? Portanto, esta é a

minha interface e usaremos essa interface específica para gravar

os dados no banco de dados nas próximas palestras.

E estes são os botões de envio.

O submit é verdadeiro para enviar o formulário e, no cancel, o submit também é verdadeiro para enviar o formulário.

E também se quisermos salvar algum valor nesses botões, sempre que eu clicar nesse botão de envio ou no

botão de cancelamento, quero que ele salve os valores de cancelamento para que eu possa criar uma regra.

Input e o tipo será, digamos, Boolean, e eu posso colocar o valor.

O valor já está lá dentro, cancelo e posso salvar dentro do cancelamento do banco.

Assim, sempre que eu clicar no botão cancelar, ele salvará o valor como true dentro do botão cancelar.

Certo? Portanto, é assim que precisamos trabalhar

com todos esses parâmetros, com essas funções.

E precisamos criar esse formulário porque cada objeto de design tem

uma configuração diferente, portanto, podemos usar o texto de ajuda se

necessário, certo? Portanto, este é o formulário que usaremos mais tarde.

E isso é tudo para esta palestra.

- Não concluído
    
    Reproduzir
    
- Não concluído
    
    Reproduzir
    
- Não concluído
    
    Parar
    

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

alerta de informação

## Programe um horário para o aprendizado
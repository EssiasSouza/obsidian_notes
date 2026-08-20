Portanto, no Appian, já temos um banco de dados conectado, que é o Mariadb.

E se eu clicar nesses pontos de navegação, poderei ver esse banco de dados em nuvem.

Portanto, se eu for navegar até esse banco de dados

na nuvem, ele me levará a esse ambiente mariadb, certo?

Portanto, a escolha é nossa e depende do nosso cliente.

Além disso, se quisermos que ele use esse banco de dados, poderemos fazê-lo.

Mas se não quisermos usar esse banco de dados para fins de segurança ou por qualquer outro motivo,

também temos a possibilidade de ir ao console de administração e adicionar um novo banco de dados.

Então, vou lhe dizer como podemos fazer isso.

Só preciso entrar no console de administração e, se eu rolar a tela

para baixo, poderei ver as fontes de dados, certo? Então, posso clicar nessa

nova fonte de dados e adicionar a fonte de dados que estiver lá.

Por exemplo, há uma fonte de dados externa que é o MySQL.

Em seguida, posso dar um nome, posso colocar o nome de usuário e a senha onde quer que eu

tenha definido naquele local e, em seguida, posso selecionar que é o MySQL e, depois, a string conectada.

Portanto, é isso que queremos, essa string conectada que eu quero e meu provedor de

banco de dados me fornecerá essa string e, então, poderei colocá-la e testar a conexão.

Mas, neste cenário, estaremos entendendo esse banco de dados, que é o mariadb,

e temos todas as opções disponíveis, o que quer que esteja disponível

em outros bancos de dados, como criar uma tabela, uma visualização, um

procedimento ou funções, certo? Portanto, isso é bastante útil e podemos entrar

diretamente no SQL, que é este aqui, e começar a escrever consultas.

Mas, se não quisermos, também temos a opção de usar nossos recursos automáticos.

Por exemplo, para criar uma tabela, basta colocar o nome da tabela e todos

os detalhes necessários e pressionar o botão Enviar, e a tabela será criada.

Portanto, temos poucas opções.

Vamos explorar essas opções e veremos como podemos criar uma tabela usando essas opções.

Portanto, para meus aplicativos que criei ou para o aplicativo, este rastreador de

despesas, precisaremos de uma tabela de banco de dados, certo? Como esse formulário

de despesas que criamos, precisamos gravá-lo no banco de dados, certo? Então, para

isso, precisamos de uma tabela de banco de dados para que possamos armazenar

os dados dentro dessa tabela, certo? Portanto, precisamos criar uma tabela para isso.

E o que é uma tabela de banco

de dados? Tabela de banco de dados.

Essas são as tabelas que contêm todos os

dados, certo? Em qualquer banco de dados.

Temos essas estruturas como se fossem visualizações de tabelas.

Portanto, as tabelas são usadas para armazenar os dados e há diferentes tipos de

tabela, como as tabelas de dados ou as tabelas métricas, certo? Para Então, há

uma diferença entre ambas, pois a tabela de dados é a tabela em que

estaremos escrevendo todos os dados e ela armazenará, certo, os dados serão armazenados lá.

Podemos usá-lo, podemos ler, escrever e tudo o que quisermos fazer.

E a tabela de métricas é a tabela.

Por exemplo, há tabelas em que os dados já estão presentes.

E quando digo que os dados já estão lá, isso significa que

o cliente me fornecerá os dados, certo? Por exemplo, criarei um aplicativo,

digamos que seja apenas um rastreador de despesas, e quero preencher alguns

menus suspensos, certo? Portanto, o usuário não preencherá essas listas suspensas.

Certo? Então, essas categorias, existem gadgets, aprendizado

em casa? Então, quem me fornecerá essas

categorias? Meu cliente dirá que eu tenho

essas categorias, como gadgets, casa e aprendizado.

Portanto, preciso criar as tabelas e escrever manualmente todos os detalhes fornecidos pelo cliente.

E usarei essa tabela para mostrar os dados dentro das minhas interfaces.

Certo? Então, essa tabela nós temos, certo? Portanto, agora

o que precisamos é de uma tabela onde eu

possa armazenar os detalhes, seja qual for a despesa

que estou enviando, quero que ela armazene isso.

Então, para isso, vou criar uma tabela de dados e só

preciso clicar nesse novo botão, certo? Então, esse é o recurso,

certo? Podemos clicar nesse botão e criar a tabela diretamente.

Mas se não quisermos isso, também podemos escrever consultas SQL, certo? Então,

vamos preencher todos os detalhes necessários e ver o que é necessário.

Aqui temos o nome da tabela, portanto, precisamos colocar o nome da tabela aqui.

E precisamos nos certificar de mais uma coisa, pois estamos usando o prefixo do aplicativo em todos os lugares do banco de dados.

Também precisamos usar o prefixo do aplicativo, certo? Portanto,

precisamos usar e depois a tabela de despesas.

Portanto, vou nomear isso como e t e underscore and.

Despesas, digamos, dados, certo? E precisamos preencher todos esses detalhes

sobre todas as colunas, o nome da coluna, o tipo

de coluna, o comprimento da coluna e os valores padrão.

Se houver alguma chave nula, incrementada automaticamente ou primária.

Certo.

Como não estamos escrevendo consultas SQL, precisamos configurar todas essas

coisas, certo? E será muito fácil preencher todos esses detalhes.

E mais uma coisa.

Sempre que precisamos projetar algum aplicativo, certo? Por exemplo,

estamos projetando este aplicativo agora, portanto, sempre precisamos criar

a estrutura do banco de dados primeiro, certo? Porque

precisamos projetar o modelo do banco de dados.

Só então prosseguiremos com o aplicativo, certo? Porque essa é a primeira coisa a fazer sempre que

estamos começando com um novo aplicativo, então a primeira coisa é projetar a parte do banco de

dados, que é aqui que precisamos projetar toda a tabela do banco de dados com o relacionamento,

as chaves primárias, as chaves estrangeiras e qualquer que seja a estrutura do banco de dados.

Precisamos decidir que precisamos criar algo e, em seguida, precisamos criar a estrutura do banco de dados.

Depois de criar a estrutura do banco de dados, seguiremos em frente com o aplicativo, como no Appian.

Precisamos criar chaves para isso e para outros objetos que podemos criar depois disso.

Portanto, vamos fazer a mesma coisa.

Antes de mais nada, vamos criar essa tabela e preciso criar a chave primária.

Portanto, nomearei essa chave primária como ID de despesa, ID de despesa e, em seguida, ID de despesa.

Valor da despesa.

Depois disso.

Detalhes da despesa.

Certo.

Portanto, esses são os detalhes.

Valor e detalhes.

Despesa, valor e detalhes da despesa.

Estou escrevendo aqui dentro da minha interface.

E também estou escrevendo a categoria de despesas.

Portanto, vou criar um.

Este eu usarei.

Portanto, preciso preencher os dados nessas três colunas.

E mais uma coisa que precisamos garantir, porque sempre que estivermos projetando as

tabelas do banco de dados, há algumas práticas recomendadas que precisamos seguir.

Por exemplo, há algumas colunas que sempre construímos, certo?

Porque essas colunas nos ajudarão em várias coisas.

Então, o que são essas colunas? Essas são as

cinco colunas que sempre criamos em qualquer tipo de

tabela de dados, certo? Então, clicaremos em Go (Ir).

É assim que posso adicionar mais colunas a essa tabela e quais são essas colunas.

As colunas são criadas por criadas em, atualizadas por e atualizadas em e estão ativas.

Portanto, essas são as colunas que sempre criamos, certo? Porque sempre

que estamos escrevendo alguma entrada em uma tabela, precisamos ter certeza

de que estamos escrevendo a data e a hora e

quem criou essa entrada. Precisamos escrever esses detalhes na tabela.

Portanto, para fins de auditoria, também para o usuário, para mostrar esses detalhes nas interfaces, se

necessário, ou em alguma outra grade de detalhes, podemos usar essas informações e, sempre que estivermos

atualizando algo, precisamos nos certificar de que estamos atualizando o atualizado por e o atualizado em.

E mais uma coisa que está ativa.

Então, por que estamos usando isso é porque, por exemplo,

queríamos ocultar alguns dados, filtrar alguns dados provenientes dessa tabela.

Em seguida, podemos modificar a coluna Is active.

Podemos transformá-lo em um número inteiro minúsculo ou em um booleano apenas, e podemos transformá-lo em verdadeiro se

necessário; se não for necessário, podemos transformá-lo em falso para não precisarmos excluir a entrada em si.

Certo? Então, vamos selecionar os

tipos de colunas de dados.

Portanto, esse é um número inteiro.

Os detalhes de despesas inteiras são uma cadeira de rodas e a categoria é a mesma.

Criado por também é o mesmo criado em como data e atualizado em também é uma

data atualizado por é novamente o mesmo e está ativo é um número inteiro minúsculo.

Neste caso, preciso dar o valor como um e atualizá-lo.

Não é necessário fornecer nenhum valor atualizado por 255 a 55 ou qualquer que seja o limite preferido.

Podemos corrigir isso.

Estou colocando diretamente o valor de 255 e aqui estão os detalhes da despesa para que eu possa fazer isso como 500.

Talvez os detalhes sejam bastante longos aqui.

Posso mencionar que este é um número inteiro de comprimento 15, digamos, e este é

de comprimento ou podemos aumentá-lo para 20, se necessário, apenas para fins de teste.

Estamos fazendo isso e posso fazer o incremento automático porque essa é uma chave primária.

Automaticamente, o índice é selecionado como chave primária porque,

em qualquer tabela, a chave primária é obrigatória.

Sempre que estivermos escrevendo algum dado ou sempre que precisarmos atualizar

algum dado, precisaremos dessa chave primária, certo? Porque se não tivermos

a chave primária, será criada uma nova entrada na tabela.

Se estivermos tentando gravar alguns dados, se tivermos a chave primária, também poderemos atualizar os dados.

Portanto, é uma identificação exclusiva, como no AP, e chamamos isso de

identificador, e isso é obrigatório, certo? Temos que fazer isso e, nas

outras colunas, podemos armazenar nulo se o valor não estiver lá.

Certo? Então, estou fazendo isso porque,

se eu tentar escrever valores vazios

dentro dessa tabela, haverá um erro.

Se não for nulo, se já estiver marcado como nulo, o erro não ocorrerá.

Então, criei o nome da tabela e a configuração da coluna.

Eu só preciso clicar nisso, mais nada.

Assim, você pode ver que a tabela foi criada.

Estou dentro da estrutura e posso ver toda a configuração da coluna, o

nome da tabela e todos os detalhes, certo? E você pode ver que

esta é a minha chave primária, que é um ID de despesa.

E agora, se eu quiser ver os valores ou os dados dentro dessa tabela, posso entrar no browse.

Posso ver que não há nada dentro do SQL.

Posso executar qualquer consulta ou o que eu quiser para fazer a edição, inserção, atualização, o que for.

Posso escrevê-la aqui ou escolher estas opções para ajudar na consulta.

E, na pesquisa, posso pesquisar os dados, se houver muitos dados e precisarmos pesquisar

com alguns elementos de dados, e podemos colocá-los aqui e clicar em pesquisar.

E inserir é a opção que pode nos ajudar a inserir alguns dados manualmente,

certo? Por exemplo, temos que escrever alguns dados aqui, então posso escrever que

o valor da despesa é 100 e isso é para fins de teste.

Posso mencioná-lo como um teste e, se eu tentar pressionar go, ele inserirá diretamente alguns dados.

Assim, você pode ver que, na navegação, temos a despesa como um, o valor e os detalhes,

e também posso torná-la ativa como um, o que é verdade, e a exportação como exportação e

as importações são para a implantação, o que discutiremos sobre a implantação mais tarde e as operações.

Podemos criar algumas operações, como alterar o nome da tabela, renomear, copiar,

excluir e alterar a estrutura ou esvaziar, truncar ou excluir a tabela,

certo? Portanto, todos esses recursos que temos, podemos usar essas opções e

utilizar todas essas opções, certo? E agora minha tabela foi criada.

Agora, preciso ir ao meu designer de aplicativos e criar um for this,

pois usaremos o for para nos referirmos à anexação dessa tabela específica.

Certo.

E isso é chamado de tipo de dados personalizado, que abordaremos na próxima aula.

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

alerta de informação

## Programe um horário para o aprendizado
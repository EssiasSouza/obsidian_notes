E agora criaremos a tabela que criamos nas aulas anteriores.

E, mais uma vez, é necessário criar um clique nesse novo e eu preciso selecionar meu tipo de dados.

Type é o tipo de dados personalizado que estamos criando no momento.

Portanto, agora temos várias opções para criar esse tipo de

dados, certo? Podemos criá-lo do zero ou duplicar o tipo

de dados existente que também vimos em outros objetos.

Mas temos algumas outras opções, como criar a partir do banco de dados e importar XSD.

Então, quais são essas opções? Vamos entender

esses aspectos antes de entrar nessa opção.

Há um.

Há uma coisa que precisamos entender primeiro, e essa coisa é basicamente a

abordagem que precisamos seguir ao criar as tabelas do banco de dados final.

Portanto, se você puder ver essa tabela que criamos na aula anterior, primeiro criamos

a tabela do banco de dados e, em seguida, estamos criando a semente.

Portanto, essa abordagem é chamada de abordagem de baixo para cima.

E se criarmos uma tabela de banco de dados primeiro e depois isso for um fundo, se fizermos o contrário.

Por exemplo, se criarmos a primeira e depois avançarmos com a criação da tabela do banco de dados.

Portanto, essa é a abordagem de cima para baixo.

Portanto, qual é a abordagem que precisamos seguir e precisamos entender

isso, o que precisamos seguir, qual é a melhor abordagem.

Portanto, o melhor é de baixo para cima.

E há uma razão por trás disso, certo? Porque

há certas configurações que podemos fazer no banco de

dados, mas na criação é difícil fazer isso.

Certo, porque há opções limitadas na seleção do tipo de

banco de dados e do tipo de dados da coluna.

Certo.

Porque.

Vou lhe mostrar como podemos criar o que veremos mais adiante.

Certo.

Portanto, o que faremos agora, como já criamos a tabela do banco de

dados, basta clicar nessa terceira opção e selecionar minha fonte de dados.

Como todos sabem, minha fonte de dados é o Jdbc Appian e você pode

ver que há mais uma fonte de dados que vem do sistema conectado.

Portanto, se estivermos usando alguma fonte de dados externa, essa opção aparecerá.

Mas, se você não estiver usando nenhum sistema, ele será conectado automaticamente, que é o Jdbc e o Tomcat.

Portanto, preciso selecionar esse e, depois disso, selecionar o esquema.

Essa única tabela ou visualização.

E qual é o nome da minha

mesa? O nome da minha mesa é.

É mesmo? E

dados de despesas.

Portanto, só precisamos selecionar a fonte de dados e a tabela e continuaremos com isso.

Depois disso, precisamos colocar o namespace e, no namespace, usaremos o prefixo do aplicativo.

Então, por que estamos usando isso? Porque sempre que estivermos escrevendo o

banco de tipos que usaremos para acessar o tipo de dados personalizado,

usaremos apenas os tipos de dados que criamos para o nosso aplicativo.

Por isso, colocamos diretamente o prefixo do aplicativo no meu CD e o nome da tabela, que

é exatamente igual ao da tabela do banco de dados, e depois a descrição que podemos dar.

No momento, não estou fornecendo nenhuma descrição e este é o nome da minha coluna, que vem do banco

de dados, e este é o nome do meu campo, que será o que usaremos para o DT.

Portanto, o que farei, que também é uma prática recomendada, é tornar ambos os nomes semelhantes.

Portanto, no futuro, não ficaremos confusos com esses nomes porque essas são

as palavras-chave que precisamos usar quando estivermos nos referindo a esses

campos, certo? Por exemplo, estamos filtrando alguns dados com esses campos.

Em seguida, precisamos escrever o nome do campo nesse momento.

Portanto, farei com que ambos sejam semelhantes.

Portanto, qualquer que seja o nome que usaremos, ele será semelhante em todos os lugares.

E isso é o que eu fiz, só isso.

E agora vou desmarcar essa opção.

Explicarei essa opção para você.

E não precisamos fazer nada porque já temos a

configuração feita na tabela do banco de dados,

certo? E eu só preciso clicar em Create.

Portanto, é assim que precisamos criar o.

E agora há uma coisa que precisamos entender, pois criamos a tabela.

Também criamos o.

Agora, como essas duas coisas estão conectadas.

Como vamos nos certificar de que isso está se referindo a essa tabela de dados.

Portanto, o que precisamos fazer para verificar o mapeamento entre ambos é criar um armazenamento de dados.

Esse único armazenamento de dados é usado para verificar os mapeamentos da tabela com o banco de

dados e, em um aplicativo, pode haver vários, mas o armazenamento de dados será sempre um.

Talvez se estivermos usando alguns componentes diferentes, poderemos criar vários armazenamentos de dados.

Mas é melhor criar um único armazenamento de dados, certo? Porque, em

um armazenamento de dados, podemos criar um número n de entidades de

armazenamento de dados para as quais criaremos o armazenamento de dados.

Rastreador de despesas.

E a fonte de dados, mais uma vez, precisa ser especificada, pois

se tivermos diferentes fontes de dados, precisaremos criar diferentes armazenamentos de dados.

Portanto, para o meu exemplo, meu banco de dados, que já

está lá, usarei isso, a segurança e clicarei em Salvar.

E agora preciso criar minha entidade de armazenamento de dados.

Portanto, essa entidade que estou criando será usada em todos os lugares, onde

quer que eu busque os dados, escreva os dados e faça qualquer

coisa relacionada a essa tabela, certo? Portanto, é isso que precisamos usar.

Por isso, estou criando essa entidade de armazenamento de dados e vou nomeá-la novamente como.

Despesas.

Rastreador.

Ou o nome da tabela é basicamente dados de despesas.

Portanto, agora preciso selecionar o que criei.

Portanto, meu é um isso está chegando apenas e salvar e verificar diretamente.

Assim, você pode ver como é rápido.

Só preciso salvar e publicar.

Certo.

Portanto, se houver alguma configuração que não esteja correta, há algo que não está correto.

Então, esse mapeamento nos confirmará.

Certo? E então precisamos

corrigir esse erro.

Portanto, na próxima aula, veremos como podemos atualizar o banco de dados, porque sempre que nosso cliente disser ou

sempre que tivermos que modificar algumas colunas no banco de dados, precisaremos modificar o banco de dados de acordo.

Em seguida, precisamos verificar novamente o mapeamento do armazenamento de dados.

Certo.

Então, vamos ver isso na próxima palestra.
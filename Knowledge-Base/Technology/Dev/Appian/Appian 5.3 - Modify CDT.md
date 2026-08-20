Vamos ver a modificação de como podemos fazer isso e como

podemos lidar se houver algum erro no armazenamento de dados.

Então, vamos acessar aqui novamente e veremos primeiro a estrutura dessa tabela.

Portanto, há mais uma coisa que precisamos entender antes de vermos como podemos

modificar a tabela do banco de dados Or, se houver alguma modificação.

Então, uma coisa que precisamos entender é que na tabela do

banco de dados podemos criar várias colunas de tabela, certo?

E pode haver várias colunas e pode haver colunas que

não estamos usando e que não criamos dentro do CD.

Portanto, isso não será nada que não seja um

erro, certo? Portanto, em qual cenário, o erro ocorrerá.

O erro ocorrerá nesse cenário em que temos colunas no banco de dados, mas não

temos esse mapeamento e a própria coluna na tabela do banco de dados, certo?

Porque tudo, o que quer que estejamos fazendo, estamos fazendo aqui no

designer, certo? Estamos usando o CD para fazer referência a esse campo.

Portanto, se o campo estiver lá no banco de dados, mas

não estiver lá para gravar os dados, isso será um problema.

Mas se os dados no banco de dados tiverem algumas colunas

extras e essas colunas não estiverem no CD, não poderemos consultá-las

e não haverá nenhum erro, certo? Então, para entender isso, vou

diretamente remover isso ou, digamos, vou remover isso atualizado por coluna.

Certo.

Então, agora eu removi essa coluna aqui e a coluna está lá dentro da coluna.

Tabela do banco de dados, certo? Portanto, essa

é uma condição em que não haverá erro.

E atualizarei meu armazenamento de dados.

E verá a verificação.

Você pode ver que não há nenhum problema.

Mas se a coluna estiver lá, digamos, se a coluna estiver ativa, estiver dentro do

meu banco de dados e eu tiver excluído a coluna aqui do banco de dados.

Estou caindo.

Isso está ativo e agora tentarei verificar meu armazenamento de dados para ver se o mapeamento está correto ou não.

Então, estou recebendo o erro e é assim que podemos dar uma olhada nesse erro.

Você pode ver que a coluna ausente está ativa

na tabela, certo? Portanto, é assim que precisamos fazer.

Entenda isso e como podemos resolver esse problema.

Antes de tudo, temos duas opções.

Podemos também remover essa coluna ativa daqui ou adicionar uma coluna que esteja ativa.

Posso adicionar uma coluna depois de atualizada por e ir, e posso nomeá-la como

is active integer e tiny integer one e null como yes e salvar.

Portanto, agora minha coluna está classificada.

Vou atualizar isso novamente.

E eu verificarei isso e está feito.

Vou salvar e publicar novamente.

Portanto, essa é uma maneira de lidar com o erro.

Certo.

Mas há mais um aspecto em que precisamos modificar a peça.

Por exemplo, há uma condição ou um requisito em

que precisamos modificar a duração desse detalhe de despesa.

Preciso chegar a 1.000, então como posso fazer isso.

Portanto, nesse cenário, precisamos modificar o arquivo XSD.

Portanto, vamos ver como podemos fazer isso em nossa próxima palestra.
Nesta palestra, abordaremos os grupos e a segurança dos grupos.

Portanto, primeiro começaremos com os grupos.

O que são grupos? Então, no Appian, sempre que

estamos criando qualquer aplicativo e queremos definir alguma hierarquia

ou queremos definir alguma segurança de objetos nos

aplicativos Appian ou a segurança do aplicativo, então

criamos grupos, certo? E podemos usar esse objeto.

Só preciso entrar nesse novo botão e depois clicar nos grupos.

Portanto, sempre que estivermos criando um grupo ou qualquer um dos objetos do

aplicativo, só precisamos nos certificar de que estamos usando o prefixo do aplicativo.

Portanto, como você pode ver, estamos usando o prefixo do aplicativo no nosso

caso, que é far, e criarei um grupo que será todos os usuários.

Portanto, em nosso aplicativo, a prática recomendada é sempre criar dois grupos.

O primeiro é o de todos os usuários e o segundo é o de grupos de administradores.

Portanto, agora estou criando todos os grupos de usuários e posso dar a descrição como todos os grupos de usuários e, no

momento, não há outros grupos, portanto, o grupo pai deve estar vazio e os membros do grupo pai devem estar vazios.

Portanto, nos membros do grupo, vamos adicionar um membro do grupo que é

low code, que é meu usuário, e o tipo de grupo é personalizado.

Posso escolher qualquer outro tipo de grupo se eu tiver criado um.

Portanto, neste aplicativo, não criei nenhum tipo de grupo e essas são algumas das opções padrão.

Você não precisa se preocupar com isso agora.

Essas são apenas opções simples.

Posso pressionar diretamente o botão Criar e, mais uma vez, preciso definir a segurança.

Mas, atualmente, não há grupos de administradores porque não podemos dar nenhum outro nível de permissão aos grupos.

Você pode ver que não há acesso.

Se eu tentar criar mais um, somente a permissão administrativa

deverá estar presente e não poderemos nem mesmo editá-lo.

Portanto, como atualmente não temos nenhum grupo de administradores, não usarei isso.

Vou clicar diretamente no botão Salvar e agora meu grupo de todos os usuários está criado.

Você pode ver que há apenas um único membro, um único membro e que é de baixo código.

Se eu voltar, também posso criar mais um grupo, que será meu grupo de administradores.

A mesma coisa que preciso fazer e posso nomear esse grupo de administradores ou diretamente admins e vou escrever

aqui todos os usuários administradores e, em seguida, o grupo pai que posso selecionar, mas vamos selecioná-lo mais tarde.

E agora, novamente, preciso fornecer o nome do membro do grupo.

Portanto, selecionarei um membro, que é Mukesh, e o tipo de grupo é personalizado e outras opções por padrão.

Agora já criei esse grupo para administradores.

Esse é o meu grupo de administradores que usarei em meu aplicativo.

Agora posso usar esse grupo para dar o nível de permissão, portanto,

usarei o mesmo grupo que acabei de criar para os administradores.

E você pode ver que o aviso não está chegando.

Portanto, essa é a prática recomendada para usar grupos nesse nível de permissão;

não usar diretamente grupos de usuários é a melhor coisa a fazer.

Certo? E já selecionamos

o grupo de administradores.

Vamos clicar em Salvar e agora meus dois grupos estão criados.

Portanto, é assim que podemos criar um grupo.

E vamos fazer uma coisa.

Você pode ver que esses dois botões estão lá.

A primeira é a visão plana e a outra é a visão hierárquica.

Então, atualmente, quando estou clicando neste e no outro, a

mesma coisa está acontecendo, certo? Porque não há hierarquia.

Esses são objetos independentes.

Ou podemos dizer grupos independentes.

Então, o que eu preciso fazer é definir o pai do

admin, certo? Então, para fazer isso, vou entrar nas propriedades.

Dentro das propriedades.

Isso está vazio.

Agora posso selecionar para todos os usuários.

Certo? Portanto, é assim que

podemos selecionar o pai.

Mas por que estamos selecionando esse pai? Porque

os membros dos grupos de administradores devem ser

todos os usuários, certo? É uma coisa simples.

Por exemplo, sempre que eu selecionar o grupo de administradores para dar alguma permissão ou algum critério de

visibilidade a qualquer objeto, quem fizer parte do grupo de administradores deverá ter esse nível de permissão.

Certo.

E os membros do grupo que não fazem parte desse grupo de administradores não devem ter essa permissão.

Certo.

Então, vou lhe mostrar.

É um pouco confuso, mas é simples.

Agora, você pode ver que selecionei o pai do admin como todos os usuários, portanto,

se eu entrar nessa hierarquia, você poderá ver que todos os usuários só estão aparecendo.

E se eu entrar em todos os usuários, você poderá ver que todos os membros estão vindo com os grupos.

Também porque o selecionamos quando criança.

É por isso que os usuários que estão dentro disso agora estão vindo para cá.

Além disso, você pode ver meu nome

aqui, certo? Eu sou o administrador.

Meu nome também está aqui no all user.

Então, neste momento, eu sou a criança, certo? Portanto, terei a permissão

do grupo de administradores, bem como do grupo de todos os usuários.

Essa é a relação entre esses grupos, certo? E sempre que

selecionamos qualquer grupo, por exemplo, estamos configurando a segurança de

uma pasta e agora selecionamos um grupo aqui, e os

filhos, que criaremos dentro dessa pasta, herdarão automaticamente essa segurança.

Veremos isso quando estivermos definindo a segurança do aplicativo.

Portanto, agora tudo está relacionado aos grupos, como podemos criar os grupos e como

podemos gerenciar o relacionamento entre os grupos definindo o relacionamento pai e filho.

E podemos usar grupos para fins de segurança.

Por exemplo, também precisamos verificar alguns critérios de visibilidade.

Então, também podemos usar grupos ou precisamos definir a segurança de qualquer objeto ou a segurança do aplicativo.

Isso é tudo nesta palestra.
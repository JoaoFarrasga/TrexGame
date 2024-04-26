# Jogo do T-Rex

Este jogo foi feito por [Yeti47](https://github.com/Yeti47).

Trabalho de Tecnologias de Desenvolvimento de VideoJogos.

Feito por João Antunes, a23478 e João Freitas, a23475.

## T-Rex

### GameState.cs

1. O namespace é uma maneira de organizar e agrupar tipos relacionados em um escopo específico, neste caso o namespace TrexRunner.
2. O public enum GameState é onde começa a ser definido um novo tipo de enumeração chamado GameState. Neste contexto, GameState é usado para representar os
   diferentes estados do jogo sendo eles:

a.      Initial - que representa o estado inicial do jogo.

b.     Transition - que representa o estado de transição do jogo.

c.      Playing - que representa o estado em que o jogo está a ser efetivamente jogado.

d.     GameOver - que representa o estado em que o jogo acaba, seja por derrota ou conclusão.

Neste caso, o GameState é usado para controlar o fluxo do jogo e determinar o comportamento com base no estado atual do jogo.

![GameState](https://i.imgur.com/AagfVOK.png)

### Program.cs

![Program](https://i.imgur.com/zLVXrKn.png)

1. public static class Program é uma classe estáticas que é classes que não podem ser instanciadas e servem como containers para métodos e propriedades estáticas.
2. [STAThread] Este atributo é usado para indicar que a thread principal da aplicação deve ser tratada como uma thread de interface do usuário comum.
3. static void Main() Este é o ponto de entrada do programa, onde a execução do mesmo começa.
4. using (var game = new TRexRunnerGame()) game.Run(); cria uma instância da classe TRexRunnerGame e a utiliza-a para executar o jogo.

Esta classe é o ponto de entrada do programa e é responsável por iniciar a execução do jogo TRexRunnerGame.

### SaveSate.cs

![Save State](https://i.imgur.com/FrCVcoc.png)

1. [Serializable] indica que a classe SaveState pode ser convertida em uma sequência de bytes para armazenamento ou transmissão.
2. public class SaveState Define uma classe chamada SaveState, que representa o estado de save do jogo.
3. public int Highscore { get; set; } Uma propriedade que representa a pontuação mais alta alcançada.
4. public DateTime HighscoreDate { get; set; } Uma propriedade que representa a data e hora em que a pontuação mais alta foi alcançada.

Esta classe é utilizada para armazenar informações sobre o progresso do jogador. Estas informações podem ser salvas em disco para serem carregadas posteriormente.

### TRexRunnerGame.cs

![TRexRunnerGame](https://i.imgur.com/sjZugsH.png)

Um enum que ajuda a definir o modo de display do jogo,
default ou com zoom.

![Constantes 1](https://i.imgur.com/VnEaFob.png)

Conjunto de variáveis que vão ser usadas na construção do jogo como por exemplo as dimensões da janela, a posição inicial onde o jogador vai começar o jogo, efeitos sonoros, entre outras.

![Construtor](https://i.imgur.com/hSgV30j.png)

TRexRunnerGame() é um construtor que é responsável por gerir coisas como as configurações gráficas, o diretório onde estão os recursos usados pelo jogo(a pasta Content), a visibilidade do rato, as entidades do jogo, como inimigos e o jogador, o estado do jogo e um efeito de fade-in na tela utilizado nas transições entre telas e/ou estados do jogo.

Initialize() é um método que é essencial para configurar o ambiente gráfico do jogo, iniciar recursos e definir o estado inicial do jogo antes deste começar a ser executado.

![7](https://i.imgur.com/6SOaXgD.png)

O método LoadContent() é parte do ciclo de vida do jogo e é chamado uma vez durante a iniciação para carregar todos os recursos necessários para o jogo, como texturas de sprites, efeitos sonoros e outros dados do conteúdo do jogo.

![8](https://i.imgur.com/IJ3alzM.png)

Estes dois métodos são chamados em resposta a eventos específicos que acontecem durante o jogo:

1. O primeiro é chamado quando o jogador colide com um obstáculo e perde o jogo, chamando a tela de game over e desabilitando o gestor de obstáculos.
2. O segundo é chamado quando o jogo começa e é responsável por iniciar coisas como o render do jogador e o gestor de obstáculos.

![alt text](https://i.imgur.com/2eYlL3o.png)

O método Update é chamado em cada frame do jogo e é responsável por atualizar o estado do jogo com base nas entradas do jogador e
em outras condições.

Neste caso temos as condições onde

1. É verificado se a tecla “escape” ou o botão “back” é pressionado e caso seja chama o exist que fecha o jogo
2. Se o estado do jogo for Playing, processa as entradas do jogador através do controlador de entrada
3. Se o estado do jogo for "Transition" atualiza a posição da textura de
   fade-in.
4. Se o estado do jogo for Initial verifica se o jogador pressionou a tecla de início. Se sim, inicia o jogo chamando o método StartGame().
5. Verifica se a tecla F8 foi pressionada e não tinha sido pressionada na última atualização. Se sim, chama o método ResetSaveState() para redefinir o estado do jogo.
6. Verifica se a tecla F12 foi pressionada e não tinha sido pressionada na última atualização. Se sim, chama o método ToggleDisplayMode() para alternar entre os modos de exibição.
7. Atualiza o estado anterior do teclado para o estado atual, para ser
   usado na próxima iteração.

![alt text](https://i.imgur.com/jCUersA.png)

Este método Draw é chamado em cada frame do jogo e é responsável por carregar todos os elementos visuais na tela.

1. Verifica se o gestor do céu é nulo. Se for limpa o dispositivo gráfico com a cor branca.
2. Limpa o dispositivo gráfico com a cor definida pelo gestor do céu. Isto define a cor de fundo da tela de acordo com o céu, permitindo um efeito de transição suave durante o jogo.
3. Inicia o desenho de sprites com o objeto SpriteBatch. Define o estado do amostrador para PointClamp para garantir um carregamento nítido dos pixels. Também aplica a matriz de transformação _transformMatrix, que pode ser usada para escalar ou transformar o carregamento dos sprites, dependendo do modo de exibição atual.
4. Chama o método Draw do gestor de entidades, passando o objeto SpriteBatch e o tempo de jogo como parâmetros. Isto desenha todas as entidades no jogo.
5. Verifica se o estado do jogo é "Initial" ou "Transition". Se for, desenha a textura de fade-in na tela para criar um efeito de transição suave.
6. Chama o método Draw da classe base para continuar o processo de carregamento d outros componentes do jogo. Isto garante que todos os elementos visuais do jogo sejam carregados corretamente.

![alt text](https://i.imgur.com/LPT9C2P.png)

Estes dois métodos são responsáveis por iniciar ou reiniciar o jogo.

1. O primeiro é chamado quando o jogo inicia e verifica se o estado atual do jogo é "Initial". Se não for, significa que o jogo já está em andamento ou já foi iniciado anteriormente, então o método retorna false. Caso contrário, ele redefine a pontuação para zero, define o estado do jogo como "Transition" e inicia o “BeginJump”. Por fim, retorna true para indicar que o jogo foi iniciado com sucesso.
2. O segundo é chamado quando o jogador decide jogar novamente após o jogo ter terminado. Ele verifica se o estado atual do jogo é "GameOver". Se não for, significa que o jogo não pode ser reiniciado e retorna false. Caso contrário, ele redefine o estado do jogo como "Playing" (em jogo), reinicia o TRex, redefine os obstáculos e habilita-os. Além disso, desativa a tela de "Game Over", redefine a pontuação para zero, reinicia o chão e bloqueia temporariamente a entrada do jogador. Por fim, retorna true para indicar que o jogo foi reiniciado com sucesso.

Método responsável por salvar o estado atual do jogo em
um arquivo:

![alt text](https://i.imgur.com/e5WeTEL.png)

Ele primeiro cria uma instância de SaveState, em seguida, ele tenta serializar essa instância em um arquivo usando a classe BinaryFormatter, que converte objetos em uma sequência de bytes e vice-versa, se a for bem-sucedida, o arquivo será criado e os dados do saveState serão
gravados nele.

Se ocorrer algum erro durante esse processo, uma exceção será lançada e o erro será tratado, registando uma mensagem de erro no console de depuração com detalhes sobre o problema.

Método responsável por carregar o estado salvo do jogo a partir de um arquivo.

![alt text](https://i.imgur.com/7yyDGqh.png)

Ele tenta abrir um arquivo usando FileStream para leitura e escrita, com FileMode.OpenOrCreate. Isso significa que se o arquivo existir, ele será aberto; caso contrário, um novo arquivo será criado, em seguida, ele cria uma instância de BinaryFormatter para desserializar o conteúdo do arquivo.

Se for bem-sucedida e o objeto saveState for diferente de null, ele atualiza o HighScore do ScoreBoard com o valor carregado do arquivo e também atualiza a data do highscore.

Se ocorrer algum erro durante esse processo, uma exceção será lançada e o erro será tratado, registando uma mensagem de erro no console de depuração.

![img](https://i.imgur.com/uEHl2QW.png)

Estes métodos são responsáveis por alterar o estado do jogo em relação à exibição e ao estado do highscore.

1. O primeiro método redefine o highscore para zero e a data do highscore para o valor padrão de DateTime, em seguida, chama o método SaveGame() para guardar essas alterações.
2. O segundo método altera entre os modos de exibição padrão e ampliado. Se o modo de exibição atual for Default, ele alterna para Zoomed. Além disso, ajusta a matriz de transformação para escalar a tela com base no fator de zoom.

Se o modo de exibição atual for Zoomed, ele alterna para Default, restaurando as dimensões do buffer e redefine a matriz de transformação para a identidade.

Finalmente, chama ApplyChanges() para aplicar as alterações feitas nas dimensões.

### Entities

#### IGameEntity.cs

"IGameEntity" é uma "Interface" dentro do "TrexRuneer.Entities" "namespace", onde especifica os métodos e as propriedades das Entidades para ser depois implementadas nas classes. Esta "Interface" contém três Elementos:

* 'int DrawOrder { get; }' : Esta propriedade define um número inteiro que indica a ordem que as Entidades devem ser desenhadas no ecrã.
* 'voidUpdate(GameTimegameTime)' : Este método representa a lógica que irá ser executada no "Update" da entidade. Recebe um pârametro do tipo "GameTime", que fornece informação sobre o tempo que decorreu desde o último "Update".
* 'voidDraw(SpriteBatchspriteBatch, GameTimegameTime)': Este método representa a lógica para renderizar a entidade no ecrã. Recebe dois parâmetros:
  * 'SpriteBatch': Usado para renderizar gráficos 2D para os objetos.
  * 'GameTime': Como explicado em cima, fornece a informação sobre o tempo do tempo decorrido.

![img](https://i.imgur.com/jV24OOu.png)

#### EntityManager.cs

"Entity Manager" é um componente importante para a organização e manipulação das entidades neste jogo, gerenciado todas as entidades do jogo, permitindo adicionar, remover, atualizar e desenhar entidades de forma eficiente e organizada. Fornecendo uma maneira estruturada de lidar com a lógica e a renderização das entidades do jogo.

Para gerir todas as entidades do jogo, são usados três campos privados, onde são armazenadas entidades para então depois serem usadas nos métodos, "entities" é uma lista que armazena todas as entidades do jogo, "entitiesToAdd" e "_entitiesToRemove" são listas auxiliares usadas para adicionar e remover entidades.

![img](https://i.imgur.com/44Vd4kz.png)

"Entity Manager" contém seis métodos que ajudam na organização das entidades neste jogo: "Update", "Draw", "AddEntity", "RemoveEntity", "Clear", "GetEntitiesOfType".

* Método "Update": Itera todas as entidades e chama o método 'Update' de cada uma, passando o GameTime como o parâmetro. Remove as entidades que estão na lista "entitiesToRemove". Adiciona as entidades que estão na lista "entitiesToAdd".
* Método "Draw": Itera todas as entidades e chama o método' Draw' de cada uma, passando o "SpriteBatch" e o "GameTime" como parâmetros.

![img](https://i.imgur.com/yVGD50M.png)

* Métodos "AddEntity" e "RemoveEntity": são usados para adicionar entidades as listas auxiliares para depois serem usadas no "Update" para adicionar e remover, ambos os métodos aceitam apensa entidades não nulas.
* Método "Clear": Adiciona todas as entidades da lista "entities" à lista "entitiesToRemove" para serem removidas assim limpando o jogo das entidades.
* Método "GetEntitiesOfType": Retorna todas as entidades de um tipo especificado da lista "_entities".

![img](https://i.imgur.com/I2arC8g.png)

#### TrexState.cs

"TrexState" define um Enum usado para os estados no Trex neste jogo, definindo os diferentes comportamentos possíveis parao TRex, permitindo que o jogo saiba como ele deve se comportar em diferentes situações. O Enum tem: "Idle", "Running", "Jumping", "Ducking", "Falling".

![img](https://i.imgur.com/6IOPIg9.png)

#### Trex.cs

"Trex" defina a classe "Trex", que representa o personagem principal do jogo, contendo as constantes, variedades e métodos que o "Trex" necessita para ser, ou renderizado, ou até para saltar.

Esta classe contém múltiplas Constantes utilizadas para determinar valores relacionados ao personagem, como velocidade de queda, altura mínima do salto, gravidade, entre outros.

Também como as Constantes, existem múltiplas variáveis, sendo tudo preciso para ser usado para renderizar os "Sprites", para os sons que o "Trex" faz quando salta, para as animações que ele faz seja a correr, ou a baixar-se, também guardando a posição inicial, posição atual, velocidade, a ordem de renderização e por final o Estado do "TrexState" é que o "Trex" está.

Métodos:

* "Initialize()": Inicializa o Trex com os valores padrão.
* "Draw(SpriteBatchspriteBatch, GameTimegameTime)": Usado para renderizar o T-Rex no ecrã com base no estado atual.

![img](https://i.imgur.com/73dlKL3.png)

* "Update(GameTimegameTime)": Atualiza a lógica do Trex com base no estado atual e no tempo decorrido.
* "BeginJump()", "CancelJump()", "Duck()", "GetUp()", "Drop()": Métodos para iniciar o salto, cancelar o salto, agachar, levantar e cair, respetivamente.

![img](https://i.imgur.com/hvTCXrb.png)

* "Die()": Faz o Trex morrer, alterando o seu estado e sinalizando o evento 'Died'.

![img](https://i.imgur.com/MINEPOc.pnghttps://i.imgur.com/MINEPOc.png)

#### ICollidable

"ICollidable" é uma "interface" usada para representar objetos que podem colidir com outros objetos no jogo, contendo uma única propriedade, "CollisionBox", sendo uma propriedade de apenas leitura do tipo "Rectagle", que representa a área de colisão neste jogo.

![img](https://i.imgur.com/8mS6xvR.png)

#### ScoreBoard

"ScoreBoard" implementa uma classe chamada "ScoreBoard" que representa o "ScoreBoard" do jogo, contendo múltiplas Constantes, Variáveis e Métodos para que a "ScoreBoard" funcione como é desejada.

As Constantes guardam valores que não podem ser mudados, como as coordenadas onde a "ScoreBoard" está no ecrã, a quantidade de dígitos que a "ScoreBoard" pode ter, seguindo do valor máximo que a "ScoreBoard" pode ter e entre outras Constantes.

As Variáveis não são tantas quanto as Constantes, mas ainda sendo precisas algumas para que a classe funcione, como o "Score" que é a pontuação atual do jogo, "DisplayScore" a pontuação atual a ser exibida na "ScoreBoard", "HighScore" a pontuação máxima alcançada, "HasHighScore" indica se existe uma pontuação máxima definida, "DrawOrder" como explicado anteriormente é a ordem de renderização e por final a "Position" que é a posição da "ScoreBoard" no ecrã.

Os Métodos:

* "Draw", renderiza a "ScoreBoard" no ecrã, incluindo a pontuação atual e, se houver, a pontuação máxima.
* "Update", atualiza a "ScoreBoard" ao longo do tempo, aumento a pontuação com base na velocidade do "Trex". Também controla a animação de flash quando a pontuação atinge um novo centésimo.

![img](https://i.imgur.com/i8GAjg0.png)

* "SplitDigits", divide um número inteiro em seus dígitos individuais.
* "GetDigitTextureBounds", obtém os limites da textura para um dígito específico com base em sua posição no sprite.

![img](https://i.imgur.com/i8GAjg0.png)

#### Obstacle

"Obstacle" define uma classe abstrata chamada "Obstacle" que representa os obstáculos no jogo.

Variáveis, esta classe contém quatro variáveis, "_trex" uma referência ao objecto "Trex", "CollisionBox" é uma propriedade abstrata que representa a caixa de colisão do obstáculo, "DrawOrder", "Position" uma variável protegida que indicada a posição do obstáculo no ecrã.

No Construtor, ele receber dois parâmetros uma referência ao objeto "Trex" e a sua posição atual.

Nos Métodos existe o "Draw", um método abstrato para renderizar o obstáculo na tela, o "Update", que atualiza a posição do obstáculo com base na velocidade do "Trex", além disso verifica se houve colisão entre o obstáculo e o "Trex", e para acabar o "CheckCollision", um método privado que verifica se houve colisão entre o obstáculo e o "Trex". Se houver, chama o método "Die()" do "Trex", indicando que o jogador perdeu o jogo.

![img](https://i.imgur.com/d4IWpH6.png)

#### ObstacleManager.cs

"ObstacleManager" implementa um gerenciador de obstáculos para o jogo, e é responsável por gerenciar a criação e remoção de obstáculos durante o jogo, garantido que eles apareçam de acordo com a progressão do jogador e as regras de spawn definidas.

As Variáveis desta classe armazenam múltiplas coisas que vão ser usadas para que a classe funcione, como a última pontuação em que um obstáculo for gerado, a distância entre obstáculos,referênciasao "EntityManager", ao "Trex", à "ScoreBoard", e múltiplos "bool" para ativar ou desativar os gerenciadores.

No construtor recebe-se um "EntityManager", "Trex", "ScoreBoard" e "SpriteSheet" para então utilizar estas variáveis recebidas nos métodos desta classe.

![img](https://i.imgur.com/lTxbojE.png)

Os Métodos desta classe, são o "Draw", "Update", "SpawRandomObstacle" e "Reset".

* "Update" atualiza o gerenciador de obstáculos e verifica se é possível gerar novos obstáculos e remove os existentes conforme necessário.

![img](https://i.imgur.com/8Ku0HSg.png)

* "SpawnRandomObstacle" gera um obstáculo aleatório com base numa determinada taxa de spawn, podendo gerar grupos de Cactos ou Dinossauros Voadores.

![img](https://i.imgur.com/3dPbToN.png)

* "Reset" que limpa todos os obstáculos existentes e redefina as variáveis internas do gerenciados de obstáculos.

![img](https://i.imgur.com/ABYUIw6.png)

#### Flying Dino

"FlyingDino" implementa a classe "FlyingDino" que representa um obstáculo no jogo, representando um dinossauro voador e controla a sua animação e movimento na tela. Também possui funcionalidades para lidar com colisões com o jogador ("Trex").

![img](https://i.imgur.com/3ph66my.png)

#### CactusGroup.cs

"CactusGroup" implementa a classe "CactusGroup", que representa um grupo de cactos como um obstáculo no jogo, criando e desenhando um grupo de cactos com base no tamanho especificado e na textura do sprite sheet fornecida. Também possui funcionalidades para lidar com colisões com o jogador (“Trex”).

![img](https://i.imgur.com/QWwsAyv.png)

#### IDayNightCycle.cs

"IDayNightCycle" define uma interface chamade "IDayNightCycle", que relaciona o ciclo de dia e noite no jogo, fornecendo uma maneira de gerenciar e consultar informações sobre o ciclo de dia e noite no jogo, contendo um inteiro "NightCount", que retorna o númeor de ciclos noturnos que ocorreram, um "boolean" "IsNight", que retorna o valor "True" se estiver de noite e "False" se estiver de dia, e uma cor "ClearColor", para a cor de fundo.

![img](https://i.imgur.com/lISXbsp.png)

#### SkyObject.cs

"SkyObject" define uma classe abstrata chamada "SkyObject", que representam os objetos no céu do jogo, contendo o "Trex", o "DrawOrder", a "Velocidade" e a "Position", contêm um construtor que é usado para definir o "Trex" e a "Position".

Depois contêm dois Métodos o "Draw" e o "Update", um método abstrato e um virtual, respetivamente, o "Update" atualiza a posição do objeto devido a Velocidade e se o "Trex" aidna estiver vivo.

![img](https://i.imgur.com/agSwVAe.png)

#### Star.cs e Moon.cs

"Star" e "Moon" definem duas classes "Star" e "Moon", respetivamente, que representam uma estrela e uma lua no céu do jogo, controlando as suas animações e as suas renderizações, garantindo que elas sejam visíveis apenas durante a noite e que suas animações sejam atualizadas apenas quando o** "Trex" estiver vivo.

![img](https://i.imgur.com/wq2POMF.png)

![img](https://i.imgur.com/KcVyLBQ.png)

#### Cloud.cs

"Cloud" define a classe "Cloud", que representa uma nuvem no céu do jogo, como a "Star" e a "Moon", controla a sua animação e renderização, mas este objeto aparece durante o dia e a noite, não ficando preso à noite.

![img](https://i.imgur.com/QQoUs42.png)

#### SkyManager.cs

"SkyManager" define a classe "SkyManager", que é responsável por gerenciar os objetos no céu do jogo, como nuvens, estrelas e a transição entre o dia e a noite, sendo uma parte crucial no sistema do fundo do jogo, controlando elementos visuais do céu e a transição entre o dia e anoite com base no progresso do jogador.

Contêm múltiplas constantes que guardam as "DrawOrder" dos objetos, as posições máximas e mínimas de cada, e a distância máxima entre eles, com o Score de quando o tempo muda para noite, e quando tempo ela fica assim**.**

Contêm também algumas varáveis, principalmente referencias para os gerenciadores como "EntityManager", "Trex", "ScoreBoard", para serem usados auxiliando a renderização dos objetos no céu.

![img](https://i.imgur.com/ioJvarQ.png)

![img](https://i.imgur.com/WZyKMbX.png)

Os Métodos contêm um construtor para que o SkyManager consiga receber tudo o que precisa quando é ativado, o "Draw", "Update", "TrasitionToNighTime", "TrasitionToDayTime", "HandleCloudSpawning", "HandleStarSpawning" e "UpdateTrasition", que são responsáveis pela rederização dos objetos do céu, atualizar o estado o SkyManager, iniciar e terminar a transição entre o dia e anoite, gerenciar o spawn das nuvens e estrelas, e atualizar a transição entre o dia e a noite, respetivamente.

![img](https://i.imgur.com/A0QJ4HA.png)

#### GameOverScreen.cs

"GameOverScreen" defina a classe "GameOverScreen" que representa a tela do jogo quando o jogo terminar e permite que o jogador reinicie o jogo.

Com constantes para definir a posição do botão para reiniciar o jogo e a de "Game_Over" e o tamanho delas. Com variáveis para definir a sua "DrawOrder" e se está a aparecer ou não, sendo usadas para renderizar e atualizar conforme necessário. Contendo um construtor para conseguir usar a textura do "Game_Over" e a textura do butão.

![img](https://i.imgur.com/MEKOZIS.png)

Contendo dois métodos, o "Draw" e o "Update", uma para renderizar o ecrã para o fim do jogo, renderizando o "GameOver" e o botão para reiniciar o jogo, e o outro para atualizar o ecrã, e verifica se o botão de reinício foi pressionado e em caso de sim chama o "Replay()" do jogo, respetivamente.

![img](https://i.imgur.com/oV7nA2e.png)

#### GroundTile.cs

"GroundTile" define uma classe chamada "GroundTile" que implementa a interface, documentada acima, "IGameEntity".

Contendo quatro propriedades, uma privada que armazena a posição Y do Tile, seguida da posição X, Sprite e DrawOrder, para acessar e definir cada uma com a posição X do Tile, o Sprite associado ao Tile e como explicado anteriormente para definir a ordem para os Tiles serem Renderizados.

Segue com um construtor, para definir essas propriedades, com o Update e o Draw.

![img](https://i.imgur.com/ZkLuP2R.png)

#### GroundManager.cs

O "GroundManager" é responsável por gerenciar os tiles do chão neste Jogo. Ele utiliza uma lista de "GroundTile", como explicado em cima, para rastrear os tiles Atuais, seguido com mais duas listas uma para remover e uma para adicionar, com isso esta class trata completamente dos Tiles.

Contendo as mesmas constantes que todas as outras entidades têm, as constantes que guardam em que posição da spritesheet está o sprite desejado.

Tem o DrawOrder como todos os outros para definir a ordem da renderização no jogo.

Segue um construtor que recebe os parâmetros que o Manager precisa para funcionar, com um Update que é responsável por atualizar o GroundManager com base no tempo, Verificando se existem tiles no chão e, se o tile que está mais à direita estiver fora da tela, removendo esse e adicionando um novo à esquerda, e por final atualiza os tiles existententes com a rapidez do Trex.

![img](https://i.imgur.com/PmZxmmg.png)

Contem quatro funções, o Initialize que limpa as listas dos tiles existentes, e começa por criar um tile novo na posição 0, o CreateRegularTile que cria um tile com o sprite normal, o CreateBumpyTile que cria um tile com o sprite diferente, e por final o SpawnTile que gera um tile nova numa posição especificada.

![img](https://i.imgur.com/Or5xS5P.png)

### Extensions

#### Texture2DExt.cs

Este código define uma classe estática chamada Texture2DExt, que contém um método de extensão para a classe Texture2D. O método de extensão InvertColors permite inverter as cores de uma textura 2D.

1. Este método estende a classe Texture2D, o que significa que agora ele pode ser chamado em instâncias de Texture2D.Dentro do método, ele verifica se a textura fornecida é nula. Se sim, lança uma exceção.

![img](https://i.imgur.com/FTvYJM0.png)

Em seguida, cria uma textura 2D chamada result com as mesmas dimensões da textura original. Em seguida, inverte as cores de cada pixel. Se excludeColor for fornecida e um pixel na textura original corresponder a essa cor, ele mantém o pixel inalterado. Caso contrário, inverte a cor do pixel.

Por fim, os dados de pixel invertidos são definidos na textura resultante usando o método SetData, e a textura resultante é retornada.

### Graphics

#### Sprite.cs

Este código tem como propriedades a texture que representa a textura do sprite, x e y que representam as coordenadas, width e height que representam a largura e altura do sprite respetivamente e tintcolor que representa a cor.

Possui um construtor que recebe como argumentos a textura do sprite, as coordenadas X e Y na textura e as dimensões Width e Height.

Possui ainda o método Draw que desenha o sprite na tela utilizando spritebatch e vector2 para especificar onde o sprite vai ser desenhado.

É passado como parâmetros a textura, a posição, um retângulo onde são passadas a altura, largura, bem como o X e Y onde vai ser desenhado e a sua cor.

![img](https://i.imgur.com/IfFPeKb.png)

#### SpriteAnimationFrame.cs

Este código define uma classe que representa um quadro de animação de um sprite.

Tem um campo que armazena o sprite e propriedades que fornecem o acesso a esse sprite e outra que armazena a data e hora de um frame.

Tem um construtor que recebe um objeto Sprite e um valor de dta/hora do frame e faz a iniciação das propriedades da classe.

![img](https://i.imgur.com/opR5V1B.png)

#### SpriteAnimation.cs

Este código define uma classe que representa uma animação de sprites composta por vários quadros.

Tem um campo que representa os frames da animação e propriedades que retornam o número total de frames e outra que retorna o frame atual.

Tem ainda um index que permite ter acesso a um frame específico da animação e um método que retorna esse frame.

De maneira a facilitar a manipulação de animações esta classe permite o acesso aos frames individuais e ao frame atual com base no progresso de reprodução.

![img](https://i.imgur.com/GLUncis.png)

A propriedade Duration calcula e retorna a duração total da animação. Primeiro, verifica se a lista de frames não está vazia e se não estiver vazia retorna o valor máximo de data/hora entre todos os frames na lista.

A seguir temos variáveis que indicam se a animação está em reprodução, que controlam o progresso atual da reprodução da animação e que propriedade indicam se a animação deve ser repetida.

Posto isto temos um método que é usado para adicionar um novo quadro à animação.

![img](https://i.imgur.com/IgpKd0l.png)

O primeiro método é usado para atualizar o estado da animação.

Se a animação estiver em reprodução o progresso da reprodução é atualizado. Em seguida, verifica-se se o progresso da reprodução excedeu a duração total da animação. Se isso acontecer e a animação deve ser repetida, caso contrário, a reprodução é interrompida.

O segundo método é usado para desenhar o quadro atual da animação na tela.

Obtém o quadro atual da animação. Se o quadro não for nulo, desenha o sprite.

O terceiro método inicia a reprodução da animação.

O último método interrompe a reprodução da animação.

Juntos, estes métodos permitem controlar quando e como uma animação de sprite é reproduzida e desenhada no jogo.

![img](https://i.imgur.com/GF69RIr.png)

O primeiro método retorna o frame da animação localizado no índice especificado. Primeiro verifica se o índice fornecido está dentro dos limites válidos dos quadros da animação. Se não estiver, ele lança uma exceção e em seguida, retorna o quadro correspondente ao índice.

O segundo método limpa todos os frames da animação e interrompe qualquer reprodução em andamento.

O terceiro é um método usado para criar uma animação de sprite simples.

Recebe como entrada uma textura, a posição inicial do sprite, a largura e altura do sprite, o deslocamento entre os sprites adjacentes, o número total de quadros na animação e a duração de cada quadro em segundos.

![img](https://i.imgur.com/5Ay3J06.png)

### System

#### InputController.cs

O InputController é responsável pelos controlos do jogador.

Possui três campos que representam o player e armezenam o estados do teclado na iteração anterior.

Tem dois metodos onde o primeiro é chamado a cada quadro e verifica se a tecla de salto foi pressionada, se sim e o player não estiver atualmente a saltar, chama o método para iniciar o salto. Se o player estiver a saltar e a tecla de salto não estiver a ser pressionada, chama o método que interromper o salto.

Se a tecla para abaixar for pressionada enquanto o player estiver a saltar ou a cair, chama o método para cair rapidamente, caso contrário, chama o método para agachar.

Caso o player esteja agachado e a tecla de abaixar não estiver a ser pressionada, chama o método para levantar o player.

O segundo método é usado para bloquear temporariamente as entradas.

![img](https://i.imgur.com/hVmKvrr.png)

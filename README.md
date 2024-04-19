# Jogo do T-Rex

Este jogo foi feito por [Yeti47](https://github.com/Yeti47).

Trabalho de Tecnologias em Desenvolvimento de VideoJogos.

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

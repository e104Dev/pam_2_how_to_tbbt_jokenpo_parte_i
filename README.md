# The Big Bang Theory JoKenPo

Este tutorial tem por finalidade explorar e condicionar nosso entendimento e habilidades para com a utilização de Widgets e criação de interfaces em Flutter.

> O Tuorial foi dividio em 2 partes as quais, na primeira iremos criar a interface com componentes reutilizáveis, uso tema customizado e também utilizaremos 2 widgets para tratarmos responsividade. Na segunda parte iremos utilizar P.O.O para aplicar as regras de negócio.

> Devido à importação do pacote FontAwesome, os ícones **não poderão ser importados quando você utilizar o DartPad**; então utilize o VSCode ou Android Studio. Caso tenha impossibilidade de utilizar as IDEs [VSCode e AndroidStudio], utilize os ícones nativos do Material Design, você poderá visualizá-los [aqui](https://fonts.google.com/icons) e escolher os ícones ao seu gosto.

## Parte I - Interface Gráfica

### Estrutura inicial do App

Nesta parte criaremos o Widget padrão que será o ponto de partida do nosso App, siga os passo abaixo:

1. Remova o arquivo de testes e todo o conteúdo do `main.dart`.
2. Crie a estrutura de diretórios como o exemplo abaixo:

```
lib
  '-->pages
       '--> home_page
              '--> model
              '--> widgets
```

4. Adicione a dependência do `FontAwesome`, no arquivo `pubspec.yaml`, abaixo de `cupertino_icons: ^1.0.2`, inclua o techo abaixo e salve o arquivo para realizar a importação:

```yml  
  font_awesome_flutter: ^10.1.0
```

5. Crie o arquivo `\lib\pages\home_page\widgets\home_page_widgets.dart`. Este arquivo irá conter os widgets que serão rutilizados, lembre-se se um trecho de código se repete em seu projeto, ele deveria estar em uma função, em nosso caso deveria estar em um Widget.

### Criação dos Widgets reutilizáveis do projeto

1. Adicione o código do Widget que irá exibir as jogas do usuário e do computador:

```dart
import 'package:flutter/material.dart';

class CardWidget extends StatelessWidget {
  final double tamanho;
  final IconData icone;
  final String? texto;

  const CardWidget(
      {Key? key, required this.tamanho, required this.icone, this.texto})
      : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Card(
          color: Colors.white10,
          margin: const EdgeInsets.only(bottom: 16),
          child: SizedBox(
            width: tamanho,
            height: tamanho,
            child: Icon(
              icone,
              size: tamanho * 0.75,
              color: Theme.of(context).primaryColor,
            ),
          ),
        ),
        Text(
          texto ?? '',
          style: const TextStyle(fontSize: 24, color: Colors.white30),
        ),
      ],
    );
  }
}
``` 

2. Crie o Widget que irá criar os 5 botões que o usuário irá escolher [ Pedra, Papel, Tesoura, Lagarto, Spock ].

```dart
class ButtonWidget extends StatelessWidget {
  final double tamanho;
  final IconData icone;
  final String texto;
  final VoidCallback? jogar;

  const ButtonWidget(
      {Key? key,
      required this.tamanho,
      required this.icone,
      required this.texto,
      this.jogar})
      : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: const EdgeInsets.all(8),
      child: TextButton(
        onPressed: jogar,
        child: SizedBox(
          width: 75,
          height: 75,
          child: Column(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            children: [
              Icon(icone, size: tamanho),
              Text(
                texto,
                style: const TextStyle(color: Colors.white30),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
``` 

### Criando a página principal Jokenpo

Este será o Widget que nosso App irá lançar no método `main`; vamos criá-lo agora.

1. Crie o arquivo `\lib\pages\home_page\home_page.dart`.
2. Adicione o trecho abaixo ao arquivo.

```dart
import 'package:flutter/material.dart';
import 'package:font_awesome_flutter/font_awesome_flutter.dart';
import 'widgets/home_page_widgets.dart';

class MyHomePage extends StatefulWidget {
  final String title;

  const MyHomePage({
    Key? key,
    required this.title,
  }) : super(key: key);

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  IconData iconUserPlay = FontAwesomeIcons.question,
      iconComputerPlay = FontAwesomeIcons.question,
      iconStatus = FontAwesomeIcons.question;

  String statusJogo = 'Sua joagada aparecerá aqui!';
  bool finalidado = false;

  @override
  void initState() {
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.black87,
      appBar: AppBar(
        title: Text(widget.title),
        actions: [
          IconButton(
            onPressed: () {
              reiniciar();
            },
            icon: const Icon(Icons.restart_alt),
          )
        ],
      ),
      body: Column(
        mainAxisSize: MainAxisSize.max,
        children: [
          Expanded(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.spaceAround,
              children: [
                const SizedBox(width: double.infinity),
                Icon(
                  iconStatus,
                  size: 48,
                  color: Colors.lightGreen,
                ),
                CardWidget(
                  tamanho: 75,
                  icone: iconComputerPlay,
                  texto: 'Computador',
                ),
                CardWidget(
                  tamanho: 150,
                  icone: iconUserPlay,
                  texto: statusJogo,
                ),
              ],
            ),
          ),
          Wrap(
            alignment: WrapAlignment.spaceAround,
            children: [
              ButtonWidget(
                texto: 'Pedra',
                tamanho: 36,
                icone: FontAwesomeIcons.handFist,
                jogar: () {
                },
              ),
              ButtonWidget(
                texto: 'Papel',
                tamanho: 36,
                icone: FontAwesomeIcons.solidHand,
                jogar: () {
                },
              ),
              ButtonWidget(
                texto: 'Tesoura',
                tamanho: 36,
                icone: FontAwesomeIcons.solidHandPeace,
                jogar: () {
                },
              ),
              ButtonWidget(
                texto: 'Lagarto',
                tamanho: 36,
                icone: FontAwesomeIcons.solidHandLizard,
                jogar: () {
                  jogar(Lagarto());
                },
              ),
              ButtonWidget(
                texto: 'Spock',
                tamanho: 36,
                icone: FontAwesomeIcons.solidHandSpock,
                jogar: () {
                },
              ),
            ],
          ),
        ],
      ),
    );
  }
}
``` 

3. Crie um stateless widget `BBTJokenpo` no `main.dart` e mude o tema principal para `Lime Green`.

```dart
void main() => runApp(const BBTJokenpo());

class BBTJokenpo extends StatelessWidget {
  const BBTJokenpo({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(primarySwatch: Colors.lightGreen),
      home: const MyHomePage(title: 'Big Bang Jokenpo'),
    );
  }
}
``` 

### Terminamos, teste o App e valide a interface gráfica

## Parte II - Regras de Negógio(Regras do Jogo)

To be continued...

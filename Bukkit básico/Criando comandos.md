# Criando comandos
  Nesse tutorial você vai aprender a criar comandos.  
  Versão do bukkit: Todas  
  Data: 12/12/2015  

#### Autores principais:
* Neitan96([WebSite](http://www.nathanalmeida.com.br/), Skype:DevNeitan)

# Resumo

Para fazer um comando temos que criar 2 coisas, a primeira é o comando que armazena as informações sobre o comando, a segunda é o executor do comando que é onde fica o código do comando, cada executor pode ser responsável por mais de um comando, ele pode estar na sua classe principal ou em outra classe.

## plugin.yml

Antes de criar o executor do comando precisamos registrar o comando, no seu **plugin.yml** dentro da tag *commands* coloque outra tag com o nome do seu comando e dentro vamos colocar  as seguintes informações:
- **description** - (Opcional) (String) Descrição do comando.
- **aliases** - (Opcional) (List) Aliases do comando, é uma lista de "apelidos" para seu comando.
- **permission** - (Opcional) (String) A permissão para usar o comando.
- **usage** - (Opcional) (String) Mensagem caso o comando falhe.
- **permission-message** - (Opcional) (String) Mensagem caso o player não tenha permissão.

Exemplo:
```Yaml
name: Nome do plugin
version: 1.0
description: Um plugin qualquer

author: Neitan96
website: http://www.nathanalmeida.com.br/

main: br.neitan96.nomedoplugin.NomeDoPlugin

commands:
  comando1:
    description: 'O meu primeiro comando.'
    aliases: ['cmd1', 'c1']
    permission: 'nomedoplugin.comando1'
    usage: 'Erro! Comando invalido.'
    permission-message: 'Você não tem permissão para usar esse comando'
```

Exemplo2:
```Yaml
commands:
  dar:
    description: 'Dar um item ao um player.'
    aliases: ['give', 'entregar']
    permission: 'nomedoplugin.dar'
    usage: 'Erro! Modo de usar: /<command> <Nome do item> [Propriedades].'
    permission-message: 'Você não tem permissão para usar esse comando.'
```

## Preparando o executor

Agora, para criar o executor do comando você tem duas opções de onde colocar ele, na sua classe principal ou numa classe separada, vou ensinar a fazer as duas.  

#### Classe separada

Caso você não queria criar o comando em uma classe separada pule essa etapa.  

Primeiro crie a sua classe com o nome que você preferir e em seguida implemente o **CommandExecutor**, assim:
```Java
public class Commando1 implements CommandExecutor{

}
```

#### onCommand

Agora, na classe que você escolheu colocar(Na principal ou numa separada) reescreva a função **onCommand** na sua classe, ela deve retorna um boolean indicando se o comando foi executado corretamente, caso retorne a true não acontecera mais nada, caso retorne a false ele exibira a mensagem configurada no **plugin.yml** no *usage*, e a função deve ter 4 parâmetros:
- **CommandSender** *sender* - Quem executor o comando.
- **Command** *cmd* - O comando que foi executado.
- **String** *commandLabel* - Nome chamado para executar o comando.
- **String[]** *args* - Uma lista como os argumentos do comando.

Assim:
```Java
public class Commando1 implements CommandExecutor{

    @Override
    public boolean onCommand(CommandSender sender, Command cmd, String commandLabel, String[] args){
        return false;
    }

}
```

## Verificando nome do comando

Depois de criar o **onCommand** vamos verificar qual o comando foi executado, isso serve para separar vários comandos no mesmo executor, para fazer isso vamos comparar o nome do comando, assim:
```Java
@Override
public boolean onCommand(CommandSender sender, Command cmd, String commandLabel, String[] args){

    if(cmd.getName().equalsIgnoreCase("comando1")){ // Verificando o nome do comando

        return true;
    }

    return false;
}
```

Um exemplo de vários comandos no mesmo executor:
```Java
@Override
public boolean onCommand(CommandSender sender, Command cmd, String commandLabel, String[] args){

    if(cmd.getName().equalsIgnoreCase("comando1")){

        //Codigo do comando 1
        return true;
    }else if(cmd.getName().equalsIgnoreCase("comando2")){

        //Codigo do comando 2
        return true;
    }else if(cmd.getName().equalsIgnoreCase("comando3")){

        //Codigo do comando 3
        return true;
    }else if(cmd.getName().equalsIgnoreCase("comando4")){

        //Codigo do comando 4
        return true;
    }else if(cmd.getName().equalsIgnoreCase("comando5")){

        //Codigo do comando 5
        return true;
    }

    return false;
}
```

*Caso o executor receba somente um comando não é necessário fazer essa verificação.*

## Somente para players

Em alguns casos queremos que somente players possam executar o comando, para isso verificamos se o sender é uma instancia de **Player** com **instanceof**, assim:

```Java
@Override
public boolean onCommand(CommandSender sender, Command cmd, String commandLabel, String[] args){

    if(cmd.getName().equalsIgnoreCase("comando1")){
        // Verificando se o sender é um player.
        if(sender instanceof Player){

            // Fazendo o cast do sender para transforma em Player.
            Player player = (Player) sender;

            // Codigo do comando

        }else{
            sender.sendMessage("Somente players podem executar esse comando!");
        }
        return true;
    }

    return false;
}
```

## Argumentos

Agora vamos aprender a usar os argumentos, argumentos são todas as palavras digitadas depois no nome do comando,  exemplo:
```
/dar Neitan96 DIAMOND 64
```

O nome do comando seria **dar**, o nome **Neitan96** seria o argumento **0**, o nome **DIAMOND** seria o argumento **1** e o nome **64** seria o argumento **2** e assim vai.  
*OBS: No Java a numeração de listas começa do 0.*  

Quando um comando é executado todos os argumentos é passados em forma de lista(Array) pelo parâmetro **String[] args**, para acessar um parâmetro basta escrever o nome da lista e depois o numero do argumento(chamando de index) entre colchetes, assim:
```Java
args[1];
```

No exemplo abaixo vamos usar o comando do exemplo acima:
```Java
@Override
public boolean onCommand(CommandSender sender, Command cmd, String commandLabel, String[] args){

    if(cmd.getName().equalsIgnoreCase("comando1")){
        if(sender instanceof Player){

            Player player = (Player) sender;

            String playerName = args[0]; // Neitan96
            String materialName = args[1]; // DIAMOND
            String qnt = args[2]; // 64

        }else{
            sender.sendMessage("Somente players podem executar esse comando!");
        }
        return true;
    }

    return false;
}
```

#### Numero de argumentos

Quando criamos um comando que é obrigatório passar determinado numero de argumentos e o player não passa esse determinado numero de argumentos, então tentamos obter esse argumento, acontece um error, mas para prevenir isso verificamos o numero de argumentos passado com o a função **length**, essa função retorna ao numero de elementos na lista, exemplo:
```Java
if(cmd.getName().equalsIgnoreCase("comando1")){
    if(sender instanceof Player){

        Player player = (Player) sender;

        // Verificando se o numero de argumentos passados é igual a 3
        if(args.length == 3){

            String playerName = args[0];
            String materialName = args[1];
            String qnt = args[2];

        }else{
            // Retornando a false caso ele não passou a quantidade de parâmetros determinada.
            return false;
        }

    }else{
        sender.sendMessage("Somente players podem executar esse comando!");
    }
    return true;
}
```

*OBS: Mesmo o Java numerando os elementos começando por 0 ele retorna a quantidade real de elementos.*

## Exemplo

Agora você ja tem todas ferramentas para criar um comando, mas para um melhor compreendimento vou colocar um exemplo de um comando que cura o player.  

No **plugin.yml**:
```Yaml
commands:
  curar:
    description: 'Cura o player.''
    aliases: ['life']
    permission: 'nomedoplugin.comando.curar'
    usage: 'Use: /<command> [Player].'
    permission-message: 'Você não tem permissão para usar esse comando'
```

O **onCommand**:

```Java
@Override
public boolean onCommand(CommandSender sender, Command cmd, String label, String[] args){

    // Verificando se é o comando de curar.
    if(cmd.getName().equalsIgnoreCase("curar")){

        Player target = null;

        // Verificando se o player passou algum argumento.
        if(args.length > 0){
            // Pagando o player com o nome do argumento passado.
            target = Bukkit.getPlayer(args[0]);

            // Verificando se o player existe.
            if(target == null){
                sender.sendMessage("Player inválido");
                return true;
            }

        }else{

            // Caso não passou um argumento ele verifica se o sender é um player.
            if(sender instanceof Player){
                target = (Player) sender;
            }else{
                sender.sendMessage("Por favor digite o nome do player.");
                return true;
            }

        }

        // Curando o target
        if(target.getHealth() < 20)
            target.setHealth(20);

    }

    return false;
}
```

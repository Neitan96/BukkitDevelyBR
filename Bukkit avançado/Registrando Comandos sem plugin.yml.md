# Registrando Comandos sem plugin.yml
  Aprenderemos rapidamente um método simples e eficiente (além de seguro) para registrar comandos no Bukkit sem necessidade de plugin.yml.  
  Versão do bukkit: Todas versões acima de 1.4.X (Não testado na 1.7.4+)  
  Data: 16/12/2015  

## Autores principais:
* João Pedro (ReedFlake ou Atom) - Skype: jpedro2014

## O que utilizaremos
* Reflection (Fields)
* CommandMap

## Inicialmente
Inicialmente declare uma variável global (fora de voids e afins) estática (static) e privada (private) ou de sua preferência conforme for acessar, como demonstrado abaixo:
```java
private static CommandMap comandos = null;
```

## Agora no void onEnable, coloque
```java
try
{
  if (getServer() instanceof CraftServer)
  { //apenas por segurança, verificar validade do getServer()
    final Field field = CraftServer.class.getDeclaredField("commandMap"); // estamos invadindo a classe do Bukkit, e pegando o commandMap, onde é salvo os comandos registrados por plugin.yml.
    f.setAccessible(true); //estamos nos auto-permitindo obter e editar essa variável da classe deles.
    comandos = (CommandMap) field.get(Bukkit.getServer()); //estamos obtendo a variável para usarmos no futuro; note que agora nossa variável 'comandos' não será mais nula.
  }
}
catch (Exception e)
{
  e.printStackTrace(); //se ocorrer algum erro (de field, principalmente) tratar aqui
}
```

## Registrando o comando
#### Primeiro passo
Crie uma nova classe no mesmo pacote, chamada "Comando" com o conteúdo:
```java
public final class Comando extends Command
{

  private CommandExecutor exe = null;

  protected Comando(String nomeDoComandoSemBarras)
  {
    super(nomeDoComandoSemBarras);
  }

  // Isso será usado pelo Bukkit apenas.
  @Override
  public boolean execute(CommandSender sender, String commandLabel,String[] args)
  {
     if(exe != null)
     {
       return exe.onCommand(sender, this, commandLabel,args);
     }
     sender.sendMessage("Registre o local onde o comando deverá ser executado usando comando.definir(commandexecutor)");
     return false;
  }

  // Usaremos esse void mais tarde para definir onde está o onCommand() que será usado para tratar o novo comando.
  public void definir(CommandExecutor exe){
    this.exe = exe;
  }
}
```

#### Segundo passo
Agora que temos a classe responsável por 'guardar' o comando, volte a classe Main (que estende JavaPlugin) e registre o comando desta forma como o exemplo abaixo.
```java
public static void registrarComando(String comandoSemBarra, CommandExecutor classe)
{
  Comando cmd = new Comando(comandoSemBarra); // Se o comando for '/ajuda' coloque 'ajuda'
  comandos.put("", cmd); // deixe a primeira field vazia, é prefixo; não use se não souber o que faz.
  cmd.definir(classe); //define onde é pra executar o comando (uma classe que implemente CommandExecutor) e tenha o onCommand().
  // Pronto!
}
```

## Exemplo
```java
public final class Main extends JavaPlugin implements CommandExecutor
{

  private static CommandMap comandos = null;

  @Override
  public void onEnable()
  {
    try
    {
      if (getServer() instanceof CraftServer)
      { //apenas por segurança, verificar validade do getServer()
        final Field field = CraftServer.class.getDeclaredField("commandMap"); // estamos invadindo a classe do Bukkit, e pegando o commandMap, onde é salvo os comandos registrados por plugin.yml.
        f.setAccessible(true); //estamos nos auto-permitindo obter e editar essa variável da classe deles.
        comandos = (CommandMap) field.get(Bukkit.getServer()); //estamos obtendo a variável para usarmos no futuro; note que agora nossa variável 'comandos' não será mais nula.
      }
    }
    catch (Exception e)
    {
      e.printStackTrace(); //se ocorrer algum erro (de field, principalmente) tratar aqui
    }

    registrarComando("ajuda", this); // this = classe Main = implementa CommandExecutor e tem onCommand().
  }

  @Override
  public boolean onCommand(CommandSender command, Command cmd, String label, String[] args)
  {
    sender.sendMessage("Comando /ajuda funciona! =D");
  }

  public static void registrarComando(String comandoSemBarra, CommandExecutor classe)
  {
    Comando cmd = new Comando(comandoSemBarra); // ou seja /ajuda
    comandos.put("", cmd); // deixe a primeira field vazia, é prefixo; não use se não souber o que faz.
    cmd.definir(classe); //define onde é pra executar o comando (uma classe que implemente CommandExecutor) e tenha o onCommand().
    // Pronto!
  }

}
```

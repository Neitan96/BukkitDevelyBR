# Gerenciando configs 1
  Nesse tutorial você vai aprender a manipular a config.yml  
  Versão do bukkit: Todas  
  Data: 14/12/2015  

#### Autores principais:
* Neitan96([WebSite](http://www.nathanalmeida.com.br/), Skype:DevNeitan)

## Paths

Paths são o endereço de uma variável na config, exemplo:  
Na config:
```Yaml
name: Neitan96
```
A path de **Neitan96** é **name**, na config:
```Yaml
Players:
  Neitan96:
    Level: 10
```

A path de **10** é **Players.Neitan96.Level**, ou seja, quando temos uma path dentro da outra colocamos um ponto entre as paths.

## Listas (Apenas uma curiosidade)  

Listas nas config podem ser representadas de duas formas, a primeira forma é a mais conhecida:
```Ỳaml
Lista:
- 'Item 1'
- 'Item 2'
- 'Item 3'
- 'Item 4'
- 'Item 5'
```

A segunda forma é:
```Ỳaml
Lista: ['Item 1', 'Item 2', 'Item 3', 'Item 4', 'Item 5']
```

Você pode fazer das duas formas, que a config vai ser interpretada do mesmo jeito.

## Salvando config padrão

Como você deve ter visto, plugins tem configurações padrões, para fazer primeiro você deve criar sua config no seu projeto da mesma forma que o **plugin.yml**, mas com o nome **config.yml**.

Depois disso, use a função **saveDefaultConfig()** no seu onEnable, exemplo:
```Java
@Override
public void onEnable(){
    saveDefaultConfig();
}
```

A função **saveDefaultConfig()** irá verificar se o arquivo **config.yml** existe, caso não exista ele irá pegar a config da sua jar e salva-la no diretorio do plugin.

Depois disso temos que carregar o arquivo salvo com as configurações, para fazer usamos a função **reloadConfig()**, exemplo:
```Java
@Override
public void onEnable(){
    saveDefaultConfig();
    reloadConfig();
}
```

Pronto, até agora o seu plugin já salva no diretorio do plugin a sua config padrão.

## Obtendo valores

Para ter acesso a sua config basta usar a função **getConfig()**.

Para pegar uma String use a função **getString** da sua config, exemplo:
```Java
String msg = getConfig().getString("PATH");
```

Em **PATH** você deve colocar a path da variável na config como explicado anteriormente.  
Para pegar um inteiro use a função **getInt** da sua config, exemplo:
```Java
int inteiro = getConfig().getInt("PATH");
```

E assim vai, existe muitas função que pegar vários tipos de valores, fiz um lista:
* getBoolean(String path)
* getBooleanList(String path)
* getDouble(String path)
* getDoubleList(String path)
* getFloatList(String path)
* getInt(String path)
* getIntegerList(String path)
* getItemStack(String path)
* getList(String path)
* getLong(String path)
* getLongList(String path)
* getOfflinePlayer(String path)
* getString(String path)
* getStringList(String path)

Você também pode consultar a [documentação oficial](https://hub.spigotmc.org/javadocs/bukkit/).

#### Exemplo

Config:
```Yaml
Players:
  Neitan96:
    Status: 'Eae povão!'
    Xp: 10.75
    Level: 10
    Staff: true
  Jonathan:
    Status: 'Nao ha atualizacoes de status para mostrar.'
    Xp: 3.5
    Level: 15
    Staff: false
```

Java:
```Java
//Vai retornar a "Eae povão!"
String statusNeitan = getConfig().getString("Players.Neitan96.Status");
//Vai retornar a 10.75
double xpNeitan = getConfig().getDouble("Players.Neitan96.Xp");
//Vai retornar a 10
int levelNeitan = getConfig().getInt("Players.Neitan96.Level");
//Vai retornar a true
boolean staffNeitan = getConfig().getBoolean("Players.Neitan96.Staff");

//Vai retornar a "Nao ha atualizacoes de status para mostrar."
String statusJonathan = getConfig().getString("Players.Jonathan.Status");
//Vai retornar a 3.5
double xpJonathan = getConfig().getDouble("Players.Jonathan.Xp");
//Vai retornar a 15
int levelJonathan = getConfig().getInt("Players.Jonathan.Level");
//Vai retornar a false
boolean staffJonathan = getConfig().getBoolean("Players.Jonathan.Staff");
```

## Definindo valores

Para colocar algum valor na config basta usar a função **set**, assim:
```Java
getConfig().set("PATH", VALOR);
```

Exemplo:
```Java
getConfig().set("Players.Jonathan.Status", "O novo status de Jonathan");
```

Você pode colocar quase qualquer tipo de variável.

## Salvando a config

Depois de modificar os valores você precisa salvar, você pode salvar assim que você modificar a config, no onDisable ou com uma scheduler.

Para savar a config basta usar a função **saveConfig()**.

Para salvar a config quando o plugin desligar basta coloca-la no onDisable do seu plugin, assim:
```Java
@Override
public void onDisable(){
    saveConfig();
}
```

Eu recomendo também fazer uma scheduler para salvar a config a cada x minutos, mas como aqui é um tutorial de configs e não de scheduler isso fica por você, claro que você pode consultar o tutorial de scheduler, fica a dica.

Pronto, você já aprendeu a criar a config padrão, manipular valores e salvar a config. Até o próximo tutorial sobre configs.

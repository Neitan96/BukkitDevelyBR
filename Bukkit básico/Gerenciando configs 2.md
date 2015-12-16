# Gerenciando configs 2
  Nesse tutorial vamos aprender coisas uteis sobre configs.  
  Versão do bukkit: Todas  
  Data: 15/12/2015  

#### Autores principais:
* Neitan96([WebSite](http://www.nathanalmeida.com.br/), Skype:DevNeitan)

## Valores padrões

Quando você for obter um valor da config você pode determinar um valor caso a path que você colocou não exista, basta usar o segundo parâmetro das funções **get** da config, exemplo:
Config:  
```Yaml
Players:
  Neitan96:
    Status: 'Eae povão!'
    Level: 10
    Xp: 10.75
    Staff: true
```

Java:
```Java
//Vai retornar a "Eae povão!"
String statusNeitan = getConfig().getString("Players.Neitan96.Status", "Sem status.");
//Vai retornar a 10.75
double xpNeitan = getConfig().getDouble("Players.Neitan96.Xp", 0);
//Vai retornar a 10
int levelNeitan = getConfig().getInt("Players.Neitan96.Level", 1);
//Vai retornar a true
boolean staffNeitan = getConfig().getBoolean("Players.Neitan96.Staff", false);

//Vai retornar a "Sem status."
String statusJonathan = getConfig().getString("Players.Jonathan.Status", "Sem status.");
//Vai retornar a 0
double xpJonathan = getConfig().getDouble("Players.Jonathan.Xp", 0);
//Vai retornar a 1
int levelJonathan = getConfig().getInt("Players.Jonathan.Level", 1);
//Vai retornar a false
boolean staffJonathan = getConfig().getBoolean("Players.Jonathan.Staff", false);
```

Recomendo usar sempre, caso o dono do servidor apagou alguma parte da config e você não usar os valores padrões o irá ocorrer erros.

## Valores padrões 2

Você também pode definir valores padrões a partir de outra config com a função **setDefaults**, assim:
```Java
getConfig().setDefaults(CONFIG_PADRÃO);
```

No bukkit existe um função chamada **getResource** com ela você obtem o **InputStream** se um arquivo de dentro da sua *.jar*, e a classe **YamlConfiguration** tem uma função que carrega uma config a parti de um **InputStream**, ou seja se usamos:
```Java
YamlConfiguration configDefault = YamlConfiguration.loadConfiguration(getResource("config.yml"));
getConfig().setDefaults(configDefault);
```

Vamos obter os valores padrões da config que está dentro da sua *.jar*.


## Ancoras & Referencias

Em arquivos Yaml temos o que chamamos de Ancoras(&) e Referencias(\*), eles servem para repetir configs.

Você pode criar ancoras colocando na path que você quer repetir o sinal **&** e depois um nome para a ancora, e você pode criar um referencia para um ancora colocando o sinal **\*** e depois o nome da ancora.
A ancora é a path que será repetida, e as referencias será a path que vão copiar os valores da ancora, exemplo:
```Yaml
Players: &NomeQualquer
  Neitan96:
    Level: 10
    Xp: 1000
  Jonathan: *NomeQualquer
  Luigi: *NomeQualquer
```

Nessa config os player **Jonathan** e **Luigi** terão o mesmo **Level** e **Xp** que Neitan96, Sendo **Neitan96** a ancora e **Jonathan** e **Luigi** referencias da ancora.

## Obtendo chaves

Para obter todas as paths da config podemos usar a função **getKeys**, exemplo:  
Config:
```Yaml
UUID: true
XpUp: 10000
Players:
  Neitan96:
    Xp: 10.75
    Level: 10
  Jonathan:
    Xp: 3.5
    Level: 15
```

Java:
```Java
Set<String> keys = getConfig().getKeys(true);
//Vai retornar a uma lista com:
// UUID
// XpUp
// Players
// Players.Neitan96
// Players.Neitan96.Xp
// Players.Neitan96.Level
// Players.Jonathan
// Players.Jonathan.Xp
// Players.Jonathan.Level
```

Caso você não queira as sub-paths é so colocar false como parâmetro, assim:
```Java
Set<String> keys = getConfig().getKeys(false);
//Vai retornar a uma lista com:
// UUID
// XpUp
// Players
```

Caso você queira pegar a keys de um path, como pegar todos os players da config, primeiro você tem que pegar o **ConfigurationSection** da path que você quer pegar as keys, para isso usamos a função **getConfigurationSection** e depois pegar as keys, exemplo:
```Java
Set<String> keys = getConfig().getConfigurationSection("Players").getKeys(false);
//Vai retornar a uma lista com:
// Neitan96
// Jonathan
```

## ItemStack

Você tambem pode armazenar ItemStack nas configs, para colocar na config use o **set** normalmente, assim:
```Java
getConfig().set("Item", new ItemStack(Material.DIAMOND, 64));
```

E para pegar o ItemStack de novo use **getItemStack**, assim:
```Java
ItemStack item = getConfig().getItemStack("Item");
```

# Criando a base do plugin
  Nesse tutorial vamos criar a base do seu plugin.  
  Versão do bukkit: Todas  
  Data: 13/12/2015  

## plugin.yml

Primeiro temos que criar o plugin.yml, no seu projeto dentro de nenhum package crie um arquivo chamado *plugin.yml*, nele irá conter todas as informações do seu plugin, para ver a documentação oficial clique [aqui](http://wiki.bukkit.org/Plugin_YAML).

primeiro vamos colocar o nome do plugin, para isso usaremos a path *name*, assim:
```Yaml
name: Nome do plugin
```
Definimos o nome do plugin, agora vamos definir a versão, descrição, autor e WebSite, assim:

```Yaml
name: Nome do plugin
version: 1.0
description: Um plugin qualquer

author: Neitan96
website: http://www.nathanalmeida.com.br/
```
Agora o *plugin.yml* está quase pronto.

## Classe principal

Para criar a classe principal temos que criar um package para seu plugin, um package é tipo um enderenço de um rua dentro do Java, e nessa rua temos as casas que são as classes.  

Para cada plugin temos que criar um package único, geralmente os desenvolvedores usam o nome do seu site de tras pra frente e depois o nome do plugin, exemplo:  
**br.com.nathanalmeida.nomedoplugin**  
Caso não tiver um site, geralmente é usado a sigla do seu país depois seu nick e depois o nome do plugin, exemplo:  
**br.neitan96.nomedoplugin**  

Depois de criar o package do seu plugin, temos que criar a classe principal, geralmente os desenvolvedores cria uma classe chamada *Main*, mas particularmente gosto de criar uma classe com o nome do plugin, exemplo: *NomeDoPlugin*, lembre-se você deve criar suas classes dentro da package do seu plugin.

## JavaPlugin

Depois de criar sua classe principal, ela deve tar mais ou menos assim:

```Java
package br.neitan96.nomedoplugin;

public class NomeDoPlugin{
}
```

Para definir que essa classe é um plugin temos que estende-la a *JavaPlugin* assim:

```Java
package br.neitan96.nomedoplugin;

import org.bukkit.plugin.java.JavaPlugin;

public class NomeDoPlugin extends JavaPlugin{
}
```

Pronto, sua classe principal está pronta.

## onEnable e onDisable

Agora vamos definir o que o plugin irá fazer quando o plugin ligar e quando desligar, para isso temos os dois métodos *onEnable* e *onDisable*, primeiro vamos criar o *onEnable*, assim:

```Java
package br.neitan96.nomedoplugin;

import org.bukkit.plugin.java.JavaPlugin;

public class NomeDoPlugin extends JavaPlugin{
    @Override
    public void onEnable(){
      //Código ao ligar
    }
}
```
Vamos colocar uma mensagem de sucesso quando ligar, assim:
```Java
@Override
public void onEnable(){
  //Código ao ligar
  getLogger().log(Level.INFO, "Plugin iniciado com sucesso!");
}
```

Agora vamos criar o *onDisable*, assim:
```Java
package br.neitan96.nomedoplugin;

import org.bukkit.plugin.java.JavaPlugin;
import java.util.logging.Level;

public class NomeDoPlugin extends JavaPlugin{
    @Override
    public void onEnable(){
        //Código ao ligar
        getLogger().log(Level.INFO, "Plugin iniciado com sucesso!");
    }

    @Override
    public void onDisable(){
        //Código ao desligar
        getLogger().log(Level.INFO, "Plugin desligado com sucesso!");
    }
}
```

## Classe principal e plugin.yml

Depois de criar sua classe principal, temos que informar ao Bukkit onde ela está, para isso que ter a referencia da classe que é o nome do package mais ponto mais o nome da classe, exemplo:  
**br.neitan96.nomedoplugin.NomeDoPlugin**  
Agora temos que colocar no *plugin.yml* na path *main*, assim:  

```Yaml
name: Nome do plugin
version: 1.0
description: Um plugin qualquer

author: Neitan96
website: http://www.nathanalmeida.com.br/

main: br.neitan96.nomedoplugin.NomeDoPlugin
```

Pronto, agora é so compilar e ligar o plugin.

-------------------------------------------
#### Autores principais:
  * Neitan96([WebSite](http://www.nathanalmeida.com.br/), Skype:DevNeitan)

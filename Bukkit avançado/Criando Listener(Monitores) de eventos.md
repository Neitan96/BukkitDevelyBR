# Criando e gerenciando eventos
Neste tutorial você aprenderá a criar, registrar e gerenciar Eventos no Bukkit.

>  Versão do bukkit: Todas

>  Data: 13/12/2015

## Criando uma classe para seu evento e registrando

### Listeners.
Um listener é uma classe que monitora os eventos que ocorrem no Bukkit, como a ação de quebrar blocos, ou simplesmente eventos de outros plugins.

#### Criando a classe e registrando
Crie uma nova classe e coloque o nome que deseja, e implemente a classe Listener do pacote `org.bukkit.event.Listener`:
```java
import org.bukkit.event.Listener;

public class MeuListener implements Listener {

}
```
Ao implementar a classe Listener você informa ao Bukkit que sua classe monitora eventos.

Mas não acaba ai, você ainda precisa registrar-la, em sua classe principal, no método `onEnable`:
```java
// MeuListener = Classe que você criou
// this = Plugin responsável por este Listener
this.getServer().getPluginManager().registerEvents(new MeuListener(), this);
```
E também é altamente recomendando remover nossos Listeners da lista quando o plugin desligar, no método `onDisable`:
```java
/** this = Plugin responsavel pelo listener,
      informando o plugin todos os listener registrados em seu nome pelo método registerEvents serão removidos!**/
HandlerList.unregisterAll(this);
```

Vamos voltar a nossa classe `Listener` agora, no meu caso `MeuListener`

#### EventHandler

EventHandler é uma anotação que indica que seu método irá tratar um evento, nela também é possivel especificar a prioridade do seu método, e se ele **não** irá ser chamado caso o evento tenha sido cancelado.

##### Opções da anotação:
* priority
 * Padrão: `EventPriority.NORMAL` (Prioridade NORMAL)
 * É um método do tipo `EventPriority`, aqui você especifica a prioridade de seu método.
* ignoreCancelled
 * Padrão: `false` (não)
 * É uma `boolean` que informa se o método será ignorado se o evento tive sido cancelado. Para ignorar caso o evento tenha sido cancelado defina como `true`.

##### Monitorando e gerenciando o evento
###### Monitorando
A classe que você deve importar é a `org.bukkit.event.EventHandler`, para monitorar é necessário adicionar a anotação acima de seu método e o nome do evento como um argumento do método, exemplo:
```java
@EventHandler
public void aoColocarUmBloco(BlockPlaceEvent event){

}
```
Neste caso, você também precisará importar a classe `org.bukkit.event.block.BlockPlaceEvent`
###### Gerenciando
Antes de gerenciarmos devemos saber que alguns eventos podém ter métodos diferentes, por exemplo, o `BlockBreakEvent` uma classe filha de `BlockEvent` e é implementação de `Cancellable`.
Todas classes que implementam `Cancellable` podem ser canceladas e tem o método `setCancelled` e `isCancelled`.
Todas classes filhas de `BlockEvent` tem o método `getBlock`.

Vamos fazer com que sempre que o jogador coloque uma esponja ou uma birgorna no mundo ela seja substituida por uma pedra. Veja como irá ficar:
```java
Material blockType = event.getBlock().getType();
// Aqui verificamos se o tipo é Material.SPONGE ou Material.ANVIL
if(blockType == Material.SPONGE
    || blockType == Material.ANVIL){
  //Aqui definimos como uma pedra (Material.STONE)
  event.getBlock().setType(Material.STONE);
}
```
Você também pode usar `switch` ao invés de `if`
```java
Material blockType = event.getBlock().getType();
switch(blockType){
  case SPONGE:
  case ANVIL:
    event.getBlock().setType(Material.STONE);
    break;
}
```
Você precisará importar a classe `org.bukkit.Material`, muita atenção, caso você não utilize a API recomendada para programação poderá haver outras classes como `net.minecraft.server.v1_8_R3.Material`, certifique-se de sempre usar classes que começam com `org.bukkit`, e **não** usar as classes do pacote `org.bukkit.craftbukkit`.

Também é possivel ter mais de um evento na mesma classe, vamos criar um outro:

```java
// Definimos como a maior prioridade e ignoramos se já tiver sido cancelado
@EventHandler(priority = EventPriority.HIGHEST, ignoreCancelled = true)
public void quebrarUmBloco(BlockBreakEvent event){
    Material blockType = event.getBlock().getType();
    // Verificamos aqui se o bloco quebrado é o de diamante, e cancelamos o evento caso seja.
    if(blockType == Material.DIAMOND_BLOCK){
        event.setCancelled(true);
    }
}
```

## Gerenciando as classes de eventos.

Além de podermos registrar os eventos, e remover-los da lista de registro, podemos também, por meio da `HandlerList` (classe do pacote `org.bukkit.event`) gerenciar nosso Listeners.

###### Removendo um Listener

```java
HandlerList.unregisterAll(listener);
```

###### Removendo TODOS os Listeners
```java
HandlerList.unregisterAll();
```

###### Listando os Listeners do nosso plugin e vendo suas informaçoes
```java
for(RegisteredListener registeredListener : HandlerList.getRegisteredListeners(this)){
    EventPriority priority = registeredListener.getPriority();
    boolean isIgnoringCancelled = registeredListener.isIgnoringCancelled();
    ...
}
```
###### Listando os Handlers
```java
for(HandlerList handlerList : HandlerList.getHandlerLists()){
    ...
}
```
***Observação***: Os plugins registram seus `Listener` dentro de instancias de `HandlerList` que são registradas dentro de uma váriavel estática da própria classe (`HandlerList`), logo os `Listeners` estão em diferentes instancias!

**Observação²**: Você pode registrar quantos `Listerners` desejar.

-------------------------------------------
#### Autores principais:
  * Jonathan (Skype: jonathan_scripter, [Blog](http://www.souumbyte.tk/))

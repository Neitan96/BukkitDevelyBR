## Dando Itens Para Um Jogador
**Descrição:** Irei ensinar nesse tutorial a dar itens para um jogador de forma bem simples e resumida, você poderá mudar o nome do item e dar
a ele uma descrição.

**Versão Do Bukkit:** 1.5.2 <br/>
**Data:** 13/12/2015

## Criando O Item
Primeiramente temos que criar o item, para isso iremos criar um **ItemStack** que serve para instanciar um novo item além de ser possivel
criar mais de um item de uma vez, para isso iremos utilizar o seguinte código:

> ItemStack item = new ItemStack(Material.DIAMOND, 10);

Com isso estamos instanciando 10 diamantes e salvando na variavel *item*, agora iremos apreender a editar o nome e dar uma descrição a esse item.

## Editando O Nome Do Item E Criando Uma Descrição
Para editar essas coisas temos que pegar o *ItemMeta* do item pois é lá onde fica armazenadas essas informações, para fazer isso temos que ter o
seguinte código:

> ItemMeta meta = item.getItemMeta();

Com isso salvamos o ItemMeta do nosso ItemStack na váriavel **meta**, agora podemos setar o display name e sua lore fazendo o seguinte:

> meta.setDisplayName("§bNome Muito Maneiro");<br/>
> ArrayList<String> lista = new ArrayList<String>();<br/>
> lista.add("§5Esse item é muito bom!");<br/>
> lista.add("§5Oi tudo bem com voce?");<br/>
> meta.setLore(lista);<br/>

Bem, primeiramente setamos o display name do item, ou seja, mudamos o nome do item, seguindo crie um **ArrayList** de strings, os ArrayLists são listas
de coisas que podemos criar, eu criei uma lista de strings, ou seja, uma lista de textos, depois adicionei nessa lista duas mensagens, eu poderia colocar mais
ou menos, depois disso tudo setamos a lore com essa lista.

## Finalizando
Para finalizar teremos que setar o novo ItemMeta no nosso ItemStack para que ele receba as nossas alterações e dar esse item para o jogador, então vamos fazer isso:

> item.setItemMeta(meta);<br/>
> player.getInventory().addItem(item);<br/>
> player.updateInventory()<br/>

E assim está pronto, criamos um item customizado e entregamos ele para o nosso jogador, veja abaixo o código completo:

> ItemStack item = new ItemStack(Material.DIAMOND, 10);<br/>
> ItemMeta meta = item.getItemMeta();<br/>
> meta.setDisplayName("§bNome Muito Maneiro");<br/>
> ArrayList<String> lista = new ArrayList<String>();<br/>
> lista.add("§5Esse item é muito bom!");<br/>
> lista.add("§5Oi tudo bem com voce?");<br/>
> meta.setLore(lista);<br/>
> item.setItemMeta(meta);<br/>
> player.getInventory().addItem(item);<br/>
> player.updateInventory()<br/>

<hr/>
####Autores Principais
* Herobrinedobem

####Contribuidores

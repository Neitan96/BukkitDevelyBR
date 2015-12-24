# Checando items no inventário
  Iremos aprender a verificar se contém algum item no inventário do jogador.  
  Versão do bukkit: 1.5.2 - 1.8  
  Data: 23/12/2015  

#### Autores principais:
* Crazy(Skype:hugosilvaf2)

## Método
```Java
public boolean checkItemStacks(ItemStack[] ises){
  for(ItemStack is : ises){
    if(is != null && is.getType() != Material.AIR)
      return true;
  }
  return false;
}
```
## Utilizando método
```Java
ItemStack[] armors = player.getInventory().getArmorContents();
if(checkItemStacks(armors)){
  player.sendMessage("Você está utilizando armadura.");
}
```

## Checando inventário do jogador
```Java
ItemStack[] armors = player.getInventory().getArmorContents();
ItemStack[] contents = player.getInventory().getContents();
if(checkItemStacks(armors) || checkItemStacks(contents)){
  player.sendMessage("Remova todos items de seu invetário para entrar na arena!");
}
```

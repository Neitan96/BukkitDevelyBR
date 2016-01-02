# Transformando Array em String
  Aprenderemos a transformar um Array em String  
  Data: 29/12/2015  

#### Autores principais:
* Crazy, Skype: hugosilvaf2)

## Método
```Java
public String parseArray(String[] array){
	return parserArray(array, 0);
}

public String parseArray(String[] array, int i){
	StringBuilder  builder = new StringBuilder();
	String CHARACTER_SEPARATOR = " ";
	for(int length = i; length < array.length; length++){
		builder.append(builder.toString().isEmpty()? array[length] : CHARACTER_SEPARATOR + array[length]);
	}
	return builder.toString();
}
```
## Utilizando método
```Java
// Criamos um array
String[] array = new String[]{"/comando", "Oi", "broadcast", "message"};
// Imprimimos o array transformado
System.out.println(parseArray(array, 1));// Saida >> Oi broadcast message
System.out.println(parseArray(array, 0));// Saida >>/comando Oi broadcast message
System.out.println(parseArray(array));// Saida >> /comando Oi broadcast message
```

# Resumão classe Math em Java
  Ola pessoal tudo bem! Estou aqui para fazer um resumão dos principais método da classe Math do Java, ela é muito útil pois facilita bastante operações matemáticas, eu meio que fiz uma tradução e bolei os exemplos. Ela não está completa falta algumas sobre trigonometria no qual não achei muito necessário citar, mas quem quiser colocar a vontade!
  Versão do java: A partir do Java 1
  Data: 15/12/2015  

#### Autores principais:
* Luigi([Twitter](https://twitter.com/LuigiOliveira__),[Github](https://github.com/luigieai))

#### Contribuidores:
 Nenhum até agora :(

## abs
Retorna um valor absoluto do numero do parâmetro(módulo)
```Java
double e = Math.abs(5.4333);
double f = Math.abs(-5);
double z = Math.abs(-22F);

//retorna:
5.4333
5.0
22.0
```

## ceil  
Retorna o **Maior** número inteiro do valor
```Java
double e = Math.ceil(6.2);
double c = Math.ceil(5.6);
double e = Math.ceil(22.0);

//Retorna
7.0
6.0
22.0
```

## cos
Retorna o cosseno do angulo dado
```Java
double e = Math.cos(60);
double f = Math.cos(30);
//Retorna
-0.9524129804151563
0.15425144988758405
```

## floor
Retorna o **Menor** número inteiro do valor
```Java
double e = Math.floor(4.5);
double f = Math.floor(3.3);
//Retorna
4.0
3.0
```

## log
Retorna o logaritmo natural do valor
```Java
double e = Math.log(36);
//Retorna
3.58351893845611
//Recomendo pesquisar sobre logaritimo natural no google
```

## max
 Retorna o maior numero entre os fornecidos no parâmetro
```Java
int e = Math.max(36,24);
double f = Math.max(103,103.3);
//Retorna
36
103.3
```

## min
Retorna o menor numero entre os fornecidos no parâmetro
```Java
double e = Math.min(36.2,24.1);
int f = Math.min(3,103);
//Retorna
24.1
3
```

## pow
Faz uma potencialização entre os números fornecidos, sendo no formato A^B
```Java
double e = Math.pow(2,2);
double f = Math.pow(3,10);
//Retorna
4.0
59049.0
```
## random
Gera um número aleatório entre 0 e 1 (1 nunca será gerado)
```Java
double z = Math.random();
double z = Math.random();
double z = Math.random();
//Retorna
0.8787478169176183
0.33156182603908335
0.799473087674712
//Ou seja, aleatórios kk
```

## round
Retorna o **long** mais próximo do valor
```Java
long e = Math.round(2.3);
long f = Math.round(10.22);
//Retorna
2L
10L
```

## sin
Retorna o seno do valor
```Java
double e = Math.sin(30);
double f = Math.sin(60);
//Retorna
-0.9880316240928618
-0.3048106211022167
```

## tan
Retorna a tangente do valor
```Java
double e = Math.tan(30);
double f = Math.tan(60);
//Retorna
-6.405331196646276
0.320040389379563
```

## sqrt
Retorna a raiz quadrada do valor
```Java
double e = Math.sqrt(25);
double f = Math.sqrt(4);
//Retorna
5
2
```

##exp
Retorna o valor da constante de Euller "e" elevada ao valor
```Java
double e = Math.exp(2);
//Retorna
7.38905609893065
```

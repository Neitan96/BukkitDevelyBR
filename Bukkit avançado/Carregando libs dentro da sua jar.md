# Carregando libs dentro da sua jar
  Nesse tutorial você aprender a carregar libs(outras .jar's) para usar no seu plugin.  
  Versão do bukkit: Todas  
  Data: 26/01/2016  

#### Autores principais:
* Neitan96([WebSite](http://www.nathanalmeida.com.br/), Skype:DevNeitan)

## Verificando lib carregada

Antes de você carregar uma lib você tem que verificar se a lib ja foi carregada, isso pode ocorrer de outro plugin também esta usando a mesma lib, ou no caso do reload do plugin.  
Para verificar tentaremos obter uma classe qualquer da lib, para isso temos que usar a função **forName** da classe **Class** e vamos passa como parametro a referencia da classe a lib, exemplo:
```Java
try{
    Class.forName("br.neitan96.clockschedulerapi.ClockSchedulerAPI");
    // A lib foi carregada
}catch (ClassNotFoundException e){
    // A lib não foi carregada
}
```

Vamos criar um função para facilitar as coisas:
```Java
public boolean libLoaded(String classRef){
    try{
        Class.forName(classRef);
        return true;
    }catch (ClassNotFoundException e){
        return false;
    }
}
```

## Copiando a lib

Infelizmente o Java não carrega lib dentro de jar's então teremos que copiar a lib para um arquivo temporário.  

#### Verificando se a lib está na jar

Antes de começar o processo temos que verificar se a lib está na jar, para isso vamos usar a função **getResource** da classe **JavaPlugin**(Sua classe principal) com essa função podemos obter qualquer arquivo de dentro da jar do nosso plugin, para verificar faremos assim:
```Java
// Obtendo a lib dentro da jar do plugin.
InputStream libInStream = getResource("ClockSchedulerAPI.jar");

// Verificando se a lib existe.
if(libInStream == null){
    // Se não existe ele manda um log de erro
    Bukkit.getLogger().log(Level.WARNING, "Erro o ClockSchedulerAPI não está dentro da jar!");
    return false;
}
```

#### Criando um arquivo temporario

Para copiar a lib para um arquivo temporário temos que cria-lo primeiro, para criar usaremos a função **createTempFile** da classe **File**, passando como parâmetros duas String's, sendo a primeira o prefix do arquivo e a segunda o sufix do arquivo, você pode colocar o prefix e sufix que achar melhor, exemplo:
```Java
File libOutFile = File.createTempFile("Bukkit", "ClockSchedulerAPI");
```

#### Copiando

Agora vamos copiar a lib para o arquivo temporário, não vou explicar esse processo mas você pode dar um rápida pesquisada no Google que vai achar muito conteúdo sobre isso, assim:
```Java
// Abrindo o arquivo temporário para escrita
FileOutputStream libOutStream = new FileOutputStream(libOutFile);

// Copiando a lib para o arquivo temporário
byte[] buffer = new byte[1024];
int length;

while ((length = libInStream.read(buffer)) != -1)
    libOutStream.write(buffer, 0, length);

libInStream.close();
libOutStream.close();
```

## Carregando a lib

Para carregar a lib vamos "hackear" o **classLoader** do nosso plugin.  
*O ClassLoader é uma classe responsável por gerenciar todas a libs carregadas*.  
Mãos na massa:
```Java
// Obtendo a URL do arquivo temporário
URL libURL = libOutFile.toURI().toURL();

// Obtendo o ClassLoader do nosso plugin
URLClassLoader classLoader = (URLClassLoader) getClass().getClassLoader();

// Obtendo a função do ClassLoader que faz o carregamento das libs
Method addURL = URLClassLoader.class.getDeclaredMethod("addURL", URL.class);
// "Hackeado" a função para torna-la acessível
addURL.setAccessible(true);
// Usando a função e carregando a lib
addURL.invoke(classLoader, libURL);
```

## Usando tudo
```Java
public class Main extends JavaPlugin{

    public void onEnable(){
        String libName = "ClockSchedulerAPI.jar";
        String classRef = "br.neitan96.clockschedulerapi.ClockSchedulerAPI";
        if(!libLoaded(classRef))
            if(!loadLib(libName))
                Bukkit.getLogger().log(Level.WARNING, "Não foi possivel carregar o ClockSchedulerAPI!");

    }

    public boolean libLoaded(String classRef){
        try{
            Class.forName(classRef);
            return true;
        }catch (ClassNotFoundException e){
            return false;
        }
    }

    public boolean loadLib(String filename){
        try{

            // Obtendo a lib dentro da jar do plugin.
            InputStream libInStream = getResource(filename);

            // Verificando se a lib existe.
            if(libInStream == null){
              // Se não existe ele manda um log de erro
              Bukkit.getLogger().log(Level.WARNING, "Erro o "+filename+" não está dentro da jar!");
              return false;
            }

            // Criando o arquivo temporário
            File libOutFile = File.createTempFile("Bukkit", filename);

            // Abrindo o arquivo temporário para escrita
            FileOutputStream libOutStream = new FileOutputStream(libOutFile);

            // Copiando a lib para o arquivo temporario
            byte[] buffer = new byte[1024];
            int length;

            while ((length = libInStream.read(buffer)) != -1)
                libOutStream.write(buffer, 0, length);

            libInStream.close();
            libOutStream.close();

            // Obtendo a URL do arquivo temporário
            URL libURL = libOutFile.toURI().toURL();

            // Obtendo o ClassLoader do nosso plugin
            URLClassLoader classLoader = (URLClassLoader) getClass().getClassLoader();

            // Obtendo a função do ClassLoader que faz o carregamento das libs
            Method addURL = URLClassLoader.class.getDeclaredMethod("addURL", URL.class);
            // "Hackeado" a função para torna-la acessivel
            addURL.setAccessible(true);
            // Usando a função e carregando a lib
            addURL.invoke(classLoader, libURL);

            return true;
        }catch (Exception e){
            e.printStackTrace();
            return false;
        }
    }

}
```

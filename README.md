# Principios-de-dise-o
Curso de principios de diseño
#SESION 1
# SRP
separacion de responsavilida unica
``` java
// este clas rompe con el pricipio SRP
public class Empledo{
     public void  calcularSueldo();

     public void enviarEmail();

     public void activarBono();
}


// este sera un eje,plo de SRP
public class Empleado{
 // atributos
 double sueldo;
 // constructor

}

public class CalcularBono{
    public double calcular(Empleado empleado){
        return empleado.sueldo*0.1;
    }
}

public class EmviarEmail{
    public void enviar(Empleado empleado){
              System.ut.printl(" enviando email s");

    }
}
```
# LSP
todos los hijos pueden reemplazar a su clase padre  sin romper los comportamientos
``` java

// aca si se cumple con LSP
public class Animal {
     public void comer(){
        System.ut.printl("animal come");
     }
}

public Perro extends Animal{
    @Override
    public void comer(){
        System.ut.printl("animal come");

    }
}

Animal animal= new Perro();
animal.comer(); // OK


// aca no se cumple con LSP
  
 public class  Animal{
    public void volar(){

    }
 }


 public class Pinguino extends Animal{
      @Override
    public void volar(){
        // Error  un pinguino no pude volar

    }

 }

 Animal animal= new Pinguino();
 animal.volar(); // ERROR

 ``` 

 # OCP

 abierto a agregar cerrado a modificar

  ```  java
   public class CalularBono{
     public double calcular(String tipo,sueldo){
        if(tipo.equals("Normal")) return sueldo *0.1;

        if(tipo.equals("Gernte")) return sueldo *0.1;
       if(tipo.equals("ADmisnistrador")) return sueldo *0.1;


     }
   }
   // si creo una nueva  tipo de bono  se modifica la calse CalcularBono   por lo que rompe con OCP
    

    // este es la  que se extendie  y la clase bono no se modifica solo recive el mismo Objeto o Intefas y calcula  lo que nesecita   automaticamente  nno se crean multiples if
    interface Bono{
        double calcular();
    } 
    public class CalcularBono{
        public double calcular(Bono bono){
            return bono.calcular();
        }
    }
    class BonoNormal{
        @Override
        public double calcular(double sueldo){
            return sueldo* 0.1;
        }

    }

    Bono bono= new BonoNormal();
    CalcularBono calcular= new CalcualrBono();
     calcular.calcular(bono)

  ```

# SESION 2
 # Patron singlenton
 consiste en una sola instancia de clase
 ejemplo:

 ```  java
class ConexionDB {
    // 1. La instancia debe ser static para que pertenezca a la clase
    private static ConexionDB instance;

    // 2. El constructor es privado para evitar el 'new ConexionDB()' externo
    private ConexionDB() {
        // Inicialización de la conexión si es necesario
    }

    // 3. El método debe ser static para poder llamarlo sin instanciar la clase
    public static ConexionDB getInstance() {
        if (instance == null) {
            instance = new ConexionDB();
        }
        return instance;
    }
}
 ``` 

# Patron prototype
consiste en clonar una clase ya existente evitando usar el new  para esto se usa la interface Cloneable
ejemplo:
 ```  java

 ``` 
class Persona implements Cloneable{
  string nombre;
public Persona clonable(){
   return new Persona(this.nombre)
}

}
# Patron Factory Metho




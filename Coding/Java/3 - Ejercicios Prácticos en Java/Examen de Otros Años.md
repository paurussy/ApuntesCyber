Para la copa américa, se requiere de un sistema en java que permitaalmacenar la información de los equipos y partidos que se juegan:
```java
public class Jugador {
    private String nombre;
    private int numCamiseta;

    // Constructor
    public Jugador(String nombre, int numCamiseta) {
        setNombre(nombre);
        setNumCamiseta(numCamiseta);
    }

    // Getter para nombre
    public String getNombre() {
        return nombre;
    }

    // Setter para nombre con validación
    public void setNombre(String nombre) {
        this.nombre = nombre;
    }

    // Getter para numCamiseta
    public int getNumCamiseta() {
        return numCamiseta;
    }

    // Setter para numCamiseta con validación
    public void setNumCamiseta(int numCamiseta) {
        if (numCamiseta < 1 || numCamiseta > 99) {
            throw new IllegalArgumentException("El número de camiseta debe estar entre 1 y 99");
        }
        this.numCamiseta = numCamiseta;
    }

    // Método main para pruebas (opcional)
    public static void main(String[] args) {
        try {
            Jugador jugador1 = new Jugador("Lionel Messi", 10);
            System.out.println("Jugador creado: " + jugador1.getNombre() + " con número " + jugador1.getNumCamiseta());

            // Intento de crear un jugador con nombre vacío (debe fallar)
            Jugador jugador2 = new Jugador("", 7);
        } catch (IllegalArgumentException e) {
            System.out.println(e.getMessage());
        }

        try {
            // Intento de crear un jugador con número de camiseta inválido (debe fallar)
            Jugador jugador3 = new Jugador("Cristiano Ronaldo", 100);
        } catch (IllegalArgumentException e) {
            System.out.println(e.getMessage());
        }
    }
}
```
![[Pasted image 20240521143304.png]]

The factory design pattern is used when we have a superclass with multiple subclasses and based on input, we return one of the subclass. This pattern takes out the responsibility of the instantiation of a class from the client program to the factory class.

```java
abstract class Computer {
  public abstract String getRAM();
  public abstract String getHDD();
  public abstract String getCPU();

  @Override
  public String toString(){
    return "RAM= "+this.getRAM()+", HDD="+this.getHDD()+", CPU="+this.getCPU();
  }
}

class PC extends Computer {
  private String ram;
  private String hdd;
  private String cpu;

  public PC(String ram, String hdd, String cpu){
    this.ram=ram;
    this.hdd=hdd;
    this.cpu=cpu;
  }

  @Override
  public String getRAM() {
    return this.ram;
  }

  @Override
  public String getHDD() {
    return this.hdd;
  }

  @Override
  public String getCPU() {
    return this.cpu;
  }
}

class Server extends Computer {
  private String ram;
  private String hdd;
  private String cpu;

  public Server(String ram, String hdd, String cpu){
    this.ram=ram;
    this.hdd=hdd;
    this.cpu=cpu;
  }

  @Override
  public String getRAM() {
    return this.ram;
  }

  @Override
  public String getHDD() {
    return this.hdd;
  }

  @Override
  public String getCPU() {
    return this.cpu;
  }
}

class ComputerFactory {
  public static Computer getComputer(String type, String ram, String hdd, String cpu){
    if("PC".equalsIgnoreCase(type)) return new PC(ram, hdd, cpu);
    else if("Server".equalsIgnoreCase(type)) return new Server(ram, hdd, cpu);

    return null;
  }
}

public class TestFactory {
  public static void main(String[] args) {
    Computer pc = ComputerFactory.getComputer("pc","2 GB","500 GB","2.4 GHz");
    Computer server = ComputerFactory.getComputer("server","16 GB","1 TB","2.9 GHz");
    System.out.println("Factory PC Config::"+pc);
    System.out.println("Factory Server Config::"+server);
  }
}
```
live code https://repl.it/@lubaochuan/SomberVainFormats

## Factory Design Pattern Advantages
* It provides access to interface rather than implementation.
* It removes the instantiation of actual implementation classes from client code.
* It makes our code more robust, less coupled and easy to extend. For example, we can easily change PC class implementation because client program is unaware of this.

sources:
* https://www.journaldev.com/1392/factory-design-pattern-in-java

Composite design pattern is used when we have to represent a part-whole hierarchy. In the structure all objects are to be treated the same way,

```java
import java.util.ArrayList;
import java.util.List;


public class TestCompositePattern {
  public static void main(String[] args) {
    Shape t1 = new Triangle();
    Shape t2 = new Triangle();
    Shape c = new Circle();

    Drawing drawing = new Drawing();
    drawing.add(t2);
    drawing.add(t2);
    drawing.add(c);

    drawing.draw("Red");

    drawing.clear();

    drawing.add(t1);
    drawing.add(c);
    drawing.draw("Green");
  }
}

class Drawing implements Shape{
  //collection of Shapes
  private List<Shape> shapes = new ArrayList<Shape>();

  @Override
  public void draw(String fillColor) {
    for(Shape sh : shapes){
      sh.draw(fillColor);
    }
  }

  //adding shape to drawing
  public void add(Shape s){
    this.shapes.add(s);
  }

  //removing shape from drawing
  public void remove(Shape s){
    shapes.remove(s);
  }

  //removing all the shapes
  public void clear(){
    System.out.println("Clearing all the shapes from drawing");
    this.shapes.clear();
  }
}

interface Shape {
  public void draw(String fillColor);
}

class Triangle implements Shape {
  @Override
  public void draw(String fillColor) {
    System.out.println("Drawing Triangle with color "+fillColor);
  }
}

class Circle implements Shape {
  @Override
  public void draw(String fillColor) {
    System.out.println("Drawing Circle with color "+fillColor);
  }
}
```

live code https://repl.it/join/dihdazvr-lubaochuan

sources:
* https://www.journaldev.com/1535/composite-design-pattern-in-java

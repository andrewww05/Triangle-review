## Code review

---

### 1. Consider using more semantic variables
### Current:
```java
    private double a;
    private double b;
    private double c;
```
### Suggested:
```java
    private double edgeA;
    private double edgeB;
    private double edgeC;
```

---

### 2. Missing input validation in constructor

### Current:
```java
    public Triangle(double a, double b, double c) {
        this.a = a;
        this.b = b;
        this.c = c;
    }

    public Triangle() {
    }
```

### Suggested:
```java
    public Triangle(double edgeA, double edgeB, double edgeC) {
        if (edgeA <= 0 || edgeB <= 0 || edgeC <= 0) {
            throw new IllegalArgumentException("Edges must be positive");
        }
        if (edgeA + edgeB <= edgeC || edgeA + edgeC <= edgeB || edgeB + edgeC <= edgeA) {
            throw new IllegalArgumentException("Edges do not form a valid triangle");
        }
        this.edgeA = edgeA;
        this.edgeB = edgeB;
        this.edgeC = edgeC;
    }
```
---

### 3. Wrong getter/setter methods naming

### Current:
```java
    // Same for the remaining edges
    public double takeA() {
        return a;
    }

    public void putA(double a) {
        this.a = a;
    }
    ...
```
### Suggested:
```java
    // Same for the remaining edges
    public double getEdgeA() {
        return a;
    }

    public void setEdgeA(double a) {
        this.a = a;
    }

    ...
```

---

### 4. Wrong method naming: Method names are typically verbs or verb phrases.

### Current:
```java
    public double perim() { return a + b + c; }
```

### Suggested:

```java
    public double calculatePerimeter() {
        return a + b + c;
    }
```

---

### 5. Wrong method naming: same as above, applies to area()

### Current:
```java
    public double area() { return Math.sqrt(0.5*perim()*(0.5*perim()-a)*(0.5*perim()-b)*(0.5*perim()-c)); }
```

### Suggested:
```java
    public double calculateArea() {
        double halfPerimeter = 0.5 * calculatePerimeter();
        return Math.sqrt(halfPerimeter * (halfPerimeter - edgeA) * (halfPerimeter - edgeB) * (halfPerimeter - edgeC));
    }
```

---

### 6. Wrong method naming and unnecessary complexity

### Current:
```java
    public  boolean  equilateral(){
        if (a == b && b == c){
            return true;
        } else return false;

   }
```

### Suggested:
```java
    public boolean isEquilateral(){
        return a == b && b == c;
    }
```

---

### 7. Consider using more readable description for the triangle

### Current:
```java
    // Out: Triangle{a=[a], b=[b], c=[c]}
    @Override
    public String toString() {
        return "Triangle{" +
                "a=" + a +
                ", b=" + b +
                ", c=" + c +
                '}';
    }
```

### Suggested:
```java
    // Out: Triangle edges: {A: [a], B: [b], C: [c]}
    @Override
    public String toString() {
        return "Triangle edges: {" +
                "A: " + edgeA +
                ", B: " + edgeB +
                ", C: " + edgeC +
                '}';
    }
```

---

### 8. Missing comparator implementation

### 8.1. Current:
```java
    public class Triangle {
```

### 8.1. Suggested:
```java
    import java.util.Comparator;
    ...
    public class Triangle implements Comparator<Triangle> {
```

### 8.2. Current:
```java
    -
```

### 8.2. Suggested:
```java
    @Override
    public int compare(Triangle firstTriangle, Triangle secondTriangle) {
        return Double.compare(firstTriangle.calculateArea(), secondTriangle.calculateArea());
    }
```

---

### 9. Improve equals() implementation

### Current:
```java
    @Override
    public final boolean equals(Object o) {
        if (!(o instanceof Triangle triangle)) return false;
    
        return Double.compare(a, triangle.a) == 0 && Double.compare(b, triangle.b) == 0 && Double.compare(c, triangle.c) == 0;
    }
```

### Suggested:
```java
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Triangle triangle)) return false;
    
        return Double.compare(triangle.edgeA, edgeA) == 0 &&
                Double.compare(triangle.edgeB, edgeB) == 0 &&
                Double.compare(triangle.edgeC, edgeC) == 0;
    }
```

---

### 10. Simplify method

### Current:
```java
    @Override
    public int hashCode() {
        int result = Double.hashCode(a);
        result = 31 * result + Double.hashCode(b);
        result = 31 * result + Double.hashCode(c);
        return result;
    }
```

### Suggested:
```java
    @Override
    public int hashCode() {
        return Objects.hash(edgeA, edgeB, edgeC);
    }
```
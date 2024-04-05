# CHALLENGE 5

## 5.A

We use a unique module to get inside of the package shape

5_A/
├── Paquete/
│   ├── __init__.py
│   └── Shape.py
└── punto_5_a.py

### Normal code without __name__ and "__main__"

``` python
import math

class Point:
    # Definition of the Point class
    definition: str = "Abstract geometric entity that represents a location in space."

    # Constructor method for the Point class
    def __init__(self, x: float = 0, y: float = 0):
        self.x = x
        self.y = y

    # Method to move the point to a new location
    def move(self, new_x: float, new_y: float):
        self.x = new_x
        self.y = new_y

    # Method to reset the point to the origin
    def reset(self):
        self.x = 0
        self.y = 0
    
    # Method to calcule the distance of two points
    def compute_distance(self, point)-> float:
        distance = (((self.x - point.x)**2) + ((self.y - point.y)**2))**(0.5)
        return distance

class Line:
    # Constructor method to initialize the Line object with start and end points
    def __init__(self, start, end):
        self.start = start  # Start point of the line
        self.end = end      # End point of the line
        self.length = start.compute_distance(end)
        

class Shape: # Super class
    def __init__(self, regular: bool, vertices: list, edges: list, inner_angles: list):
        self.regular = regular # The form is regular?
        self.vertices = vertices # Vertices of the form
        self.edges = edges  # Calculate the length of the points
        self.inner_angles = inner_angles # Angles
    
    def compute_area(self): # Funtion to calcule the area
        pass
    
    def compute_perimeter(self): # Funtion to calculate the perimeter
        pass
    
    def compute_inner_angles(self): # Funtion to calculate angles
        pass

class Triangle(Shape): # Class
    def __init__(self, regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(regular, vertices, edges, inner_angles)
        self.compute_perimeter() # Call perimeter 
    
    def compute_perimeter(self):
        if len(self.edges) == 3: # Len of vertices need to be three
            perimeter = self.edges[0] + self.edges[1] + self.edges[2]
            self.perimeter = perimeter
            return perimeter # Return the perimeter
        pass

    def compute_area(self):
        if len(self.edges) == 3:
            s_heron = self.perimeter / 2 # I use the heron formula to calculate the area because it´s funtion for all triangles
            area = (s_heron*(s_heron-self.edges[0])*(s_heron-self.edges[1])*(s_heron-self.edges[2]))**0.5
            return area
        else:
            pass
    
    def compute_inner_angles(self):
        if len(self.edges) == 3: # We use arcoseno to calculate the angles of the traingle, this funtion in all triangles
            angle_a = math.degrees(math.acos((self.edges[1]**2 + self.edges[2]**2 - self.edges[0]**2) / (2 * self.edges[1] * self.edges[2])))
            angle_b = math.degrees(math.acos((self.edges[2]**2 + self.edges[0]**2 - self.edges[1]**2) / (2 * self.edges[0] * self.edges[2])))
            angle_c = math.degrees(math.acos((self.edges[0]**2 + self.edges[1]**2 - self.edges[2]**2) / (2 * self.edges[0] * self.edges[1])))
            self.inner_angles = [angle_a, angle_b, angle_c]
            return self.inner_angles
        else: pass

class Rectangle(Shape): # Class 
    def __init__(self, regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(regular, vertices, edges, inner_angles)
        self.is_rectangle()
    
    def is_rectangle(self): # Try to see if the points that the user enter make a rectangle
        if len(self.edges) == 4:
            sorted_vertices = sorted(self.vertices, key=lambda point: point.x) # Order the points
            if sorted_vertices[0].x == sorted_vertices[1].x:
                if sorted_vertices[2].x == sorted_vertices[3].x:
                    if sorted_vertices[0].y == sorted_vertices[2].y:
                        if sorted_vertices[1].y == sorted_vertices[3].y: # If they are a rectangle
                            width = self.vertices[2].x - self.vertices[0].x # Calculate the width
                            height = self.vertices[1].y - self.vertices[0].y # Calculate the height
                            self.width = width
                            self.height = height
                            return True
                        else: 
                            return False # In all other cases the form is not a rectangle, and base of that we don´t need to do the other
                    else: 
                        return False
                else:
                    return False
            else: 
                return False
        else:
            return False
            
    def compute_perimeter(self):
        if len(self.edges) == 4:
            perimeter = (2*self.width + 2*self.height) # Calculate perimeter in base of with and height
            return perimeter
        else:
            pass

    def compute_area(self):
        if len(self.edges) == 4:
            area = self.height * self.width # Caclulate the area of a form with 4 lines
            return area
        else:
            pass
    
    def compute_inner_angles(self):
        if len(self.edges) == 4:
            self.inner_angles = [90, 90, 90, 90] # If is a rectangle, you don´t need to calculate angles, there are 90 
            return self.inner_angles
        else: pass

class Square(Rectangle): # Class Herence
    def __init__(self, regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(regular, vertices, edges, inner_angles)
    
    def is_square(self):
        super().is_rectangle() # Prove if is a rectangle but why?
        if self.height == self.width: # To call this things and see if they are equal, in case that not, it´s not a square
            return True # Is square
        else:
            return False # Is´nt an square
        
    def compute_perimeter(self):
        return super().compute_perimeter() # Call perimeter from rectangle

    
    def compute_area(self):
        return super().compute_area() # Call area from rectangle
    
    def compute_inner_angles(self):
        return super().compute_inner_angles() # Call inner angles from rectangle
        
class Isoceles(Triangle): # Class Herence
    def __init__(self, regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(regular, vertices, edges, inner_angles)
        
    def is_isoceles(self): # Define if is a isoceles, 2 lines need to be equal
        if self.edges[0] == self.edges[1] or self.edges[0] == self.edges[2] or self.edges[1] == self.edges[2]:
            return True
        else: return False
    
    def compute_perimeter(self):
        return super().compute_perimeter() # Call perimeter in traingle
    
    def compute_area(self):
            return super().compute_area() # Call area in trinagle
    
    def compute_inner_angles(self): # Call inner angles in triangle
        return super().compute_inner_angles()

class Equilateral(Triangle): # Class herence
    def __init__(self, regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(regular, vertices, edges, inner_angles)
        
    def is_equilateral(self): # We need to see if is a equilateral, it is if all the lines are equal
        if self.edges[0] == self.edges[1] and self.edges[1] == self.edges[2]:
            return True
        else: return False
    
    def compute_perimeter(self):
        return super().compute_perimeter() # Call perimeter from triangle

    
    def compute_area(self):
        return super().compute_area() # Call area from triangle
    
    def compute_inner_angles(self):
        return super().compute_inner_angles() # Call inner angles from triangle

class Scalene(Triangle): # Class herence
    def __init__(self, regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(regular, vertices, edges, inner_angles)
        
    def is_scalene(self): # We need to si if is scalen, the three lines are diferente
        if self.edges[0] != self.edges[1] and self.edges[0] != self.edges[2] and self.edges[1] != self.edges[2]:
            return True
        else: return False
    
    def compute_perimeter(self): # Call perimeter from triangle
        return super().compute_perimeter()
    
    def compute_area(self):
        return super().compute_area() # Call area from tringle

    
    def compute_inner_angles(self):
        return super().compute_inner_angles() # Call inner angles from triangle

class Trirectangle(Triangle): #Class herence
    def __init__(self, regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(regular, vertices, edges, inner_angles)

    def is_trirectangle(self): # We need to si if a rectnagle triangle, we considere a few calculate mistake that happens in python
        super().compute_inner_angles()
        for i in self.inner_angles:
            if abs(i - 90) < 0.1:
                return True
        else: return False
            
    def compute_perimeter(self):
        return super().compute_perimeter() # call perimeter from triangle
    
    def compute_area(self): # call area from triangle
        return super().compute_area()
    
    def compute_inner_angles(self): # call inner angles from triangle
        return super().compute_inner_angles()
```

### Import code

``` python
from Paquete.reto_4 import Point 
from Paquete.reto_4 import Line
from Paquete.reto_4 import Rectangle
from Paquete.reto_4 import Isoceles
from Paquete.reto_4 import Equilateral
from Paquete.reto_4 import Scalene
from Paquete.reto_4 import Trirectangle
from Paquete.reto_4 import Square

if __name__ == "__main__":
    # Ask the user whether they want to input 3 or 4 points
    figure = int(input("You want to enter 3 or 4 points: "))
    flag = True

    # Validate the user input
    while flag == True:
        if figure == 3 or figure == 4:
            break
        else:
            figure = int(input("You want to enter 3 or 4 points: "))

    # Process based on the number of points entered by the user
    if figure == 3:
        # Obtain the coordinates of three points from the user
        p1 = Point(int(input("Enter the x coordinate of the p1: ")), int(input("Enter the y coordinate of the p1: ")))
        p2 = Point(int(input("Enter the x coordinate of the p2: ")), int(input("Enter the y coordinate of the p2: ")))
        p3 = Point(int(input("Enter the x coordinate of the p3: ")), int(input("Enter the y coordinate of the p3: ")))

        # Create lines using the provided points
        line1 = Line(p1, p2)
        line2 = Line(p2, p3)
        line3 = Line(p3, p1)

        # Create instances of different types of triangles
        triangulo = Isoceles(True, [p1, p2, p3], [line1.length, line2.length, line3.length], [0, 0, 0])
        triangulo_1 = Equilateral(True, [p1, p2, p3], [line1.length, line2.length, line3.length], [0, 0, 0])
        triangulo_2 = Scalene(True, [p1, p2, p3], [line1.length, line2.length, line3.length], [0, 0, 0])
        triangulo_3 = Trirectangle(True, [p1, p2, p3], [line1.length, line2.length, line3.length], [0, 0, 0])

        # Determine the type of triangle and perform calculations accordingly
        if triangulo.is_isoceles() == True:
            if triangulo_3.is_trirectangle() == True:
                print("The triangle is an isoceles and a rectangle triangle")
                per_trin = triangulo.compute_perimeter()
                print("The perimeter of the triangle is: " + str(per_trin))
                area_trin = triangulo.compute_area()
                print("The area of the triangle is: " + str(area_trin))
                angles_trin = triangulo.compute_inner_angles()
                print("The inner angles of the triangle is: " + str(angles_trin))
            else:
                print("The triangle is an isoceles")
                per_trin = triangulo.compute_perimeter()
                print("The perimeter of the triangle is: " + str(per_trin))
                area_trin = triangulo.compute_area()
                print("The area of the triangle is: " + str(area_trin))
                angles_trin = triangulo.compute_inner_angles()
                print("The inner angles of the triangle is: " + str(angles_trin))

        elif triangulo_1.is_equilateral() == True:
            print("The triangle is an Equilateral")
            per_trin = triangulo_1.compute_perimeter()
            print("The perimeter of the triangle is: " + str(per_trin))
            area_trin = triangulo_1.compute_area()
            print("The area of the triangle is: " + str(area_trin))
            angles_trin = triangulo_1.compute_inner_angles()
            print("The inner angles of the triangle is: " + str(angles_trin))

        elif triangulo_2.is_scalene() == True:
            if triangulo_3.is_trirectangle() == True:
                print("The triangle is an Scalene and a rectangle triangle")
                per_trin = triangulo_2.compute_perimeter()
                print("The perimeter of the triangle is: " + str(per_trin))
                area_trin = triangulo_2.compute_area()
                print("The area of the triangle is: " + str(area_trin))
                angles_trin = triangulo_2.compute_inner_angles()
                print("The inner angles of the triangle is: " + str(angles_trin))
            else: 
                print("The triangle is an Scalene")
                per_trin = triangulo_2.compute_perimeter()
                print("The perimeter of the triangle is: " + str(per_trin))
                area_trin = triangulo_2.compute_area()
                print("The area of the triangle is: " + str(area_trin))
                angles_trin = triangulo_2.compute_inner_angles()
                print("The inner angles of the triangle is: " + str(angles_trin))

    if figure == 4:
        # Obtain the coordinates of four points from the user
        p1 = Point(int(input("Enter the x coordinate of the p1: ")), int(input("Enter the y coordinate of the p1: ")))
        p2 = Point(int(input("Enter the x coordinate of the p2: ")), int(input("Enter the y coordinate of the p2: ")))
        p3 = Point(int(input("Enter the x coordinate of the p3: ")), int(input("Enter the y coordinate of the p3: ")))
        p4 = Point(int(input("Enter the x coordinate of the p4: ")), int(input("Enter the y coordinate of the p4: ")))

        # Sort the points based on their x-coordinate
        listi = [p1, p2, p3, p4]
        listi = sorted(listi, key=lambda p: (p.x, p.y))

        # Create lines using the provided points
        line1 = Line(listi[0], listi[1])
        line2 = Line(listi[0], listi[2])
        line3 = Line(listi[1], listi[3])
        line4 = Line(listi[2], listi[3])
        # Create instances of Rectangle and Square
        rectangulo = Rectangle(True, [listi[0], listi[1], listi[2], listi[3]], [line1.length, line2.length, line3.length, line4.length], [0, 0, 0, 0])
        cuadrado = Square(True, [listi[0], listi[1], listi[2], listi[3]], [line1.length, line2.length, line3.length, line4.length], [0, 0, 0, 0])

        # Check if the figure is a square or a rectangle
        if cuadrado.is_square() == True: # If it's a square
            print("The form is a square")
            per_cua = cuadrado.compute_perimeter() # Compute the perimeter
            print("The perimeter of the square is: " +str(per_cua))
            area_cua = cuadrado.compute_area() # Compute the area
            print("The area of the square is: " +str(area_cua))
            angles_cua = cuadrado.compute_inner_angles() # Compute the inner angles
            print("The inner angles of the square is: " +str(angles_cua))

        elif rectangulo.is_rectangle() == True: # If it's a rectangle
            print("The form is a Rectangle")
            per_rect = rectangulo.compute_perimeter() # Compute the perimeter
            print("The perimeter of the rectangle is: " +str(per_rect))
            area_rect = rectangulo.compute_area() # Compute the area
            print("The area of the rectangle is: " +str(area_rect))
            angles_rect = rectangulo.compute_inner_angles() # Compute the inner angles
            print("The inner angles of the rectangle is: " +str(angles_rect))

```

if you want to see all or prove it open the carpet 5, and open 5_A

## 5.B

We use a multiple module to get inside of the package shape

Reto_5b/
├── paquete_5b/
│   ├── __init__.py
│   ├── Shape.py
│   ├── Triangle.py
│   ├── Rectangle.py
│   ├── Square.py
│   ├── Equilateral.py
│   ├── Isoceles.py
│   ├── Scalene.py
│   └── Trirectangle.py
└── punto_5_b.py

### Shape

``` python
class Point:
    # Definition of the Point class
    definition: str = "Abstract geometric entity that represents a location in space."

    # Constructor method for the Point class
    def __init__(self, x: float = 0, y: float = 0):
        self.x = x
        self.y = y

    # Method to move the point to a new location
    def move(self, new_x: float, new_y: float):
        self.x = new_x
        self.y = new_y

    # Method to reset the point to the origin
    def reset(self):
        self.x = 0
        self.y = 0
    
    # Method to calcule the distance of two points
    def compute_distance(self, point)-> float:
        distance = (((self.x - point.x)**2) + ((self.y - point.y)**2))**(0.5)
        return distance

class Line:
    # Constructor method to initialize the Line object with start and end points
    def __init__(self, start, end):
        self.start = start  # Start point of the line
        self.end = end      # End point of the line
        self.length = start.compute_distance(end)
        
class Shape: # Super class
    def __init__(self, regular: bool, vertices: list, edges: list, inner_angles: list):
        self.regular = regular # The form is regular?
        self.vertices = vertices # Vertices of the form
        self.edges = edges  # Calculate the length of the points
        self.inner_angles = inner_angles # Angles
    
    def compute_area(self): # Funtion to calcule the area
        pass
    
    def compute_perimeter(self): # Funtion to calculate the perimeter
        pass
    
    def compute_inner_angles(self): # Funtion to calculate angles
        pass
```

### Triangle

``` python
import math

from .Shape import Shape

class Triangle(Shape): # Class
    def __init__(self, regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(regular, vertices, edges, inner_angles)
        self.compute_perimeter() # Call perimeter 
    
    def compute_perimeter(self):
        if len(self.edges) == 3: # Len of vertices need to be three
            perimeter = self.edges[0] + self.edges[1] + self.edges[2]
            self.perimeter = perimeter
            return perimeter # Return the perimeter
        pass

    def compute_area(self):
        if len(self.edges) == 3:
            s_heron = self.perimeter / 2 # I use the heron formula to calculate the area because it´s funtion for all triangles
            area = (s_heron*(s_heron-self.edges[0])*(s_heron-self.edges[1])*(s_heron-self.edges[2]))**0.5
            return area
        else:
            pass
    
    def compute_inner_angles(self):
        if len(self.edges) == 3: # We use arcoseno to calculate the angles of the traingle, this funtion in all triangles
            angle_a = math.degrees(math.acos((self.edges[1]**2 + self.edges[2]**2 - self.edges[0]**2) / (2 * self.edges[1] * self.edges[2])))
            angle_b = math.degrees(math.acos((self.edges[2]**2 + self.edges[0]**2 - self.edges[1]**2) / (2 * self.edges[0] * self.edges[2])))
            angle_c = math.degrees(math.acos((self.edges[0]**2 + self.edges[1]**2 - self.edges[2]**2) / (2 * self.edges[0] * self.edges[1])))
            self.inner_angles = [angle_a, angle_b, angle_c]
            return self.inner_angles
        else: pass
```

### Rectangle

``` python
import math

from .Shape import Shape

class Rectangle(Shape): # Class 
    def __init__(self, regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(regular, vertices, edges, inner_angles)
        self.is_rectangle()
    
    def is_rectangle(self): # Try to see if the points that the user enter make a rectangle
        if len(self.edges) == 4:
            sorted_vertices = sorted(self.vertices, key=lambda point: point.x) # Order the points
            if sorted_vertices[0].x == sorted_vertices[1].x:
                if sorted_vertices[2].x == sorted_vertices[3].x:
                    if sorted_vertices[0].y == sorted_vertices[2].y:
                        if sorted_vertices[1].y == sorted_vertices[3].y: # If they are a rectangle
                            width = self.vertices[2].x - self.vertices[0].x # Calculate the width
                            height = self.vertices[1].y - self.vertices[0].y # Calculate the height
                            self.width = width
                            self.height = height
                            return True
                        else: 
                            return False # In all other cases the form is not a rectangle, and base of that we don´t need to do the other
                    else: 
                        return False
                else:
                    return False
            else: 
                return False
        else:
            return False
            
    def compute_perimeter(self):
        if len(self.edges) == 4:
            perimeter = (2*self.width + 2*self.height) # Calculate perimeter in base of with and height
            return perimeter
        else:
            pass

    def compute_area(self):
        if len(self.edges) == 4:
            area = self.height * self.width # Caclulate the area of a form with 4 lines
            return area
        else:
            pass
    
    def compute_inner_angles(self):
        if len(self.edges) == 4:
            self.inner_angles = [90, 90, 90, 90] # If is a rectangle, you don´t need to calculate angles, there are 90 
            return self.inner_angles
        else: pass

```

### Square

``` python
from .Rectangle import Rectangle

class Square(Rectangle): # Class Herence
    def __init__(self, regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(regular, vertices, edges, inner_angles)
    
    def is_square(self):
        super().is_rectangle() # Prove if is a rectangle but why?
        if Rectangle.is_rectangle() == True:
            if self.height == self.width: # To call this things and see if they are equal, in case that not, it´s not a square
                return True # Is square
            else:
                return False # Is´nt an square
        else:
            return False
        
    def compute_perimeter(self):
        return super().compute_perimeter() # Call perimeter from rectangle

    
    def compute_area(self):
        return super().compute_area() # Call area from rectangle
    
    def compute_inner_angles(self):
        return super().compute_inner_angles() # Call inner angles from rectangle
  
```

### Equilateral

``` python
from .Triangle import Triangle # Import equilateral

class Equilateral(Triangle): # Class herence
    def __init__(self, regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(regular, vertices, edges, inner_angles)
        
    def is_equilateral(self): # We need to see if is a equilateral, it is if all the lines are equal
        if self.edges[0] == self.edges[1] and self.edges[1] == self.edges[2]:
            return True
        else: return False
    
    def compute_perimeter(self):
        return super().compute_perimeter() # Call perimeter from triangle

    
    def compute_area(self):
        return super().compute_area() # Call area from triangle
    
    def compute_inner_angles(self):
        return super().compute_inner_angles() # Call inner angles from triangle

```

### Isoceles

``` python
from .Triangle import Triangle

class Isoceles(Triangle): # Class Herence
    def __init__(self, regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(regular, vertices, edges, inner_angles)
        
    def is_isoceles(self): # Define if is a isoceles, 2 lines need to be equal
        if self.edges[0] == self.edges[1] or self.edges[0] == self.edges[2] or self.edges[1] == self.edges[2]:
            return True
        else: return False
    
    def compute_perimeter(self):
        return super().compute_perimeter() # Call perimeter in traingle
    
    def compute_area(self):
            return super().compute_area() # Call area in trinagle
    
    def compute_inner_angles(self): # Call inner angles in triangle
        return super().compute_inner_angles()

```

### Scalene

``` python
from .Triangle import Triangle

class Scalene(Triangle): # Class herence
    def __init__(self, regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(regular, vertices, edges, inner_angles)
        
    def is_scalene(self): # We need to si if is scalen, the three lines are diferente
        if self.edges[0] != self.edges[1] and self.edges[0] != self.edges[2] and self.edges[1] != self.edges[2]:
            return True
        else: return False
    
    def compute_perimeter(self): # Call perimeter from triangle
        return super().compute_perimeter()
    
    def compute_area(self):
        return super().compute_area() # Call area from tringle

    
    def compute_inner_angles(self):
        return super().compute_inner_angles() # Call inner angles from triangle

```

### Trirectangle

``` python
from .Triangle import Triangle

class Trirectangle(Triangle): #Class herence
    def __init__(self, regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(regular, vertices, edges, inner_angles)

    def is_trirectangle(self): # We need to si if a rectnagle triangle, we considere a few calculate mistake that happens in python
        super().compute_inner_angles()
        for i in self.inner_angles:
            if abs(i - 90) < 0.1:
                return True
        else: return False
            
    def compute_perimeter(self):
        return super().compute_perimeter() # call perimeter from triangle
    
    def compute_area(self): # call area from triangle
        return super().compute_area()
    
    def compute_inner_angles(self): # call inner angles from triangle
        return super().compute_inner_angles()
  
```

### Point imported

``` python
from Paquete_b.Shape import Point #Import all de moduls
from Paquete_b.Shape import Line
from Paquete_b.Triangle import Triangle
from Paquete_b.Rectangle import Rectangle
from Paquete_b.Isoceles import Isoceles
from Paquete_b.Equilateral import Equilateral
from Paquete_b.Scalene import Scalene
from Paquete_b.Trirectangle import Trirectangle
from Paquete_b.Square import Square

if __name__ == "__main__":
    # Ask the user whether they want to input 3 or 4 points
    figure = int(input("You want to enter 3 or 4 points: "))
    flag = True

    # Validate the user input
    while flag == True:
        if figure == 3 or figure == 4:
            break
        else:
            figure = int(input("You want to enter 3 or 4 points: "))

    # Process based on the number of points entered by the user
    if figure == 3:
        # Obtain the coordinates of three points from the user
        p1 = Point(int(input("Enter the x coordinate of the p1: ")), int(input("Enter the y coordinate of the p1: ")))
        p2 = Point(int(input("Enter the x coordinate of the p2: ")), int(input("Enter the y coordinate of the p2: ")))
        p3 = Point(int(input("Enter the x coordinate of the p3: ")), int(input("Enter the y coordinate of the p3: ")))

        # Create lines using the provided points
        line1 = Line(p1, p2)
        line2 = Line(p2, p3)
        line3 = Line(p3, p1)

        # Create instances of different types of triangles
        triangulo = Isoceles(True, [p1, p2, p3], [line1.length, line2.length, line3.length], [0, 0, 0])
        triangulo_1 = Equilateral(True, [p1, p2, p3], [line1.length, line2.length, line3.length], [0, 0, 0])
        triangulo_2 = Scalene(True, [p1, p2, p3], [line1.length, line2.length, line3.length], [0, 0, 0])
        triangulo_3 = Trirectangle(True, [p1, p2, p3], [line1.length, line2.length, line3.length], [0, 0, 0])

        # Determine the type of triangle and perform calculations accordingly
        if triangulo.is_isoceles() == True:
            if triangulo_3.is_trirectangle() == True:
                print("The triangle is an isoceles and a rectangle triangle")
                per_trin = triangulo.compute_perimeter()
                print("The perimeter of the triangle is: " + str(per_trin))
                area_trin = triangulo.compute_area()
                print("The area of the triangle is: " + str(area_trin))
                angles_trin = triangulo.compute_inner_angles()
                print("The inner angles of the triangle is: " + str(angles_trin))
            else:
                print("The triangle is an isoceles")
                per_trin = triangulo.compute_perimeter()
                print("The perimeter of the triangle is: " + str(per_trin))
                area_trin = triangulo.compute_area()
                print("The area of the triangle is: " + str(area_trin))
                angles_trin = triangulo.compute_inner_angles()
                print("The inner angles of the triangle is: " + str(angles_trin))

        elif triangulo_1.is_equilateral() == True:
            print("The triangle is an Equilateral")
            per_trin = triangulo_1.compute_perimeter()
            print("The perimeter of the triangle is: " + str(per_trin))
            area_trin = triangulo_1.compute_area()
            print("The area of the triangle is: " + str(area_trin))
            angles_trin = triangulo_1.compute_inner_angles()
            print("The inner angles of the triangle is: " + str(angles_trin))

        elif triangulo_2.is_scalene() == True:
            if triangulo_3.is_trirectangle() == True:
                print("The triangle is an Scalene and a rectangle triangle")
                per_trin = triangulo_2.compute_perimeter()
                print("The perimeter of the triangle is: " + str(per_trin))
                area_trin = triangulo_2.compute_area()
                print("The area of the triangle is: " + str(area_trin))
                angles_trin = triangulo_2.compute_inner_angles()
                print("The inner angles of the triangle is: " + str(angles_trin))
            else: 
                print("The triangle is an Scalene")
                per_trin = triangulo_2.compute_perimeter()
                print("The perimeter of the triangle is: " + str(per_trin))
                area_trin = triangulo_2.compute_area()
                print("The area of the triangle is: " + str(area_trin))
                angles_trin = triangulo_2.compute_inner_angles()
                print("The inner angles of the triangle is: " + str(angles_trin))

    if figure == 4:
        # Obtain the coordinates of four points from the user
        p1 = Point(int(input("Enter the x coordinate of the p1: ")), int(input("Enter the y coordinate of the p1: ")))
        p2 = Point(int(input("Enter the x coordinate of the p2: ")), int(input("Enter the y coordinate of the p2: ")))
        p3 = Point(int(input("Enter the x coordinate of the p3: ")), int(input("Enter the y coordinate of the p3: ")))
        p4 = Point(int(input("Enter the x coordinate of the p4: ")), int(input("Enter the y coordinate of the p4: ")))

        # Sort the points based on their x-coordinate
        listi = [p1, p2, p3, p4]
        listi = sorted(listi, key=lambda p: (p.x, p.y))

        # Create lines using the provided points
        line1 = Line(listi[0], listi[1])
        line2 = Line(listi[0], listi[2])
        line3 = Line(listi[1], listi[3])
        line4 = Line(listi[2], listi[3])
        # Create instances of Rectangle and Square
        rectangulo = Rectangle(True, [listi[0], listi[1], listi[2], listi[3]], [line1.length, line2.length, line3.length, line4.length], [0, 0, 0, 0])
        cuadrado = Square(True, [listi[0], listi[1], listi[2], listi[3]], [line1.length, line2.length, line3.length, line4.length], [0, 0, 0, 0])

        # Check if the figure is a square or a rectangle
        if cuadrado.is_square() == True: # If it's a square
            print("The form is a square")
            per_cua = cuadrado.compute_perimeter() # Compute the perimeter
            print("The perimeter of the square is: " +str(per_cua))
            area_cua = cuadrado.compute_area() # Compute the area
            print("The area of the square is: " +str(area_cua))
            angles_cua = cuadrado.compute_inner_angles() # Compute the inner angles
            print("The inner angles of the square is: " +str(angles_cua))

        elif rectangulo.is_rectangle() == True: # If it's a rectangle
            print("The form is a Rectangle")
            per_rect = rectangulo.compute_perimeter() # Compute the perimeter
            print("The perimeter of the rectangle is: " +str(per_rect))
            area_rect = rectangulo.compute_area() # Compute the area
            print("The area of the rectangle is: " +str(area_rect))
            angles_rect = rectangulo.compute_inner_angles() # Compute the inner angles
            print("The inner angles of the rectangle is: " +str(angles_rect))
```

if you want to see all or prove it open the carpet 5, and open 5_B

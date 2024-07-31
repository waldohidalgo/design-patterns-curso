# Programming Foundations: Design Patterns

Repositorio resumen del curso [**Design Patterns** dado en Linkedin Learning](https://www.linkedin.com/learning/programming-foundations-design-patterns-2). El curso abarca los siguientes design principles:

- **Single Responsability**: a class should have only one reason to change

- **Loose coupling**: strive for loosely coupled designs between objects that interact

- **Use composition rather than inheritance**:classes should achieve code reuse using composition rather than inheritance from a superclass. This allows for a more flexible design.

- **Program to an interface, not an implementation**: clients remain unaware of the specific types of objects they use, as long as the objects adhere to the interface that clients expect

- **Encapsulate what varies**: identify the aspects of your application that vary and separate them from what stays the same

- **Open-closed**: classes should be open for extension but closed for modification

Y los patrones de diseño siguientes:

1- Strategy Pattern

2- Adapter Pattern

3- Observer Pattern

4- Decorator Pattern

5- Iterator Pattern

6- Factory Pattern

Clasificándose los patterns en los siguientes grupos:

- Behavioral Patterns: iterator, observer and strategy pattern

- Structural Patterns: adapter and decorator pattern

- Creational Patterns: factory method

## Tabla de Contenidos

- [Programming Foundations: Design Patterns](#programming-foundations-design-patterns)
	- [Tabla de Contenidos](#tabla-de-contenidos)
	- [Certificado](#certificado)
	- [Patterns](#patterns)
		- [1-Strategy Pattern](#1-strategy-pattern)
			- [Ejemplo](#ejemplo)
				- [Ejecución:](#ejecución)
		- [2-Adapter Pattern](#2-adapter-pattern)
			- [Ejemplo](#ejemplo-1)
		- [3-Observer Pattern](#3-observer-pattern)
			- [Ejemplo](#ejemplo-2)
		- [4-Decorator Pattern](#4-decorator-pattern)
			- [Ejemplo](#ejemplo-3)
				- [Ejecución:](#ejecución-1)
		- [5-Iterator Pattern](#5-iterator-pattern)
			- [Ejemplo](#ejemplo-4)
				- [Ejecución:](#ejecución-2)
		- [6-Factory Pattern](#6-factory-pattern)
			- [Ejemplo](#ejemplo-5)
				- [Ejecución:](#ejecución-3)

## Certificado

![Certificado Finalización](./certificado.webp)

## Patterns

### 1-Strategy Pattern

Definition

> This pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. This lets the algorithm vary independently from clients that use it.

Principles used: **Use composition rather than inheritance**, **Encapsulate what varies** and **Program to an interface, not an implementation**. Approach HAS-A rather than IS-A.

#### Ejemplo

Se implementa strategy pattern para variar los métodos share que posean objetos creados de subclases que heredan de una clase abstracta Phone Camera App.

Creación de clase abstracta **PhoneCameraApp**:

```java
public abstract class PhoneCameraApp {
	ShareStrategy shareStrategy;

	public void setShareStrategy(ShareStrategy shareStrategy) {
		this.shareStrategy = shareStrategy;
	}
	public void share() {
		shareStrategy.share();
	}
	public void take() {
		System.out.println("Taking the photo");
	}
	public void save() {
		System.out.println("Saving the photo");
	}
	public abstract void edit();
}
```

Creación de subclase **CameraPlusApp** que hereda de clase abstracta **PhoneCameraApp**:

```java
public class CameraPlusApp extends PhoneCameraApp {
	public void edit() {
		System.out.println("Extra snazzy photo editing features");
	}
}
```

Creación de interfaz ShareStrategy:

```java
public interface ShareStrategy {
	public void share();
}
```

Creación de subclases que implementan interfaz ShareStrategy:

```java
public class Email implements ShareStrategy {
	public void share() {
		System.out.println("I'm emailing the photo");
	}
}
public class Social implements ShareStrategy {
	public void share() {
		System.out.println("I'm posting the photo on social media");
	}
}

public class Txt implements ShareStrategy {
	public void share() {
		System.out.println("I'm txting the photo");
	}
}
```

##### Ejecución:

En base a lo ingresado por el usuario en pantalla se asigna método sharing:

```java
public class PhotoWithPhone {

	public static void main(String[] args) {

		PhoneCameraApp cameraApp = new BasicCameraApp();
		String share = getSharing();
		switch (share) {
			case("t"): cameraApp.setShareStrategy(new Txt()); break;
			case("e"): cameraApp.setShareStrategy(new Email()); break;
			case("s"): cameraApp.setShareStrategy(new Social()); break;
			default: cameraApp.setShareStrategy(new Txt());
		}
		cameraApp.take();
		cameraApp.edit();
		cameraApp.save();
		cameraApp.share();
	}
	public static String getSharing() {
		Scanner scanner = new Scanner(System.in);
		System.out.println("Share with txt (t), email (e), or social media (s)?");
		String appName = scanner.next();
		scanner.close();
		return appName;
	}
}
```

### 2-Adapter Pattern

Definition:

> This pattern converts the interface of a class into another interface that clients expect. It allows classes to work together that couldn't otherwise because of incompatible interfaces.

Principles used: **Use composition rather than inheritance**

#### Ejemplo

Se desea hacer que un dron implemente los métodos de la interfaz Duck que es la siguiente:

```java
public interface Duck {
	public void quack();
	public void fly();
}
```

Los drones se crean de las clases que implementan la siguiente interfaz **Drone**:

```java
public interface Drone {
	public void beep();
	public void spin_rotors();
	public void take_off();
}
```

Las clases que implementan las interfaces ya sea Drone ni Duck NO deben ser modificados. Aplicando el adapter pattern se crean una clase **DroneAdapter** que implementa la interfaz **Duck** siguiente:

```java
public class DroneAdapter implements Duck {
	Drone drone;

	public DroneAdapter(Drone drone) {
		this.drone = drone;
	}

	public void quack() {
		drone.beep();
	}

	public void fly() {
		drone.spin_rotors();
		drone.take_off();
	}
}
```

Dicha clase recibe un objeto de una clase que implementa la interfaz Drone (objeto de type Drone) e implementa los métodos **quack** y **fly** de la interfaz **Duck** pero ahora adaptados en base al objeto Drone de type Drone.

### 3-Observer Pattern

Definition:

> This pattern defines a one-to-many dependency between objects so that when object changes state. all of its dependents are notified and updated automatically

Principles used:**Loose coupling** entre publisher y observers

#### Ejemplo

Se implementa Observer Pattern al registrar observadores o clientes que reciben (son notificados) weather data. En primer lugar, se implementa interfaz **Subject** la cual contiene las definiciones de métodos que debe poseer la clase que notifica a observadores:

```java
public interface Subject {
	public void registerObserver(Observer o);
	public void removeObserver(Observer o);
	public void notifyObservers();
}
```

Posteriormente, se crea clase **WeatherData** que implementa la anterior interfaz:

```java
public class WeatherData implements Subject {
	private ArrayList<Observer> observers;
	private float temperature;
	private float humidity;
	private float pressure;

	public WeatherData() {
		observers = new ArrayList<Observer>();
	}

	public void registerObserver(Observer o) {
		observers.add(o);
	}

	public void removeObserver(Observer o) {
		int i = observers.indexOf(o);
		if (i >= 0) {
			observers.remove(i);
		}
	}

	public void notifyObservers() {
		for (Observer observer : observers) {
			observer.update(temperature, humidity, pressure);
		}
	}

	public void measurementsChanged() {
		notifyObservers();
	}

	public void setMeasurements(float temperature, float humidity, float pressure) {
		this.temperature = temperature;
		this.humidity = humidity;
		this.pressure = pressure;
		measurementsChanged();
	}

	public float getTemperature() {
		return temperature;
	}

	public float getHumidity() {
		return humidity;
	}

	public float getPressure() {
		return pressure;
	}

}
```

Luego se procede a definir la interfaz de los observadores:

```java
public interface Observer {
	public void update(float temp, float humidity, float pressure);
}
```

Se procede a crear clase que implementa la interfaz Observer de data de Forecast y Statistics. Cuando la temperatura cambia en la clase **WeatherData**, se llama a método **measurementsChanged** el cual notifica a los observadores del cambio.

```java
public class ForecastDisplay implements Observer, DisplayElement {
	private float currentPressure = 29.92f;
	private float lastPressure;
	private WeatherData weatherData;

	public ForecastDisplay(WeatherData weatherData) {
		this.weatherData = weatherData;
		weatherData.registerObserver(this);
	}

	public void update(float temp, float humidity, float pressure) {
        lastPressure = currentPressure;
		currentPressure = pressure;

		display();
	}

	public void display() {
		System.out.print("Forecast: ");
		if (currentPressure > lastPressure) {
			System.out.println("Improving weather on the way!");
		} else if (currentPressure == lastPressure) {
			System.out.println("More of the same");
		} else if (currentPressure < lastPressure) {
			System.out.println("Watch out for cooler, rainy weather");
		}
	}
}


public class StatisticsDisplay implements Observer, DisplayElement {
	private float maxTemp = 0.0f;
	private float minTemp = 200;
	private float tempSum= 0.0f;
	private int numReadings;
	private WeatherData weatherData;

	public StatisticsDisplay(WeatherData weatherData) {
		this.weatherData = weatherData;
		weatherData.registerObserver(this);
	}

	public void update(float temp, float humidity, float pressure) {
		tempSum += temp;
		numReadings++;

		if (temp > maxTemp) {
			maxTemp = temp;
		}

		if (temp < minTemp) {
			minTemp = temp;
		}

		display();
	}

	public void display() {
		System.out.println("Avg/Max/Min temperature = " + (tempSum / numReadings)
			+ "/" + maxTemp + "/" + minTemp);
	}
}
```

El método display se implementa en las clases display debido a que dichas clases implementan la interfaz **DisplayElement**:

```java
public interface DisplayElement {
	public void display();
}
```

### 4-Decorator Pattern

Definition:

> This pattern attaches additional responsabilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality

Principles used: **Open-closed**, **Use composition rather than inheritance**

#### Ejemplo

Se aplica decorator pattern en agregados (adicionales) a objetos creados de clases que extienden de la clase abstracta **Pizza** lo que permite generar una flexibilidad al momento de adicionar agregados. La clase abstracta Pizza es la siguiente:

```java
public abstract class Pizza {
	String description = "Basic Pizza";

	public String getDescription() {
		return description;
	}
	public abstract double cost();
}
```

Y se crean las siguientes pizzas especificas que extienden de la clase Pizza:

```java
public class ThickcrustPizza extends Pizza {

	public ThickcrustPizza() {
		description = "Thick crust pizza, with tomato sauce";
	}

	public double cost() {
		return 7.99;
	}
}
public class ThincrustPizza extends Pizza {

	public ThincrustPizza() {
		description = "Thin crust pizza, with tomato sauce";
	}

	public double cost() {
		return 7.99;
	}
}
```

Se crea decorator que extiende de la clase abstracta Pizza:

```java
public abstract class ToppingDecorator extends Pizza {
	Pizza pizza;
	public abstract String getDescription();
}
```

Y cada ingrediente adicional debe extender de dicha clase decorator:

```java
public class Cheese extends ToppingDecorator {

	public Cheese(Pizza pizza) {
		this.pizza = pizza;
	}

	public String getDescription() {
		return pizza.getDescription() + ", Cheese";
	}

	public double cost() {
		return pizza.cost(); // cheese is free
	}
}

public class Olives extends ToppingDecorator {


	public Olives(Pizza pizza) {
		this.pizza = pizza;
	}

	public String getDescription() {
		return pizza.getDescription() + ", Olives";
	}

	public double cost() {
		return pizza.cost() + .30;
	}
}
```

##### Ejecución:

Cada ingrediente se compone respecto a la pizza y la descripción y costo se van agregando en la medida que ingredientes se van sumando:

```java
public class PizzaStore {
	public static void main(String args[]) {
		Pizza pizza = new ThincrustPizza();
		Pizza cheesePizza = new Cheese(pizza);
		Pizza greekPizza = new Olives(cheesePizza);

		System.out.println(greekPizza.getDescription()
				+ " $" + greekPizza.cost());
	}
}
```

### 5-Iterator Pattern

Definition:

> This pattern provides a way to access the elements of an aggregate object sequentially **without exposing its underlying representation**

Principles used: **Encapsulate what varies**, **Single Responsability**

#### Ejemplo

Se implementa iterator pattern para recorrer elementos de objetos menú creados a partir de clases que implementan la interfaz **Menu**, dicha interfaz contiene el método **createIterator** el cual retorna un iterator de elementos de tipo **MenuItem**

```java
import java.util.Iterator;

public interface Menu {
	public Iterator<MenuItem> createIterator();
}
```

Se crean clases de menú diferentes que implementan la interfaz Menu y se agrega cuerpo al método **createIterator**. Para el caso de la clase **DinnerMenu** se tiene un iterator personalizado creado desde la clase **DinerMenuIterator** y para el caso de **PancakeHouseMenu** se tiene el iterator personalizado **PancakeHouseMenuIterator**:

```java
import java.util.Iterator;

public class DinerMenu implements Menu {
	static final int MAX_ITEMS = 6;
	int numberOfItems = 0;
	MenuItem[] menuItems;

	public DinerMenu() {
		menuItems = new MenuItem[MAX_ITEMS];

		addItem("Vegetarian BLT",
			"(Fakin') Bacon with lettuce & tomato on whole wheat", true, 2.99);
		addItem("BLT",
			"Bacon with lettuce & tomato on whole wheat", false, 2.99);
		addItem("Soup of the day",
			"Soup of the day, with a side of potato salad", false, 3.29);
		addItem("Hotdog",
			"A hot dog, with saurkraut, relish, onions, topped with cheese",
			false, 3.05);
		addItem("Steamed Veggies and Brown Rice",
			"Steamed vegetables over brown rice", true, 3.99);
		addItem("Pasta",
			"Spaghetti with Marinara Sauce, and a slice of sourdough bread",
			true, 3.89);
	}

	public void addItem(String name, String description,
	                     boolean vegetarian, double price)
	{
		MenuItem menuItem = new MenuItem(name, description, vegetarian, price);
		if (numberOfItems >= MAX_ITEMS) {
			System.err.println("Sorry, menu is full!  Can't add item to menu");
		} else {
			menuItems[numberOfItems] = menuItem;
			numberOfItems = numberOfItems + 1;
		}
	}

	public MenuItem[] getMenuItems() {
		return menuItems;
	}

	public Iterator<MenuItem> createIterator() {
		return new DinerMenuIterator(menuItems);
		//return new AlternatingDinerMenuIterator(menuItems);
	}

	// other menu methods here
}

```

Y se crea la clase **PancakeHouseMenu**:

```java
import java.util.ArrayList;

public class PancakeHouseMenu implements Menu {
	ArrayList<MenuItem> menuItems;

	public PancakeHouseMenu() {
		menuItems = new ArrayList<MenuItem>();

		addItem("K&B's Pancake Breakfast",
			"Pancakes with scrambled eggs, and toast",
			true,
			2.99);

		addItem("Regular Pancake Breakfast",
			"Pancakes with fried eggs, sausage",
			false,
			2.99);

		addItem("Blueberry Pancakes",
			"Pancakes made with fresh blueberries",
			true,
			3.49);

		addItem("Waffles",
			"Waffles, with your choice of blueberries or strawberries",
			true,
			3.59);
	}

	public void addItem(String name, String description,
	                    boolean vegetarian, double price)
	{
		MenuItem menuItem = new MenuItem(name, description, vegetarian, price);
		menuItems.add(menuItem);
	}

	public ArrayList<MenuItem> getMenuItems() {
		return menuItems;
	}

	public Iterator createIterator() {
		return new PancakeHouseMenuIterator(menuItems);
	}

	public String toString() {
		return "Objectville Pancake House Menu";
	}

	// other menu methods here
}

```

El iterator personalizado **DinerMenuIterator** aplicado a la clase **DinerMenu** es el siguiente:

```java
import java.util.Iterator;

public class DinerMenuIterator implements Iterator<MenuItem> {
	MenuItem[] list;
	int position = 0;

	public DinerMenuIterator(MenuItem[] list) {
		this.list = list;
	}

	@Override
	public MenuItem next() {
		MenuItem menuItem = list[position];
		position = position + 1;
		return menuItem;
	}

	@Override
	public boolean hasNext() {
		if (position >= list.length || list[position] == null) {
			return false;
		} else {
			return true;
		}
	}

	@Override
	public void remove() {
		if (position <= 0) {
			throw new IllegalStateException
				("You can't remove an item until you've done at least one next()");
		}
		if (list[position-1] != null) {
			for (int i = position-1; i < (list.length-1); i++) {
				list[i] = list[i+1];
			}
			list[list.length-1] = null;
		}
	}

}
```

Y el iterator personalizado **PancakeHouseMenuIterator** aplicado a la clase **PancakeHouseMenu** es el siguiente:

```java
import java.util.ArrayList;

public class PancakeHouseMenuIterator implements Iterator {
	ArrayList<MenuItem> items;
	int position = 0;

	public PancakeHouseMenuIterator(ArrayList<MenuItem> items) {
		this.items = items;
	}

	@Override
	public MenuItem next() {
		MenuItem item = items.get(position);
		position = position + 1;
		return item;
	}

	@Override
	public boolean hasNext() {
		if (position >= items.size()) {
			return false;
		} else {
			return true;
		}
	}

	@Override
	public void remove() {
    throw new UnsupportedOperationException("Remove operation not supported for PancakeHouseMenu");
  }
}

```

##### Ejecución:

Para la ejecución se crea la clase **Waitress**. Dicha clase llama al método createIterator el cual define iteradores y los recorre aplicando los métodos respectivos de las clases Iterator:

```java
public class Waitress {
	Menu pancakeHouseMenu;
	Menu dinerMenu;

	public Waitress(Menu pancakeHouseMenu, Menu dinerMenu) {
		this.pancakeHouseMenu = pancakeHouseMenu;
		this.dinerMenu = dinerMenu;
	}

	public void printMenu() {
		Iterator pancakeIterator = pancakeHouseMenu.createIterator();
		Iterator dinerIterator = dinerMenu.createIterator();

		System.out.println("MENU\n----\nBREAKFAST");
		printMenu(pancakeIterator);
		System.out.println("\nLUNCH");
		printMenu(dinerIterator);

	}

	private void printMenu(Iterator iterator) {
		while (iterator.hasNext()) {
			MenuItem menuItem = iterator.next();
			System.out.print(menuItem.getName() + ", ");
			System.out.print(menuItem.getPrice() + " -- ");
			System.out.println(menuItem.getDescription());
		}
	}

	public void printVegetarianMenu() {
		printVegetarianMenu(pancakeHouseMenu.createIterator());
		printVegetarianMenu(dinerMenu.createIterator());
	}

	public boolean isItemVegetarian(String name) {
		Iterator breakfastIterator = pancakeHouseMenu.createIterator();
		if (isVegetarian(name, breakfastIterator)) {
			return true;
		}
		Iterator dinnerIterator = dinerMenu.createIterator();
		if (isVegetarian(name, dinnerIterator)) {
			return true;
		}
		return false;
	}


	private void printVegetarianMenu(Iterator iterator) {
		while (iterator.hasNext()) {
			MenuItem menuItem = iterator.next();
			if (menuItem.isVegetarian()) {
				System.out.print(menuItem.getName());
				System.out.println("\t\t" + menuItem.getPrice());
				System.out.println("\t" + menuItem.getDescription());
			}
		}
	}

	private boolean isVegetarian(String name, Iterator iterator) {
		while (iterator.hasNext()) {
			MenuItem menuItem = iterator.next();
			if (menuItem.getName().equals(name)) {
				if (menuItem.isVegetarian()) {
					return true;
				}
			}
		}
		return false;
	}
}

```

El código de ejecución es el siguiente:

```java
public class MenuTestDrive {
	public static void main(String args[]) {
        PancakeHouseMenu pancakeHouseMenu = new PancakeHouseMenu();
        DinerMenu dinerMenu = new DinerMenu();

		Waitress waitress = new Waitress(pancakeHouseMenu, dinerMenu);
		waitress.printMenu();

	}
}
```

### 6-Factory Pattern

Definition:

> This pattern defines an interface for creating an object, but lets subclases decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses

Principles used: **Encapsulate what varies**

#### Ejemplo

Se implementa Factory Pattern el cual crea una clase factory encargada de crear una instancia específica de un grupo de tipos de pizzas (Cheese, Clam, Pepperoni y Veggie). Luego en la clase **PizzaStore** se crea la pizza la cual puede ejecutar métodos de la clase abstracta **Pizza**. Se ha aplicado el principio de encapsular lo que varía que en este caso son los tipos de pizza y lo que no varía se ha dejado fijo en la clase abstracta como método que se ejecuta cuando se ha creado una pizza desde la factory.

En primer lugar, se crea la clase abstracta **Pizza**:

```java
import java.util.ArrayList;

abstract public class Pizza {
	String name;
	String dough;
	String sauce;
	ArrayList<String> toppings = new ArrayList<String>();

	public String getName() {
		return name;
	}

	public void prepare() {
		System.out.println("Preparing " + name);
	}

	public void bake() {
		System.out.println("Baking " + name);
	}

	public void cut() {
		System.out.println("Cutting " + name);
	}

	public void box() {
		System.out.println("Boxing " + name);
	}

	public String toString() {
		// code to display pizza name and ingredients
		StringBuffer display = new StringBuffer();
		display.append("---- " + name + " ----\n");
		display.append(dough + "\n");
		display.append(sauce + "\n");
		for (String topping : toppings) {
			display.append(topping + "\n");
		}
		return display.toString();
	}
}
```

Para luego crear la clase factory de Pizzas:

```java
public class SimplePizzaFactory {

	public Pizza createPizza(String type) {
		Pizza pizza = null;

		if (type.equals("cheese")) {
			pizza = new CheesePizza();
		} else if (type.equals("pepperoni")) {
			pizza = new PepperoniPizza();
		} else if (type.equals("clam")) {
			pizza = new ClamPizza();
		} else if (type.equals("veggie")) {
			pizza = new VeggiePizza();
		}
		return pizza;
	}
}

```

Luego en la clase **PizzaStore** se crea la pizza de determinado tipo en la cual se pueden ejecutar métodos que no varian definidos en la clase **Pizza**:

```java
public class PizzaStore {
	SimplePizzaFactory factory;

	public PizzaStore(SimplePizzaFactory factory) {
		this.factory = factory;
	}

	public Pizza orderPizza(String type) {
		Pizza pizza;

		pizza = factory.createPizza(type);

		pizza.prepare();
		pizza.bake();
		pizza.cut();
		pizza.box();

		return pizza;
	}

}

```

##### Ejecución:

En la ejecución se muestra la creación de una factory la cual se asigna a la store. Dicha store puede crear pizzas a partir de la factory y cuando la pizza se crea, es posible ocupar métodos estándar o generales:

```java

public class PizzaTestDrive {

	public static void main(String[] args) {
		SimplePizzaFactory factory = new SimplePizzaFactory();
		PizzaStore store = new PizzaStore(factory);

		Pizza pizza = store.orderPizza("cheese");
		System.out.println("We ordered a " + pizza.getName() + "\n");
		System.out.println(pizza);

		pizza = store.orderPizza("veggie");
		System.out.println("We ordered a " + pizza.getName() + "\n");
		System.out.println(pizza);
	}
}
```

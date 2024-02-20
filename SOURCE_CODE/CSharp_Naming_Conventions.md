
# C# Naming Conventions for Developers

## Naming Conventions

### Naming Conventions: File
Filename should follow below conventions:
- Follow the **PascalCase** naming convention
- Filename should match the class name. Class named **OrderBook** will have a file named as **OrderBook.cs**
- Interfaces MUST start with a capital **I**
```
OrderBook.cs
IOrderBook.cs
```

### Naming Conventions: Variable
Depending on the type of variable, the naming convention differs..
- Public global variables follow the **PascalCase** naming convention
- Private global variables follow the **camelCase** naming convention
- Local variables follow the **camelCase** naming convention
- Private variables start with a **_** and follow the **camelCase**

```
public class OrderBook : IOrderBook
{
	public List<Order> OrderList;
	private List<Order> _myOrderList;

	public OrderBook(List<Order> orderList, string userId)
	{
		OrderList = orderList;
		var userOrders = orderList.Where(x => x.UserId == userId);
		_myOrderList = userOrders;
	}
}
```

### Naming Conventions: Constant
Constants should follow the **PascalCase** regardless of whether they are defined as locally or globally .

```
const MAX_NUMBER_OF_THREADS = 6;
```

### Naming Conventions: Boolean
A prefix like `is` , `are` , `has` helps developer to distinguish a boolean from another variable by just looking at it.

```
bool isVisible = true;
```

### Naming Conventions: Methods
In C#, functions are written in **PascalCase**. Method arguments are written in **camelCase** similar to local variables.

```
GetOrder(string userId)
DeleteOrder(int orderId)
CreateOrder(Order order)
```

### Naming Conventions: Class and Interfaces
- Classes and interfaces should be defined with **PascalCase**.
- Classes and interfaces should be named as defined in the filename. For example, a class named **OrderBook** will have a file name **OrderBook.cs**
- Interfaces MUST start with a capital **I** and so should their file name. For example, an interface named **IOrderBook** will have a file name **IOrderBook.cs**

```
public interface IOrderBook 
{

}

public class OrderBook 
{

}
```
### Parentheses Style
It is recommended to use the Allman parentheses because they help with code comprehension.
 
Allman Parentheses
```
if(isVisible)
{

}
```

### Consistency is the key!!
The key to following conventions is consistency. You MUST ensure that you define and stick to a particular set of conventions.

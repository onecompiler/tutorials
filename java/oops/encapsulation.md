Encapsulation is a mechanism to protect private hidden from other users. It wraps the data and methods as a single bundle. `private` is the keyword used to declare the variables or methods as private. You can make public `set` and `get` methods to access private variables.

```java
public class Employee{
    public static void main(String args[]){
         EmployeeDetails o = new EmployeeDetails();
         o.setname("foo");
         o.setempId(12345);
         o.setsalary(20000);
         System.out.println("Employee Name: " + o.getname());
         System.out.println("Employee ID: " + o.getempId());
         System.out.println("Employee Salary: " + o.getsalary());
    } 
}

class EmployeeDetails{
    private String name;
    private int empId;
    private int salary;

    //Getter and Setter methods
    public int getempId(){
        return empId;
    }

    public String getname(){
        return name;
    }

    public int getsalary(){
        return salary;
    }

    public void setempId(int newId){
        empId = newId;
    }

    public void setname(String newName){
        name = newName;
    }

    public void setsalary(int newSalary){
        salary = newSalary;
    }
}
```
## Practice with more examples [here](https://onecompiler.com/java)


Polymorphism gives the meaning many forms, usually it occurs when multiple classes are present and have been inherited.

Consider an example of installing an application in your mobile, where your base class is `Mobile` and method is `installation()` and its derived classes could be installApp1, installApp2, installApp3 etc which will have its own implementation but installation method can be common which can be inherited from its base class.

```java
class News {
  public void alerts() {
    System.out.println("News Alerts");
  }
}

class Sports extends News {
  public void alerts() {
    System.out.println("subscribe Sports News");
  }
}

class Entertainment extends News {
  public void alerts() {
    System.out.println("Subscribe for Entertainment News");
  }
}

class Politics extends News {
  public void alerts() {
    System.out.println("Subscribe for politics alerts");
  }
}


class MainClass {
  public static void main(String[] args) {
    News myNews = new News(); 
    Sports mySports = new Sports();
    Entertainment myEntertainment = new Entertainment();
    Politics myPolitics = new Politics();
    myNews.alerts();
    mySports.alerts();
    myEntertainment.alerts();
    myPolitics.alerts();
  }
}
```

## Practice with more examples [here](https://onecompiler.com/java)

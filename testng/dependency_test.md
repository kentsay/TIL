# TestNG Dependency Test

In TestNG, we use dependOnMethods and dependsOnGroups to implement dependency testing. If a dependent method is fail, all the subsequent test methods will be skipped, NOT failed.

### dependOnMethods

```java
import org.testng.annotations.Test;

public class App {

	@Test
	public void method1() {
		System.out.println("This is method 1");
	}

	@Test(dependsOnMethods = { "method1" })
	public void method2() {
		System.out.println("This is method 2");
	}

}

```

### dependsOnGroups

Very userful for module testing. See follow examples to show how to use TestNG group dependency testing:

```java
import org.testng.annotations.Test;

//all methods of this class are belong to "deploy" group.
@Test(groups="deploy")
public class TestServer {

	@Test
	public void deployServer() {
		System.out.println("Deploying Server...");
	}

	//Run this if deployServer() is passed.
	@Test(dependsOnMethods="deployServer")
	public void deployBackUpServer() {
		System.out.println("Deploying Backup Server...");
	}
	
}

```

```java
import org.testng.annotations.Test;

public class TestDatabase {

	//belong to "db" group, 
	//Run if all methods from "deploy" group are passed.
	@Test(groups="db", dependsOnGroups="deploy")
	public void initDB() {
		System.out.println("This is initDB()");
	}

	//belong to "db" group,
	//Run if "initDB" method is passed.
	@Test(dependsOnMethods = { "initDB" }, groups="db")
	public void testConnection() {
		System.out.println("This is testConnection()");
	}

}
```

```java
import org.testng.annotations.Test;

public class TestApp {

	//Run if all methods from "deploy" and "db" groups are passed.
	@Test(dependsOnGroups={"deploy","db"})
	public void method1() {
		System.out.println("This is method 1");
		//throw new RuntimeException();
	}

	//Run if method1() is passed.
	@Test(dependsOnMethods = { "method1" })
	public void method2() {
		System.out.println("This is method 2");
	}

}
```

To trigger the above test suit, create a `testing.xml` file and put them together:
```xml
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >

<suite name="TestDependency">
  <test name="TestCase1">
	<classes>
	  <class 
		name="com.mkyong.testng.examples.dependency.TestApp">
	  </class>
	  <class 
		name="com.mkyong.testng.examples.dependency.TestDatabase">
	  </class>
	  <class 
		name="com.mkyong.testng.examples.dependency.TestServer">
	  </class>
	</classes>	
  </test>
</suite>
```
Out put:
```
Deploying Server...
Deploying Backup Server...
This is initDB()
This is testConnection()
This is method 1
This is method 2

===============================================
TestDependency
Total tests run: 6, Failures: 0, Skips: 0
===============================================
```
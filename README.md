# Spring-SpringBoot-Notes

Types of Coupling:
1. Tight Coupling: Tight Coupling occurs when classes are highly dependent on each other. Changing one class often required changed in another. This reduces flexibility and makes the code harder to maintain and test.

public class Demo{

	public static void main(String[] args){

		Animal obj1 = new Animal();
		obj1.task();
	
	}	

}


public class Animal{
	Dog dog = new Dog();
	public void task(){
		dog.eat();
	}
}


public class Dog{

	public void eat(){
		System.out.println("Dog Eating....')
	}	

}

2. Loose Coupling: It minimized dependencies between classes. Changing in one class do not heavily impact others.
public class Demo {

    public static void main(String[] args) {
        Animal obj1 = new Dog(); 
        obj1.task();
    }
}

public interface Animal {
    void task(); 
}

public class Dog implements Animal { 

    @Override
    public void task() {
        System.out.println("Dog Eating...."); 
    }
}


3. 

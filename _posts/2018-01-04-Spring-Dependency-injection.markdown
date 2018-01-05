---
layout: post
title:  "Different types of Bean Injection in Spring"
date:   2018-01-04 00:00:00 +0530
categories: java spring
author: "abhishek shah"
---



## 1. Overview

In this short guide, we'll look into the different types of Bean Injection in Spring. On broader terms, there are basically two types of injection available :

* Setter Injection
* Constructor Injection

Spring applications can be configured in three ways :

* XML
* Annotations
* Java

We'll look into Dependency Injection using XML and Annotation configuration.

## 2. Project set up

Add the following maven dependency :

```
<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>5.0.2.RELEASE</version>
</dependency>
```

And, let us create few classes - *model, repository* and a *service* :

```
public class Customer {

	private String firstName;
	private String lastName;

}
```

```
public class CustomerRepositoryHibernate implements CustomerRepository {

    public List<Customer> findAll() {

        List<Customer> customers = new ArrayList<Customer>();
        Customer customer = new Customer();
        customer.setFirstName("John");
        customer.setLastName("Doe");

        customers.add(customer);

        return customers;
    }

}
```

```
public class CustomerServiceImpl implements CustomerService {

    private CustomerRepository customerRepository;

    public List<Customer> getAllCustomers() {
        return customerRepository.findAll();
    }

}
```

## 2. Spring XML Configuration

We should use XML configuration when we want configuration code outside our application code. The following points highlight the usage of XML configuration :

* It was the first approach
* very simple to use
* separation of concerns - configuration out of code

### 2.1. Setter Based Dependency Injection using XML Configuration

In, *application context* file :

```
<bean name="customerRepository"
   		 class="com.baeldung.repository.CustomerRepositoryHibernate"/>

<bean name="customerService"
   		 class="com.baeldung.service.CustomerServiceImpl" >
   		<property name="customerRepository" ref="customerRepository"></property>
</bean>
```

We have defined two beans, namely, and we are injecting *customerRepository* into *customerService bean*. To use setter injection we declare a property tag :

* Attribute *name* should be the identifier of the property we want to inject
* Attribute *ref* should be passed with a bean name we want to inject

### 2.2. Constructor Based Dependency Injection using XML Configuration

In, *application context* file :

```
<bean name="customerRepository"
   		 class="com.baeldung.repository.CustomerRepositoryHibernate"/>

<bean name="customerService"
   		 class="com.baeldung.service.CustomerServiceImpl" >
   		<constructor-arg index="0" ref="customerRepository"/>
</bean>
```

We use the *Constructor* tag for constructor based dependency injection :

* Attribute *index* is the argument position in the constructor
* And *ref* is the name of the bean we would like to pass in the constructor

In this case, we are passing *customerRepository* bean name as *ref* at index 0.

## 3. Spring Configuration using Annotations - XML

Spring Configuration using annotations helps to see how are our code is tied up. The following points highlight the usage of annotations :

* We can see when and where dependency is injected
* Easier to understand
* Configure while coding

There are 3 types of class level bean annotations Spring provides, these are known as *Stereotype Annotations* :

* *@Component* - any bean can be annotated with this
* *@Repository* - is a subtype of *@Component*, preferably used with data layer repository class
* *@Service* - is a subtype of *@Component*, preferably used with service class

Spring provides with one more additional way of dependency injection other than setter and constructor :

* Member variable injection

Spring provides with an *@Autowired* annotation to inject dependency. To enable this, let us tell Spring where to find our Stereotype Beans.

In application context file:

```
<context:annotation-config/>
<context:component-scan base-package="com.baeldung"/>
```

We are letting Spring search all the sub-packages and classes inside *com.baeldung* package to find Stereotype beans and register them in Spring context.

### 3.1. Setter Based Dependency Injection using Annotations

We will use *@Autowired* over a setter method to inject dependency :

```
@Autowired
 public void setCustomerRepository(CustomerRepository custRepository){
        this.customerRepository = custRepository;
}
```

### 3.2. Constructor Based Dependency Injection using Annotations

We will use *@Autowired* over field constructor to inject dependency :

```
@Autowired
 public CustomerServiceImpl(CustomerRepository customerRepository) {
        super();
        this.customerRepository = customerRepository;
}

```

### 4. Conclusion

We have seen there are primarily two types of dependency injection in Spring - setter and constructor based. Member variable Injection gets introduced with the usage of *@Autowired* annotation.

We still need application context file even if we use annotation based injections. Spring also provides an option to completely remove the usage of application context file. In such case, all configuration is written inside a Java class file.

All the code written here can be accessed at GitHub [here](https://github.com/ashah2012/scalable-search-engine).

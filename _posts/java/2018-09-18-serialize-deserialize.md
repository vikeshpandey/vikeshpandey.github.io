---
layout: post
title:  "How to serialize/deserialize objects in java"
date:   2018-09-18 20:33:57 +0530
categories: java
permalink: /:categories/serialize-deserialize.html
---

In this tutorial, we are going to learn how to serialize and de-serialize java objects to/from json.

For serialization to json, there are multiple libraries provided which can be used for this purpose.
Though the most widely used libraries are [jackson](https://github.com/FasterXML/jackson), [gson](https://github.com/google/gson)
and [json-simple](https://github.com/fangyidong/json-simple).

`json-simple`'s github repo has been inactive since 2014 so that goes out of comparison right away (You don't want to 
use a dead library !!). `jackson` is quite popular but in terms of performance, `gson` scores better. 
Hence in this article, we are going to focus on `gson`.

Let us quickly get down to action. Below are examples of java<->json using `gson`.

+ First of all, we need to include `gson` jar into our project. If you are using any dependency management tool like `maven`
 or `gradle`, you can include the `gson jar` in the following way:
  
  `gradle`:
  ````
  dependencies {
    implementation 'com.google.code.gson:gson:2.8.5'
  }
  ````
  
  `maven`:
  ````
  <dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.8.5</version>
  </dependency>
  ````
  If you are working in native style, you can download the latest `gson` jar from [here](https://mvnrepository.com/artifact/com.google.code.gson/gson)
  
  PS: Whichever way you include `gson`, please ensure to use latest version.
  
+ Next thing, let us quickly create a sample java class and try to convert it into json.
  ````
    public class Employee {
        private int id;
        private String name;
        private List<Address> addresses;

        public Employee(final int id, final String name, final List<Address> addresses) {
            this.id = id;
            this.name = name;
            this.addresses = addresses;
        }

        @Override
        public String toString() {
            return "[id = " + id + ", name = " + name + ", address = [" + addresses + "]]";
        }
    }
    
    //And the Address class looks like this:
    public class Address {
        private String city;
    
        public Address(final String city) {
            this.city = city;
        }
    
        @Override
        public String toString() {
            return "city = " + city;
        }
  ````
  Few things to note:
    + fields are marked `private`, you can also mark them `public` but it is just poor design.
    + Fields are initialized in constructor. If you don't like that, you can simply get rid of the constructor and just have `setter` for each field.
    + The class looks pure `POJO` with zero influence of `gson`.
    + The `toString` method is added purely to print the object nicely on the console.

+ Next thing, we would initialize the Gson class, like:
  ````
   Gson gson = new Gson();
  ````
   OR
  ````
   Gson gson = new GsonBuilder().create();
  ````  
  Though, the latter one gives you more control over `gson` object. You can toggle features like, null serialization and many more using the latter one.

+ `gson` provides an intuitive method `toJson` to convert a java object TO `json`. Let us create object of `Employee` class
   and supply that object in the `toJson` method, like:
   ````
    //prepare the employee object
    Address temporaryAddress = new Address("New York");
    Address permanentAddress = new Address("New Jersey");
    List<Address> addresses = new ArrayList<Address>(2);
    addresses.add(temporaryAddress);
    addresses.add(permanentAddress);
    
    Employee employee = new Employee(1, "John Doe", addresses);
    
    // json-ify the object
    Gson gson = new Gson();
    String json = gson.toJson(employee);
    System.out.println("json string is : " + json); 
   ````      
   This gives the output as:
   ````
    json string is : {"id":1,"name":"John Doe","addresses":[{"city":"New York"},{"city":"New Jersey"}]}
   ````
   As we can see, the object has been successfully converted to json.

+ Now let us convert the json back to java object. To do that, again, `gson` provides a naturally obvious named method `fromJson`
  to get a java object FROM `json`. The code for it looks like:
  ````
   Employee convertedFromJson = gson.fromJson(json, Employee.class);
   System.out.println("Employee object created from json : " + convertedFromJson);
  ````
  This gives us output as:
  ````
   Employee object created from json : [id = 1, name = John Doe, address = [[city = New York, city = New Jersey]]]
  ````
  
  The reason we see the employee object being nicely printed is due to `toString` implementation provided. We can see 
  that the object has been successfully converted back to java.
  <br/>
  <br/>

This concludes our tutorial explaining conversion of java object to/from json using `gson`. For downloading the complete code
of the above example, please go to this [link](https://github.com/vikeshpandey/serialize-deserialize).

If you want to explore more advanced features of `gson`, please visit this [link](https://github.com/google/gson/blob/master/UserGuide.md)  

If you have questions or feedback, please reach out to me on my email or on my github page.

Happy Learning !!
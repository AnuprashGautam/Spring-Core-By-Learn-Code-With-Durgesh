# Spring Config File Boilerplate

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<beans xmlns="http://www.springframework.org/schema/beans" 
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
       xmlns:context="http://www.springframework.org/schema/context" 
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context 
                           http://www.springframework.org/schema/context/spring-context.xsd">  
	
	
	<!-- This is our bean for Student class. -->
	
    <bean class="com.springcore.Student" name="student1">
    	<property name="studentId">
    		<value>22344</value>
    	</property>
    	
    	<property name="studentName">
    		<value>Anuprash Gautam</value>
    	</property>
    	
    	<property name="studentAddress">
    		<value>Delhi</value>
    	</property>
    </bean>

</beans>
```
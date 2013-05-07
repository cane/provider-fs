Provider fs
===========

Provides the ability to operate the file system, Based file system for large blocks of data to provide key/value storage.
 
Restrictions:
-------------
* The maximum length of a single file can not exceed Integer.MAX_VALUE.
* You can store files in each directory in the maximum number determined by the file system itself.
* Allow the number of open files at the same time how much is determined by the runtime operating system.

Transaction support:
--------------------
Simulation file system transaction support, but is not guaranteed completely reliable transaction.

----

Usage:
------

	FsProvider fp = new FsProvider();
	FsResourceProvider rp = new FsResourceProvider("/var/data/");
	FsEntityFactory factory = (FsEntityFactory)fp.getFactory("default", rp);
	
	Entity e = factory.get(...);
	...
	
#### For Spring:
	<bean id="fsResProvider" class="org.canedata.provider.fs.FsResourceProvider">
		<property name="path">
			<value>/var/data/</value>
		</property>
	</bean>
	<bean id="fsProvider" class="org.canedata.provider.fs.FsProvider">
	</bean>
	<bean id="fsFactory" class="org.canedata.provider.fs.entity.FsEntityFactory"
		factory-bean="fsProvider" factory-method="getFactory" singleton="true">
		<constructor-arg type="java.lang.String" value="default"></constructor-arg>
		<constructor-arg ref="fsResProvider"></constructor-arg>
	</bean>

	<bean id="photo" class="*.PhotoServiceImpl">
		<property name="fsFactory" ref="fsFactory"></property>
	</bean>
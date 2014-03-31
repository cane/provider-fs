Provider fs
===========

Provides the ability to operate the file system, Based file system for large blocks of data to provide key/value storage.
 

#### Restrictions:
* The maximum length of a single file can not exceed Integer.MAX_VALUE, use the stream except(use `FsWritableField` not `put('body', in)`).
* You can store files in each directory in the maximum number determined by the file system itself.
* Allow the number of open files at the same time how much is determined by the runtime operating system.

#### Transaction support:
Simulation file system transaction support, but is not guaranteed completely reliable transaction.
Isolation level currently is restricted to read committed.

	Entity e1 = fac.get(...);
	Entity e2 = fac.get(...);
	
	e1.transaction();
	e1.put(...).update(...);
	e2.delete(...);
	
 	e1.commit();
 	//equivalent to:
	e2.commit();
 
#### Readable fields:
* name `String`
* parent `File`
* path `String`
* canread `boolean`
* canwrite `boolean`
* canexecute `boolean`
* freespace or free `long`
* totalspace or total `long`
* usablespace or space `long`
* isdir `boolean`
* isfile `boolean`
* ishidden `boolean`
* lastmodified or date `long`
* length or size `long`
* uri `String`
* body or content `byte[]`
 
#### Writable fields:
* body or content `byte[]`
* executable `boolean`
* readable `boolean`
* readonly `boolean`
* writable `boolean`

----

Usage:
------

	CaneProvider fp = new FsProvider();
	ResourceProvider rp = new FsResourceProvider("/var/data/");
	EntityFactory factory = fp.getFactory("default", rp);
	
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
	
#### CRUD:

	Entity e = factory.get(...);
	
	e.put("body", "test").create("testOne");//or
	e.field("body").set("test").create();//Use UUID as the file name.
	
	Fields f = e.restore("testOne");
	
	e.put("body", "updated").update("testOne");
	e.put("body", " end").opt(Options.APPEND, true)
		.updateRange(e.expr().equals("name", "f-0")
		.or().equals("name", "f-1"));
										
	e.delete("testOne");
	e.deleteRange(e.expr().equals("name", "cane")
		.or().equals("name", "hib"));
	e.deleteRange(e.expr().like("name", "f-"));
	
	
	
	
	
	
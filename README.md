linguine-maps
=============

/*
	Linguine Maps Programmatic Visualization Library
	Copyright (C) 2005 Pavel Simakov
	http://www.softwaresecretweapons.com

	This library is free software; you can redistribute it and/or
	modify it under the terms of the GNU Lesser General Public
	License as published by the Free Software Foundation; either
	version 2.1 of the License, or (at your option) any later version.

	This library is distributed in the hope that it will be useful,
	but WITHOUT ANY WARRANTY; without even the implied warranty of
	MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
	Lesser General Public License for more details.

	You should have received a copy of the GNU Lesser General Public
	License along with this library; if not, write to the Free Software
	Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301, USA
*/


Version Info
============

Version: 1.4
Last build date: April 4, 2006
Release date: April 4, 2006


About this Package
==================

This package contains full Java source code for programmatic visualization of:
- Apache ANT build files
- Document Type Definition (DTD) for XML documents
- Object Relation Bridge mapping files
- Hibernate mapping files

You can use this package to perform programmatic visualization of any of the above files.
You could run programmatic visualization from command line or integrate it into a build process. 
We provided full set of Apache ANT tasks for  integrating programmatic visualization with the build.
This package also contains demo applications that shows you how to visualize abstract 
graphs.


Samples images included with Linguine Maps
==========================================

There is a very large number of visualizations available from our web site at 
http:/www.softwaresecretweapons.com. This includes many official schema files from 
leading software development teams:

ANT Build files:
* Apache Tomcat 5.0.25 start up script
* Apache Object Relational Bridge 1.0.0 build
* Hibernate 3.0 Ant build

XML DTD Files:
* XML DTD for a Connector 1.0 resource adapter (raa) module
* XML DTD for the JDO 1.0 Metadata
* XML DTD for the JavaServer Faces 1.1 Application Configuration File
* XML DTD for a Servlet 2.3 web-app (war) modulec
* XML DTD Hibernate 2.0 
* XML DTD Hibernate 3.0 
* XML DTD Apache Object Relational Bridge 1.0.0

OJB mapping files
* ObJectBridge 1.0.0 mapping of classes used in junit tests
* ObJectBridge 1.0.0 mapping of classes used in junit tests and tutorials
* ObJectBridge 1.0.0 mapping of classes used in testing references

Hibernate mapping files
* HIBERNATE - Relational Persistence for Idiomatic Java (mappings from Chapter 24)
* HIBERNATE Mapping files shipped with Hibernate 3.0 release

Files in this package
=====================

This package contains:

\bin 
	Graphviz 2.4 binary files for Windows platform

\dist
	jar file with classes from this package
	
\doc
	sample image files produced by this package

\lib
	jar files needed for this package

\src
	all source files for this package


Changes from release 1.3
========================

- file names are not double quoted for Linux platform
- ANTtoGRAPH did not set proper node caption for "depends" and "antcall"
- added HBMCtoGIFTask.java to allow processing Hibernate org.hibernate.cfg.Configuration from Ant

Changes from release 1.2
========================

- added ability to specify multiple XML files for hbm2gif task
- added support for visualization of Hibernate org.hibernate.cfg.Configuration object via com.oy.shared.lm.ext.HBMtoGRAPH

Changes from release 1.1
========================

- added WSDL visualization
- added support for XML namespaces; name spaces are used only for WSDL visualization 
- added new Ant task attribute "qualifiedNames" to strip package name from names of classes; default value is "true"
- added new Ant task attribute "expandEntityRef" to allow processing of XML ENTITY; default value is "false"
- ANTtoGRAPH was updated to group together targets called via <ant>
- grouped derived classes with super classes (OJB and Hibernate) to improve layout
- added concept of "group" for "node"; nodes that belong to the same group are clustered together
- added handling for <ant> that used to show up as empty box
- fixed depends="" showed up as empty box
- runme.xml did not work since distribution was missing dtd files in src\com\oy\shared\lm\test\dtd folder
- added "hollow diamond" arrow tail type
- added SMIL 1.0 DTD sample
- added double quotes around file names that contain spaces before forking dot.exe


Changes from release 1.0
========================

- properly escaped header, footer, caption for "\n"
- improved handling of several elements in HBM schema
- added stereotype to HBM collections
- allowed for out put file in any format supported by Graphviz
- moved all samples from GIF to PNG file format to save space
- added check for root XML elements before visualization begins
- added more samples
- added options to hide details; usefule for large DTD's and E/R mappings


How to use this package with Hibernate 3.0 org.hibernate.cfg.Configuration object
=================================================================================

Starting with Linguine Maps 1.2 it is possible to create entity-relation diagrams by 
directly accessing object-Hibernate 3.0 relational mapping information provided by 
org.hibernate.cfg.Configuration. A class com.oy.shared.lm.ext.HBMtoGRAPH should be 
used for this purpose.

By default com.oy.shared.lm.ext.HBMtoGRAPH is excluded from buil.xml. Please define 
"ext" property with value "true" to enable its compilation. Compilation also requires 
"hibernate3.jar" in the "lib-ext" folder. 

Following example illustrates how to create entity-relation diagrams using an instance 
org.hibernate.cfg.Configuration:

	private void sampleFoo() throws Exception {
						
		// create hbm configuration object, add one (or more) mapping file
		Configuration conf = new Configuration();
		conf.addFile("c:/test/foo.hbm.xml");
		conf.buildMappings();
				
		// set caption and colors
		TaskOptions opt = new TaskOptions();
		opt.caption = "Foo Test Diagram";
		opt.colors = "#FF5937, black, blue";
		
		// create generic graph model
		IGraphModel graph = HBMtoGRAPH.load(opt, conf);
				
		// convert graph model into GIF (PNG, JPG, SVG, etc.) image file
		GRAPHtoDOTtoGIF.transform(
			graph, "c:/test/foo.dot", "c:/test/foo.gif", "c:/test/bin/graphviz/bin.dot.exe"
		);
	}
 
In order to run this example above the following files should be available on the classpath:
	- all of the class files required by your hbm.xml schema
	- hibernate3.jar
	- commons-collections-2.1.1.jar
	- commons-logging-1.0.4.jar
	- dom4j-1.6.jar
	- junit-3.8.1.jar
	- oy-lm-1.2.jar


How to use this package from ANT build files
============================================

There are several tasks for programmatic visualization of various files. You can define
custom tasks as follows:

<taskdef name="ant2gif" classname="com.oy.shared.lm.ant.ANTtoGIFTask" classpath="${dist}/lib/oy-lm-1.2.jar"/>	
<taskdef name="ojb2gif" classname="com.oy.shared.lm.ant.OJBtoGIFTask" classpath="${dist}/lib/oy-lm-1.2.jar"/>	
<taskdef name="hbm2gif" classname="com.oy.shared.lm.ant.HBMtoGIFTask" classpath="${dist}/lib/oy-lm-1.2.jar"/>	
<taskdef name="dtd2gif" classname="com.oy.shared.lm.ant.DTDtoGIFTask" classpath="${dist}/lib/oy-lm-1.2.jar"/>	
<taskdef name="wsdl2gif" classname="com.oy.shared.lm.ant.WSDLtoGIFTask" classpath="${dist}/lib/oy-lm-1.2.jar"/>	

After new tasks have been defined you can use them in the Ant build files, for example:

	<ant2gif 
		caption="ObJectBridge 1.0.0 ANT build configuration."
		rotated="true"
		colors="cyan, lightcyan, orange, black, black"
		inFile="${src}/com/oy/shared/lm/test/ant/db-ojb-1.0.0-build.xml" 
		dotFile="${doc}/test/ant/db-ojb-1.0.0-build.dot" 
		outFile="${doc}/test/ant/db-ojb-1.0.0-build.gif"
		exeFile="${dot.exe}"
	/>

All tasks have identical set of parameters, with following usage:

    inFile - qualified name for the input file (depends on the task); required;
    dotFile - qualified name for the intermediate output file; required
    outFile - qualified name for the images output file; required; can end with .gif, .png, .jpg
    exeFile - qualified name for the Graphviz 2.4 executable; required
    colors - comma separated list of colors (depends on the task); optional (can use #rrggbb format)
    includes - comma separated list of entity names that are included into the diagram; optional
    excludes - comma separated list of entity names that are excluded from the diagram; optional
    rotated - "true|false"; controls page orientation; optional; default "false"
    detailed - "true|false"; controls amount of details in the diagram; optional; default "true"    
    caption - title of the diagram; optional
    fontName - font name for the diagram; optional; default "Helvetica"
    fontSize - base font size for the diagram; optional; default "10"
    attr - custom Graphviz attributes for the diagram; optional
    nodeAttr - custom Graphviz attributes for the nodes; optional
    edgeAttr - custom Graphviz attributes for the edges; optional
	qualifiedNames - "true|false"; strips package names class from names; default "true"; optional
	expandEntityRef - "true|false"; forces processing of XML ENTITY; default "false"; optional
    
A "hbm2gif" task in addition to a inFile attribute supports nested <fileset> 
elements. In this case a single diagram is produced reflecting contents of all mapping 
files contained in the <fileset>. For example:

	<hbm2gif  
		caption="HIBERNATE - Animal &amp; Beings"
		rotated="true"
		colors="#FF5937, black, black"
		dotFile="${doc}/test/hbm/AnimalBeings.dot" 
		outFile="${doc}/test/hbm/AnimalBeings.png"
		exeFile="${dot.exe}"
	>
		<fileset dir="${src}/com/oy/shared/lm/test/hbm/">
		  	<include name="Animal.hbm.xml"/>
			<include name="Beings.hbm.xml"/>
		</fileset>
	</hbm2gif>
    
Please review runme.xml files for more examples of use of this tasks and parameters.

How to use this package from Java projects
==========================================

Linguine Maps provides clean object-oriented diagramming API for graph visualization.
We have a sample that shows how to construct diagram from Java code at runtime.
Please review class com.oy.shared.lm.test.TextAll. You can also review the source for 
the numerous Ant tasks that conduct programmatic visualization for several file 
formats.


Relation to Graphviz
====================

Linguine Maps is object-oriented wrapped around Graphviz. A set of official binaries
of Graphviz 2.4 is included with this distribution. Linguine Maps allows you to focus
on programmatic visualization of graphs without worrying about format of Graphviz dot 
files. Linguine Maps operates with objects that represent nodes and edges in the diagram.
A formal representation of a drawing is converted into Graphviz behind the scenes.


Motivation for this Package
============================

Do you how long it takes to learn new DTD, XML schema, or an object model for an 
object-relational mapping? It actually takes very long time to learn! Some of these 
files are 20 pages or longer. If you want to take a quick glance at them - forget it!
It will take serious effort to make it through...

This is why we have created this package. This package conducts programmatic visualization
of various schema files. The parsers will process your source files and will create 
an easy to understand entity-relation diagrams. With a diagram it will take you minutes 
now to get familiar with new schemas. And you can always go back to source files when more 
details are needed.

Linguine Maps is written in Java and provides clean object-oriented diagramming API for 
graph visualization. We developed full set of Apache ANT tasks for integrating programmatic 
visualization with the build process. This package also contains demo applications that 
shows you how to visualize abstract graphs. 
- Apache ANT build files; for these files we draw task dependency diagrams
- Document Type Definition (DTD) for XML documents; for these files we draw relations 
between various entities and their attributes
- Object Relation Bridge mapping files; for these files we draw UML-style class
diagrams
- Hibernate mapping files; for this files we draw UML-style class diagrams

Programmatic visualization is very effective communication tool for a software 
development teams. It can be attached to a build process and it helps to keep 
documentation up to date automatically. All members of your development team 
will have a common set of visual documents, constructed automatically from the 
source code. 

It is well known that developers only trust the source code. All diagrams 
produced by the Linguine Maps are precise reflection of the source code! They 
will be trusted and accepted by your development team. 

Please enquire further if you need help with similar projects.
 
Regards,


Dr. Pavel Simakov
Email:  <psimakov@softwaresecretweapons.com>
WWW:    http://www.softwaresecretweapons.com
---
layout: post
title:  Java Refresh
categories: [blog ]
tags: [study,programing,merge,read,calendar, ]
description: These exercises are about the use of files, collections and dates/times in Java.
--- 

**Exercise 1**
Write a Java program that:
a) loads the rows of a text file called dictionary.txt, using a Collection (you are free to
choose the specific type to use) of String objects (one object per row);
b) reads the rows of a second text file, whose name is passed as argument on the command line, and
writes the rows of the second file that do not occur in the first one to standard output.

2016-10-06-programming-type.md
    /*
     * Date: 10/10/2016
     * Objective: loads the rows of a text file called dictionary.txt, using a Collection (you are free to
    choose the specific type to use) of String objects (one object per row);
     * Idea: BufferReader
     * */
    import java.io.*;
    import java.util.ArrayList;
    
    public class LoadFile1 {
    
    	private String fileName;
    	private String filePath;
    	ArrayList<String> strings;
    	
    	public LoadFile1(String fileName, ArrayList<String> strings){
    		this.fileName = fileName;
    		this.filePath = fileName;
    		this.strings = strings;
    	}
    	
    	public ArrayList<String> read() throws IOException{
    		strings = new ArrayList<String >();
    		FileInputStream fin = null;
    		int i = 0;
    		try {
    			fin = new FileInputStream(filePath);
    		} catch (IOException e) {
    			System.out.println(e.toString()+"!!!");
    			System.out.println("File " + fileName + " not found");
    		}
    		
    		DataInputStream in = new DataInputStream(fin);
    		BufferedReader br = new BufferedReader(new InputStreamReader(in));
    		String strLine = null;
    		
    		while ((strLine = br.readLine())!=null) {
    			strings.add(strLine);
    			System.out.println("line " + i + " is " + strLine);
    			i++;
    		}
    		
    		in.close();
    		
    		System.out.println("the size of the array is " + strings.size());
    		return strings;
    	} 
    }
    
    
**Exercise 2**
Write a Java program that reads a text file whose name is assigned to the system property
it.polito.dp2.file and whose rows are each made up of a series of six integers representing
a time instant in the local time zone (the integers are, respectively, hour, minute, second, day, month
and year). Such data must be loaded into objects of type GregorianCalendar. After loading,
the program must print the most recent date to standard output.
In order to read a system property you can use the System.getProperty() method like in the
following example:
// Get the "it.polito.dp2.file" system property
String dir = System.getProperty("it.polito.dp2.file");
A system property can be set or overridden by specifying the -D option to the java command when
running your program, as in the following example:
$ java -Dit.polito.dp2.file="filename" MyApp

    /* finish date 14/10/2016
     * idea: set system property using  System.setProperty("1", dir);(1st para:key, 2nd para:value, which is file name here)
     * or use command line to input like following
     * Z:\Documents\dp\JavaRefresh\src>java -D1="Z:\Documents\dp\JavaRefresh\it.polito.
    dp2.file.txt" Test
    * read the property which is like reading a file
    * add to the Calendar list then use compareTo method to compare to get most recent date
    */
    import java.io.IOException;
    import java.time.LocalDateTime;
    import java.util.ArrayList;
    import java.util.Calendar;
    import java.util.Collections;
    import java.util.Comparator;
    import java.util.Date;
    import java.util.GregorianCalendar;
    import java.util.TimeZone;
    import java.util.function.Function;
    import java.util.stream.Collectors;
    
    public class PrintDate {
    
    	private String fileName;
    	private String path;
    	private ArrayList<String> strings = new ArrayList<String >();
    	private LoadFile2 loadFile2;
    	
        public PrintDate(String fileName, String path) {
    		this.fileName = fileName;
    		this.path = path;
    		loadFile2 = new LoadFile2(fileName, path, strings);
    	} 
    	public void loadProperty() throws IOException {
    		// loadFile2.read();
    		
    		String dir ="it.polito.dp2.file.txt";// = System.getProperty("it.polito.dp2.file");
    		// System.out.println("sYSTEM PROPERTY IS :" + dir);
    		
    		String[] split;
    		int hour;
    		int year;
    		int day;
    		int month;
    		int minutes;
    		int second;
    	
    		// Calendar calendar = new GregorianCalendar(TimeZone.getTimeZone("GMT"));
    		Calendar calendar;
    		// GMT?
    		ArrayList<Calendar> cal = new ArrayList<Calendar >();
    		int i =0;
    		System.out.println("sYSTEM PROPERTY value IS :" + dir);
    		dir = System.getProperty("1");
    		LoadFile1 loadFile1 = new LoadFile1(dir,  strings);
    		System.out.println("hdad"+ strings.size());
    		loadFile1.read();
    		System.out.println("hdadsdasa"+ strings.size());
    		for (String item: strings) {
    			System.out.println("hdad");
    			item = item.replaceAll("\\s+", "");
    			split =item.split(",");
    			hour = Integer.parseInt(split[0]);
    			minutes = Integer.parseInt(split[1]);
    			second = Integer.parseInt(split[2]);
    			day = Integer.parseInt(split[4]);
    			month = Integer.parseInt(split[3]);
    			year = Integer.parseInt(split[5]);
    			calendar = new GregorianCalendar(year, month, day, hour, minutes, second);
    			System.out.println("hour is "+split[0]+" Calendar hour is "+calendar.HOUR_OF_DAY);
    		    cal.add(calendar);
    		    // System.out.println(cal.get(i));
    		    i++;
    		}
    		// ?how to get most recent one? what is exact value of cal array
    	
    		Calendar temp = cal.get(0);
    		for (Calendar c:cal) {
    			if (temp.compareTo(c) <= 0)
    				temp = c;
    		}
    		
    		System.out.println("the most recent date is:" + temp.get(Calendar.HOUR_OF_DAY) + ":" + temp.get(Calendar.MINUTE) + ":" +  
    				temp.get(Calendar.SECOND) + " " + temp.get(Calendar.DAY_OF_MONTH) + "_" + temp.get(Calendar.MONTH) + "_" 
    				+ temp.get(Calendar.YEAR)) ;
    	}
    }
    
**Exercise 3**
Write a Java program that reads two files, and merges them into a single third file with the lines
ordered in ascending alphabetic order.
The names of the files must be read from the command line.

    /*
     * idea: compareTo() compares two string with regard to their alphabetical order.
     * */
    import java.io.IOException;
    import java.util.ArrayList;
    import java.util.Collections;
    
    public class Merge {
    
    	private String fileName1;
    	private String fileName2;
    	private ArrayList<String> strings1 = new ArrayList<String >(); 
    	private ArrayList<String> strings2 = new ArrayList<String >(); 
    	private LoadFile1 loadFile1;
    	private LoadFile1 loadFile2;
    	
    	public Merge(String fileName1, String fileName2) {
    		this.fileName1 = fileName1;
    		this.fileName2 = fileName2;
    		loadFile1 = new LoadFile1(fileName1, strings1);
    		loadFile2 = new LoadFile1(fileName2,  strings2);		
    	}
    	
    	public void sort() throws IOException {
    		strings1 = loadFile1.read(); //why must do in this way, then the strings will have value?
    		strings2 = loadFile2.read();
    		ArrayList<String> strings3 = new ArrayList<String>(strings1);
    		strings3.addAll(strings2);
    		
    		Collections.sort(strings3, String.CASE_INSENSITIVE_ORDER);
    		
    		for (String s : strings3) {
    			System.out.print(s + " ");
    		}
    	}
    }
  
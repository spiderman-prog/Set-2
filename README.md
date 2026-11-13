# Set-2
8.Develop a swing application using JList, JTree, JTable, JTabbedPane and JScrollPane.

import javax.swing.*;
import javax.swing.event.*;
import javax.swing.tree.DefaultMutableTreeNode;
import java.awt.*;
public class SwingComponentsDemo extends JFrame {
private JLabel listLabel; public SwingComponentsDemo() {
setTitle("Swing Components Demo");
setSize(700, 500);
setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
JTabbedPane tabbedPane = new JTabbedPane();
JPanel listPanel = new JPanel(new BorderLayout()); 
String[] fruits = {"Apple", "Banana", "Mango", "Orange", "Grapes"}; 
JList<String> jList = new JList<>(fruits); 
listLabel = new JLabel("Select an item from the list");
jList.addListSelectionListener(new ListSelectionListener() { 
public void valueChanged(ListSelectionEvent e) {
if (!e.getValueIsAdjusting()) {
listLabel.setText("Selected Item: " + jList.getSelectedValue()); 
} } });
listPanel.add(new JScrollPane(jList), BorderLayout.CENTER);
listPanel.add(listLabel, BorderLayout.SOUTH);
treePanel = new JPanel(new BorderLayout());
DefaultMutableTreeNode root = new DefaultMutableTreeNode("Courses");
DefaultMutableTreeNode javaNode = new DefaultMutableTreeNode("Java"); 
javaNode.add(new DefaultMutableTreeNode("Swing"));
javaNode.add(new DefaultMutableTreeNode("JDBC"));
DefaultMutableTreeNode pythonNode = new DefaultMutableTreeNode("Python");
pythonNode.add(new DefaultMutableTreeNode("Django")); 
pythonNode.add(new DefaultMutableTreeNode("Flask"));
root.add(javaNode); 
root.add(pythonNode);
JTree tree = new JTree(root); 
treePanel.add(new JScrollPane(tree), BorderLayout.CENTER);
tablePanel = new JPanel(new BorderLayout());
String[] columns = {"ID", "Name", "Department"};
Object[][] data = {
{"101", "John", "CSE"},
{"102", "Alice", "ECE"}, 
{"103", "David", "IT"},
{"104", "Sophia", "ME"},
{"105", "James", "EEE"},
{"106", "Emma", "CSE"},
{"107", "Robert", "IT"},
{"108", "Olivia", "ECE"} };
JTable table = new JTable(data, columns);
JScrollPane tableScrollPane = new JScrollPane(table);
tablePanel.add(tableScrollPane, BorderLayout.CENTER);
tabbedPane.addTab("JList", listPanel); 
tabbedPane.addTab("JTree", treePanel); 
tabbedPane.addTab("JTable", tablePanel);
add(tabbedPane); 
setVisible(true);
}
public static void main(String[] args) { 
SwingUtilities.invokeLater(()-> new SwingComponentsDemo());
} }

9.Write a Java program for a simple calculator using GridLayout.

import javax.swing.*;
import java.awt.*; 
import java.awt.event.*;
public class SimpleCalculator extends JFrame implements ActionListener { 
JTextField num1Field, num2Field, resultField;
JButton addBtn, subBtn, mulBtn, divBtn; 
public SimpleCalculator() { 
setTitle("Simple Calculator"); 
setSize(400, 250);
setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); 
setLayout(new GridLayout(4, 2, 10, 10));
add(new JLabel("First Number:"));
num1Field = new JTextField();
add(num1Field);
add(new JLabel("Second Number:"));
num2Field = new JTextField(); 
add(num2Field); 
addBtn = new JButton("+"); 
subBtn = new JButton("-");
mulBtn = new JButton("*");
divBtn = new JButton("/"); 
addBtn.addActionListener(this);
subBtn.addActionListener(this);
mulBtn.addActionListener(this);
divBtn.addActionListener(this); 
JPanel buttonPanel = new JPanel(new GridLayout(1, 4));
buttonPanel.add(addBtn); 
buttonPanel.add(subBtn); 
buttonPanel.add(mulBtn); 
buttonPanel.add(divBtn); 
add(buttonPanel);
add(new JLabel("Result:")); 
resultField = new JTextField(); 
resultField.setEditable(false);
add(resultField); setVisible(true);
} 
public void actionPerformed(ActionEvent e) {
try {
if (num1Field.getText().trim().isEmpty() | num2Field.getText().trim().isEmpty()) { 
resultField.setText("Error: Enter both numbers");
return;
} 
double num1 = Double.parseDouble(num1Field.getText());
double num2 = Double.parseDouble(num2Field.getText()); 
double result = 0;
if (e.getSource() == addBtn) { 
result = num1 + num2;
} 
else if (e.getSource() == subBtn) { 
result = num1- num2; 
}
else if (e.getSource() == mulBtn) {
result = num1 * num2;
} 
else if (e.getSource() == divBtn) { 
if (num2 == 0) {
resultField.setText("Error: Division by zero"); 
return;
} 
result = num1 / num2;
} 
resultField.setText(String.valueOf(result));
}
catch (NumberFormatException ex) { 
resultField.setText("Error: Invalid input");
} } 
public static void main(String[] args) { 
new SimpleCalculator(); 
} }

10.Write a servlet to a) handle parameter passing (employee login page) 

1)HOME.html:
<html> 
 <head>
  <title>login form</title>
 </head>
 <body>
  <form method="post" action="LOGIN">
   Email ID:<input type="text" name="email"  required=""/><br/> 
   Password:<input type="text" name="pass" required="" /><br/> 
   <input type="submit" value="login" /> 
  </form> 
 </body>
</html>
2)index.html: 
<!DOCTYPE html>
<!-- Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license Click nbfs://nbhost/SystemFileSystem/Templates/JSP_Servlet/Html.html to edit this template -->
<html> 
 <head>
  <title>TODO supply a title</title> 
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0"> 
 </head> 
 <body> 
  <div>TODO write content</div> 
 </body>
</html> 

3)Welcome.html :
<html>
 <head> 
  <title>login form</title> 
 </head> 
 <body> 
  <form method="post" action="Welcome">
   <h1>LOGIN SUCCESS</h1> 
   phone number:<input type="text" name="abc" /><br/> 
   <input type="submit" value="submit" />
  </form>
 </body>
</html> 

4)Web.xml:
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" version="3.1"> 
<welcome-file-list> 
<welcome-file>HOME.html</welcome-file>
</welcome-file-list>  
<servlet> 
<servlet-name>
LOGIN</servlet-name>
<servlet-class>
LOGIN</servlet-class>
</servlet> 
<servlet-mapping>
<servlet-name>
LOGIN</servlet-name>
<url-pattern>
/LOGIN</url-pattern> 
</servlet-mapping>
<servlet>
<servlet-name>
Welcome</servlet-name>
<servlet-class>Welcome</servlet-class>
</servlet> 
<servlet-mapping> 
<servlet-name>Welcome</servlet-name> 
<url-pattern>/Welcome</url-pattern> 
</servlet-mapping>
</web-app>

5) LOGIN.java:
 import java.io.IOException;
import java.io.PrintWriter;
 import javax.servlet.*;
 import javax.servlet.http.*;
 import javax.servlet.annotation.WebServlet;
@WebServlet(urlPatterns = {"/login"})
 public class LOGIN extends HttpServlet {
 protected void doPost(HttpServletRequest request, HttpServletResponse response)
           throws ServletException, IOException {
         response.setContentType("text/html;charset=UTF-8");
          PrintWriter out = response.getWriter();
             String email = request.getParameter("email");
          String pass = request.getParameter("pass");
               if(Validate.checkUser(email, pass))
        {
     RequestDispatcher rs = request.getRequestDispatcher("Welcome.html");
             rs.forward(request, response);
          }
         else         {
        out.println("Username or Password incorrect");
                      RequestDispatcher rs = request.getRequestDispatcher("Home.html");
               rs.include(request, response);
          }      }  }

6) Validate.java:
 import java.sql.*;
public class Validate {
 public static boolean checkUser(String email,String pass)
     {
                boolean st=false;
           try {
          Class.forName("com.mysql.jdbc.Driver");
             Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/login1","root","");            PreparedStatement ps = con.prepareStatement("select * from login where email=? and pass=?");             ps.setString(1, email);
              ps.setString(2, pass);
                          ResultSet rs =ps.executeQuery();
             st = rs.next();
        }
         catch(Exception e) {
             e.printStackTrace();
            }
          return st;                      }

6)Welcome.java:
import java.io.IOException;
import java.io.PrintWriter; 
import java.sql.*; 
import javax.servlet.ServletException; 
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet; 
import javax.servlet.http.HttpServletRequest; 
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.Cookie; 
public class Welcome extends HttpServlet {   
protected void doPost(HttpServletRequest request, HttpServletResponse response)  
throws ServletException, IOException {    
response.setContentType("text/html;charset=UTF-8");    
String email = null,pass=null,username=null,add=null;               
int phno=0;      
Cookie[] cookies = request.getCookies();  
if (cookies != null) {     
for (Cookie cookie : cookies) {   
if (cookie.getName().equals("username")) {  
email = cookie.getValue();       
}                    
if (cookie.getName().equals("pass")) { 
pass=cookie.getValue();              
break;          
}           
}
try (PrintWriter out = response.getWriter()) {
out.println("<!DOCTYPE html>"); 
out.println("<html>");
out.println("<head>");
out.println("<title>Servlet Welcome</title>"); 
out.println("</head>"); out.println("<body>"); 
out.println("<a href=HOME.html><b>LOGOUT</b></a>");
out.println("<h1>email : " + email+ "</h1>");
out.println("</body>"); 
out.println("</html>");
} }} } 

11.Write a Java program to perform JDBC operations (connect, insert, update, delete) 

import java.sql.*;
import java.util.*; 
public class Week11 { 
static final String DB_URL = "jdbc:mysql://localhost:3306/login1"; 
static final String USER = "root";  
static final String PASS = "";
public static void main(String[] args) {      
  try       {      
  Class.forName("com.mysql.jdbc.Driver");    
  Connection conn = DriverManager.getConnection(DB_URL, USER, PASS);   
  Statement stmt = conn.createStatement();    
  Scanner sc =new Scanner(System.in);    
  boolean a=true;     
  while(a)      {     
  System.out.println("entr choice"); 
  System.out.println("i:insert,u:update,d:delete,s:select,e:exit");   
  String ch=sc.next();              
  switch (ch) {    
  case "i":               {          
System.out.println("Inserting records into the table...");     
String email="aaaab",pass="bbbbc",username="ccccd",add="eeeef"; int phno=123; 
PreparedStatement ps=conn.prepareStatement("INSERT INTO login VALUES (?, ?,?, ?, ?)");         ps.setString(1,email);
ps.setString(2,pass); 
ps.setString(3,username);
ps.setInt(5,phno);
ps.setString(4, add);
int i=ps.executeUpdate();    
System.out.println("Inserted records into the table...");  
break;      
}    
case "u":               {  
String sql = "UPDATE login " +    
"SET username = 'yyyyy' WHERE phno in (123)";    
stmt.executeUpdate(sql);     
break;             
}           
case "d":               { 
int result=stmt.executeUpdate("delete from login where phno=123"); 
System.out.println(result+" records affected"); 
break;    
}   
case "s":               {   
ResultSet rs=stmt.executeQuery("select * from login");
while(rs.next()) 
System.out.println(rs.getString(1)+"  "+rs.getString(2)+"  "+rs.getString(3)+" "+rs.getInt(4)+"  "+rs.getString(5)); 
break;     
}       
case "e":               {    
a=false;           
}           
default:           {  
System.out.println("enter correct choice");  
}           }       } }
catch (Exception e) {        
e.printStackTrace();    
}         
} } 

2.Write a Java program to a) check whether a given string is a palindrome or not 

import java.util.Scanner; 
public class PalindromeCheck {
public static void main(String[] args) { 
Scanner sc = new Scanner(System.in);  
System.out.print("Enter a string: "); 
String str = sc.nextLine();    
String reversed = "";     
for (int i = str.length() - 1; i >= 0; i--) { 
reversed += str.charAt(i);     
}      
if (str.equals(reversed)) {      
System.out.println("Palindrome");     
}
else {     
System.out.println("Not Palindrome");     
}       
sc.close(); 
} } 

2b) sort a list of strings using command-line input 

import java.util.Arrays; 
public class StringSort {
public static void main(String[] args) { 
if (args.length == 0) {     
System.out.println("No input provided");    
return;   
}    
Arrays.sort(args);  
for (String str : args) {    
System.out.print(str + " ");  
}     } } 

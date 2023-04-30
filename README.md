Download Link: https://assignmentchef.com/product/solved-cs235-project-3-polymorphism
<br>
<strong>  </strong>

<ol>

 <li>Modify the Person, Student, TeachingAssistant and Instructor classes to support <strong>Polymorphism</strong>.</li>

 <li>Modify Roster to inherit from List and <strong>store pointers to </strong><strong>Person </strong>in a List, where the pointers will actually point to either Student, TeachingAssistant or Instructor</li>

</ol>

<strong>GitHub Classroom assignment link: </strong><strong>https://classroom.github.com/a/hoMvuAFl</strong>

<strong>Implementation – 2 parts:  </strong>

<strong>Part1:  Modify Person and derived class</strong>

Modify the Person class by adding a <strong>pure</strong> virtual function display() with void return type. Each derived class can then override display() to display data specific to the derived object as follows:

<ul>

 <li>Student::display()</li>

</ul>

Sample output:

“<strong><em>first_name_</em></strong> <strong><em>last_name_</em></strong> majors in <strong><em>major_</em></strong> with gpa: <strong><em>gpa_
”</em></strong>

<ul>

 <li>TeachingAssistant::display()
 
<strong>Space, not new line</strong></li>

</ul>

Sample output:

“<strong><em>first_name_</em></strong> <strong><em>last_name_</em></strong> majors in <strong><em>major_</em></strong> with gpa: <strong><em>gpa_</em></strong> working [part-time/full-time] as a <strong><em>ta_role_</em></strong><em>
”</em>




Outputs part-time if <strong><em>hours_per_week_</em></strong> &lt; 8,  otherwise outputs fulltime

<ul>

 <li>Instructor::display()
 
<strong>Space, not new line</strong></li>

</ul>

Sample output:

“<strong><em>first_name_</em></strong>  <strong><em>last_name_ –</em></strong> office: <strong><em>office_</em></strong>, email:

<strong><em>contact_</em></strong>
”




<strong>Part2:  Modify the Roster class </strong>

Modify the Roster class to <strong>inherit from List </strong>and store <strong><u>pointers to Person </u></strong>(You will find the List class with the starter files on the project repo). Roster must have at least (but is not limited to) the following methods:

<ol>

 <li><strong>Roster();</strong> //default constructor for empty roster</li>

 <li>/**parameterized constructor</li>

</ol>

<strong>@pre</strong> the input file is expected to be in csv         (comma separated value) where each line has format:

“id,first_name_last_name,title,derived_data1,derived_data12<strong>
</strong>”        where derived_data1 and derived_data2 are specific to the title,         that is, the private members specific to the derived class, in the         order in which they appear in the derived class’ interface         (for example, if title is ‘student’, derived_data1 will hold         the value for major_ and derived_data2 the value for gpa_)

<strong>@param</strong> input_file_name the name of  the input csv file

<strong>@post</strong> Pointers to Person-derived objects are stored in the Roster,
 
each pointer pointing to either a Student, Instructor or
 
TeachingAssistant object as per the data in the input file and
 
specified by the title field

**/

<strong>Roster(</strong><strong>std</strong><strong>::</strong><strong>string</strong><strong> input_file_name);</strong>




Note that there is a lot going on here. For each line in the input file it will:

<ul>

 <li>Instantiate an object of type Student, TeachingAssistant or Instructor, as indicated by the title field (the fourth field on each line), with possible values in {student, teaching_assistant, instructor}</li>

 <li><strong>Derived-class specific data</strong> (gpa_, major_, hours_per_week_, ta_role, office_, contact_), are provided in the input file as the two last data fields, derived_data1 and derived_data2, and they will correspond to the derived class indicated by the title Teaching assistants are also students. For this project you can <u>assume that all TAs are Computer Science majors and have a 4.0</u> <u>gpa.</u></li>

 <li>Add a pointer of type Person that points to the newly instantiate object to the Roster</li>

</ul>

<ol start="3">

 <li>~Roster(); //destructor</li>

</ol>

<strong> </strong>

Recall that, with inheritance, <u>derived class destructors are called first and base</u> <u>class destructors are always called next</u>. The Roster class will store pointers to dynamically allocated objects. The List class constructor does not know about this and will only delete the dynamically allocated Nodes, but not the objects pointed to by the pointers stored in the Nodes. The Roster destructor must delete the dynamically allocated Person-derived objects. Don’t worry bout efficiency, just delete the Person-derived objects and the List destructor will take care of deleting the nodes.

<ol start="4">

 <li>For the same reason that we need to provide a destructor, we need to <em>override</em> remove, to delete the Person-derived dynamic object as well as the node. Just copy the implementation of List::remove() from the List class and <u>modify it to</u> <u>delete the Person-derived objects</u> before it deletes the node. Roster is not a template and T will be Person*.</li>

</ol>

/**

<strong>@param </strong>position indicating point of deletion

<strong>@post </strong>node at position is deleted, if any. Roster order is retained

<strong>@return </strong>true if there is a node at position to be deleted, false otherwise */     <strong>bool</strong> remove(size_t position) <strong>override</strong>;

<ol start="5">

 <li>There will also be a problem with the copy constructor and assignment operator, because the List will make deep copies of the Nodes and pointers stored in the nodes, but not of the dynamic Person-derived objects. To not over-complicate the project, <u>I will not ask you to implement a copy constructor and assignment</u> <u>operator</u> for the Roster class (doing so would require further modifications to Person and derived as well). Instead, your Roster class should <u>indicate/enforce that</u> <u>a Roster cannot be copied</u>. You can do so by <em>deleting</em> these methods in your interface, this will cause a compiler error if anyone using your class tries to make a copy (think of the implications about passing by value etc.). The syntax is as below (you add this only in the interface) and it also serves as documentation.</li>

</ol>




// prevent copy to avoid shallow copies of Person-derived objects Roster(<strong>const</strong> Roster&amp; a_roster) = <strong>delete</strong>;




// prevent copy to avoid shallow copies of Person-derived objects

Roster&amp; <strong>operator</strong>=(<strong>const</strong> Roster&amp; a_roster) = <strong>delete</strong>;




<ol start="6">

 <li>/**<strong>@post</strong> displays all data in roster, on e per line as per each object’s specific display method<strong>n</strong>”</li>

</ol>

**/     <strong>void</strong><strong> display(); </strong>

This function will call the display() method of each Person-derived object to display that object’s data. This is where you see Polymorphism in action!

<strong>You may have as many additional private helper methods as you see fit</strong>.

<strong>Testing (<u>Incrementally</u>):                  </strong>

In main() (not for submission) first test your modifications from Part 1. Instantiate objects of type Student, TeachingAssistant and Instructor and make calls to each object’s display() to check that data is displayed correctly.

When Part 1 is implemented and tested, then move on to Part 2. Implement the Roster constructor  and destructor and test it by instantiating a Roster object in main() and test that it reads the correct amount of data items by checking its size. If the Roster does not have the correct size, debug; you must be reading the file incorrectly. Then implement and test remove() and check the size again. Now implement and test Roster::display(). Make sure you are reading the file correctly and that Polymorphism is correctly implemented (each different object type is correctly calling its own version of display()).

A sample input file roster.csv will be available on the project repo for testing.

JavaScript Object Notation (JSON) made easy in C++ (And more)

This takes all the trouble out of working with JSON in C++.  No parsing or formatting of data.  

Just give it a JSON string, point to a JSON file or build it yourself and it gives you a C++ class hierarchy.  Read, update or otherwise manipulate it and the tell it to convert in back into JSON for storing in a file or sending out serially.

Access to parts of the message can be accessed through absolute or relative path names, or pointers like any other C object.

Static functions or the “new” operator can be used to create the hierarchy.  Just remember to delete it when finished.  But, don’t delete and child objects unless you “extract” them from their parent first.  

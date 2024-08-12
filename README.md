
JavaScript Object Notation (JSON) made easy in C++ (And more)

This takes all the trouble out of working with JSON in C++.  No parsing or formatting of data.  

Just give it a JSON string, point to a JSON file or build it yourself and it gives you a C++ class hierarchy.  Read, update or otherwise manipulate it and the tell it to convert in back into JSON for storing in a file or sending out serially.

Access to parts of the message can be accessed through absolute or relative path names, or pointers like any other C object.

Static functions or the “new” operator can be used to create the hierarchy.  Just remember to delete it when finished.  But, don’t delete and child objects unless you “extract” them from their parent first.  
Example in its simplest form the following lines:

    COMap obj( "{\"name\":\"Fred\"}" );
    
    fprintf( stderr, "%s\n", obj.c_str() );

output:
    {
    "name": "Fred"
    }

But if you want a more formatted output:
  
    COMap obj( "{\"name\":\"Fred\"}" );
    
    so = obj.toJsonString();
   
    fprintf( stderr, "%s\n", so->c_str() );
    
    delete so;
    
output:
    {
        "name": "Fred"
    }
Or if we want a minimul string output for a message we might do

	COMap obj( "{\"name\":\"Fred\"}" );
 	
  	so = obj.toCompactJsonString();
   
   	fprintf( stderr, "%s\n", so->c_str() );
    
    	delete so;
     
output:
    {"name":"Fred"}

Of course this is all easy but try this:

  COMap obj("{\"name\":\"Fred's Bait Shop\",\"address\":\"125 W. Main st.\",\"city\":\"Grape Vine\",\"state\":\"Texas\",\"employees\": [{\"name\":\"Fred\",\"position\":\"Owner\",\"age\":53},{\"name\":\"Alice\",\"position\":\"Boss\",\"age\":51}],\"jobs\":{\"Owner\":\"Fred\",\"Boss\":\"Alice\",\"Casher\":\"Ethel\",\"Stock boy\":\"Jim\"}}")
	
 	so = obj.JsonString();
	
 	fprintf( stderr, "%s\n", so->c_str() );
	
 	delete so;

And you will get something like:
    {
        "name": "Fred%60s Bait Shop",
        "address": "125 W. Main st.",
        "city": "Grape Vine",
        "state": "Texas",
        "employees": 
        [
            {
                  "name": "Fred",
                  "position": "Owner",
                  "age": 53
              },
              {
                  "name": "Alice",
                  "position": "Boss",
                  "age": 51
              }
        ],
        "jobs": 
        {
            "Owner": "Fred",
            "Boss": "Alice",
            "Casher": "Ethel",
            "Stock boy": "Jim"
        }
    }
    
Of course that's just I/O  but what if you want to read something like the "city"?  Add the line:
    
    fprintf( stderr, "The City is: %s\n", obj.findElement( "city" )->c_str() );
    
and the additional output will be:
    
    The City is: Grape Vine

Of course this is dangerous.  You might want to use a little checking like the following:
    
    COString *city = obj.findElement( "city" );
    
    if( CppON::IsString( city ) )
    
    {
    
	fprintf( stderr, "The City is: %s\n", cit->c_str() );
    
    } else {
    
	fprintf( stderr, "ERROR: No city found!\n" );
    
    }
    
Or maybe you want to know who owns the bait shop.  You can use this notation to get the owner:
    
    COString *owner = obj.findElement( "jobs.Owner" );
    
	or
    
    COString "owner = obj.findElement( "jobs/Owner" );
    
But maybe you might plan on doing a lot with the jobs list you might want to get a pointer to the whole structure.
    
    COMap     *jobs = obj.findElement( "jobs" );
    
    COString  *owner = NULL;
    
    if( CppON::isMap( jobs ) )
    
    {
    
	owner = jobs->findElement( "Owner" );
    
    }
    
Or maybe you are into using "iterators" you might use
    
    std::map< std::string, CppON *>::iterator it = jobs->find( "Owner" );
    if( jobs.end != it )
    {
	fprintf( stderr, "Owner is %s\n", it->second.c_str() );
    } else {
	fprintf( stderr, "Owner not in list of jobs\n" );
    }
    
NOTE: the call to "findElement" returns a pointer to the C++ object.  Because it is destroyed when the root object is destroyed 
when going out of scope, you do not want to delete it.

  


    
    

    





 

Today i had the HM round of Tekion , 
did not go so well ,

I learnt a lot of things from this experience 

Learning 1  : 

Whenever you are going for an interview dont take it lightly ,  even if you are not going to join the company 
By lightly i dont mean you have to be laser focused all the time , but a day before revise your weak topics , 
like you did not know about 

Learning 2 : 

The Hiring manager asked me to write the update syntax for the inventory reservation system , but i could not write that 
he asked me while am updating the db , what will the db return , if the query suceeds 

Q)  , he asked what will the update command return ,  
I was not clearly able to answer this 

A) Remember it returns the number of affected rows 

What the DB responds with (in general):
When you execute this kind of UPDATE query, the DB does not return the actual rows — it returns the number of rows affected (i.e., how many rows got updated).

🔧 How to capture it in Java:
If you're using JDBC, here's how you can get that number:

java
Copy
Edit
String sql = "UPDATE Inventory SET qty = qty - ? " +
             "WHERE itemId = ? AND distributorId IN (?, ?, ?) AND qty >= ?";

try (PreparedStatement stmt = connection.prepareStatement(sql)) {
    stmt.setInt(1, requestedQty);
    stmt.setString(2, "item1");
    stmt.setString(3, "dc1");
    stmt.setString(4, "dc2");
    stmt.setString(5, "dc3");
    stmt.setInt(6, requestedQty);

    int rowsUpdated = stmt.executeUpdate();

    if (rowsUpdated > 0) {
        System.out.println("✅ Inventory updated successfully for " + rowsUpdated + " rows.");
    } else {
        System.out.println("⚠️ No inventory rows matched the criteria. Maybe not enough stock?");
    }

} catch (SQLException e) {
    e.printStackTrace();
}
💡 TL;DR:
executeUpdate() returns an int representing number of rows updated.

You can use that to check if the update happened (> 0) or not (== 0).



Learning 3 : 

He said the update syntax that i have written is wrong , 
how to correct it 

what is the syntax that i wrote

update invevntory_table 
set qty = Max(qty - requestedQty , 0 ) 
where condition 

update Inventory Table 
SET qty = qty - requestedQty
where itemId = 'item1' and distributorId in ('dc1','dc2','dc3')
and qty >= requestedQty;




Learning 4 : 

He asked me to describe about synchronized keyword , 
i explained him everything well , but i wrote the spelling of synchronized wrong man 

Object o = new Object();
() {


 synchronised(o) {  --------> i should not have made this mistake , i wrote synchronized 
    
    o.wait(); 

}


Learning 5 : He asked me to write Singleton Class , but i wrote it wrong 

  class CustomClass {
    
    private static CustomClass customClass ;

    private CustomClass() {
    }
    
    public CustomClass getInstance() {       --------> i did not add the static keyword in the method signature , now how would someone know 
                                                          if 
           if(customClass == null ) { 
            customClass = new CustomClass();
        }
        return customClass;
    }
    

}


  now imagine if he told me to write thread safe how would i write it ? 
  such negligence is intolerable , you cannot be this casual and expect to perform well in interviews ,

  take no interview lightly ever again 


Behevariol interview questions he asked :- 

1. what was the last feedback your manager gave you ? 
2. tell me about an incident where you left a code better than you found it . Basically refactored it .

  



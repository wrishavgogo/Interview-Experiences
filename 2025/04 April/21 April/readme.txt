Had interviews at paisabazar.com , 

was a weird interview asked me to code idk whatever problem of linkedlIST

its a very very small startup with abt a lean team size of 3 employees  

Ranjeet Jha took my interview 

questions  




function -> accept two parameters 

boolean function(List<String> l1 , String name ) {
	

	int n = l1.size();

	boolean isPresent = false;

	if(l1.contains(name)) {
		isPresent = true;
		l1.removeAll(name);
	}

	return isPresent;
}

class Node {
	
	int val;
	Node next;
}

3 

 2 - >  - > 4 - > 5 
 head 	prev   cur
cur

	prev.next = cur.next
	prev = prev.next 
	cur = cur.next



Node function(Node head , int val ) {
	
	
	Node cur = head;
	Node prev = null;

	while(cur != null ) {

		if(cur.val == val ) {


			if(prev == null ) {
				head = head.next;
				cur.next = null;
				cur = head;
			} else {

				prev.next = cur.next;
				cur = cur.next;


			}
		} else {

			prev = cur;
			cur = cur.next;
		}
	}

	return head;
}

# Week 3 Lab Report
Welcome and congradulations on making into the third week of the CSE 15L course. Today, I, your instructor, will show you the way on how to \
1.)  Create a server that adds strings and prints it on top of itself \
2.) Talk about bugs that have happened during the Lab Session today, and how you could possibly fix it. \
3.) Talk about things that was new overall. 

Are you ready? If so, lets get down to it!

## Part 1 - creating the site
So during week two of the CSE 15L course, we learned about how to make a server during our lab session. This material is very essential in the overall creation of our server.
Head to the [wavelet github](https://github.com/ucsd-cse15l-f22/wavelet) and download it onto your computer. 

Honestly speaking, you will not need the NumberServer.java since you will be recreating something else known as StringServer. \
Create a new file in the same folder called StringServer and import the following. 
```
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;
```

By importing those, we can create ArrayList within the code that can be printed on the server/website.
Next fill the file up with the following code.
```
class Handler implements URLHandler {
    
    ArrayList<String> store = new ArrayList<>();

    public String handleRequest(URI url) {
        String holder = "";
        //used to check if the end of the url path is equivalent to something
        if (url.getPath().equals("/")) {
            // saves it all in holder
            for(int i = 0; i < store.size(); i++){
                holder += store.get(i) + "\n";
            }
            return holder;


        } else {
            System.out.println("Path: " + url.getPath());
            //used to see if the end of the url path is = /add-message and adds the word(s) after the /message
            if (url.getPath().contains("/add-message")) {
                // splits the url and the word with the equal sign
                String[] parameters = url.getQuery().split("=");

                store.add(parameters[1]);
                // saves it all in holder
                for(int i = 0; i < store.size(); i++){
                    holder += store.get(i) + "\n";
                }
                return holder;
                
            }
            return "404 Not Found!";
        }
    }
}


public class StringServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```
The following code uses the information that you use in the url and display them on the text. \
In the command prompt Javac the following:
```
javac Server.java StringServer.java
```
Next, you follow it up with a:
```
java StringServer {whatever port you want}
```
This allows you to start up the server with the program.

Within the url you can type after your port `/add-message?s={whatever message you want}`. Below is an example.
![image](https://i.imgur.com/aRsBrMv.png) 

You can add more and it will look something similar like this. \
![image](https://i.imgur.com/9iFHksY.png) 
![image](https://i.imgur.com/NiCGCw9.png)

### Q/A
**Which methods in your code are called?** \
We have several different methods that does different things that overall contribues to the overall gathering of information. From the `.getPath()`, the program looks at the pathing of the url, and sees what to output. At the default output it just outputs whats currently within the store ArrayList. after checking if the url path `.contains` the /add-message, thats when the system understands that they have to add the value to the store and then have `holder` return whats currently in the store. 


**What are the relevant arguments to those methods, and the values of any relevant fields of the class?** \
In the url, we are greeted with the `https://localhost:1024` and that stand alone only prints out what has been in the store ArrayList. However, once we add the `add-message?s=` with our personal hand crafted message. It saves the input within the ArrayList, then at the end it would be "placed" in holder with a new line every time. That way we are able to print out everything within the store while having new lines to seperate each inputs. 


**How do the values of any relevant fields of the class change from this specific request? If no values got changed, explain why.** \
It is evident that we have used an ArrayList to store all of the values. It isn't until the end of each processes, we store all the values in the ArrayList in a variable called "holder" that ends up printing all the information. The ArrayList takes the input from after the `= split` where it takes in any string that we may say after the equal sign.

## Part 2 - the Bugs from Lab 3

In our ArrayTest.java, we have attempted to test reverseInPlace() method that is found within the ArrayExamples.java folder. However, evidently there were some bugs. \
Our code is the following:
```
@Test
 public void testLongArrayReverse(){
    int[] input1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 10, 9, 8, 7, 6, 5, 4, 3, 2, 1}, input1);
```
So essentially what we get is
`10, 9, 8, 7, 6, 6, 7, 8, 9, 10`
![image](https://i.imgur.com/8oDAL4q.png)
![image](https://i.imgur.com/6vTwy7n.png)
![image](https://i.imgur.com/nOTVZ9R.png)

It was shown that we have expected the array to be reversed, however, we ended up reversing half the array and then re-reversing the half that we have copied. \
The non-failure test that one could do in this situation would be to only do an array with the length of only one. That way no matter how it's "reversed" it will still technically be reversed. \
For example:
```
@Test
 public void testLongArrayReverse(){
    int[] input1 = {1};
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{1}, input1);
 ```
 
 So the fix could probably be something along the lines of making two different variables and twao different arrayssuch as the following.
 ```
 int j = input1.length();
 int[] tempInputArray = new Integer[j];
 for(int i = 0; i < intput1.length(); i++){
    tempInputArray[i] = input1[j];
    j--;
    }
 return tempInputArray;
 ```
 This way instead of forcing a singular array to hold everything, instead we make a second array that returns the proper results.
 
## Part 3 - What's to be learned
 So for Week 2 and Week 3, I actually properly learned how to JUnit test within lab. Additionally, I have learned how to set up servers, whether it's through the `ieng6` or just local. I also learned how to edit so that the servers can server different purposes in general.

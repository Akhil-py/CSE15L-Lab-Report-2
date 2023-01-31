# Part 1
Code for StringServer:

```java
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;

class Handler implements URLHandler {

    String display = "";

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return String.format(display);
        }
        else {
            System.out.println("Path: " + url.getPath());
            if (url.getPath().contains("/add-message")) {
                String[] parameters = url.getQuery().split("=");
                if (parameters[0].equals("s")) {
                    display += parameters[1] + "\n";
                    return String.format(display);
                }
            }
            return "404 Not Found!";
        }
    }
}

class StringServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! " + 
                    "Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```


![image](https://user-images.githubusercontent.com/61783850/215295010-07b22f53-dacb-4acf-a44a-0daaec6db88d.png)

When StringServer is run, it calls its main method. The main method calls the start function in the Server class, which in turn calls createContext and start of the HttpServer class in order to start the web server. 
When the URL is changed from "http://localhost:4202/" to "http://localhost:4202/add-message?s=Hello!", the handleRequest function is called. This function checks to see if the url contains "/add-message". If it does, then the url after the query (after the ? sign, so "s=Hello!") is split from the equal sign into two argument, and each argument is added to the parameters array. This means parameters[0] is s, and parameters[1] is "Hello!". Parameter[0] is checked to verify if it is "s" to ensure correct syntax. Once verified, parameter[1], "Hello!", is appended to the variable `display`, along with "\n" afterwards. Display is then returned. This essentially adds the string we want to display next to the `display` variable along with a new line after it as specified in the writeup. Once returned to the handleRequest function, it displays it on the web server.

![image](https://user-images.githubusercontent.com/61783850/215297636-6dbbaa4a-ee10-4bee-b85b-5f36af41212a.png)

Simillarly, when the url is changed from "localhost:4202/" to "localhost:4202/add-message?s=This is my lab 2 report!", it calls the handleRequest function. It takes the string, "This is my lab 2 report!" after the query and appends it to the `display` variable with "\n" afterwards to create a new line. The updated Display string is then returned to the handleRequest function and displayed on the web server.

# Part 2
In the ArrayExamples class there are two buggy methods, reverseInPlace and reversed.

Here is the bugged code for the ArrayExamples class:

```java
public class ArrayExamples {

  // Changes the input array to be in reversed order
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }

  // Returns a *new* array with all the elements of the input array in reversed
  // order
  static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
    return arr;
  }
}
```

The symptom of reverseInPlace is that it mirrors the right half of the array onto the left instead of reversing the order of the array like it's supposed to be doing.

![image](https://user-images.githubusercontent.com/61783850/215634780-8d291848-fd7c-4215-9d50-3d5cc8e90e9e.png)

This JUnit test shows one test that passes and another test that fails. When input1 (an array with only one element) is reversed we get the result expected because the element reflected upon itself. 
However, when input2 (an array with multiple elements) is reversed, the JUnit test fails as the function did not correctly reverse the array.

The bug lies in the for loop where the elements are "swapped". The for loop is only supposed to run through half the length of the array, swap the leftmost element with the rightmost element, and move in towards the center of the array while repeating the same process.

Here is the correct implementation for reverseInPlace:

```java
  static void reverseInPlace(int[] arr) {
    int temp;
    for(int i = 0; i < arr.length / 2; i += 1) {
      temp = arr[i];
      arr[i] = arr[arr.length - i - 1];
      arr[arr.length - i -1] = temp;
    }
  }
```
  
I fixed this code by introducing a temp variable to hold the leftmost element which will be replaced by the rightmost element. Once this element is replaced, we replace the rightmost element with temp. This correctly swaps both elements. As the for loop iterates, inner elements of the array are swapped until the middle element is reached.
  
Now let's look at the reversed method. This method is supposed to take in an array and return a new array with the elements of the passed array reversed. The symptoms of this method is that it modifies the passed array (which we don't want), and resets all the elements in this array to 0.

![image](https://user-images.githubusercontent.com/61783850/215638090-e6b53f11-1aca-4c5e-97bd-5ff9687d0a49.png)

The JUnit tests show that an empty array yields the expected output, but the array with several elements does not.
The bug is that the method incorrectly copies all the elements of the new array (which by default only have elements that are equal to 0) onto the passed array. This modifies the passed array and resets all its elements to 0. Then instead of returning the new array, the passed array is returned back to the function.

Here is the fixed implementation of reversed:

```java
  static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      newArray[arr.length - i - 1] = arr[i];
    }
    return newArray;
  }
```
I switched the postions of `newArray[arr.length - i - 1]` and `arr[i]` so that now all the elements from `arr[i]` (which is our passed array) is being copied over to `newArray[arr.length - i - 1]` (which is our new array). Then I also returned the newArray instead of the passed array.

# Part 3
I didn't know how to host a local web server. I was impressed by the ability to host a web server on a remote computer in the CSE basement using ssh and access this site from any device. The SearchEngine lab educated me about this and I am fascinated about how much you can control with URL manipulation. I found it really cool that any device connected to my web server could affect it by changing its URL. For example in the NumberServer from week 2, I was able to increment the number of my lab partner's web server from my computer by changing the URL. I also didn't know what each component of the URL meant and about how it affects the output we see. This was the first time I heard about queries (denoted by ?) and about anchors (denoted by #). I now know that the random string of letters/numbers after the '#' in the URL determines where I left off on a webpage or video (so that's how I can share a youtube video link to make it play at a specific time frame!)

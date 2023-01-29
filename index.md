# Part 1
Code for StringServer:
![image](https://user-images.githubusercontent.com/61783850/215294821-d7d975a4-13e8-451f-8082-af6444473a76.png)

![image](https://user-images.githubusercontent.com/61783850/215295010-07b22f53-dacb-4acf-a44a-0daaec6db88d.png)
When StringServer is run, it calls its main method. The main method calls the start function in the Server class, which in turn calls createContext and start of the HttpServer class in order to start the web server. 
When the URL is changed from "localhost:4202/" to "localhost:4202/add-message?s=Hello!", the handleRequest function is called. This function checks to see if the url contains "/add-message". If it does, then the url after the query (after the ? sign, so "s=Hello!") is split from the equal sign. This means parameter[0] is s, and parameter[1] is "Hello!". Parameter[0] is checked to verify if it is "s" to ensure correct syntax. Once verified, parameter[1], "Hello!", is appended to the variable `display`, along with "\n" afterwards. Display is then returned. This essentially adds the string we want to display next to the `display` variable along with a new line after it as specified in the writeup. Once returned to the handleRequest function, it displays it on the web server.

![image](https://user-images.githubusercontent.com/61783850/215297636-6dbbaa4a-ee10-4bee-b85b-5f36af41212a.png)
Simillarly, when the url is changed from "localhost:4202/" to "localhost:4202/add-message?s=This is my lab 2 report!", it calls the handleRequest function. It takes the string, "This is my lab 2 report!" after the query and appends it to the `display` variable with "\n" afterwards to create a new line. The updated Display string is then returned to the handleRequest function and displayed on the web server.

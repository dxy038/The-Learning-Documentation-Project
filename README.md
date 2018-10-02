# Learning.PraveenGorla.org

## Documentation : ## 

**Documentation started:** *02-oct-2018*

*This documentation is about the new learning(s) and issue(s) resolvings on different aspects of academic, professional and personal thoughts.* 

It includes(or/with links): *Research topics(& resources), Programming(issues and code),  Advanced Linux commands, Networking commands, bash script command utilization, Linux packages(& scripts), Machinine learning, text documentation, images, tutorial(links) and gitlab links(Personal repository links).*

Note : *This documentation is mainly intend to reduce time consumption for solving some issues which are already solved/used earlier, This is from my past experience of wasting my time for searching for issues solving* -- by @gorlapraveen

---------------------------------------------------------------------------------------------------

### Advanced Used Linux Commnd - Utilization for scripting:

  **AWK Command:**
        for eaxmple to extract substring in a string .

            Ex: str1 =abcdefgh
  `awk substr($str1,2,4)`:output = `bcdef`











--------------------------------------------------------------------------------------------------
### Bash Scripting - With some used commands
**Arrays in bash script:**

        Ex: arr=(Helloworld)
Prints: for `arr[0]`=`Hello`, for `arr[1]`=`world`

         for : echo ${arr[0]} ${arr[1]}

prints `Hello world`

Extracting a Substring from Variabel:
     
         test="Welcome to the Land of Linux"
         echo "Our variable test is ${#test} characters long"
         test1=${test:0:7}
         test2=${test:15:13}
         test3=${test:0}
         echo $test1
         echo $test2
         echo $test3

Output:

 
        Our variable test is 28 characters long
        Welcome
        Land of Linux
        Welcome to the Land of Linux



--------------------------------------------------------------------------------------------------
### Used Python Scripting :











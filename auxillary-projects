# AUX PROJECT

  

# Shell Scripting

You have been learning about Linux for some time, and it is time to start getting a feel of how to automate some work using Shell Scripts.

Welcome to this set of ten shell scripting tasks! This collection of exercises is designed to help you develop your shell scripting skills and deepen your understanding of the fundamentals of working with the command line. Each of the tasks presented here is carefully crafted to challenge your problem-solving abilities and expose you to a wide range of shell scripting concepts and techniques.

By completing these tasks, you will learn how to take user input, perform basic math operations, work with directories and files, create loops, and interact with external websites. You will also learn how to create backups of files, generate random numbers, and convert temperature units. Moreover, by combining these tasks into a single script, you will learn how to structure your code effectively, use conditional statements and loops to control program flow, and write reusable functions.

This set of tasks offers a fun and challenging way to build your skills and develop your proficiency with the command line. So get ready to dive in, learn something new, and have fun along the way!

### **TASK :**

* * *

Complete all these 10 tasks into a single shell script

```
1. Ask the user for their name and age, and output a message with their name and the year they were born.

2. Create a new directory with a name provided by the user, and navigate into it.

3. List all files in the current directory, sorted by file size.

4. Count the number of files in the current directory and output the result.

5. Take a list of numbers as input from the user and output the sum of those numbers.

6. Output a random number between 1 and 100.

7. Create a backup of a specified file by copying it to a backup directory with a timestamp in the filename.

8. Check if a website is online and output a message indicating whether it is up or down.

9. Convert a temperature in Celsius to Fahrenheit, using input from the user.

10. Ask the user for a sentence, then output the sentence in reverse order. For example, if the user enters "Hello, world!", the script should output "!dlrow ,olleH".
```

1. Create the project folder called Shell

```
mkdir Shell
```

  

![mkdir shell](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/bdcbe458-a3af-4d74-945f-fcdba30438c7)

  

1. Move into the Shell folder

  

```
cd Shell
```

1. In the Shell directory, create a file for your Shell script named **"script\_task.sh"**

```
touch script_task.sh
```

  

![shell script](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/4e19eded-57b6-4282-9031-7c4b20f2358c)

  
1. Copy/write your Shell script into the \***"script\_task.sh"** file using vim editor.

```
#!/bin/bash

# Task 1: Ask the user for their name and age, and output a message with their name and the year they were born.
read -p "Enter your name: " name
read -p "Enter your age: " age
current_year=$(date +%Y)
birth_year=$((current_year - age))
echo "Hello, $name! You were born in $birth_year."

# Task 2: Create a new directory with a name provided by the user, and navigate into it.
read -p "Enter a name for the new directory: " dir_name
mkdir "$dir_name"
cd "$dir_name"

# Task 3: List all files in the current directory, sorted by file size.
ls -S

# Task 4: Count the number of files in the current directory and output the result.
num_files=$(ls | wc -l)
echo "Number of files in the current directory: $num_files"

# Task 5: Take a list of numbers as input from the user and output the sum of those numbers.
read -p "Enter a list of numbers separated by spaces: " numbers
sum=0
for num in $numbers; do
  sum=$((sum + num))
done
echo "Sum of the numbers: $sum"

# Task 6: Output a random number between 1 and 100.
random_num=$((RANDOM % 100 + 1))
echo "Random number between 1 and 100: $random_num"

# Task 7: Create a backup of a specified file by copying it to a backup directory with a timestamp in the filename.
read -p "Enter the name of the file to backup: " file_name
backup_dir="backup"
mkdir -p "$backup_dir"
cp "$file_name" "$backup_dir/${file_name}_$(date +%Y%m%d%H%M%S)"

# Task 8: Check if a website is online and output a message indicating whether it is up or down.
read -p "Enter a website URL to check if it's online: " website_url
if curl --output /dev/null --silent --head --fail "$website_url"; then
  echo "$website_url is online."
else
  echo "$website_url is down."
fi

# Task 9: Convert a temperature in Celsius to Fahrenheit, using input from the user.
read -p "Enter a temperature in Celsius: " celsius
fahrenheit=$(echo "scale=2; ($celsius * 9/5) + 32" | bc)
echo "${celsius}°C is equal to ${fahrenheit}°F."

# Task 10: Ask the user for a sentence, then output the sentence in reverse order.
read -p "Enter a sentence: " sentence
reversed_sentence=$(echo "$sentence" | rev)
echo "Reversed sentence: $reversed_sentence"
```

```
vi script_task.sh
```

**NOTE:** You can develop your shell script on a note pad, before copying and paste into the **"script\_task.sh"** file.

1. Make the script file executable:

```
chmod +x script_task.sh
```

1. Run the script :

```
sudo ./script_task.sh
```

####   

![Code Run](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/6d7be9ea-2c15-4a27-a58b-680d21464c5f)  

  
### CONGRATULATIONS!

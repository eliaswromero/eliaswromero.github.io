---
layout: post
title: Animal Crossing Lab Analysis
subtitle: By Elias Romero
cover-img: /assets/img/ac.jpg
thumbnail-img: /assets/img/crossin.jpg
share-img: /assets/img/ac.jpg
tags: [books, test]
---
**note: I worked with Darson, Bulyn, Tuhin, and Claire on this assignment.*

## **Questions**
###### ***Discuss your process of how you worked on this lab. Include details such as who you worked with, what methods you tried, what worked or didn’t work, what could have gone better, and what you learned during this lab. Focus more on the programming side of the lab! Feel free to attach images, screenshots, pseudocode, etc to elaborate on your response.***

---

### Discuss how you used the API to obtain the dataset.
I went about the process almost exactly how I queried the API for the pokemon assignment, except for this API I had to use indecies to move through the different data. This was my code to create the API: 

```py
import requests
key = "ArtOfDataKEY123"
index = 0
payload = {'key': "ArtOfDataKEY123", 'idx': index}
response = requests.get("https://hm-cs.herokuapp.com"+"/socks", params=payload)


while(response.ok):
    payload = {'key': "ArtOfDataKEY123", 'idx': index}
    response = requests.get("https://hm-cs.herokuapp.com"+"/socks", params=payload)
    data = response.text
    print(data)
    index += 1
else:
    print(response.status_code, response.text)

```
I first imported requests and defined the key, index (which was set to 0), and payload. Then, I had to create a while loop that iterates through every index and prints the data, and then adds one to the index. I struggled with figuring out how to stop the while loop once the index variable exceeded the number of rows, but it was contained used `while(response.ok)`. This line makes it so as long as the querying process works, I can print each index of data. Once the index exceedes the actual number of lines of data, the response isn't ok and the code automatically stops. This code printed a CSV identical to the spreadsheet, all about socks.

---

### Which sock has the most variations? If there is more than one answer, then list all of them.

```py
    name_vari = {}
    for sock in data:
        sockname = sock['Name']
        variation = sock["Variation"]
        if sockname not in name_vari:
            name_vari[sockname] = 0
        name_vari[sockname] += 1

biggest = []
largest_variation = 1

for sock in name_vari:
    if name_vari[sock] > largest_variation:
        largest_variation = name_vari[sock]
        biggest = []
        biggest.append(sock)
    elif name_vari[sock] == largest_variation:
        biggest.append(sock)

x = ", ".join(biggest)
print("The Socks with the Most Variations are " + x + ".")
```
To find this answer, I first had to create an empty dictionary in which the keys would be the sock name and the values would be the number of variations. I iterated through each sock in our CSV (after defining sock and variation), and if the name wasn't in the dictionary, I added it with a value of 0. Then, every time the name shows up in the CSV, 1 is added to that sock's value. The result was a dictionary that had every sock and its number of variations.

I then created an empty list that would hold our sock/socks with the most variations, and a variable with a value of 1, which would represent the largest variation of socks. Then, I iterated through the dictionary I previously made. If the value of the dictionary's key is greater than the largest variation, then replace the largest variation with the value of that key and make a new list with that sock in it. If the value is equal to the largest variation of that sock, then add that sock to the list.

One thing I wanted to fix was how to make the list look nice in a print statement, so I used the line `",".join(biggest)` to separate the list items by a comma. Thus, the final statement prints out a list of socks with the greatest number of variations. :D

---

### How many socks of each color are there? If a sock has two different colors, it should be counted in both. However, if a sock has the same Color1 and Color2, make sure you don’t double count it!
```py
color_counter = {}
for sock in data:
    color1 = sock["Color 1"]
    color2 = sock["Color 2"]
    if color1 not in color_counter:
        color_counter[color1] = 0
    color_counter[color1] += 1
    if color2 not in color_counter:
        color_counter[color2] = 0
    elif color2 == color1:
        continue
    color_counter[color2] += 1

print(color_counter)
```
I first made a dictionary that would hold all the colors and number of socks with that color. I iterated through all socks in our CSV and said that if color 1 of the sock wasn't in the counter, then add it with a value of 0. Otherwise, add 1 to the value every time we see the color again. For color 2, I also said that if it wasn't in the counter, then add it with a value of 0. However, I had to include the `continue` statement if color 2 was the same as color 1. When both colors are the same, `continue` skips all code after it and continues iterating. Thus, our results show all colors and how many socks are that color, but does not count a color twice if a sock has the same color 1 and color 2.

### Conclusion

In conclusion, it was valuable to learn how to query an API like this that uses indeces (and how to iterate through it), instead of one like in the pokemon lab in which we distinguished using the pokemon name. I also learned how to utilize the `continue` statement and the `.join` method. I also experienced trouble with iterating through the values of a dictionary in problem 1, as I was using `sock` instead of `name_vari[sock]`, but learned it was a quick fix. 
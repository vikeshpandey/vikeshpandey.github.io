
 
Machine learning is a subfield of artificial intelligence. Early AI programs typically excelled at just one thing. _Eg: deep blue could play chess at a championship level but that's all it could do._

Today, we want to write one program that can solve many problems without needing to be rewritten. _Alphago_ is a great example of that as we speak. It is competing in the _World GO Championship_, but similar software can also learn to play Atari games as well. Machine learning is what makes that possible. It is the study of algorithms that learn from examples and experience instead of relying on hard-coded rules. 
 
 Let us look into a problem that sounds easy, but is impossible to solve without machine learning.
 _Can you write code to tell the difference between an apple and an orange?_

Imagine you are asked to write a program that takes an image file as input does some analysis and outputs the types of fruit. How can you solve this?

you'd have to start by writing lots of manual rules, eg: you could write code to count how many orange pixels there are and compare that to the number of green ones the ratio should give you a hint about the type of fruit. That works fine for simple images, but as you dive deeper into the problem you find the real world is messy and the rules you write start to break.

How do you write code to handle black and white photos or images with no apples or oranges in them at all in fact for just about any rule you write I can find an image where it won't work you'd need to write tons of rules and that's just to tell the difference between apples and oranges! if you are given a new problem, you need to start all over again clearly we need something better to solve this.

we need an algorithm that can figure out the rules for us so we don't have to write them by hand. To achieve this, we're going to train a classifier. For now, you can think of a classifier as a function that takes some data as input and assigns a label to it as output. Eg: A picture that we would want to classify it as an apple or an orange or an email and we want to classify it as spam or not spam, the technique to write the classifier automatically is called supervised learning.

Please click on the link below to head over to the first program we are going to write for supervised machine learning.

[Hello-World](hello-world.md)

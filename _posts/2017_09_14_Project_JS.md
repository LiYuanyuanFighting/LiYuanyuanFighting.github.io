### To change triangle into rectangle  
0. [rapheal introduction](https://blog.thebrickfactory.com/2013/03/how-to-create-dynamic-svgs-with-raphael/)
1. studying Raphael.path to convert triangle into rectangle  
https://www.packtpub.com/books/content/paths-and-curves-raphael-js-vector-graphics  
var d = "M 10,30 L 60,30 L 10,80 L 60,80";
This translates to: “Move (M) to the coordinates (10,30), draw a line (L) to the coordinates (60,30),   
then a line (L) to (10,80), and then a line (L) to (60,80).”  
https://www.safaribooksonline.com/library/view/raphaeljs/9781449365356/ch04.html  
If you end your path with a z, it connects to the beginning automatically.  
2. to calculate the distance  
[Viviani's theorem](https://en.wikipedia.org/wiki/Viviani%27s_theorem)  

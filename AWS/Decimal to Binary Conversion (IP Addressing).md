dotted decimal notation vs binary notation had 8 bits or 4 bytes or 4 octet
133.33.33.7

each number has a decimal value 
bellow is a binary map 
Here is the text from the image:

| Position              | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   |
| --------------------- | --- | --- | --- | --- | --- | --- | --- | --- |
| Binary Position Value | 128 | 64  | 32  | 16  | 8   | 4   | 2   | 1   |
| Binary Value          | 1   | 0   | 0   | 0   | 0   | 1   | 0   | 1   |

Is there anything specific you need to do with this text?

rule 1 if the human num is great the binary position value, you subtract it 

133-128 = 5

then go throug othe binary positon till you find another binary posion wher the value is greater 
6-5 = 1

you will end up with 10000101
# Scene file presentation

What we call a 'scene file' is a text document where our entities are serialized in text format.  Thanks to these files, we can have a readable and modifiable scene workspace, and we can read it with the Engine's **Serializer** to create the scene in the game. It is a light file, so really easy to export or git.

## **Convention**

Our Engine has its own scene file extension: **_.kk_**. This is a very strict writing format, you have to be exactly like the example below:
```
>entity  
>attribute
    value
>subattribute
    value1, value2, value3
```  
_attribute example:_
```
>position
    1.00, -2, 500.64
```  

If there is no problem, only the engine should write these .kk files. However, we are keeping a human language so that we can check the scene save.
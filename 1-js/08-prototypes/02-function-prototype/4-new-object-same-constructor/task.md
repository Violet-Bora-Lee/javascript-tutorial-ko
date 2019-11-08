importance: 5 
2 2    
 
3 3    --- 
4 4    
 
5    -# Create an object with the same constructor 
 5   +# ������ ������ �Լ��� ��ü ����� 
6 6    
 
7    -Imagine, we have an arbitrary object `obj`, created by a constructor function -- we don't know which one, but we'd like to create a new object using it. 
 7   +������ �Լ��� �ϳ� �ְ�, �� ������ �Լ��� ����� ���� ������ ��ü `obj`�� �ִٰ� �����غ��ô�. ������ �� ������ �Լ��� ����� ���ο� ��ü�� �������ϴ� ��Ȳ�Դϴ�.   
8 8    
 
9    -Can we do it like that? 
 9   +��ü�� �𸣴� ������ �Լ��� ����� ���ο� ��ü�� ����°� �����ұ��? 
10 10    
 
11 11    ```js 
12 12    let obj2 = new obj.constructor(); 
13 13    ``` 
14 14    
 
15    -Give an example of a constructor function for `obj` which lets such code work right. And an example that makes it work wrong. 
 15   +���� ���� �ڵ带 ����� ��ü�� ���� �� �ְ� ���ִ� ������ �Լ��� �ۼ��غ�����. ���⿡ ���Ͽ� ���� ���� �ڵ尡 �������� �ʵ��� �ϴ� ���õ� �ϳ� ��������. 

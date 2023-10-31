Book summary of C++ master class notes 

Compiler takes cpp and move it to obj files 
Linker takes obj and link it with external c++ lib to produce exe (binary files) 
Preporccsoor -> just load other files before the compile sees it .. (e.g., hash include <algorithum> to load this file) .. it also remove comments 
Namespace is like a pkg path to help code conflict. It is better to import only the functions needed (using namespace std::cin) better than importing the whole lib (using namespace) 
Variables are abstractions on memory locations 

Do not use Hash define pi 12.2 .. as it is implemented via pre-process to replace the word Pi with 12.2 ..  

C++ was created to develop OS, embedded system , you want to be in complete control of the memory/hardware and deal with the memory directly. we can not afford interruptions for garbage collection. It gives you more power over your machine.

Pointer is a variable that stores an address 
All pointers has size of 4 bu as it is the size of the max address. Double* p; size of (p) is always 4 bytes as it is the size of the address .. but size of *p is size of double which is 8 

Static memory allocations happens to compile time, where variables get allocated on the stack 
Dynamic memory allocations happens at runtime, where variables get allocation on the heap 

Int * p; cout<< *p++.. means de-reference first , then move the pointer to the next address / element 

Dangling pointer is a pointer that points to a released address.

c++ new can through an exception if it fails to allocation the object on the heap. It will throw a bad_alloc exception 


for(auto str : myStringVector){str=“11”; } is actually a copy string so it will not change vector elements to “”11 …. If you want to actually change the vector, you should access it via reference and not copy for(auto & str: myStringVector) {str=“11”}.. this will go through them via refeenrece 


Reference is just an alias to the variable 
Int a =10; int &b = a; a=20; .. in that case b is also 20 as b is just another alias to a both pointing to the same address 


L-values are variables that are names and addressable 


Guards in header files insure that the content of the file is not copied more than once into separate class (imagine you imported it from 2 places). This stops a compilation problem of duplicate definitions 
# ifndef _ACCOUNT_H
# define _ACCOUNT_H

#endIf 

# with <> means include a system file 
# with “” means include a local file 


Default no args contributor only exist if you did not define any constructs (zero constructors) 

Use intalized constructor 
Class B{
Int a;
B(): a(1){}
}

Class B{
Int a;
B(int aa){ a = aa; // assignment not initiallization 
}
}

Copy constructors do a shallow copy by default so be carful that you need to provide one if you have pointers. During copy constructor the c++ provide a default copy constructor. It works mostly fine for most objects. Unless it is a pointer, it do a shallow copy and not a deep one . So it is recommends that you have a copy constructor on your class if you have pointers 

Int x = 2;
X is l-value and 2 Is r-value 
Void f(int &num) // expect an l-value. So int x = 2 ; func(x) will work but func(200) will not. 
Void f(int &&num)// expect an r-value. So int x = 2; func(x) will not work. But func(200) will work 

Move constructor are r-value based and they move the whole parameter. They steal what is passed to it and destroy it. 
Move(const Move& move) // copy-constructor 
Move(const Move && move) // move constructor {this.data = move.data; move.data = nullptr;} 


Const objects needs to be handled carefully; only const functions can be called on const objects, although they work with non-const objects also. 
We need to tell the compiler which function that will not change the object by adding const at the end. 

Only difference between struct or class 
- Data in struct is public 
- Struct do not declare methods 


Friend class have access to private class members 
Class P {friend void d(); in x;} .. this d() function have access to all public and private fiends and functions 

Function overloading (ploymorphysim) … same name but different parameters. return type is not considered. 

When passing array to a function. Array element is not copied. Only a copy of the address to the first element. 

The class by default has on default assignment operator ‘=‘. It does a shallow copy by default. 
String s1 = “12312”; // this is will call the assignment operator constructor 

Do not get confused 

String s = “1”;
String s2 = s; // this is not assignment operator .. this is a copy constructor similar to string s2 (s);

S2 = s // this will call the assignment operator when both of them already insitlaized 

Copy constructor can be in two shapes 
String s1 = “1”;
String s2 = s1 … or string s2 (s1)

Operator = (string & a)
Operator = (string && a)
String s1 = “1”;
String s2;
S2 = s1; // s1 is l-value so it will call normal operator 
S2 =“12312”// string is r-value so it will move operator 

for(auto x : collection) .. calls copy constructor
vector.push_back(some object) .. calls copy constructor only if it is l-value like .push_back(m1) ,.. T m1; … if it is r-value like .push_back(T(2,3,1)).. it will use move_construcotr 
Function (SomeClass x )… calling a function that expects a parameter by value triggers copy constructor 
In inheritance a base class constructor will go first then child constructor 
In inheritance a child class destructor will go first then base destructor 

If you used Base::Bases it will inherit all consturcutor into child class 
Operatros and constructs are not auto inherited from base class 


Static binding is calling the function on the object. 
Base* s  = new Child() .. this is static binding .. it will call e() function on the base at compile time 
s.e()



Smart pointers delete themselves auto 
You can not do arithmetic operations such as ++ or — on smart pointers 

Unique pointer : only 1 unique pointer will reference the object. No more than 1 unique pointer will own the object .. only 1 

Copy Is not allowed.  Assignment is not allowed. But move is allowed. So vector<unique_ptr<int>> vet;unique_ptr<int>ptr {new int{100}}; vec.push_back(ptr)// is not allowed as copy constructor is disabled.. instead use move constructor vec.push_back(move(ptr));.. the vector now owns the pointer 

Auto p = make_unique<int>(2); is more safe than new .. incase of exceptions to allocate on heap. I think make_unique handles these scenarios better. 

vec,push_back(unique_ptr<int>(2)) .. is not calling copy constructor instead it is calling the move constructor.. remember it is r-value 

Shared_ptr will auto remove the object if number of shared pointers referencing is zero. One of more can reference the same object. They can be copied, assigned and moved. 
So vector<shared_ptr<int>> vet;shared_ptr<int>ptr {new int{100}}; vec.push_back(ptr)// is allowed as copy conductor is added 


Weak_pointer provided a non-owing “weak” reference 
Always created from shared_pointer and does not increment the shared_ownership_count (p1.use_count())
Good for iterators , they just pass through and do not own the object 

They are good to prevent circle reference problem. If you have 2 objects that has 2 shared pointers pointing to each other. They will not get deleted as both of them owns each other.
weak_pointers are good hear to make one owns the other. In that 

You can pass a custom deleter function to smart pointer shared_ptr<int> ptr = new shared_ptr<>(new int(2), myCustomDeleteFunction);



for(auto it : container ) if(*it == 3) it = container.erase(it) else it ++
erase(It) .. it returns an iterator to the next element after the deleted element

Find() Iterator to firstOcourse Of 3 .... it = find(v.begin(), v.end(), 3)
count().. return number of elements int num = count(v.begin(), v.end(), 3) .. make sure you override == operator in case of complex object 
count_if((v.begin(), v.end(), [] (int x){return x%2 ==0;}); return number of even numbers 
replace(v.begin(), v.end(), 1, 100) .. replace all ones with 100 
replace_if(v.begin(), v.end(), isOddLamdaFunction, 200) replace all odd numbers with 200
Boolean s = all_of((v.begin(), v.end(), lamdaFunction)) similar to allmatch() in java
Boolean s = any_of((v.begin(), v.end(), lamdaFunction
str.replace(index, len, to_replaace_with_str)
Str.find(anotherStr)returns index to the first occurrence .. returns size_t 



vector<string> split(string s, regex sep_regex = regex{"\\s+"}) {
  sregex_token_iterator iter(s.begin(), s.end(), sep_regex, -1);
  sregex_token_iterator end;
  return {iter, end};
}

To split string on pattern space


Emplace-back vs push_back
The first is more effect as it contrasts the object inside the collection with no copies
push_back sometimes creates a temporary copy In between 

vector.find(begin(), end(), value{3}). Returns a it to the place where it find the 3 

Dequeue is different from list.. dequeue is double ended queue and it is build as a list of sub arrays linked together .. list is a double linked list  


Set in c++ use the operator < to 1) find an element that is there already or 2) sort elements 
If u have a set with {(“a,”1), {b,””2}}.. if you call set.find(“a”, 100} .. it will return the first element as the operator uses only the first element as the sorting key 
Operatpr < () {return firstElement < other.firstELement}; 
It uses < and not == operator as , a == b if !(a < b) and !(b < a) … so it does not need == 
Also map using red-black tree only rely on < operator 

In unorderd_set or unorderd_map using hash table which will need two operator 1) == and 2)hashFunction  link in java, equal and hasCOde() 

Lamda function
[=] capture all by value default all by vale 
[&] capute all by reference 
[this] capture all by this 
[=, &x] .. caplet all by value but x by reference 
[&, z] capture all by reference	but z by value 
We can not capture static, or global variables … but lambda can see them without need to capture it explicitly 

Int x = 100; Auto l = [x] (){x+=100}; statfrul lama capture x only at the start so if you called l() it will capture x with 100 and add a 100. Then on the second call l(). It will increase 200 + 100.. as it has an x stored inside it and was capture only once at the start. 
Lamda function generate a class with captured variables and one object function  

Polymorphism  means multiple forms .. same pointer can behaviour differently depending on which child ref is passed 
Early binding , static binding —> compile time 
late binding , dynamic binding  —> Runtime

Static - binding 
Parent* p = new Child() /// s static biding will do slicing 
P->do() // this will call parent-> do() 


You can enable runtime binding by using virtual function 
Virtual void do() if defined in account.. it tells the compiler to do the binding later during run-time and skip static - binding 
Virtual function are statilly bound if you call them from the same class , so they are only dynamically bound when you call them via a parent pointer 
Any virtual function must have a virtual destructor .. by default all child classes destructors will be virtual but best practice to re-state it. If you did not define a destructor as virtual, it will call the base class disttructor. 


Prute virtual function marks a class as abstract 
Virtual void f () = 0; // virtual function 
You must ovveride all pure virtual functions to be a normal class otherwise, you will import parent virtual function and you become abstract class 


c++ does to provide true interface and we use abstract classes. Interface is an abstract class that has  pure virtual function 

Override keyword does a check to make sure you exactly matching the parent function 


Virtual const char* what() const noexcept; ti implement a custom exception 









Personl Goals 

Bi-weekly goals by 15/11 

- [ ] Talking to Leeds half way through
- [ ] Entered 2 contests
- [ ] Caching Cache Replacement Policies, Cache Invalidation Strategies, CDN - https://lnkd.in/gMSr8wTg
- [ ] https://www.designgurus.io/blog/Load-Balancer-Reverse-Proxy-API-Gateway
- [ ] https://levelup.gitconnected.com/mastering-database-replication-an-essential-guide-for-2023-9fa6deb3efe4
- [ ] https://www.designgurus.io/blog/REST-GraphQL-gRPC-system-design
- [ ] Re-impelemnt the problem (valid word abbreviation) in c++ ..
- [ ] Read about raii
- [ ] Read about <typename T, typename … args> … what is the name of constuctor(Args&&… q) .. maybe I need a resource in generics 
- [ ] 






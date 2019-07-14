# JavaScript-advanced
Cái khái niệm mà ai muốn thuần thục JavaScript phải biết rõ.
## I. Object
Object là một tập các trường dữ liệu (property) và các hàm (method).
Ví dụ: Object dưới đây có 2 trường là firstName, lastName và hàm showName.
```python 
let person = {
  firstName: 'Minh Hai',
  lastName: 'Bui',
  showName: () =>{
    console.log(this.firstName + " " + this.lastName);
  }
};
```
##### 1. How to khởi tạo object???
Trong JavaScript không có class nên ta có thể khởi tạo obect bằng 1 trong 2 cách sau.
```python 
// Cách 1: Object literal {}
let person = {
  firstName: 'Minh Hai',
  lastName: 'Bui',
  showName: () => {console.log(this.firstName + " " + this.lastName)
};

// Cách 2: Object constructor
let psn = new Object();
psn.firstName = "Minh Hai";
psn.lastName = "Bui";
psn.showName = () => { console.log(this.firstName + " " + this.lastName)};
```
######## Ưu nhược điểm: 
ƯU: Với các ứng dụng đơn giản, ta có thể tạm dùng 2 cách này. -> Khởi tạo nhanh. 
Nhược: Object literal mỗi lần khởi tạo object sẽ khiến code dài và trùng lặp (vì lần nào cũng cần khai báo lại property và method)
==> Sử dụng Constructer pattern. (Na ná khai báo class trong các ngôn ngữ khác).
```python
// Cách 3 Contructor pattern
function Person(firstName, lastName){
  this.firstName = firstName;
  this.lastName = lastName;
  this.showName = () => console.log(this.firstName +" " + this.lastName);
};

// Khi muốn tạo object person thì chỉ cần gọi constructor
let psn1 = new Person('Hai', 'Bui');
let psn2 = new Person('Hanh', 'Pham');
```
Một cách khác khá hay được sử dụng là prototype.
```python 
// Cách 4: prototype
function Person() {}

person.prototype.firstName ="Minh Hai";
person.prototype.lastName ="Bui";
person.prototype.showName = () => console.log(this.firstName +" " + this.lastName);

// Object được tạo sẽ có sẵn các trường firstName, lastName và hàm showName.
let psn1 = new Person();
console.log(psn1.firstName) // Minh Hai
psn1.showName(); // Minh Hai Bui
```
##### How to "play" with object???
###### 2. Truy xuất một trường / hàm của object 
Để truy xuất một trường/hàm của object, ta có thể dùng dấu . (dot notation) hoặc dấu [] (bracket notation). Dot notation thường được sử dụng nhiều hơn., nhưng bracker notation có thể làm được nhiều trò hay hơn.
Ví dụ: 
```python 
let person = {
  firstName: "Minh Hai",
  lastName: "Bui",
  50: "Hi", // property là số không thể dùng dot notation để truy xuất.
  showName: () => console.log(this.firstName + " " + this.lastName)
}

console.log(person.firstName); // Minh Hai
console.log(person['firstName']); // Minh Hai

console.log(person.50); // Error
console.log(person["50"]); // Hi

console.log(person.showName()); // Minh Hai Bui
console.log(person["showName"]()); Minh Hai Bui
```
Làm cách nào để duyệt qua toàn bộ các trường của một object??? 
-> Chỉ cần dùng dòng for đơn giản.
```python
let person = {
  firstName: "Minh Hai"
  lastName: "Bui",
  showName: () => console.log(this.firstName + " " + this.lastName)
}
for (let prop in person){
  console.log(prop); // firstName, lastName, showName
}
```
###### 3. Thêm/Xóa một trường/hàm của object? 
Với các ngôn ngữ static typing như C#, Java, một objecdt được khởi tạo dựa trên class, do đó chúng luôn có các trường và các hàm cố định. Tuy nhiên, do JavaScript là ngôn ngữ dynamic typing, ta có thể dễ dàng thêm/xóa các trường trong code. Ví dụ: 
```python
let person = {
  firstName: "Minh Hai"
  lastName: "Bui"
  showName: () => console.log(this.firstName + " " + this.lastName)
};

delete person.lastName; // Xóa trường lastName
person.lName = "Just adding"; // Thêm trường lName

console.log(person.lastName); // undefined
console.log(person.lName); // Just adding.
```
###### 4. Serialize vaf deserialize
Để giao tiếp với server, JavaScript thường phải submit dữ liệu dưới dạng pair-value (thông qua form) hoặc JSON. Do đó, JavaScript hỗ trợ sẵn việc chuyển sang chuỗi JSON và ngược lại.
Ví dụ: 
```python
let Person = { 
  firstName: "Minh Hai",
  lastName: "Bui",
  showName: () => console.log(this.firstName + " " +  this.lastName)
};
// Serialize sẽ làm mất method, chỉ giữ các properties
JSON.stringify(person); // "{"firstName": "Minh Hai", "lastName": "Bui"}"

let jsonString = "{"firstName": "Minh Hai", "lastName": "Bui"}";
let psn = JSON.parse(jsonString); // Chuyển String thành Oject.
console.log(psn.firstName); // Minh Hai
console.log(psn.lastName); // Bui
```
## II. This (trong JavaScript)???
Khi mới học, ta thấy this cũng đơn giản và vô hai. Nếu như học qua C++, C# thì từ khóa this dùng để trỏ tới chính object gọi hàm đó. Trong JavaScript this cũng có vai trò tương tự. 
Ví dụ: this trong đoạn code dưới đây trỏ tới object person. -> in ra những gì mình muốn.
```python
let person ={
  firstName: "Minh Hai",
  lastName: "Bui",,
  showName: () => console.log(this.firstName + "  " + this.lastName)
}
// Ở đây this sẽ là object person
person.showName(); // Minh Hai Bui
```
Một trường hợp khác, khi ta khai báo biến global và hàm global, toàn bộ các biến và hàm đó sẽ nằm trong một object có tên là _window_. Lúc này, khi ta gọi hàm showName, chính object _window_ là object gọi hàm đó, this trỏ tới chính object _window_.
```python
let firstName = "Minh Hai", lastName = "Bui";
// 2 biến này nằm trong object window
function showName(){
  console.log(this.firstName + " " + this.lastName);
}

window.showName(); // Minh Hai Bui. this trỏ tới object window.
showName(); // Minh Hai Bui. Object gọi hàm showName vẫn là object window.
```
#### This gây ra bao rắc rối ??? Really maker? 
Nếu chỉ sử dụng t hoe 2 cách nêu trên, this sẽ khá dễ hiểu và không gây ra mấy khó khăn khi sử dụng. Song, sự đán sợ và khó chịu của this sẽ lộ dần ra qua các ví dụ dưới đây.
##### First Blood. Hàm được truyền vào như một callback. (Chưa hiểu callback thì chịu khó Ctrl + F gõ callback để tìm thông tin ở dưới nhé).
Giả sử, ta muốn khi người dùng clik vào một button, ta sẽ gọi hàm showName của user. Vô cùng đơn giản, ta chỉ cần truyền hàm showName vào như một _callback_ cho hàm click là xong. 
```python
let person = {
  firstName: "Minh Hai",
  lastName: "Bui"
  showName: () => console.log(this.firstName + " " + this.lastName)
}

// Ở đây this sẽ là object person
person.showName(); // Minh Hai Bui

// Đây là code jqurey
$('button').click(person.showName); // showName truyền vào như callback
```
Tuy nhiên, hàm lại không chạy như ta mong muốn. Tại vì lúc này this chính là button mà ta đã click vào, chứ không còn là object person như ví dụ trên nữa.

Trong trường hợp này, ta có thể sửa lỗi bằng cách sử dụng anonymous function, hoặc dùng hàm bind để xác định tham số this cho hàm truyền vào là được.
```python
let person = {
  firstName: "Minh Hai",
  lastName: "Bui",
  showName: () => console.log(this.firstName + " " + this.lastName)
};

$('button').click(person.showName); // showName truyền vào callback, ở đây this chính là button

// dùng anonymous function
$('button').click(function() {person.showName();});
// Có thể viết lại theo arrow
$('button').click(() => person.showName());

// Dùng bind
$('button').click(person.showName.bind(person)); // This ở đây vẫn là object person
```
##### Rắc rối 2 - Sử dụng this trong anonymous function
Giả sử, object person có một danh sách bạn bè, bạn muốn viết một function show toàn bộ bạn bè của person đó. Theo lý thuyết ta viết như sau: 
```python
let person = {
  firstName: "Minh Hai",
  lastName: "Bui",
  friends: ["Minh", "Sang", "Khoa", "Hoang"],
  showFriend: () => {
    for(let i = 0; this.friends.length ; i++){
      console.log(this.firstName + " hava a friend " + this.friends[i]);
    }
  },
  
  showFriendThis: () => {
    this.friends.forEach((fr) => console.log(this.firstName + " hava a friend " + fr)
  }
}
person.showFriend(); // Hàm chạy đúng

person.showFriendThis(); // Hàm chạy sai.
```

Với hàm showFriend, khi ta dùng hàm for thường, hàm chạy đúng như mong muốn. Tuy nhiên, trong trường hợp dưới, khi ta dùng forEach, truyền vào một anonymous function, this ở đây lại thành object _window_, do đó trường firstName bị underfined.

Trong trường hợp này, cách giải quyết ta thường dùng là tạo một biến để gán giá trị this vào và truy xuất tói giá trị đó trong anonymous function 
```python
let person = {
  firstName: "Minh Hai",
  lastName: "Bui",
  friends: ["Minh", "Sang", "Khoa", "Hoang"],
  showFriendFixed: () => {
    let personObj = this; // Gán giá trị this vào biến personObj
    this.friends.forEach((fr) => console.log(personObj.firstName + " hava a friend " + fr)
  }
};

person.showFriendFixed();  // Hàm chạy đúng.
```
##### Rắc rối 3 - Khi hàm được gán vào một biến
Trường hợp này ít xảy ra. Đó là trường hợp khi ta gán một hàm vào một biến, sau đó gọi hàm đó. Hàm sẽ không chạy như ta mong muốn, vì object gọi hàm lúc này chính là object _window_.
```python
let person = {
  firstName: "Minh Hai",
  lastName: "Bui",
  showName: () => console.log(this.firstName + " " + this.lastName)
};

// Ở đây this sẽ là object person, chạy đúng
person.showName();

let showNamefunc = person.showName; // Gán function vào biến showNamefunc
showNamefunc(); // Chạy ra, ở đây this sẽ là object window
```
Để giải quyết, ta cũng sử dụng hàm bind như trường hợp trên cùng, quá đơn giản phải không man? 
let's to the code following.
```python
let person = {
  firstName: "Minh Hai", 
  lastName: "Bui",
  showName: () => console.log(this.firstName + " " + this.lastName)
};

let showNameFunc = person.showName.bind(person); // Gán function vào biến showNameFunc
showNameFunc(); // Chạy đúng vì this bây giờ là object person, vì ta đã bind.
```

## III. Prototype JavaScript là gì???
  Khi một thằng developer khác cứ đi theo và hỏi bạn "Prototype là cái quái gì?", hãy trả lời nó: Là cái đầu "cha" mày, hỏi hỏi cl. Câu trả lời có thể hổ báo nhưng nó lại khá chính xác, có thể hiểu prototype nôm na là khuôn hoặc là "cha" của một object.

  Trong JavaScript, trừ undefined, toàn bộ các kiểu còn lại đề là object. Các kiểu string, number, boolean, lần lượt là object dạng String, Number, Boolean. Mảng là object dạng Array, hàm là object dạng Function. Prototype của mỗi object chính là cha của nó, cha của String là String.prototype, cha của Number là Number.prototype, của Array là Array.prototype.
  
  Trong JavaScript, `Việc kế thừa được thực hiện thông qua prototype`. Khi ta gọi property hoặc function của một object, JavaScript sẽ tìm trong chính Object đó, nếu không có thì tìm lên cha của nó. Do đó, ta có thể gọi các hàm toUpperCase, trim trong String là do các hàm đó đã tồn tại trong String.prototype.

Khi ta thêm function cho prototype, toàn bộ những thằng con của nó cũng học được function tương tự. Ví dụ 
```python
let str = "abc"; str là string, cha nó là String.

// Nhân đôi chuỗi đưa vào
String.prototype.duplicate = () => this + this;

console.log(str.duplicate()); // Tìm thấy hàm duplicate trong prototype 
// Kết quả là: abcabc
```
Như đã trình bày ở trên, Array, Number hay String có cha là Object, do đó chungs đều có các hàm như constructor, hasOwnProperty, toString thuộc về Object.prototype

Nhắc lại một chút về Object. Ta có 2 cách để khởi tạo object, đó là sử dụng object literal và Constructor Function. Nếu dùng Object literal, object được tạo ra sẽ có prototype là Object.prototype. Nếu dùng constructor function, object sẽ có một prototype mới, prototype này kế thừa Object.prototype.
```python
let person = {
  firstName: "Minh Hai",
  lastName: "Bui"
  showName: () => console.log(this.firstName + "  " + this.lastName)
}; // Object này có prototype là Object.prototype

function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
  this.showName = () => console.log(this.firstName + " " + this.lastName);
}
let otherPerson = new Person("Hang", "Pham"); // Object này có prototype là Person.prototype
// Prototype mới: Person.prototype được tạo ra.
// Person.prototype kế thừa từ Object.prototype.
```
Những object được tạo ra bằng cách gọi new Person() đều có prototype là Person.prototype. Nếu muốn thêm trường hay hàm cho các object này, chỉ cần thêm 1 lần vào prototype là xong. Hiểu nôm na thì prototype cũng có vài phần giống với class.
```python
 function Person (firstName, lastName){
  this.firstName = firstName;
  this.lastName = lastName;
 }
 Person.prototype.love = () => console.log("love light");
 
 let otherPerson = new Person("Minh Hai", "Bui");
 otherPerson.love(); // love light
 ```
 ### Prototype dùng để làm gì??? what's up man?
 Trong JavaScript không có khái niệm class, do vây, để kế thừa các trường/hàm của một object, ta phải sử dùng prototype.
 ```python
 function Person(){
  this.firstName = "Per";
  this.lastName ="son";
  this.sayName = () => this.firstName + " " +this.lastName
 }
 // Viết một Constructor Function khác
 function SuperMan(firstName, lastName){
  this.firstName = firstName;
  this.lastName = lastName;
 }
 
 // Ta muốn SuperMan sẽ kế thừa các thuộc tính của Person
 // Sử dụng Prototype để kế thừa.
 SuperMan.prototype = new Person();
 // Tạo một object mới bằng Construtor Function
 let sm = new SuperMan("Minh Hai", "Bui");
 sm.sayName(); // Minh Hai Bui. Hàm này kế thừa từ prototype của Person
 ```
 
 ### IV. OOP Trong JavaScript.
 Các đặc tính của OOP trong JavaScript. 
 - Encapsulation: Tính bao đóng và che giấu thông tin. Tính chất này không cho phép người dùng sử dụng các đối tượng thay đổi trạng thái nội tại của một đối tượng. Chỉ có các phương thức nội tại của đối tượng cho phép thay đổi trạng thái của nó. Việc cho phép bên ngoài tác động lên các dữ liệu nội tại của một đối tượng tùy thuộc vào người viết mã. Đây là tính chất đảm bảo sự toàn vẹn của đối tượng.
 
 - Inheritance: Tính kế thừa. Đặc tính này cho phép một đối tượng có thể có sẵn các đặc tính mà đối tượng khác đã có thông qua kế thừa. Điều này cho phép các đối tượng chia sẻ hay mở rộng các đặc tính có sẵn mà không phải tiến hành định nghĩa lại. Tuy nhiên, không phải ngôn ngữ hướng đối tượng nào cũng có tính chất này. 
 
 - Polymophism: Tính đa hình. Thể hiện thông qua việc gửi các thông điệp ( message). Việc gửi các thông điệp này có thể so sánh như việc gọi các hàm bên trong của một đối tượng. Các phương thức dùng trả lời cho một thông điệp sẽ tùy theo đối tượng mà thông điệp sẽ tùy theo đối tượng mà thông điệp đó được gửi đến có phản ứng khác nhau. Người lập trình có thể định nghĩa một đặc tính (chẳng hạn thông qua tên của các phương thức) cho một loạt các đối tượng gần nhau nhưng khi thi hành thì dùng cùng một tên gọi mà sự thi hành của mỗi đối tượng sẽ tự động xảy ra tương ứng theo đặc tính của từng đối tượng mà không bị nhầm lẫn.
#### OOP trong Java
Java là một ngôn ngữ hướng đối tượng, do đó việc thực hiện các đặc tính OOP rất đơn giản và nhanh gọn, dễ hiểu. 

 a. Tính đóng gói trong Java thể hiện bằng cách cho khai báo các trường private, chỉ có thể truy xuất thông qua các hàm set, get.
 ```python
 public class Person{
  private String firstName; 
  private String lastName;
  private int age;
  
  public String getFristName() {return this.firstName;}
  public void setFirstName(String firstName) {this.firstName = firstName;}
  
  public String getLastName() {return this.lastName;}
  public void setLastName(String lastName){this.lastName = lastName;}
  
  public int getAge() {return age;}
  public void setAge(age) {this.age = age;}
 }
 ```
Tính kế thừa và đa hình cũng khá đơn giản, chỉ cần extend và viết hàm mới đè lên là xong.
```python
public class Person{
  private int personSecret; // Chỉ person mới truy xuất được.
  protected int age; // superman có thể truy xuất được.
  
  public void say(String message){
    System.out.println("Person said " + message);
  }
  
  // Kế thừa và override hàm say
  public class Superman extends Person{
    public void say(String message){
      System.out.println("Superman said " + message);
    }
  }
}
```
### OOP trong JavaScript
 Trong JavaScript để thực hiện tính đóng gói, ta có thể tạo ra 1 constructor function, đóng gói toàn bộ các trường và hàm vào một object. Thông thường chúng ta hay làm như sau:
 ```python
 function Person(firstName, lastName) {
  this.fristName = firstName; 
  this.lastName = lastName;
  this.showName = () => console.log(this.firstName + " "  +this.lastName)
 };
 
 let psn1 = new Person("Minh Hai", "Bui");
 // Các property khai báo vào biến this có thể truy xuất từ bên ngoài
 // Object không còn tính bao đóng nữa.
 psn1.firstName = "changed";
 console.log(psn1.firstName);
 ```
 Với các khai báo này, tính bao đóng không được đảm bảo. Các property có thể bị truy cập, thay đổi từ bên ngoài. Ở đây, ta phải sử dụng biến cục bộ.
 ```python
 function Person(firstName, lastName){
  let fstName = firstName;
  let lstName = lastName;
  
  this.setFirstName = (firstName) => fstName = firstName;
  this.setLastName = (lastName) => lstName = lastName;
  
  this.getFirstName = () => fstName;
  this.getLastName = () => lstName;
 }
 
 let psn1 = new Person("Minh Hai", "Bui");
 console.log(psn1.fstName); // Undefined, Không thể truy cập được
 console.log(psn1.getFirstName); // Minh Hai
 ```
  Tuy nhiên, biến cục bộ này chỉ có thể truy xuất trong Constructor Function, nó tương đương với các trường private trong java. Trong JavaScript, không có cách nào để tạo ra các trường protected (Chỉ có thể truy cập từ class kế thừa) như Java và C# được. Việc kế thừa còn tào lao hơn nữa, vì JavaScript không có từ khóa extends cũng như class, ta phải sử dụng prototype để kế thừa.
  Ví dụ:
  ```python
  function Person() {
    this.firstName = "Per";
    this.lastName = "son";
    this.sayName = () => this.firstName + " " + this.lastName;
  }
  
  // Viết một constructor Function khác.
  function SuperMan(firstName, lastName){
    this.firstName = firstName;
    this.lastName = lastName;
  }
  
  // Ta muốn SuperMan sẽ kế thừa các thuộc tính của Person
  // Sử dụng prototype để kế thừa.
  SuperMan.prototype = new Person();
  
  // Tạo ra một object mới bằng constructor function
  let sm = new SuperMan("Minh Hai", "Bui");
  sm.sayName(); // Minh Hai Bui. Hàm này kế thừa từ prototype của Person.
  ```
 ## V. JavaScript tào lao - BIND, CALL và APPLY
  Bộ 3 function bind, call, apply. Đây là 3 hàm tạo nên sự mạnh mẽ và bá đạo của JavaScript.
  #### Trói this lại bằng bind.
 Bind là mọt hàm nằm trong Function.prototype, d đó chỉ có function mới có khả năng gọi nó. bind được dùng để xác định tham số _this_ cho một function.
 Như trong trường hợp dưới đây, khi ta truyền hàm showName vào như một -callback- cho hàm button.click, giá trị this ở đây chính là button đó. Để hàm chạy đúng, ta dùng _bind_ để bind giá trị person và this.
 ```python
 let person = {
  firsName: "Minh Hai",
  lastName: "Bui",
  showName: () => console.log(this.firstName + "  " + this.lastName)
 };
 
 //showName truyền vào như callback, ở đây this là button
 $('button').click(person.showName);
 
 // Dùng bind để xác định giá trị this
 $('button').click(person.showName.bind(person)); // This ở đây vẫn là objet person.
 ```
Không chỉ bind được giá trị this, _bind_ còn bind được các tham số truyền vào cho hàm nữa. Do đó, Bind còn được dùng để viết partial function.

Nói một cách đơn giản, partial function tức là tạo ra 1 function mói từ 1 function cũ bằng cách gán mặc định một số tham số cho function cũ đó. Ví dụ. một hàm _log_ đơn giản có 3 tham số
```python
function log(level, time, message){
  console.log(level + " - " + time +" - "  + message);
}
```
Giả sử muốn tạo một hàm log khác, ghi lại các log error của hôm này, chúng ta có thể viết một hàm mói dựa theo hàm log cũ: 

```python
function log(level, time, message){
  console.log(level + " - " + time + " - " + messsage);
}

function logErrToday(message){
  log("Error", "Today", message);
}

logErrToday("Server die."); // Error - Today - Server die
```
Thay vì viết như thế, mình có thể viết đơn giản hơn bằng cách dùng _bind_. Ở đây _log_ là function cũ, _logErrToday_ là function mới, được tạo ra bằng cách gán mặc định 2 tham số level và time.
```python
function log(level, time, message){
  console.log(level + " - " + time + ": " + message);
}

// Không có this nên set this là null.
// Set mặc định 2 tham số level và time 
let logErrToday = log.bind(null, "Error", "Today");

// Hàm này tương ứng với log("Error", "Today", "Server die.")
logErrToday("Server die.");
// result: Error - Today: Server die.
```
  #### Call và Apply, tuy 2 mà 1, thấy 1 mà 2
  Hai hàm này nằm trong prototype của Function (Function.prototype), do đó chỉ function mới có thể gọi. Chúng có chung một chức năng lại: Gọi 1 function, xác  định tham số this, truyền các tham số còn lại.
  
  Điểm khác nhau là _apply_ truyền vào một array chứa toàn bộ các tham số, còn _call_ truyền lần lượt từng tham số.
  Để dễ nhớ: "A là một Array, C là nhiều Cục"
  Ví dụ đơn giản về call và apply: 
  ```python
    // Tim max bằng cách gọi Math.max
    Math.max(4,3,2,10);
    
    // Thay vì gọi trực tiếp hàm Math.max, ta có thể dùng call
    // Set this bằng null
    Math.max.call(null, 4,3,2,10);
    
    // Apply tương tự call, nhưng không truyền lần lượt.
    Math.max.apply(null, [4, 3, 2, 10]);
  ```
 _call_ và _apply_ thường được dùng để mượn hàm (borrowing function).
 ```python
 function test(firstParam, secondParam, thirdParam){
  let args = Array.apply(null, arguments);
  console.log(args);
 }
 
 test(1, 2, 3); // [1, 2, 3]
 ```
 
 Luư ý arguments là một biến cục bộ trong function, chứa toàn bộ các tham số được truyền vào. arguments là một object giống array nhưng không phải Array. arguments giống Array vì có field length, có thể truy cập các giá trị nó chứa thông qua index 0, 1, 2. Tuy nhiên, do arguments không phải là Array nên nó không thể gọi các hàm của Array.prototype..
 Do đó, ta phải sử dụng call/ apply để mượn một số hàm trong Array.prototype, các hàm này sẽ trả ra một array cho ta xử lý. Dòng code phía trên chuyển object arguments thành một array.
 
 Ngoài ra, _call_ và _apply_ còn được dùng để monkey-patching hoặc tạo spy. Ta có thể mở rộng chức năng của một hàm mà không cần sửa source code của hàm fđó. Ví dụ ta có hàm accessWeb của object computer.
 ```python
let computer = {
  accessWeb: (site) => console.log("Go to: " + site);
};

computer.accessWeb("google.com"); // Go to google.com
 ```
  Sử dụng call, ta có thể ghi thêm log trước và sau khi hàm accessWeb được gọi mà không can thiệt vào code của hàm đó.
```python
let computer = {
  accessWeb: (site) => console.log("Go to: " + site)
};

let oldFunction = computer.accessWeb;
// Tráo function accessWeb bằng hàm mới.
computer.accessWeb = () => {
  console.log("Con gà bắt đầu vào web");
  oldFunction.apply(this,arguments); // Giữ nguyên hàm cũ.
  console.log("Con gà đã vào web");
}

computer.accessWeb("google.com");
// Con gà bắt đầu vào web
// Go to: google.com 
// Con gà đã vào web
```
Call/Apply và _bind_ cũng ít người rành, đọc xong có thể lòe thiên hạ.
Do ít người rành nên dùng nó khi viết code phải chú thích vào.

##VI ES6 JavaScript
ES6 là chuẩn mới của JavaScript, cung cấp một số tính năng mới cho JavaScript, đồng thời giúp code tường minh và dễ viết hơn.

###1. Arrow (Lambda Expression)
  Ở ES6, thay vì khai báo function theo kiểu thông thường ,ta có thể sử dụng =>. Cách khai báo này tương tự như Lambda Expression trong C#, giúp cho code tường minh và ngắn hơn rất nhiều.
```python
let numbers= [1,2,3,4,5,6,7,8,9,10];
// Giả sử ta muốn tìm số chẵn
// Cách viết cũ
let odd = numbers.filter(function(n) { return n% 2 == 1});
console.log(odd);

// Với arrow
odd = numbers.filter(n => n%2 ==1);
console.log(odd);
```
  Ngoài ra, nhờ có arrow, ta không còn bị tình trạng this bị bind nhầm
```python
let person = {
  firstName: "Minh Hai",
  lastName: "Bui",
  friends: ["Minh", "Sang", "khoa", "Hoang"],
  showName: function() {
    this.friends.forEach((fr) {
      // với cách viết cũ, this ở đây sẽ là object window, không phải là person
      console.log(this.firstName + " hava a friend " + fr);
    });
  }
  // ta sử dụng arrow, this vẫn là object person
  this.friends.forEach(fr => console.log(this.fristName + " have a friend " + fr));
}
```

###2. Default parameter, destructuring, spread operator
  Default paremeter đã có từ lâu trong C, C++ , C#, giờ JavaScript cũng đã có... Java thì vẫn chưa có. Nhờ default parameter, ta có thể xác định giá trị mặc định của tham số truyền vào.
```python
// Cách cũ, phải check htam só truyền vào rồi xác định giá trị
function multiply(a,b){
  let b  = typeof b !== 'undefined' ?  b : 1;
  return a*b;
}

// Với ES6, chỉ cần sử dụng dấu = 
function multiply(a,b = 1){
  return a*b;
}
multiply(5); // 5
```

Destructuring cũng là một tính năng khá hay, nó cho phép ta "phân rã" các phần tử trong một array hoặc 1 object
```python
// Với array
let foo =["one", "two", "three"];

// Cách cũ
let one = foo[0];
let two = foo[1];
let three = foo[2];
// Cách mới [one,two,three] = foo;

// với object
let obj = {
  firstName:"Minh Hai", 
  lastName: "Bui"
};
// Cách cũ
let firstName = obj.firstName;
let lastName = object.lastName;
// dùng destructuring
let {firstName, lastName} = obj;
// Nếu muốn lấy tên biến khác tên field của object
let {
  firstName: fn,
  lastName:ln
} = obj;
// fn: Minh Hai, ln = Bui
```
###3. Cải tiến syntax class và object
Trong ES6, đã có class.
class có hỗ trợ constructor, get/ set, việc kế thừa cũng rất dễ thực hiện bằng từ khóa extends
```python
class Animal {
  construcotr(name){
    this.name = name;
  }
  
  speak(){
    console.log(this.name + " makes a noise.");
  }
}

class Dog extends Animal{
  speak(){
    console.log(this.name +" barks.");
  }
}
```
  Với cách khai báo object mới, ta có thể viết code một cách ngắn gọn và rõ ràng hơn nhiều.
```python
let firstName = "Minh Hai";
let lastName ="Bui";
// Khai báo object cũ
let obj = {
  firstName: firstName,
  lastName: lastName,
  showName() {console.log(this.firstName + " " + this.lastName)
};
// Khai báo kiểu mới, ngắn gọn hơn, 
// Không cần lặt lại tên biến hay function
var obj = {
  firstName, 
  lastName,
  showName() {console.log(this.firstName + " " + this.lastName);}
};
```
###4. Iterator 
for...of để duyệt từng phần tử trong mảng (đừng nhần lẫn với for...in để duyệt các trường trong 1 object nha)
```python
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9];
// Cách cũ, duyệt từ đầu
for(let i = 0; i<numbers.length; i++){
  console.log(numbers[i]);
}

// Dùng forEach 
numbers.forEach((number) => console.log(number);

// Dùng for... of, để dễ viết dễ đọc
for(var number of numbers) console.log(number);
```
###5. Template String
JavaScript không có string.format, do đó phải cộng chuỗi bằng tai rất cực. Giờ đây, sử dụng template, string, ta không cần phải mất công cộng chuỗi nữa, code rõ ràng hơn nhiều.
```python
let name ="Bob";
let time = "today";
// Cách cũ
console.log("Hello " + name + " how are you + " time" + "?");
// Dùng string interpolation,
console.log(`Hello ${name}, how are you ${time}`);
```
###6. Map and Set
Map là cấu trúc dữ liệu cho phép ta lưu dữ liệu dưới dạng key-value, set là một mảng mà trong đó không có phần tử nào trùng nhau.
```python
// Sets
let s  = new Set();
s.add("hello").add("goodbye").add("hello");
s.size === 2;
s.has("hello) === true;

// Maps
let m = new Map();
m.set("hello",42);
m.set(s,34);
m.get(s) === 34;
```

###7. Promise
Promise là một khái niệm hay, giúp cho việc code asynchonous (bất đồng bộ) dễ dàng hơn. Promise được sử dụng trong rất nhiều thư viện như JQuery, AngularJS.

## Promise??? 
### Promise được dùng khá nhiều ở cả front end và back-end(Nodejs), do đó phải nắm vững khái niệm này sẽ giúp bạn rất nhiều trong việc code và trả lời phỏng vấn.

### Lập trình bất đồng bộ trong JavaScript
Ai từng làm AjAX đều biết rằng JavaScript kết nối theo server theo cơ chế bất đồng bộ, các hàm phía sau không đợi hàm AJAX chạy xong mà tiếp tục chạy luôn.
```python
  let thuHang = ajax.get("thuHangdethuong.img");
  console.log(thuHang);
  // Hàm AJAX chạy bất đồng bộ, do đó giá tri thuHang là undefined.
```
Do đó, để lấy kết quả của hàm ajax, ta phải truyền cho nó một callback. Sau khi hàm AJAX lấy được kết quả, nó sẽ gọi hàm callback vói kết quả thu được.
```python
let callback = (image) => console.log(image);
ajax.get("thuhang", callback);

// Có thể viết ngắn gọn như sau
ajax.get("thu-hang-de-thuong", (image) => console.log(image));
```
Cách làm này có gì không ổn? Sử dụng callback chồng chéo sẽ làm code trở nên rối rắm, khó đọc. Việc bắt exception, hiển thị lỗi cũng trở nên phức tạp.
```python
// do những hàm dưới chạy bất đồng bộ, bạ không thể lấy dữ liệu kiểu lần lượt kiểu này
let xe = xin_me_mua_xe();   // Chờ cả năm mới có xe
let gai = cho_gai_di_choi(xe);  // Lấy xe chở gái đi chơi
let abcd = cho_gai_vao_hotel(y);  // Đi chơi chở gái đi đâu đó

// Mà phải sử dùng đống callback  này, tạo thành callback HEO (hell)
xin_me_mua_xe(
  (xe) => {
    cho_gai_di_choi((xe,gai)=>{
      cho_gai_vao_hotel((hotel, z) =>{
        //do something
      });
    });
  }
);
```

### Promise là gì???
The promise object is used for asynchronous computations. A promise represents an operator that hasn't completed yet, but is expected in the future.
Đại loại là Promise dùng cho tính toán xử lý bất đồng bộ. Một lời hứa đại diện cho một hoạt động chưa hoàn thành, nhưng dự kiến sẽ hoàn thành trong tương lai.

Để đễ hiểu, hãy gọi Promise là lời hứa. Tương tự như trong thực tế, có người hứa rồi làm, có người hứa rồi... thất hứa.

Một lời hứa có 3 trạng thái sau:
- pending: Hiện lời hứa chỉ là lời hứa suông, còn đang chờ người khác thực hiện.
- fulfilled: Lời hứa đã được thực hiện.
- reject: Bạn đã bị thất hứa, hay còn gọi là bị "xù"

Ví dụ: Khi xưa, để dụ bạn cố gắng học hành, bố mẹ bảo: "Ráng đậu đại học, bố mẹ sẽ mua cho con BMW đi học cho bằng bạn bằng bè". Lúc này, thứ bạn nhận được là một lời hứa, chứ không phải là BMW.
```python
function hứa_cho_có(){
  return Promise((thuc_hien_loi_hua, that_hua) =>{
    // Sau 1 thời gian dài oi là dài dài và daifiiiiiiiiiiiiiiiii.
    // Nếu bố mẹ vui sẽ thực hiện lời hứa.
    if(vui){
      thuc_hien_loi_hua("Xe BMW");
      // Lúc này trạng thái lời hứa là fulfilled.
    } else {
      that_hua("xe dap");
      // Lúc này trạng thái của lời hứa là rejected.
    }
  });
}

// Lời hứa bây giờ đang là penđing
// Nếu được thực hiện, bạn có "Xe BMW"
// Nếu bị reject, bạn có "Xe Đạp"
let promise = hứa_cho_có();
promise
  .then((xe_bmw) = > {
    console.log("Được chiếc BMW vui quá");
  })
  .catch((xe_dap) =>{
    console.log("Được chiếc xe đạp ...");
  });
```

Khi lời hứa được thực hiện, promise sẽ gọi callback trong hàm then. Ngược lại, khi bị thất hứa, promise sẽ gọi callback trong hàm catch.
Một ví dụ khác
```python
function get(url){
  return new Promise((resolve, reject) => {
    // Lấy tiền từ money.com
    // Nếu bị lỗi thì đành thất hứa
    if(error) reject("Error");
    // Nếu lấy được thì thực hiện lời hứa
    resolve(money);
  });
}

let promise = ajax.get("money.com");

// Có tiền thì mua mac không có tiền thì thôi.
promise
  .then(money => mac)
  .catch(error => alert(error));
```
### Uả, cũng là dùng callback thôi ???
Promise hay ở những điểm sau: 
1. Promise sẽ hỗ trọ "Chaining"
2. Promise sẽ giúp bắt lỗi dễ dàng hơn.
3. Xử lý bất đồng bộ 

###1. Promise chaining
Giá trị trả về của hàm then là 1 promise khác. Do đó ta có thể dùng promise gọi liên tiếp các hàm bất đồng bộ. Có thể viết lại callback hell ở trên như sau:
```python
// dùng callback heo 
xin_me_mua_xe((xe) => {
    cho_gai_di_choi((xe,gai)=>{
      cho_gai_vao_hotel((hotel, z) =>{
        //do something
      });
    });
  }
);

// Dùng promise, code gọn nhẹ dễ đọc
xin_me_mua_xe
  .then(cho_gai_di_choi)
  .then(cho_gai_vao_hotel)
  .then(() => { Do something });
```
###2. Bắt lỗi với promise
Trong ví dụ trên, ta gọi lần lượt 3 hàm:
xin_me_mua_xe, cho_gai_di_choi, cho_gai_vao_hotel

Chỉ cần 1 trong 3 hàm này bị lỗi, promise sẽ chuyển qua trạng thái reject. Lúc này callback trong hàm catch sẽ được gọi. Việc bắt lỗi trở nên dễ dàng rất nhiều.

```python
function cho_gai_vao_hotel() => {
  return new Promise((response, reject) => {
    reject("xin lỗi hôm nay em đèn đỏ");
  }); 
}
xin_me_mua_xe
  .then(cho_gai_di_choi)
  .then(cho_gai_vao_hotel)
  .then(() => { // do something })
  .catch(err => {
    console.log(err); // Xin lỗi hôm nay em đèn đỏ
    console.log("xui vl");
  });
```

###3. Xử lý bất đồng bộ
Giả sử bạn muốn 3 hàm AJAX khác nhau. Bạn cần kết quả trả về của 3 hàm này trước khi tiếp tục chạy. Với callback, việc này sẽ khó thực hiện. Tuy nhiên, promise hỗ trợ hàm Promise.all, cho phép gộp kết quả của nhiều promise lại vói nhau.
```python
// Ba hàm này phải được thực hiện "cùng lúc"
// Chứ không phải là lần lượt.

let sờ_trên = new Promise((resolve, reject) => {
  resolve("Phê trên");
});

let sờ_dưới = new Promise((resolve, reject) => {
  resolve("Phê dưới");
});

let sờ_tùm_lum = new Promise((resolve, reject) => {
  resolve("Phê tùm lum");
});

// Truyền 1 array chứa toàn bộ promise vào hàm Promise.all
// Hàm này trả ra 1 promise, tổng hợp kết quả của các promise đưa vào
Promise.all([sờ_trên, sờ_dưới, sờ_tùm_lum])
  .then((result) =>
    console.log(result)); // ["Phê trên", "Phê dưới", "Phê tùm lum"]
```

## Sự bá đạo của ASYNC/ AWAIT trong JavaScript
### Nhắc lại kiến thức
JavaScript là ngôn ngữ single-thread, tức là chỉ có một thread duy nhất để thực thi các dòng lệnh. Nếu chạy theo cơ chế đồng bộ (synchonous) thì khi thực hiện tính toán phức tạp, gọi AJAX request tới server, gọi database (trong NodeJS), thread này sẽ dừng để chờ, làm toàn bộ trình duyệt bị ... treo.

Để tránh điều này, hầu hết code gọi AJAX request hoặc database trong JavaScript đều chạy theo cơ chế bất đồng bộ (asynchonous). Ban đầu, việc chạy code asynchonous trong JavaScript được thực hiện nhờ callback (như đoạn code dưới đây)
```python
let callback = function(image) => console.log(image);
ajax.get("HangDeThuong.info", callback);

// Có thể viết gọn như sau
ajax.get("ThuHangDeThuong.info", (image) => console.log(image));
```
Tất nhiên, vì callback có một số nhược điểm như code dài dòng, callback hell,... Nên người ta tạo ra 1 pattern mới gọi là Promise!

### Từ callback, promise đến Async/ Await
Promise đã giải quyết khá tốt những vấn đề của callback. Code trở nên dễ đọc, tách biệt và dễ bắt lỗi hơn.

  Tuy nhiên, dùng promise đôi khi ta vẫn thấy hơi khó chịu v ì phải truyền callback vào hàm then và catch. Code cũng sẽ hơi dư thừa và khóa debug, vì toàn bộ các hàm then chỉ được tính là 1 câu lệnh nên không debug riêng từng dòng được. => Sự ra đời của async / await.
  
  Vậy async/await có gì hay ho? Chúng giúp ta viết code có vẻ đồng bộ (synchonous), nhưng thật ra lại chạy bất đồng bộ (asynchonous).
  ví dụ: 
  ```python
  //--- code javascript
  function findRandomImgPromise(tag){
    const apiKey = "a89c66e48519481ab448a3f8356e635c";
    const endpoint = `https://api.giphy.com/v1/gifs/random?api_key=${apiKey}&tag=${tag}`;
    return fetch(endpoint)
      .then(rs => rs.json())
      .then(data => data.data.fixed_width_small_url);
  }
  
  $("#request").click(async () => {
    const imgUrl = await findRandomImgPromise('cat');
    $("#cat").attr("src", imgUrl);
  });
  
  //----- code html
  <button id="request"> get random animal</button>
  <br/>
  <img id="cat"/>
  <img id="dog"/>
  <img id="fish"/>
  ```
  hàm findRandomImgPromise là hàm bất đồng bộ, trả về một Promise. Với từ khóa _await_, ta có thể coi hàm này là đồng bộ, câu lệnh phía sau chỉ được chay sau khi hàm này trả về kết quả.
  
### Tại sao nên dùng async/await
nó có một số ưu điểm vượt trộ so với promise:
- Code dễ đọc hơn rất rất nhiều, không cần then rồi catch gì hết, chỉ viết như code chạy tuần tự, sau đó dùng try/catch thông thường để bắt lỗi.
- Viết vòng lặp qua từng phần tử trở nên vô cùng đơn giản, chỉ việc await trong mỗi vòng lặp.
- Debug dễ hơn nhiều, vì mỗi lần dùng await được tính là một dòng code, do đó ta có thể đặt debugger để debug từng dòng như bình thường.
- Khi có lỗi, exception sẽ chỉ ra lỗi ở dòng số máy chứ không chung chung như là un-resolve promise.
- Với promise hoặc callback, việc kết hợp if/else hoặc retry với code asynchonous là một cực hình vì ta phải viết code lòng vòng, rắc rối. Với async/await, việc này vô dùng dễ dàng.
get source example here
https://codepen.io/minh-hi-the-styleful/pen/VJOXyw

### Bất cập của async/await
Tất nhiên, async/await cũng có một số bất cập mà cần phải lưu ý trước khi sử dụng:
- Không chạy được trên các trình duyệt cũ. Nếu dự án yêu cầu phải chạy trên các trình duyệt cũ, bàn phải dùng Babel để transpliler code ra ES5 để chạy
- Khi ta await một promise bị reject, JavaScript sẽ throw một Exception. Do đó, nếu dùng async await mà quên try catch là thi lâu lâu chúng ta sẽ bị... nuốt lỗi hoặc code ngừng chạy. 
- async await bắt buộc phải đi kèm với nhau! await chỉ dùng đượctrong hàm async, nếu không sẽ bị syntax error. Do đó, async await sẽ lann dần ra toàn bộ các hàm trong code của bạn

### Áp dụng async/await vào code
Về bản chất, một hàm async sẽ trả ra một promise, tương ứng với Task trong C#. Do vậy, để có thể dùng async await một cách hiệu quả, chúng ta phải nắm rõ cơ chế làm việc của Promise.
Ngoài ra nếu dùng Nodejs, có thể sử dụng combo Promisify + Async/Await như sau:
1. Sử dụng Bluebird hoặc util.promisify (node 8 trở lên) để biến các hàm callback của NodeJs thành Promise.
2. Dùng async/await để lấy kế quả từ các Promise này.

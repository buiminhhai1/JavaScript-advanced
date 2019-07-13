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
 

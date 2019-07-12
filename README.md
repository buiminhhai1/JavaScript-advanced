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

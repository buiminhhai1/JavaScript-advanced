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


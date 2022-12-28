# Hướng dẫn làm game flappy bird

> **Requirement (Yêu cầu kiến thức)**

- **Canvas**
- [ ] drawImage
- [ ] clearRect
- [ ] getContext('2d')
- [ ] beginPath
- [ ] Coordinates
- **Data type**
- [ ] String (split, toString,...)
- [ ] Integer
- [ ] Array (splice, push,...)
- **Loop**
- [ ] For
- [ ] Foreach
- **Event**
- [ ] click
- [ ] keyup
- [ ] load
- **Condition**
- [ ] if else
- **Class**
- [ ] constructor
- [ ] method
- **Object**
- **Function**
- [ ] arrow function
- [ ] normal function
- **Method Support**
- [ ] requestAnimationFrame
- [ ] setInterval
- [ ] setTimeout
- **HTML**
- [ ] image
- [ ] canvas
- **Keywor**
- [ ] this
- [ ] let, var, const
- **Operator**
- [ ] ++, %, /, +, -, =, !=,...

> **Idea**

- Clone lại game flappy bird
- Game là 1 vòng loop thay đổi hình ảnh mỗi giây (**frame**) như 1 bộ phim
- Dùng setTimeout, requestAnimationFrame,... để lặp lại liên tục
- Mỗi lần lặp ta thực hiện các thay đổi như: di chuyển đối tượng, kiểm tra va chạm, xử lý a, xử lý b,...
- Dựa vào các thay đổi đó vẽ lại frame theo các thay đổi

## B1: Vẽ màn start game

### Tạo vòng loop cho game

> **Idea**

- Dùng setTimeout, requestAnimationFrame,... để lặp lại liên tục

_Mã giả_

```js
requestAnimationFrame(run);
```

### Vẽ background

> **Idea**

- Đặt kích thước canvas (chiều rộng, chiều dài)
- Tạo biến ảnh background
- Dùng hàm **drawImage** vẽ ảnh background vào canvas theo kích thước ảnh

_Mã giả_

```js
canvas.width = width;
canvas.height = height;
ctx.drawImage(background)
```

### Vẽ hướng dẫn chơi

> **Idea**

- Tạo biến ảnh hướng dẫn
- Dùng hàm **drawImage** vẽ ảnh hướng dẫn vào giữa canvas theo kích thước ảnh

_Mã giả_

```js
ctx.drawImage(tutorial, canvas.width / 2)
```

### Vẽ nền đất

> **Idea**

- Tạo class Ground
- Tạo biến ảnh nền đất
- Dùng hàm **drawImage** vẽ ảnh nền đất vào canvas theo kích thước ảnh

_Mã giả_

```js
ctx.drawImage(ground)
```

### Vẽ bird

> **Idea**

- Tạo class Bird
- Tạo biến ảnh bird
- Dùng hàm **drawImage** vẽ ảnh vào canvas theo kích thước ảnh

_Mã giả_

```js
ctx.drawImage(bird)
```

## B2: Xử lý animation bird

### Xử lý animation

> **Idea**

- Tạo các biến ảnh hiển thị các trạng thái của bird
- Dựa vào frame ảnh để thay đổi các ảnh liên tục cho nhau
- Vẽ lại ảnh bird với trạng thái mới

_Mã giả_

```js
if(frame % 16 == 0) changeImage();
if(frame % 36 == 0) changeImage();
ctx.drawImage(newBirdImage)
```

## B3: Di chuyển nền đất

### Xử lý hiệu ứng di chuyển nền đất

> **Idea**

- Mỗi frame thay đổi vị trí **x** của nền đất 1 khoảng **a** từ phải qua trái nào đó
- Vẽ lại nền đất với vị trí mới

_Mã giả_

```js
loop() {
  ground.x -= a;
  ctx.drawImage(ground);
}
```

> **Issue**

- Chiều rộng của nền đất có giới hạn
=> khi di chuyển nền đất bị mất không thể hiển thị được liên tục

> **Resolve**

- Tạo và vẽ mảng các đối tượng nền đất liên tiếp nhau
- Khi có đối tượng nền đất di chuyển ngoài màn hình thì xử lý xóa
- Sau khi xóa, thêm và vẽ tiếp 1 đối tượng nền đất vào cuối mảng

_Mã giả_

```js
var arrGround = [ground1, ground2, ground3, ground4];

if(ground1.x < -ground1.width){
  // remove ground1
}

var newGround = new Ground();
arrGround.push(newGround);

// draw again
}
```

## B4: Vẽ đường ống

### Vẽ đường ống trên

> **Idea**

- Tạo class Pipe
- Tạo biến ảnh đường ống trên
- Dùng hàm **drawImage** vẽ đường ống vào canvas vị trí ở trên

_Mã giả_

```js
ctx.drawImage(pipeTop, canvas.width, -pipeTop/2)
```

### Vẽ đường ống dưới

> **Idea**

- Tạo biến ảnh đường ống dưới
- Dùng hàm **drawImage** vẽ đường ống vào canvas vị trí ngay dưới dường ống trên

_Mã giả_

```js
ctx.drawImage(pipeBottom, canvas.width, -pipeTop/2 + pipeTop)
```

### Xử lý khoảng cách giữa 2 ống

> **Idea**

- Tạo hàm random giá trị giữa 2 số min và max
- Tăng vị trí **y** của ống dưới 1 khoảng random với ống trên

_Mã giả_

```js
function random(min, max){
  //handle
}

ctx.drawImage(pipeBottom, canvas.width, -pipeTop/2 + pipeTop + random(min, max))
```

### Vẽ random vị trí

> **Idea**

- Tư duy tính toán xử dụng lại hàm random để tạo các vị trí xuất hiện ngẫu nhiên của 2 ống trên và dưới

_Mã giả_

```js
function random(min, max){
  //handle
}

var x = random(min, max);
var y = random(min, max);

ctx.drawImage(pipe, x, y)
```

## B5: Di chuyển đường ống

### Xử lý hiệu ứng di chuyển ống

> **Idea**

- Mỗi frame thay đổi vị trí **x** của cả 2 ống trên và dưới 1 khoảng **a** từ phải qua trái nào đó
- Vẽ lại 2 ống với vị trí mới

_Mã giả_

```js
loop() {
  pipeTop.x -= a;
  pipeBottom.x -= a;
  ctx.drawImage(pipeTop);
  ctx.drawImage(pipeBottom);
}
```

> **Issue**

- Mới có 1 ống được chạy

> **Resolve**

- Tương tự giống nền đất
  - Tạo mảng
  - Kiểm tra vượt màn hình
  - Xóa đối tượng đầu
  - Thêm đối tượng mới vào cuối mảng

## B6: Hiển thị điểm

### Vẽ điểm lên màn hình

> **Idea**

- Tạo class Score
- Tạo các biến tương ứng với các hình số và mảng chứa các số đó
- Tạo biến điểm
- Dựa vào giá trị biến điểm thực hiện xử lý các ảnh số vẽ lên canvas

_Mã giả_

```js
var score = 0;
var arrNumber = [];

class Score {
  constructor(){
    this.value = score;
  }

  draw() {
    // xử lý vẽ điểm
  }
}
```

## B7: Di chuyển đối tượng bird

### Xử lý trọng lực

> **Idea**

- Giống như đời thực xử lý bird tự động rơi theo trọng lực **g** nhanh dần mỗi frame 1 khoảng **v** (velocity):
**v = v + g**

_Mã giả_

```js
var v += g;
brid.y += v;
ctx.drawImage(brid);
```

### Xử lý sự kiện nhấn click

### Xử lý sự kiện nhấn space

### Xử lý độ cao

> **Idea**

- Mỗi khi nhấn space hoặc click tăng độ cao **y** của bird bằng các set lại giá trị **v < 0**

_Mã giả_

```js
function click() {
  brid.v = -7;
}
```

## B8: Kiểm tra va chạm

### Kiểm tra va trạm với nền đất

> **Idea**

- Kiểm tra vị trí **y** của bird + **chiều cao bird** với chiều cao của nền đất
- Nếu chạm thì game kết thúc

_Mã giả_

```js
if(check){
  // gameover
}
```

### Kiểm tra va trạm với ống theo chiều x

> **Idea**

- Kiểm tra vị trí **x** của bird + **chiều rộng bird** với vị trí **x** của ống
- Kiểm tra vị trí **x** của bird với vị trí **x** của ống + **chiều rộng ống**
- Nếu chạm thì game kết thúc

_Mã giả_

```js
if(check){
  // gameover
}
```

### Kiểm tra va trạm với ống theo chiều y

> **Idea**

- Kiểm tra vị trí **y** của bird với vị trí **y** của ống trên + **chiều cao ống**
- Kiểm tra vị trí **y** của bird + **chiều cao bird** với vị trí **y** của ống dưới + **chiều cao ống** + **khoảng cách giữa 2 ống**
- Nếu chạm thì game kết thúc

_Mã giả_

```js
if(check){
  // gameover
}
```

> **Hint**:
_Vẽ ra giấy hoặc ứng dụng bất kỳ kích thước bird, kích thước ống để tưởng tượng dễ hơn_

## B9: Xỷ lý điểm người chơi

### Kiểm tra bird vượt qua ống

### Xử lý tăng điểm người chơi

> **Idea**

- Kiểm tra vị trí **x** của bird với vị trí **x** của ống + **chiều rộng ống**
- Nếu vượt qua thì cộng điểm

_Mã giả_

```js
if(check){
  // score++;
}
```

## B10: Vẽ màn end game

### Vẽ màn end game

> **Idea**

- Tạo biến ảnh gameover
- Dùng hàm **drawImage** vẽ ảnh gameover vào canvas theo kích thước ảnh

_Mã giả_

```js
ctx.drawImage(gameover)
```

## B11: Xử lý chơi lại

> **Idea**

- Đặt lại mọi thông số giống như game mới

### Xử lý vị trí ban đầu của bird

### Xử lý vận tốc ban đầu

### Xử lý điểm

### Xử lý đường ống

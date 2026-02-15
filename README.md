# 1. Chuẩn bị 

* 1 x Arduino Uno.
* 1 x Module Joystick 
* 1 x Servo SG90.
* Dây cắm Male to Male

# 2. Đấu nối dây

Cái này quan trọng nhất là đừng cắm ngược cực động cơ, cắm ngược là nó nóng ran rồi cháy đấy.

**A. Đấu Joystick:**

* **GND**  Arduino GND
* **+5V**  Arduino 5V
* **VRx**  Chân **A0** (Để đọc trục ngang)
* **VRy**  Chân **A1** (Để đọc trục dọc - nếu muốn dùng thêm servo nữa)
* **SW**  Bỏ qua (đây là nút bấm khi nhấn cần xuống, chưa cần dùng).

**B. Đấu Servo SG90:**

* **Dây Nâu (hoặc Đen)**  GND
* **Dây Đỏ**  5V
* **Dây Cam (hoặc Vàng)**  Chân **9** (Chân phát xung PWM).

### 3. Code điều khiển (Copy và nạp)

Nguyên lý rất đơn giản:

1. Đọc giá trị Joystick (từ 0 đến 1023).
2. Quy đổi (Map) giá trị đó sang góc quay Servo (từ 0 đến 180 độ).
3. Gửi lệnh cho Servo quay.

```cpp
#include <Servo.h> // Thư viện có sẵn của Arduino

Servo myServo;  // Tạo đối tượng servo

const int joyX = A0;   // Chân đọc trục X của Joystick
const int servoPin = 9; // Chân điều khiển Servo

int joyVal;   // Biến lưu giá trị đọc được từ Joystick
int angle;    // Biến lưu góc quay (0-180)

void setup() {
  myServo.attach(servoPin); // Kết nối servo vào chân số 9
  Serial.begin(9600);       // Bật Serial để xem giá trị nếu cần
}

void loop() {
  // 1. Đọc giá trị từ Joystick (khoảng từ 0 đến 1023)
  joyVal = analogRead(joyX);
  
  // 2. Chuyển đổi giá trị Joystick sang góc quay Servo
  // Joystick: 0 ------- 512 ------- 1023
  // Servo:    0 độ ---- 90 độ ---- 180 độ
  angle = map(joyVal, 0, 1023, 0, 180);
  
  // 3. Điều khiển Servo quay
  myServo.write(angle);
  
  // 4. In ra màn hình để kiểm tra (vào Tools -> Serial Monitor)
  Serial.print("Joystick: ");
  Serial.print(joyVal);
  Serial.print(" | Goc quay: ");
  Serial.println(angle);
  
  // Delay nhỏ để servo kịp phản hồi và tránh bị giật cục
  delay(15); 
}

```

# 창의공학 양방향 IoT 통신

# 아두이노
void setup() {
Serial.begin(9600);
pinMode(13,OUTPUT);
}
double th(int v) {
double t;
t = log(((10240000/v) - 10000));
t = 1 /(0.001129148 + (0.000234125*t) + (0.0000000876741*t*t*t));
t = t - 273.15; // 화씨를 섭씨로 바꾸어줌
return t;
}
void loop() {
int a = analogRead(A0);
Serial.println(th(a));
int b = (Serial.readString()).toInt(); 
digitalWrite(13,HIGH);
delay(b);
digitalWrite(13,LOW);
delay(b);
}

# 프로세싱
import processing.serial.*;
import processing.net.*;

Serial p;
Server s;
Client c;
void setup() {
s = new Server(this, 1234);
p = new Serial(this, "COM8", 9600);
}
String rm;
void draw() {
c = s.available();
if (c!=null) {
String m = c.readString();
if (m.indexOf("PUT")==0) {
int n = m.indexOf("\r\n\r\n")+4;
m = m.substring(n);
m += '\n';
print(m);
p.write(m);
}
else if (m.indexOf("GET")==0) {
s.write("HTTP/1.1 200 OK\r\n");
s.write("Content-length: " + rm.length() + " \r\n\r\n");
s.write(rm);
}
}
if (p.available()>0) {
String m = p.readStringUntil('\n');
if (m!=null) {
rm = m;
print(m);
}
}
}

# 앱인벤터
![1](https://user-images.githubusercontent.com/119797340/205497566-ca0324c1-3f25-4704-aa1f-1ba04ba862f6.PNG)
![2](https://user-images.githubusercontent.com/119797340/205497568-2ef9a298-503b-419f-b1c6-ff0277c3e65a.PNG)


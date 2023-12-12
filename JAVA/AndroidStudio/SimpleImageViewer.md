# 간단한 이미지 뷰어

### 커스텀 위젯을 직접 만들어 activity_main_xml에 넣어 사용
```java
public class class myPictureView extends View{
public myPictureView(Context context, @Nullable AttributeSet attrs){
super(context, attrs);
}
}
```
    
### onDraw() 메소드 오버라이딩 : 화면에 VIew가 나타날 때 자동 실행
```java


public class class myPictureView extends View{
String imagePath = null;
public myPictureView(Context context, @Nullable AttributeSet attrs){
 super(context, attrs); 
 }
@Override
protected void onDraw(Canvas canvas) {
super.onDraw(canvas);
if(imagePath != null){
Bitmap bitmap = BitmapFactory.decodeFile(imagePath);
canvas.drawBitMap(bitmap, 0, 0, null); 
bitma.recycle();
}
}
```
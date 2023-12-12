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

### 커스텀 위젯인 myPictureView 생성, 버튼 추가

```xml
<LinearLayout>
 <LinearLayout 
   android:Orientation=“horizontal”>
<Button 
  android:id=“@+id/btnPrev”
  android:layout_weight=“1”
  android:text=“ 이전그림”/>

<Button 
 android:id=“@+id/btnNext”
 android:layout_weight=“1”
 android:text=“다음그림” />
<LinearLayout>
<com.cookandroid.project8_2.myPictureView
 android:id=“@+id/myPictureView1”/>
</LinearLayout>

```

### 위젯변수 3개, SD카드에서 읽어올 이미지 파일 배열과 파일명 문자열 변수,위젯 변수에 activity_main.xml 위젯 대입

```java
public class Mainactivity extends Activity{
Button btn Prev, btnNext;
myPictureView myPicture;
int curNum = 0l
File[] imageFiles = new File[0];
String imageFname;

@Override 
public void onCreate(Bundle savedInstanceState){
super.onCreate(savedInstanceState);
setContentview(R.layout.activity_main);
setTitle(“간단 이미지 뷰어”);
ActivityCompat.requestPermission(this, new String[] {
android,Manifest.permission.WRITE_EXTERNAL_STORAGE},MODE_PRIVATE);
btnPrev = (Button) findViewById(R.id.btnPrev);
btnNext = (Button) findViewById(R.id.btnNext);
myPicture = (myPictureView) findViewById(R.id.myPictureView1);
}
}

myPicture = (myPictureView)findviewById(R.id.myPictureView1);
File[] allFiles = new File(Environment.getExternalStorageDirectory().getAbsolutePath()
+“/Pictures”).listFiles();
for (int I = 0; i<allFiles.length.length; I++)
 if(allFiles[i].isFile()) {
imagesFiles = Arrays.copyOf(imagesFiles, imageFiles.length + 1);
imageFiles[imageFiles.length-1] = allFiles[i];
}
imageFname = imageFiles[curNum].toString();
myPicture.imagePath=imageFname;
}
}
```
### 버튼을 클릭하면 동작하는 리스너

```java


btnPrev.setOnClickListener(new View.OnClickListener(){
public void onClick(View v){
if (curNum <= 1) {
Toast.makeText(getApplicationContext(),“첫번째 그림입니다”, Toast.LENGTH_SHORT)
.show();
}else {
curNum --;
imageFname = imageFiles[curNum].toString();
myPicture.imagePath = imageFname;
myPicture.invalidate();
}
});

btnNext.setOnClickListener(new View.OnClickListener(){
public void onClick(view v){
if(curNum >= imageFiles.length –1){
Toast.makeText(getApplicationContext(),“마지막 그림입니다”,Toast.LENGTH_SHORT)
.show();
}else{
curNum ++;
}
}
}

```